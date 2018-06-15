## DLT

A distributed ledger is a data structure used to store information on a P2P network

3 main parts of DLT:
- A data model that captures the current state of the ledger
- A language of transactions that changes the ledger state
- A protocol used to build consensus among participants around which transactions will be accepted, and in what order, by the ledger.

## Blockchain

-Its a P2P Dist. Ledger that uses a consensus algorithm and smart contracts to build
transactional application.
- All dist. ledgers have a an initial record (genesis block).
- End users can connect to this dist. ledger using an application (example a wallet)
- Digital assests can be transfered over this network using this app (ex. bitcoin transactions)
- There is no intermediary body in between just the peers over a network.
- This transaction is recorded onto the blockchain and visible to every peer.
- These transactions are grouped into blocks by the nodes on a blockchain.
- These blocks of transaction are added to the chain of blocks (hence the name)
  of previous transactions in a chronological order (i.e. the genesis block is
  the first one in the chain).
- In the Bitcoin blockchain, the miner nodes bundle unconfirmed and valid transactions into a block.
- Miners must solve a cryptographic challenge to propose the next block (proof of work).
- All the blocks are timestamped each referring to the previous block.

A block consists of 4 pieces of metadata:
- Reference to the previous block.
- Proof of work (nonce).
- Timestamp.
- Merkle tree root for the transactions included in that block.

> Merkle trees are used to summarize all the transactions in a block, producing an overall digital fingerprint of the entire set of transactions, providing a very efficient process to verify whether a transaction is included in a block.

### Types of Blockchain

1. Open public blockchain: Like bitcoin or ethereum.
2. Closed / Permissioned blockchains: Like Hyperledger.

### Network Architecture
Unlike the traditional client-server architecture, P2P networks are those in which
several computers are connected to each other via the internet without a central server.

- Each peer contributes to the computing power and storage to the network
- More secure since there is no central point of attack
- Permissionless P2P are slow and don't require a fixed number of peers to be
  online.

**Immutability**
> When people say that blockchains are immutable, they don't mean that the data can't be changed, they mean it is extremely hard to change without collusion, and if you try, it's extremely easy to detect the attempt

**Smart Contracts:** Smart contracts are simply computer programs that execute predefined actions when certain conditions within the system are met. Smart contracts provide the language of transactions that allow the ledger state to be modified. They can facilitate the exchange and transfer of anything of value (e.g. shares, money, content, property).
Ex: In an equity raise, transfer amount X from the investor to the company upon receiving the given shares from the company.
The smart contract encodes the agreement between the company raising funds and its investors.
The smart contract sits on the Ethereum public blockchain, and is run on the Ethereum Virtual Machine (EVM). Once hitting a triggering event, like an expiration date or a strike price that has been pre-coded, the smart contract automatically executes as per the business logic.
As an added benefit, regulators are able to scrutinize the market activity on an ongoing basis, without compromising the identity of specific players in a permissionless public blockchain, as Ethereum

#### Permissionless Blockchain and Cryptoeconomics

- The malicious actors cannot take over the network through an escalated attack.
- The malicious actors cannot collude to undertake an organized majority attack on the network.
- The payoffs of securing the network are consistently higher than the cost of attacking the network.
- The cost of attacking the network is prohibitively high.

*More at:* https://medium.com/cryptoeconomics-australia/the-blockchain-economy-a-beginners-guide-to-institutional-cryptoeconomics-64bf2f2beec4


### Consensus Algorithms
- It ensures that the data on the ledger is the same for all the nodes in the network, - This in turn, prevents malicious actors from manipulating the data.

**Types of Consensus Algorithms**
- *Proof of Work:* used by the bitcoin blockchain where nodes compete to solve a
cryptographic challenge and the first one to solve it and generate a block is
awarded 12.5 BTC. It consumes a lot of energy and is susceptible to 51% attacks.

- *Proof of Stake:* No mining is done since all the coins exist from day 1. The
nodes act as validators and get a fee for validating a transaction. The selection
of validators depends on the stake held by them, so a node with more coins will
have more probability of being called upon for validation.
  - *Proof of Deposit:* To be used in Ethereum. Its is an instance of POS in which
  nodes have to submit a deposit before validation and if they fail to generate a
  new block their security is forfited and they can lose the right to participate
  in validation.

- *Proof of Burn:* Miners have to burn some of their assets (send some cryptocurrency
  to an unspendable address so that they can't use it). The idea is to keep fuelling
  Proof of Work by burning some amount of digital assests.

- *Proof of Elapsed Time:* Its a sort of a lottery based system. Each validator
is given a random wait time. The one with the least wait time is elected the leader
and is called upon to mine the next block. Ex: Hyperledger Sawtooth.

- *Simplified Byzantine Fault Tolerance:* A single validator (a known/permissioned party) bundles up proposed transactions to form a block. Consensus is reached by the rest of the nodes ratifying the block. In order to be tolerant of a Byzantine fault, the number of nodes that must reach consensus is 2f+1 in a system containing 3f+1 nodes, where f is the number of faults in the system. This is a very fast consensus
algorithm with tansaction being committed in a matter of seconds. Ex: ByzCoin.

- *Proof of Authority:* Designated nodes act as authorities that create new blocks and secure the ledger. A majority of the authorities need to sign-off for a block to be created.

### Hyperledger
It is a permissioned blockchain where all the participants are authorized and authenticated. It is more secure by ensuring that only the participants involved in
a transaction can view the the transaction rather than the whole network.


## Other blockchains
- **ChainCore:**
- **Corda:**
- **Quorum:**
- **IOTA:**
