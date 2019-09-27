# Ubirch Python Example

[Python client](https://github.com/ubirch/ubirch-protocol-python) is a [Python3](https://www.python.org/) implementation
of ubirch protocol.

The library consists of three parts which can be used individually:

* `ubirch.API` - a python layer covering the ubirch backend REST API
* `ubirch.Protocol` - the protocol compiler which packages messages and handles signing and verification
* `ubirch.KeyStore` - a simple key store based on [pyjks](https://pypi.org/project/pyjks/) to store keys and certificates

> A full, runnable example is located [here](https://github.com/ubirch/example-python).

## Setting up `api`, `keystore` and `protocol`

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

## Initializing the KeyStore and registering keys

```python
keystore.create_ed25519_keypair(identity_uuid)
reg_message = protocol.message_signed(identity_uuid, UBIRCH_PROTOCOL_TYPE_REG,
                                      keystore.get_certificate(identity_uuid))
registration_resp = api.register_identity(reg_message)
```

## Creating a device

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

## Sending messages

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

## Sealing and verifying integrity of messages

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

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
