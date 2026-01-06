## JSON or JSON-LD

The choice of whether a DID resolves to a JSON or JSON-LD document is up to the DID Controller.

- If the original Indy [[ref: NYM]] object on the ledger for the DID has a `diddocContent` data item, and that data item contains an `@context` item, the DIDDoc will be JSON-LD. 

- In case of using Indy Besu network and `context` has been defined for DID Document object, the DIDDoc will be JSON-LD.

Otherwise, the resolved DIDDoc will be JSON.

The Indy ledger does not validate the JSON-LD on writing a DID object to the ledger. It is the responsibility of the DID Controller to ensure that a JSON-LD DIDDoc is valid JSON-LD.
