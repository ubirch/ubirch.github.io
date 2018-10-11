# Cloud Services

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
