# Documentation and Tutorials

**[Concepts](#concepts) | [SDK](#device-sdk) | [cloud service](#ubirch-cloud-services) | [Examples](#examples-and-tutorials)**

## Concepts

The **Blockchain for Things** is a trust infrastructure for data. It is a solution for ensuring the  integrity 
and authenticity at the packet level of communication. Additionally, with the integration into common blockchain 
technology, temporal integrity of data is guaranteed.

The solution consists of a number of components at the [device firmware](#device-sdk) level, a 
[data protocol](#the-ubirch-protocol) definition, [cloud services](#ubirch-cloud-services) and 
integration with external services. 

The individual components enable customer infrastructure to “seal” data packets, send these packets over secured or 
unsecured transmission channels without compromising integrity and use the cloud services to verify integrity, order, 
omission or duplication and authenticity of individual data packets or streams.

### The ubirch-protocol

The ubirch protocol is an integral part of the data communication between ubirch-enabled devices, services and the ubirch 
cloud services. It is a data format definition with the minimal required information to secure the integrity and 
authenticity of the data as well as providing information about the packet order to detect duplication and omission of 
packets in a data stream.

The protocol effectively consists of an envelope for the data transmission. This envelope “seals” and secures the payload 
it contains. The envelope is a conceptional one, though. In the actually data being exchanged between the sender 
and receiver, payload and envelope do not have to be transmitted together or in the same channel. In many applications 
it might be desirable (and is supported), to transmit the payload and the envelope/seal separately. This is accomplished 
by using a cryptographic hash function to create a digest of the original data packet and only transmitting the digest as 
the payload within the ubirch protocol envelope.
 
Separating data and cryptographic seal enables customers to integrate the ubirch trust infrastructure without touching 
the original data pipeline.

Implementations of the ubirch-protocol can be found in the [Device SDKs](#device-sdk). 
The [C/C++](https://github.com/ubirch/ubirch-protocol) implementation contains a [detailed technical description](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format).

## Device SDK

ubirch provides implementations for different platforms. The embedded SDK is available for [mbed OS 5](https://mbed.com),
[ESP32](https://www.espressif.com/en/products/hardware/esp32/overview), and [Bosch XDK](https://xdk.bosch-connectivity.com/home). The implementation should be generic enough to be portable to other C/C++ based platforms.

Additionally, for more powerful systems, a Python implementation and an SDK for node.js is available.

- **[C/C++](https://github.com/ubirch/ubirch-protocol)** (mbed, ESP32, Bosch XDK, generic)
- **[Python](https://github.com/ubirch/ubirch-protocol-python)** (Python 3)
- **[JavaScript](https://github.com/ubirch/ubirch-protocol-js)** (node.js, browser)

If no platform SDK is provided, using the [ubirch-protocol](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) definition and the API documentation allows the use of the backend
infrastructure without a specialized implementation. 

## ubirch cloud services

The ubirch cloud services are provided through a number of [APIs](http://developer.ubirch.com/docs/api/) that allow the 
registration of keys, verification of data and seals and accessing and forwarding of data of your data.

> While some of the endpoints accept JSON data, it is recommended to use the [msgpack](https://msgpack.org/index.html).
> The byte oriented msgpack format prevents issues when creating the data hash and signatures of the data packets.

- [Data/Verification API](http://developer.ubirch.com/docs/api/swagger-ui.html?url=https://raw.githubusercontent.com/ubirch/ubirchApiDocs/master/swaggerDocs//ubirch/avatar_service/1.0/ubirch_avatar_service_api.json) (v1.0)
  
  The data verification API accepts ubirch-protocol messages for key registration, signed data packets, chained data 
  packets and verifies these before forwarding to any further processing. Additionally, it provides an interface
  to verify payload hashes and retrieve the corresponding seals.
    
- [Notary API](http://developer.ubirch.com/docs/api/swagger-ui.html?url=https://raw.githubusercontent.com/ubirch/ubirchApiDocs/master/swaggerDocs//ubirch/notary_service/1.0/ubirch_notary_service_api.yaml) (v1.0)

  Internally, the ubirch cloud services use the notary API for anchoring data in the blockchain. While we recommend to 
  only anchor trusted data in a blockchain, this service is available standalone. 

> ⚠ Before using the cloud services, register at our developer [backend portal](https://ubirch.dev.ubirch.com).
 
## Examples and Tutorials



----------------------

&copy; 2018 ubirch GmbH | [ubirch.com](https://ubirch.com) | [Imprint](http://ubirch.de/impressum/)
