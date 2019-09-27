# Ubirch Nano Client

Ubirch provides implementations for different platforms.

For small, embedded devices the SDK is available in:
* [C/C++](#cc) for different ARM based platforms like ESP32 or Bosch XDK

For more powerful systems like routers, gateways or even PCs and phones the SDK is available in:
* [Python](#python), and
* [JavaScript](#javascript).

In addition there is also an implementation for *SIM Cards* - please contact us for more information on this.

## C/C++
**Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol)
**Example:** [C/C++ Example Code](examples)

The [C/C++ implementation](https://github.com/ubirch/ubirch-protocol) can be used with ARM based platforms like

* [ARM mbed OS](https://mbed.com),
* [ESP32](https://www.espressif.com/en/products/hardware/esp32/overview), and
* [Bosch XDK](https://xdk.bosch-connectivity.com/home).

It's a generic C/C++ implementation that provides the protocol-layer packaging and response verification and required encryption
libraries for ARM processors. Other hardware platforms might require a port of the [Ed25519](https://ed25519.cr.yp.to/)
algorithm or a hardware supported implementation.

This library does not provide networking functionality, as the different platforms have varying support for sending
and receiving data. The library has been successfully used using Bluetooth ([trackle](trackle.de)), Wifi
([ESP32](https://github.com/ubirch/example-esp32), HTTP/S) and generic modems.   

This implementations should be generic enough to be portable to other C/C++ based platforms.

> To get started with development, see the [C/C++ Example](examples#cc).


## Python
**Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-python)
**Example:** [Python Example Code](examples)

For aggregators and Linux based systems, a Python client is available. This client supports packaging and verification, software encryption and additionally provides easy access to the [ubirch API](api). The python implementation is by
far the quickest way to [get started](examples#python-client) with the ubirch-protocol.

You can download and install the python library using [pip](https://pypi.org/project/pip/):
```
pip install ubirch-protocol
```

## JavaScript
> THE JAVASCRIPT IMPLEMENTATION IS WORK IN PROGRESS.

**Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-js)
**Example:** [JavaScript Example Code](examples)

For [node.js](https://nodejs.org), browser, mobile apps, etc. With the JavaScript implementation it is possible to enable hybrid mobile apps and web pages to send messages using the Ubirch Trust Protocol. Special care has to be taken when using this library to ensure the safety of keys and the data.


## Other

If no platform SDK is provided, using the [Ubirch Trust Protocol](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) definition and the [API documentation](api) allows the use of the backend infrastructure without an existing specialized implementation.

Also check the **[Examples](examples)** for more.

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
