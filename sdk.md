# Ubirch Nano Client

Ubirch provides implementations for different platforms.

For small, embedded devices the SDK is available in:
* [C/C++](#cc) for different ARM based platforms like ESP32 or Bosch XDK
* [MicroPython](#micropython) for MicroPython enabled MCUs (uses the SIM)

For more powerful systems like routers, gateways or even PCs and phones the SDK is available in:
* [Python](#python)
* [GoLang](#golang)
* [Java](#java)

In addition there is also an implementation for *SIM Cards* - please contact us for more information on this.

## C/C++

* **Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol)
* **Example:** [C/C++ Example Code](https://github.com/ubirch/ubirch-protocol#api)

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

> To get started with development, see the [C/C++ Example](https://github.com/ubirch/ubirch-protocol#chained-message-example).

## MicroPython

* **Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-sim/tree/master/micropython)
* **Example:** [TestKit with SIM](https://github.com/ubirch/ubirch-testkit)

The MicroPython example uses the SIM nano client and wraps the API.

## Python

* **Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-python)
* **Example:** [Python Example Code](https://github.com/ubirch/ubirch-protocol-python/tree/master/examples)

For aggregators and Linux based systems, a Python client is available. This client supports packaging and verification, 
software encryption and additionally provides easy access to the [ubirch API](api). The python implementation is by
far the quickest way to [get started](https://github.com/ubirch/ubirch-protocol-python/blob/master/examples/example-client.py) 
with the ubirch-protocol.

You can download and install the python library using [pip](https://pypi.org/project/pip/):
```
pip install ubirch-protocol
```

## GoLang

* **Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-go)
* **Example:** [Go Client Example](https://github.com/ubirch/ubirch-protocol-go/main) / [Go SIM Client](https://github.com/ubirch/ubirch-protocol-sim/tree/master/go)

The Go client can be cross compiled to many different architectures and can easily be used on industrial controllers.

## Java

* **Source Code:** [GitHub](https://github.com/ubirch/ubirch-protocol-java)
* **Example:** [Scala Client Example](https://github.com/ubirch/ubirch-client-scala)

The Java implementation is mostly used for backend purposes and can be used to construct a JVM based client.

## Other

If no platform SDK is provided, using the [Ubirch Trust Protocol](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) definition and the [API documentation](api) allows the use of the backend infrastructure without an existing specialized implementation.

Also check the **[Examples](examples)** for more.

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
