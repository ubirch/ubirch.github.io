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

```C++
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

```C++
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

## JavaScript

*work in progress*