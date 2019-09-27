# Ubirch Trust Protocol

The Ubirch Trust Protocol (UTP) is an integral part of the data communication between ubirch-enabled devices, services and the ubirch
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
the original data pipelines.

## How it Works in a Nutshell

Short overview of the protocol. Check the [details](#utp-details) for more.

### The UTP
The **Ubirch Trust Protocol** (UTP) consists of many chained Ubirch Protocol Packages (UPP).
![UTP](img\UTP.png)

### The UPP
A **Ubirch Protocol Package** (UPP) consist of two mayor blocks, the UPP-DATA and the SIGNATURE of the UPP-DATA.
![UPP](img\UPP.png)

* **UPP-DATA:**
  * **VERSION:** Version of the protocol used
  * **UUID:** Device ID
  * **PREV-SIG:** The signature of the previous UPP, to chain the packages
  * **TYPE:** The type of DATA
  * **HASH(DATA):** Hash value of the customer data / payload itself
* **SIGNATURE:**
  * **SIG(HASH(UPP-DATA)):** Signature of the Hash of the current UPP-DATA

### UPP Chaining
As already mentioned, UPP chaining is achieved by adding the signature of the previous message to the current one.
![UPP Chained](img\UPP Chained.png)


## UTP Implementations
Implementations of the UTP can be found in the [Ubirch Nano Client](sdk), which is available in different languages, like C or Python.

## UTP Details
The [C/C++](https://github.com/ubirch/ubirch-protocol) implementation contains a [detailed technical description](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format) regarding the definition and structure of the protocol.

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
