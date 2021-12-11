## Finding Indy Ledgers

To connect and read or write from a Hyperledger Indy network instance, a client must have the configuration file (in Indy, called the "genesis" file) for the network. Given a `did:indy` DID (e.g. `did:indy:<namespace>:<namespace identifier>`), the Indy network instance on which the DID resides is known. However, there remains a challenge for the entity interested in resolving the DID&mdash;finding the genesis file for that network instance. The following documents two mechanisms resolvers can use to access required genesis files.

### Static

A client that will resolve Hyperledger Indy DIDs can be statically configured to "know" about a set of Indy networks by loading the files on startup. The files would be collected from the node operators in some way by those deploying the client software (e.g. an Aries Wallet).

When a static list of Indy networks is used, DIDs from other, organically discovered networks not on the list cannot be resolved by the client.

### Dynamic using GitHub

The Hyperledger Indy GitHub repo `indy-did-networks` enables Indy DID network operators to publish their network genesis files in a standard way. Within the repo, the folder "networks" contains a folder per primary network. Within each network folder is the genesis file for the primary network, and folders (containing a corresponding genesis file) for each subspace network. The naming format for the genesis files is:

- `pool_transactions_genesis.json`

For example, the Sovrin MainNet, StagingNet and BuilderNet genesis files will be in the repo as:

- `networks/sovrin/pool_transactions_genesis.json`
- `networks/sovrin/staging/pool_transactions_genesis.json`
- `networks/sovrin/builder/pool_transactions_genesis.json`

The committers to the repo for each network SHOULD include in the folder at least a README.md file with information about the network, plus any additional documents about the ledger instance, such as Governance Framework documents.

#### Client Usage

A client may retrieve selected network genesis files from the repo to use as their set of static files, as described in the [previous section](#static). The developers of the clients can monitor the repo for changes to the genesis files that they are using.

If a DID is obtained by the client that is from a network not already known by the client, the client MAY look for the unknown (to the client) network in the GitHub repo and decide to use (or not) the associated genesis file to connect to the network.

The security policy of the client (and perhaps the user of the client) might give options about handling unknown networks, such as:

- Never connect.
- Review and connect if permission from client operator granted.
- Always connect if the genesis file for the network can be found.

#### Repository Maintenance

Each contributing network instance operator maintains copies of their genesis files and supporting documents in the GitHub repo by submitting Pull Requests (PRs) to the repo. The community selected repo maintainers are expected to merge PRs with limited review based on their knowledge of the network operators. Their focus is not to provide editorial oversight but only to:

- prevent updates to a network's genesis file by other than known operator of the network, and
- prevent badly formatted genesis files from being added to the repository.

The maintainers are authorized to submit PRs to remove "bad actor" network folders based on notifications from the community and followup verification.

#### GitHub Update Disputes

Any disputes about the handling of PRs submitted to the repo should be escalated through the Indy Community (via the #indy channel on Hyperledger chat and/or at the [Indy Contributors](https://wiki.hyperledger.org/display/indy/Indy+Contributors+Meeting) call or its successor). If the issue is not resolved at the Indy level, the issue should be escalated to Hyperledger leadership (the Executive Director or the Technical Steering Committee).
