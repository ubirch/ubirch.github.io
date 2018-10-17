# ubirch - Documentation

The **Blockchain for Things** is a trust infrastructure for data. It is a solution for ensuring the  integrity 
and authenticity at the packet level of communication. Additionally, with the integration into common blockchain 
technology, temporal integrity of data is guaranteed.

The solution consists of a number of components at the [device firmware](sdk) level, a 
[data protocol](#the-ubirch-protocol) definition, [cloud services](cloud-services) and 
integration with external services. 

The individual components enable customer infrastructure to “seal” data packets, send these packets over secured or 
unsecured transmission channels without compromising integrity and use the cloud services to verify integrity, order, 
omission or duplication and authenticity of individual data packets or streams.

## The ubirch-protocol

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

Implementations of the ubirch-protocol can be found in the [Device SDKs](sdk). 
The [C/C++](https://github.com/ubirch/ubirch-protocol) implementation contains a [detailed technical description](https://github.com/ubirch/ubirch-protocol/blob/master/README.md#basic-message-format).

## About ubirch

Founded in 2014, ubirch has offices in Cologne and Berlin, where it services global customers. Named “Cool Vendor” by
Gartner, ubirch is a market leader in IoT security leveraging Blockchain.
