
## Privacy Considerations

Given that Indy is a publicly readable, immutable ledger, no personally identifiable information, including DIDs where a person is the DID Subject, should be placed on the network. As this DID method does not offer yet any means to delete or deactivate personal information (e.g. in the sense of GDPR), it is important to enforce these rules by organisational means, for example through an Endorser Transaction Agreement or other contractual agreements.

The further privacy properties are stated according to [Section 5 of RFC6973](https://tools.ietf.org/html/rfc6973#section-5).

### Surveillance
The DIDs and their resolved DID Documents are  public readable and therefore the content and the changes of the data are inherently suspectable to surveillance.
Furthermore, authors of read and write requests can be surveillanced by their communication to the ledger nodes. Clients sending write requests can be observed very well identified, as they are identified by the author DIDs and signatures. However, read requests are unsigned and only need to be send to one node, therfore offering choice and better protection from surveillance for the client.

### Stored Data Compromise
The compromise of stored data on the ledger is prevented by the distributed, signed and consensus-based storage of the data. The stored data of an individual ledger node shall be protected by implementing best practice in securing the IT infrastructure, like ISO27001 and Information Security Management systems (ISMS).

### Unsolicited Traffic
DID Documents can be resolved from a DID, however the DID subject can choose to include or exclude service endpoints that expose itself to unsolicited traffic. The nodes of the ledger itself are exposed to any unwanted traffic as explained in the Denial-of-Service section.

### Misattribution
DIDs are self-certifying and immutable, the control flow of the ledger nodes prevents any misattributation given that it is implemented correctly.

### Correlation
The Hyperledger Indy ecosystem with Anoncreds 1 was designed to prevent correlation by design. No DIDs nor any data or credentials of natural persons are stored on the ledger, the revocation system guarantees a high degree of anonymity.
However, DID's on the ledger, used by organizations, enable correlation by the pseudonymous DID iteself or through data like service endpoints from the resolved DID Document as described in the DID-Core specification. Ledger nodes are prohibited to collect any metadata of (read) requests to the ledger to prevent correlation.

### Identification
The Hyperledger Indy ecosystem prevents identification of natural persons as they do not have a DID on the ledger. Identification of DIDs through the data of the resolved DID Document is possible and usually desired as issuing or verifying organizations want to authentically disclose their identity.

### Secondary Use
As all data written to the ledger is inherently public, clients sending write requests should be aware of possible secondary use and cautiously decide whether data appropriate data is published. No personal data shall be send to the ledger.

### Disclosure
Disclosure of data send to the ledger is not an issue, as written data is public anyway. Ledger nodes are prohibited to collect any metadata of (read) requests to the ledger to prevent disclosure.

### Exclusion
Any read request to the ledger nodes in unauthorized preventing exclusion to the ledger data.

