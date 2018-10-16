# Examples and Tutorials

## Before you start

Head over to the [ubirch backend portal](https://ubirch.dev.ubirch.com) and [register](cloud-services#registration) yourself to get access to the
backend services. While you could start sending data directly between two devices and do the verification by hand,
using the ubirch backend brings you an intermediate step that verifies the signatures, handles the identities 
(public keys), and provides streaming and blockchain services for verified data.

You will need to register to receive an authorization token for using the [API](http://developer.ubirch.com/docs/api/swagger-ui.html?url=https://raw.githubusercontent.com/ubirch/ubirchApiDocs/master/swaggerDocs//ubirch/avatar_service/1.0/ubirch_avatar_service_api.json)

## C/C++

The [C/C++ implementation](https://github.com/ubirch/ubirch-protocol) is a generic implementation and only depends on 
the availability of the required crypto functions ([Ed25519](https://ed25519.cr.yp.to/)) and the 
[msgpack-c](https://github.com/msgpack/msgpack-c) library.

The ubirch-protocol follows the coding paradigm of the msgpack-c implementation:

1. create a buffer to write to (`msgpack_sbuffer_new()`)
2. initialize the protocol and the msgpack packer (`proto`, `pk`)
3. write the protocol header (`ubirch_protocol_start()`)
4. write payload (`msgpack_pack_*()`)
5. finish the protocol (`ubirch_protocol_finis()`)
6. send the data from the buffer 
7. clean up (`msgpack_sbuffer_clear()`, `ubirch_protocol_free()`)
 
A simple example to send a _single signed message_:

```c
msgpack_sbuffer *sbuf = msgpack_sbuffer_new();
ubirch_protocol *proto = ubirch_protocol_new(proto_chained, 0, sbuf, msgpack_sbuffer_write, ed25519_sign, UUID);
msgpack_packer *pk = msgpack_packer_new(proto, ubirch_protocol_write);
ubirch_protocol_start(proto, pk);
msgpack_pack_int(pk, 99);
ubirch_protocol_finish(proto, pk);
msgpack_packer_free(pk);
ubirch_protocol_free(proto); 
``` 

The corresponding __chained message__, where we connect subsequent messages using their signature is shown below:

```c
msgpack_sbuffer *sbuf = msgpack_sbuffer_new();
ubirch_protocol *proto = ubirch_protocol_new(proto_chained, 0, sbuf, msgpack_sbuffer_write, ed25519_sign, UUID);
msgpack_packer *pk = msgpack_packer_new(proto, ubirch_protocol_write);

ubirch_protocol_start(proto, pk);
msgpack_pack_raw(pk, strlen(TEST_PAYLOAD));
msgpack_pack_raw_body(pk, TEST_PAYLOAD, strlen(TEST_PAYLOAD));
ubirch_protocol_finish(proto, pk);

msgpack_sbuffer_clear(sbuf);

ubirch_protocol_start(proto, pk);
msgpack_pack_raw(pk, strlen("CHAINED"));
msgpack_pack_raw_body(pk, "CHAINED", strlen("CHAINED"));
ubirch_protocol_finish(proto, pk);

msgpack_packer_free(pk);
ubirch_protocol_free(proto); 
```

## Python Client
[Python client](https://github.com/ubirch/ubirch-protocol-python) is a [Python3](https://www.python.org/) implementation
of ubirch protocol.

The library consists of three parts which can be used individually:

* `ubirch.API` - a python layer covering the ubirch backend REST API
* `ubirch.Protocol` - the protocol compiler which packages messages and handles signing and verification
* `ubirch.KeyStore` - a simple key store based on [pyjks](https://pypi.org/project/pyjks/) to store keys and certificates

**Example usage**

> A full, runnable example is located [here](https://github.com/ubirch/example-python).

### Setting up `api`, `keystore` and `protocol`

```python
import ubirch 

# minimal ubirch protocol impl that allows you to send signed messages
class Proto(ubirch.Protocol):
    def __init__(self, keystore):
        super().__init__()
        self.__ks = keystore
    def _sign(self, uuid, message):
        return self.__ks.find_signing_key(uuid).sign(message)
        

keystore = ubirch.KeyStore("example-keystore.jks", "example-password")
api = ubirch.API("<YOUR_AUTH_TOKEN>", "dev")

protocol = Proto(keystore)
```

### Initializing the keysore and registering the keys in ubirch backend

```python
keystore.create_ed25519_keypair(identity_uuid)
reg_message = protocol.message_signed(identity_uuid, UBIRCH_PROTOCOL_TYPE_REG,
                                      keystore.get_certificate(identity_uuid))
registration_resp = api.register_identity(reg_message)
```

### Creating the device

```python
device_create_resp = api.device_create({
    "deviceId": str(device_uuid),
    # device types available at Avatar service's endpoint /device/deviceType
    "deviceTypeKey": device_type,  
    "deviceName": device_name,
    # you can put group uuids here, so other users see the device
    "groups": ["db1488ae-becc-40a3-a5c2-b6daadd6715b"],
    "hwDeviceId": str(device_hwid),
    "tags": ["python-example", "python-client"],
    "deviceProperties": {
        "storesData": True,
        "blockChain": False
    },
    "created": "{}Z".format(datetime.utcnow()
                        .strftime('%Y-%m-%dT%H:%M:%S.%f')[:-3])
})
```

### Sending messages

In general, sending messages looks like this:
```python
message = protocol.message_*(<uuid>, <message_type>, <payload>)
response = api.send(message)
```
Here are some examples with concrete payload types:
* `0x32` - a measurement starting with a timestamp...
```python
message = protocol.message_chained(device_uuid, 0x32, 
    [posix_timestamp_micros, 42, 1337])
resp = api.send(message)
```

* ... or an array of such measurements
```python
message = protocol.message_chained(device_uuid, 0x32, [
    [posix_timestamp_micros, 42, 1337],
    [int(posix_timestamp_micros + 1e6), 7, 666]
])
resp = api.send(message)
```

* `0x53` - generic sensor message - just send a json
```python
message = protocol.message_chained(device_uuid, 0x53, 
    {"message": "Hello World!", "foo": 42})
resp = api.send(message)
```

* `0x00` - binary message
```python
message = protocol.message_chained(device_uuid, 0x00, b"just some bytes")
resp = api.send(message)
```

* a message that's not chained to the previous messages
```python
message = protocol.message_signed(device_uuid, 0x00, b"some other bytes")
resp = api.send(message)
```

### Sealing and verifying integrity of messages

This is useful when you want to verify the integrity of messages, but don't want to share 
the messages themself with ubirch. 
```python
payload = b"<YOUR_SENSITIVE_PAYLOAD>"
payload_hash = hashlib.sha512(payload).digest()  # use hashing function of your choice
message = protocol.message_chained(device_uuid, UBIRCH_PROTOCOL_TYPE_BIN, payload_hash)
seal_response = api.send(message)

send_to_your_own_backend(payload)
```

And then in your backend, to verify the validity of the received message
```python
payload = receive_sensitive_message()
payload_hash = hashlib.sha512(payload).digest()
hash_b64 = bytes.decode(base64.b64encode(payload_hash))

from urllib.parse import quote
import requests

response = requests.get(
    "https://api.ubirch.{}.ubirch.com/api/avatarService/v1/device/verify/{}"
    .format("<YOUR_UBIRCH_ENV>", quote(received_message_hash, safe="+=")), 
    headers=api._auth
)
    
is_message_valid = response.ok
```

## JavaScript

*work in progress*