# Ubirch C/C++ Example

> A full, runnable example for the ESP32 platform is located [here](https://github.com/ubirch/example-esp32).

The [C/C++ implementation](https://github.com/ubirch/ubirch-protocol) is a generic implementation and only depends on
the availability of the required crypto functions [ubirch-mbed-nacl-cm0](https://github.com/ubirch/ubirch-mbed-nacl-cm0)
(for documentation, see [Ed25519](https://ed25519.cr.yp.to/)) and the [ubirch-mbed-msgpack](https://github.com/ubirch/ubirch-mbed-msgpack) library (for documentation, see [msgpack-c](https://github.com/msgpack/msgpack-c)).

The ubirch-protocol follows the coding paradigm of the msgpack-c implementation:

1. create a protocol context and initialize it with UUID and signing callback (`ubirch_protocol_new()`). 
The protocol context contains a buffer where the protocol package (UPP) will be written to.
1. generate a UPP with your payload (`ubirch_protocol_message()`)
1. send the data from the buffer
1. clean up (`ubirch_protocol_free()`)

A simple example to send a __single signed message__:

```c
// create a ubirch protocol context
ubirch_protocol *upp = ubirch_protocol_new(UUID, ed25519_sign);

// pack a message
const char *msg = "message";    // exemplary data
ubirch_protocol_message(upp, proto_signed, UBIRCH_PROTOCOL_TYPE_BIN, msg, strlen(msg));

// SEND THE MESSAGE (upp->data, upp->size)

// free the protocol context
ubirch_protocol_free(upp);
```

The corresponding __chained message__, where we connect subsequent messages using their signature is shown below:

```c
ubirch_protocol *upp = ubirch_protocol_new(UUID, ed25519_sign);

// FIRST MESSAGE
ubirch_protocol_message(upp, proto_chained, UBIRCH_PROTOCOL_TYPE_BIN, "CHAINED", strlen("CHAINED"));

// SEND THE MESSAGE (upp->data, upp->size)

// SUBSEQUENT MESSAGE
ubirch_protocol_message(upp, proto_chained, UBIRCH_PROTOCOL_TYPE_BIN, "MESSAGE", strlen("MESSAGE"));

// SEND THE MESSAGE (upp->data, upp->size)

ubirch_protocol_free(upp);
```
> The __response verification__ is described in the  [example-esp32: message response verification](https://github.com/ubirch/example-esp32#message-response-evaluation)

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
