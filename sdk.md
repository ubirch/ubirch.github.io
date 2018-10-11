# Device SDK

ubirch provides implementations for different platforms. The embedded SDK is available for [mbed OS 5](https://mbed.com),
[ESP32](https://www.espressif.com/en/products/hardware/esp32/overview), and [Bosch XDK](https://xdk.bosch-connectivity.com/home). The implementation should be generic enough to be portable to other C/C++ based platforms.

Additionally, for more powerful systems, a Python implementation and an SDK for node.js is available.

- **[C/C++](https://github.com/ubirch/ubirch-protocol)** (mbed, ESP32, Bosch XDK, generic)
- **[Python](https://github.com/ubirch/ubirch-protocol-python)** (Python 3)
- **[JavaScript](https://github.com/ubirch/ubirch-protocol-js)** (node.js, browser)

If no platform SDK is provided, using the [ubirch-protocol](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) definition and the API documentation allows the use of the backend
infrastructure without a specialized implementation. 
