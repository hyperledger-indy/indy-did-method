
## `did:indy` DID Component Syntax

The following sections provide the syntax and ABNF for the two variable components of a `did:indy` DID, the namespace and namespace identifier.

### `did:indy` DID Namespace Syntax

The `did:indy` DID Namespace component MUST include a primary, human-friendly name of the Indy network instance, MAY include an optional ":" separator and subspace, human-friendly name, and MUST include a trailing ":" separator. The subspace name is used to identify an Indy instance related to the primary instance, such as the primary network's test or development instance. The ABNF for the namespace component is:

    namespace     = namestring (":" namestring) ":"
    namestring    = lowercase *(lowercase / DIGIT / "_" / "-")
    lowercase     = %x61-7A ; a-z

The namespace is set by the operator of the network. Although that could lead to namespace collisions, our 
[assumption about the expected number of Indy instances](#assumption-number-of-indy-instances) (low 100s at the most) eliminates that as a concern.

### `did:indy` DID Namespace Identifier Syntax

The namespace identifer is an identifier within the namespace of a Hyperledger Indy network that is unique for that namespace.

The namespace identifier (NSID) is defined by the following ABNF:

    NSIDstring    = 21*22(base58char)
    base58char    = "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9" / "A" / "B" / "C"
                  / "D" / "E" / "F" / "G" / "H" / "J" / "K" / "L" / "M" / "N" / "P" / "Q"
                  / "R" / "S" / "T" / "U" / "V" / "W" / "X" / "Y" / "Z" / "a" / "b" / "c"
                  / "d" / "e" / "f" / "g" / "h" / "i" / "j" / "k" / "m" / "n" / "o" / "p"
                  / "q" / "r" / "s" / "t" / "u" / "v" / "w" / "x" / "y" / "z"

The `NSIDString` is base58 encoded using the Bitcoin/IPFS alphabets of a 16-byte uuid. The encoding uses most alphas and digits, omitting 0 / O / I / l to avoid readability problems. This gives a NSID length of either 21 or 22 characters, and it means that identifiers are case-sensitive and may not be case-normalized, even though the prefix is always lower-case.

The namespace identifier MUST be derived from the initial `verkey` for the DID, as outlined in the [Creation](#creation) section of this document.