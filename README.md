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

## Smart Contracts
Smart Contracts are a fany new technology supported by the ehterium blockchain. Contracts are stored as a blocks' data and are automatically partially/fully executed once certain conditions are met. They enforce a choreography between the contracts' parties. Moreover, the code can be run on any node using the Ethereum Virtual Machine, and running costs a certain amount of ether to incetivize miners to process the contract. The EVM enables the chain to reach consensus about the porper output of any smart contract based on a set of inputs.
The EVMs' language, Solidity, reads a bit like javascript, is statically typed, contract-oriented, and turing complete!


#### Sources
* Satoshi Nakamoto - Bitcoin: A Peer-to-Peer Electronic Cash System
* Bentov, Gabizon, Mizrahi - Cryptocurrencies without Proof of Work
* Castro, Liskov - Pracitical Byzantine Fault Tolerance
