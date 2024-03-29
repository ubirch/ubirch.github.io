# Ubirch

Ubirch is a trust infrastructure for ESG and other data. It is a solution for ensuring the integrity
and authenticity at the packet level of data communication. Additionally, with the integration into common blockchain
technology, temporal integrity of data is guaranteed. The cryptographic integrity checks allow for a deeper automation in data-driven processes like ESG data collection and verification.

## Components

The Ubirch solution consists of a number of components:

* The **[Ubirch Trust Protocol](utp)** (UTP) definition.
* The **[Ubirch Nano Client](sdk)**, a lightweight **SDK** at thing/device firmware level.
* The **[Ubirch Trust Service](api)**, a **REST API** for handling public keys, anchoring and verifying data.
* The **[Ubirch Console](console)**, an administrative web interface for managing things identities.
* The **[Ubirch Certify App](/ubirch-certify-app/)**, Web-App to create Certificates in the UBIRCH Backend, e.g. UPPs (UBIRCH Protocol Packages) or DCCs (Digital Corona Certificates)
* The **[Ubirch Verification Widget](https://developer.ubirch.com/ubirch-verification-js/)**, a JavaScript package to verify UBIRCH Protocol Packages (UPPs)

all of them are documented with the developer pages.

The individual components enable customer infrastructure to “seal” data packets, send these packets over secured or
unsecured transmission channels without compromising integrity and use the simple cloud services to verify integrity, order,
omission or duplication and authenticity of individual data packets or streams.

## About Ubirch GmbH

Founded in 2014, ubirch has offices in Cologne, Berlin and Munich, where it services global customers. Named “Cool Vendor” by
Gartner, ubirch is a market leader in Data Integrity Services with a Focus on ESG Data collection. Find out more at [ubirch.com](https://ubirch.com)

___

&copy; 2019 ubirch GmbH : [ubirch.com](https://ubirch.com) : [Imprint](http://ubirch.de/impressum/)
