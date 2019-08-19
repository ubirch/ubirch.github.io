# Device SDK

ubirch provides implementations for different platforms. The embedded SDK is available for [ARM mbed OS](https://mbed.com),
[ESP32](https://www.espressif.com/en/products/hardware/esp32/overview), and [Bosch XDK](https://xdk.bosch-connectivity.com/home). The implementation should be generic enough to be portable to other C/C++ based platforms.

Additionally, for more powerful systems, a Python implementation and an SDK for node.js is available.

## C/C++ 

[Source Code](https://github.com/ubirch/ubirch-protocol) ([ARM mbed OS](https://mbed.com), [ESP32](https://www.espressif.com/en/products/hardware/esp32/overview), 
[Bosch XDK](https://xdk.bosch-connectivity.com/home), generic C/C++)

A generic implementation, that provides the protocol-layer packaging and response verification and required encryption 
libraries for ARM processors. Other hardware platforms require a port of the [Ed25519](https://ed25519.cr.yp.to/)
algorithm or a hardware supported implementation.

This library does not provide networking functionality, as the different platforms have varying support for sending
and receiving data. The library has been successfully implemented using Bluetooth ([trackle](https://trackle.de/en/home-en/)), Wifi 
([ESP32](https://github.com/ubirch/example-esp32), HTTP/S) and generic modems.   

> To get started with the development, see the [C/C++ Example](examples#cc). 
 
     
## Python
[Source Code](https://github.com/ubirch/ubirch-protocol-python)

For aggregators and Linux based systems, a Python client is available. This client supports packaging and verification.
software encryption and additionally provides easy access to the [ubirch API](api). The python implementation is by
far the quickest way to [get started](examples#python-client) with the ubirch-protocol. 

You can download and install the python library using [pip](https://pypi.org/project/pip/):
```bash
pip install ubirch-protocol
```
  
## JavaScript [WIP] 
[Source Code](https://github.com/ubirch/ubirch-protocol-js) ([node.js](https://nodejs.org), browser, mobile apps)

With the JavaScript implementation it is possible to enable hybrid mobile apps and web pages to send messages using the
ubirch-protocol. Special care has to be taken when using this library to ensure the safety of keys and the data.

> ⚠ THE JAVASCRIPT IMPLEMENTATION IS WORK IN PROGRESS.


---

If no platform SDK is provided, using the [ubirch-protocol](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) definition and the API documentation allows the use of the backend
infrastructure without a specialized implementation. 
