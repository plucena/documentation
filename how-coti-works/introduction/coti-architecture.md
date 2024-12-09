# COTI Architecture

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Topology of the COTI Network</p></figcaption></figure>

The COTI network is comprised of several core components:

1. **Full Nodes**: These nodes are responsible for validating transactions and ensuring the integrity of the COTI blockchain network. They act as the decentralized foundation of the network.
2. **Sequencer**: This component processes transactions (TXs) and organizes them into blocks. It coordinates transaction flow and ensures orderly addition to the chain.
3. **Executors**: Two separate executors that process blocks in a MPC manner:
   * **Red Blocks**: incoming blocks processed from the sequencer to the executors.
   * **Black Blocks**: outgoing blocks processed from the executor to the sequencer.
4. **DB Manager**: Stores the generated garbled circuits in data warehouse, these circuits are used by the executors for on-chain computation.
5. **Garbler**: Generates garbled circuits that are stored into the DB Manager to be used later on during on-chain computation.

The network uses a multi-tier architecture to optimize transaction flow and ensure scalability, with the sequencer bridging full nodes to the execution layer and the database manager and garbler handling persistence and post-processing.
