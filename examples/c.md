# Ubirch C/C++ Example

> A full, runnable example for the ESP32 platform is located [here](https://github.com/ubirch/example-esp32).

The [C/C++ implementation](https://github.com/ubirch/ubirch-protocol) is a generic implementation and only depends on
the availability of the required crypto functions [ubirch-mbed-nacl-cm0](https://github.com/ubirch/ubirch-mbed-nacl-cm0)
(for documentation, see [Ed25519](https://ed25519.cr.yp.to/)) and the [ubirch-mbed-msgpack](https://github.com/ubirch/ubirch-mbed-msgpack) library (for documentation, see [msgpack-c](https://github.com/msgpack/msgpack-c)).

The ubirch-protocol follows the coding paradigm of the msgpack-c implementation:

1. create a buffer to write to (`msgpack_sbuffer_new()`)
2. initialize the protocol and the msgpack packer (`proto`, `pk`)
3. write the protocol header (`ubirch_protocol_start()`)
4. write payload (`msgpack_pack_*()`)
5. finish the protocol (`ubirch_protocol_finish()`)
6. send the data from the buffer
7. clean up (`msgpack_sbuffer_clear()`, `ubirch_protocol_free()`)

A simple example to send a __single signed message__:

```c
msgpack_sbuffer *sbuf = msgpack_sbuffer_new();
ubirch_protocol *proto = ubirch_protocol_new(proto_signed, TYPE, sbuf, msgpack_sbuffer_write, ed25519_sign, UUID); //TYPE = 0
msgpack_packer *pk = msgpack_packer_new(proto, ubirch_protocol_write);
ubirch_protocol_start(proto, pk);
// ADD YOUR PAYLOAD DATA HERE
msgpack_pack_int(pk, 99); // exemplary data
//
ubirch_protocol_finish(proto, pk);
// SEND THE MESSAGE (sbuf->data, sbuf->size)
msgpack_packer_free(pk);
ubirch_protocol_free(proto);
```

The corresponding __chained message__, where we connect subsequent messages using their signature is shown below:

```c
msgpack_sbuffer *sbuf = msgpack_sbuffer_new();
ubirch_protocol *proto = ubirch_protocol_new(proto_chained, TYPE, sbuf, msgpack_sbuffer_write, ed25519_sign, UUID); //TYPE =  0
msgpack_packer *pk = msgpack_packer_new(proto, ubirch_protocol_write);

// FIRST MESSAGE
ubirch_protocol_start(proto, pk);
msgpack_pack_raw(pk, strlen(TEST_PAYLOAD));
msgpack_pack_raw_body(pk, TEST_PAYLOAD, strlen(TEST_PAYLOAD));
ubirch_protocol_finish(proto, pk);
// STORE THE CURRENT SIGNATURE (proto->signature, UBIRCH_PROTOCOL_SIGN_SIZE)
// SEND THE MESSAGE (sbuf->data, sbuf->size)
msgpack_sbuffer_clear(sbuf);

// SUBSEQUENT MESSAGE
memcpy(proto->signature, LAST_SIGNATURE, UBIRCH_PROTOCOL_SIGN_SIZE); // LOAD THE SIGNATURE INTO *proto
ubirch_protocol_start(proto, pk);
msgpack_pack_raw(pk, strlen("CHAINED"));
msgpack_pack_raw_body(pk, "CHAINED", strlen("CHAINED"));
ubirch_protocol_finish(proto, pk);
// STORE THE CURRENT SIGNATURE (proto->signature, UBIRCH_PROTOCOL_SIGN_SIZE)
// SEND THE MESSAGE (sbuf->data, sbuf->size)

msgpack_packer_free(pk);
ubirch_protocol_free(proto);
```
> The __response verification__ is described in the  [example-esp32: message response verification](https://github.com/ubirch/example-esp32#message-response-evaluation)

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
