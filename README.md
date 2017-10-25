# blockchain_in_a_nutshell
a simple summary of blockchain technology

## What is a block?
Consists of
* a block number
* a nonce (only in Proof of Work-Algorithms)
* data containing the transactions
* hash of the previous block
* hash of this block

## What is a block chain?
Blocks chained together, with each block pointing to its predecessor. The first block is called a “genesis block”. The chain grows with each transaction, and every participant has a copy of the whole chain. -> a public record of state changes.

### Principles behind Block Chain 
Resist mutability, decentralized, no need to keep track of which transactions were authorized in the past, persistence. Every participating node has a copy of the chain it participates in.

## How to validate transactions/”appending a block to the chain”? How do all the nodes agree on a version of the chain that is considered to be the truth? -> Consensus Algorithms!

### Consensus algorithms
* Proof of work
* Proof of stake
* PBFT (Practical Byzantine Fault Tolerance)
* Raft

### Byzantine Fault Tolerance
System tolerates the Byzantine Generals Problem. All nodes agree on a valid version of the chains’ history while not having to trust each other. A consens needs to be found using a consensus algorithm. Also, the history of the chain needs to be verifiable, so that every node can be sure the chain has not been messed around with. To further mitigate malicious attacks, nodes should run different implementations of the service code (as is possible with Etherium for instance, as multiple implementations written in different languages are provided).

### How to make sure the chain has not been fiddled with?
each block has a certain hash. Should the hash suddenly not meet the criteria mentioned above, the blocks content has changed and the copy of the chain is incorrect. Also, each block points to a previous block. The chain can be followed until the genesis block. If not, the hash where things went wrong can be found. Also multiple copies of the chain can be compared by simply looking at the last block and their hashes. Should all hashes be valid (meeting the criteria), but should one differ from all the others, this copy of the chain has been manipulated. These attacks however are very costly, as all blocks onwards from the manipulated one up to the most recent one would need to be rehashed/”mined”. Way too expensive.

### Proof of Work
creator of the next block has to do some work in order to validate transaction and create the next block (i.e. solving a cryptographical puzzle like choosing the nonce in a way that the hash of the block starts with a certain amount of zeros). Also called mining. Miners participating in validating blocks might get rewarded with a currency. Once the nonce is found, the miner announces the solution to the network and other members verify the solution. (in the case of cryptocurrencies/bitcoin they also check whether the transaction itself is valid and no double spending occurs. should the majority of participants agree, the transaction is deemed valid).

### Proof of Stake
creator of next block gets chosen by a combination of factors such as the stake and randomness.There is no puzzle to solve, but those participating in the validation receive a part of the transaction fee. If a participant choses incorrectly, the stake is lost or he is punished. Ensures that every participant aims to work on the “correct” (ie the longest) version of the chain and everyone agrees on it.

### Proof of Work vs Proof of Stake
POS is more cost-effective - way less computing power is needed to verify a transaction, and thereby, less electrical power. In POW, should a single entity possess more than 51% of the networks computing power, the chain could be critically influenced. POS may also not be optimal - nothing at stake problem.

### RAFT
Nodes can either be followers, leaders, or candidates to become a leader. Elections among the nodes are held to determine leaders. Leaders communicate with the clients and broadcast state changes to all followers that replicate the changes themselves.

## Ethereum

### Accounts

In Ethereum, the state is made up of objects called "accounts", with each account having a 20-byte address and state transitions being direct transfers of value and information between accounts. An Ethereum account contains four fields: 
* The nonce, a counter used to make sure each transaction can only be processed once 
* The account's current ether balance
* The account's contract code, if present
* The account's storage (empty by default)
 
In general, there are two types of accounts: externally owned accounts, controlled by private keys, and contract accounts, controlled by their contract code.

#### Externally owned accounts (EOAs)
An externally controlled account
* has an ether balance
* can send transactions (ether transfer or trigger contract code)
* is controlled by private keys
* has no associated code

#### Contract accounts
A contract 
* has an ether balance
* has associated code
* code execution is triggered by transactions or messages (calls) received from other contracts
* when executed - perform operations of arbitrary complexity (Turing completeness) - manipulate its own persistent storage, i.e., can have its own permanent state - can call other contracts.

### What is a transaction?
The term “transaction” is used in Ethereum to refer to the signed data package that stores a message to be sent from an externally owned account to another account on the blockchain.

Transactions contain:
* the recipient of the message
* a signature identifying the sender and proving their intention to send the message via the blockchain to the recipient
* `VALUE` field - The amount of wei to transfer from the sender to the recipient
* an optional data field, which can contain the message sent to a contract
* a `STARTGAS` value, representing the maximum number of computational steps the transaction execution is allowed to take
* a `GASPRICE` value, representing the fee the sender is willing to pay for gas. One unit of gas corresponds to the execution of one atomic instruction, i.e., a computational step.
 
### What is a message?
Contracts have the ability to send “messages” to other contracts. Messages are virtual objects that are never serialized and exist only in the Ethereum execution environment. They can be conceived of as function calls.

A message contains:
* the sender of the message (implicit)
* the recipient of the message
* a ``VALUE`` field - The amount of wei to transfer alongside the message to the contract address,
an optional data field, that is the actual input data to the contract
* a `STARTGAS` value, which limits the maximum amount of gas the code execution triggered by the message can incur.

### Public, private and consortium blockchains
`Public blockchains`: a public blockchain is a blockchain that anyone in the world can read, anyone in the world can send transactions to and expect to see them included if they are valid, and anyone in the world can participate in the consensus process – the process for determining what blocks get added to the chain and what the current state is. 

`Consortium blockchains`: a consortium blockchain is a blockchain where the consensus process is controlled by a pre-selected set of nodes; for example, one might imagine a consortium of 15 financial institutions, each of which operates a node and of which 10 must sign every block in order for the block to be valid. The right to read the blockchain may be public, or restricted to the participants, and there are also hybrid routes.

`Private blockchains`: a fully private blockchain is a blockchain where write permissions are kept centralized to one organization. Read permissions may be public or restricted to an arbitrary extent.


### Mining in Ethereum 

Ethereum, like all blockchain technologies, uses an incentive-driven model of security. Consensus is based on choosing the block with the highest total difficulty. Miners produce blocks which the others check for validity. Among other well-formedness criteria, a block is only valid if it contains proof of work (PoW) of a given difficulty. 

The Ethereum blockchain is in many ways similar to the Bitcoin blockchain, although it does have some differences. The main difference between Ethereum and Bitcoin with regard to the blockchain architecture is that, unlike Bitcoin, Ethereum blocks contain a copy of both the transaction list and the most recent state (the root hash of the merkle patricia trie encoding the state to be more precise). Aside from that, two other values, the block number and the difficulty, are also stored in the block. 

The proof of work algorithm used is called Ethash (a modified version of the Dagger-Hashimoto algorithm) and involves finding a nonce input to the algorithm so that the result is below a certain difficulty threshold. The point in PoW algorithms is that there is no better strategy to find such a nonce than enumerating the possibilities, while verification of a solution is trivial and cheap. Since outputs have a uniform distribution (as they are the result of the application of a hash function), we can guarantee that, on average, the time needed to find such a nonce depends on the difficulty threshold. This makes it possible to control the time of finding a new block just by manipulating the difficulty. 


### Smart Contracts
Smart Contracts are a fancy new technology supported by the ehterium blockchain. Contracts are stored as a blocks' data and are automatically partially/fully executed once certain conditions are met. They enforce a choreography between the contracts' parties. Moreover, the code can be run on any node using the Ethereum Virtual Machine, and running costs a certain amount of ether to incetivize miners to process the contract. The EVM enables the chain to reach consensus about the porper output of any smart contract based on a set of inputs.
The EVMs' language, Solidity, reads a bit like javascript, is statically typed, contract-oriented, and turing complete!


#### Sources
* Satoshi Nakamoto - Bitcoin: A Peer-to-Peer Electronic Cash System
* Bentov, Gabizon, Mizrahi - Cryptocurrencies without Proof of Work
* Castro, Liskov - Pracitical Byzantine Fault Tolerance
