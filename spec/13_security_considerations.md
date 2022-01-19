## Security Considerations

Hyperledger Indy is a public, permissioned distributed ledger that uses RBFT to establish a consensus between upfront well-authenticated nodes. The security mechanisms by indy-node and indy-plenum guarantee the correct processing of requests and transactions according to the rules, which are themselves part of the consensus on the ledger. In particular, this enables the creation and update of schemas, credential definitions and DIDs by their owners by authenticating with the corresponding public keys stored on the ledger.

Hyperledger uses [CurveZMQ](http://curvezmq.org/page:read-the-docs#toc3) to secure the communication between the ledger nodes and the clients as well as between the ledger nodes for the consensus. CurveZMQ is an adaptation of the low-level, TCP-based ZMQ communication protocol using the [CurveCP](http://curvecp.org/) to protect the communication layer. CurveCP was designed by Daniel J. Bernstein and uses the 256-bit Curve25519 and derived short-term session keys.

Hyperledger Indy uses a genesis file to securely authenticate the (initial) trusted nodes. This genesis file specifially includes the IP addresses and the long-term keys of the ledger nodes, as well as the DIDs and verification keys of the Trustees and Stewards. The ledger is initially started in a launch ceremony, where all th ledger nodes simultaniously start the indy-node software and ensure the correct startup. The information in the genesis file is sufficient for each client to securly connect to the ledger and gain trust in the transactions provided by the nodes.

The following sections decribe how the `did:indy` DID Method adheres to the security considerations outlined in the [DID Core 1.0 specification](https://w3c.github.io/did-core) and in accordance with RFC3552.

### Eavesdropping
The CurveZMQ protocol provides confidentiality and integrity by encrypting and authenticating each packet. 

### Replay
The CurveZQM protocol provides protection against replay attacks by including a unique nonce. An attacker cannot replay any packet except a Hello packet, which has no impact on the server or client. Any other replayed packet will be discarded by its recipient.
Additionally, the replies for read requests include a timestamp of the state proof of the consensus, therefore the client can check the freshness of the received data.

### Message Insertion and Modification
The CurveZMQ protocol prevents mailicous message insertion as all packets are authenticated and therefore protected against any form of forgery.
For write requests, the client signs the sensitive request content with his verification key to authenticate and therefore protect the data against malicious manipulation. 
Additionally, the consensus prevents forgery of the data returned from the ledger. The stateProof is a multisigned root hash of a merkle tree that includes all the ledger objects and is part of the node's reply.

### Man-In-The-Middle
The client knows the long-term public keyd of the ledger nodes from the trusted genesis file. The CurveZMQ protocol protects a Man-In-The-Middle that tries to impersonate the server (ledger node). CurveCP does not protect server from an attacker impersonating the client, as no client authentication is performed on the transport layer. However, the client authenticates on the application layer with by signatures with verification key for the senstive write requests.
Man-in-the-Middle attacks for the inter-node communication is also impossible, as both parties know each others long term public key.

### Deletion
Deletion of individual messages is not a security risk, as all both every request is answered with a signed response, therefore the lack of response will let the requesting party acknowledge the possible deletion of its message.

### Denial of Service
Several measures for protection against Denial of Service are taken into account for the indy ledger ecosystem:

 - CurveCP uses high-speed high-security elliptic-curve cryptography so that a typical CPU can perform public-key operations faster than a typical Internet connection can ask for those operations. The server does not allocate memory until a client sends the Initiate packet.
 - Separate network interfaces to prevent loss of node-to-node communications are advised
 - For read requests communication to only one ledger node is neccessary to get authentic ledger data as indy uses multisigned state proofs

### Storage or Network amplification
Amplification attacks to exhaust the storage of the ledger nodes can be easily mitigated, as only permissoned DIDs have write access and in case of massive malicious requests, these endorsement rights can be revoked.

Amplification attacks to exhaust the network bandwidth are protected to measures similar to section Denial of Service. Additionally, read requests are cheap to process for the ledger node, as the multisigned state proof is applicable to all requested ledger objects and no new signatures need be calculated except for transport layer security. CurveZMQ is also designed to ensure high efficiency and availability itself.

## Residual Risks
Residual risks for Hyperledger Indy include:

- compromise in the used cryptographic primitives (BLS-Signature, X25519, Ed25519)
- implementation bugs in the indy-node or indy-plenum software, especially as all nodes use the same software package
- external libraries

### Integrity protection and update authentication for method operations and write authorization

For all write operations that alter the ledger data(Create, Update, Deactivate) the appropriate signatures are needed, where the DIDs and `verkey`s of the signatories must be one the ledger. The client signs the body of the [write request](https://github.com/hyperledger/indy-node/blob/master/docs/source/requests.md#reply-structure-for-write-requests) (`txn`), depending on the configured leder rules, some transactions might require signatures from multiple DIDs. The Indy ledger processes the signed write request and the software enforces the integrity protection by checking the validity of the given signatures and their authorization. No authentication or authoriazion is required for read requests.

As Indy currently only provides a single `verkey` for authentication, this exposes a risk of loss of control if the  private key for the DID Controller is lost.

### Uniqueness of DIDs
DIDs on the Indy ledger are self-certifying, derived from the initial `verkey` of an Ed25519 key pair, which makes them extremely likely to be unique. Any attempt to write the same DID would only work if the signature matched (e.g. if the seed to create the DID had been lost so the literal same DID was attempted to be written), which would result in no change to the ledger, and goes against assumption of the DID Controller not protecting their seed/private key.

### Endpoint authentication
No endpoint authentication is used other than the trusted node keys from the genesis files.

### Protection of data
Transaction data on the ledger stored by the nodes is protected for integrity by maintaing cryptographically signed root hashes of the [merkle tree](https://github.com/hyperledger/indy-plenum/blob/master/docs/source/storage.md), that includes all transactions of the ledger. Data is not protected for integrity as all data is inherently public anyway.

### Protection of Key Material
The private keys associated with the public keys written to the ledger must be kept secret. Each DID Owner is responsible to store and use his private keys in a secure and sensitive way.

The ledger nodes are operating with the Validator BLS key, which is stored in software on the node. This private key and the seed used for private key generation is sensitive and can be rotated by using the Steward role of the node's operator. The Trustees and Stewards `verkey`s are highly sensitive and must be securly stored (ideally with Two-Factor-Authentication or in hardware) separate from the ledger. Custom ledger rules that require multiple Trustee and/or Steward signatures to change important configuration data of the ledger are stongly advised.

### Peer-to-Peer Computing resources
The RBFT algorithm was initially designed as a robust Concensus protocol that limits the effect of mailicous nodes. However, this requires some computations that limits the effiency and scalability of the consensus. It ist strongly advised to not increase the number of validator nodes above 25. Hyperledger Indy was designed with very limited number of write requests, but very efficient read requests (only one node needs to be queried). If scalability issues still arise, the concept of [Observer nodes](https://github.com/hyperledger/indy-plenum/blob/master/design/observers.md) can be implemented.

### New authentication methods
New authentication methods can be added to the DID Document by adding them to the `didDocContent` field. However, the only authentication method applicable to the ownership of Indy-realted DID (NYM), is the original `verkey`.
