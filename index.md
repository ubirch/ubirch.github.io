# Ubirch Developer Documentation

Ubirch is a blockchain based trust infrastructure for (IoT) data. It is a solution for ensuring the integrity
and authenticity at the packet level of data communication. Additionally, with the integration into common blockchain
technology, temporal integrity of data is guaranteed.

The Ubirch solution consists of a number of components:
* The **[Ubirch Trust Protocol](utp)** (UTP) definition.
* The **[Ubirch Nano Client](sdk)**, a lightweight client/**SDK** at thing/device firmware level.
* The **[Ubirch Trust Service](api)**, a **REST API** for handling public keys, anchoring and verifying data.
* The **[Ubirch Console](console)**, an administrative web interface for managing things identities.

The individual components enable customer infrastructure to “seal” data packets, send these packets over secured or
unsecured transmission channels without compromising integrity and use the simple cloud services to verify integrity, order,
omission or duplication and authenticity of individual data packets or streams.

## About ubirch

Founded in 2014, ubirch has offices in Cologne, Berlin and Munich, where it services global customers. Named “Cool Vendor” by
Gartner, ubirch is a market leader in IoT security leveraging Blockchain. Find out more at [ubirch.com](https://ubirch.com)

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
