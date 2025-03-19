# Running a COTI Node

Running a node helps secure and decentralize the COTI network and support the overall ecosystem. While there are some similarities to other L2 networks, COTI’s architecture has its own nuances and requirements that we’ll cover here.

### What is a COTI Node?

In the COTI network, Full Nodes are decentralized, lean clients that play a critical role in maintaining the network’s security, scalability, and overall functionality. Anyone is able to run a Full Node to support the network and optionally gain rewards.

Running a COTI full node, downloads a copy of the COTI blockchain and verifies the validity of every block. Unlike validator nodes in Ethereum, COTI full nodes do not actively participate in consensus nor in block proposal, as that is the job of the COTI sequencer (see [**COTI Architecture**](https://docs.coti.io/coti-documentation/how-coti-works/introduction/coti-architecture))

***

### Why Run a Node?

Running a COTI Node offers several benefits:

* **Network Participation**: Contribute to the decentralization and robustness of the network.
* **Community Support**: Strengthen the ecosystem and help drive adoption of COTI’s technology.
* **Rewards & Incentives**: Earn fees for processing and confirming transactions (licensed nodes only).

***

### Requirements

COTI Full Node software is provided as a docker image.

{% hint style="info" %}
**Disclaimer:** Successfully operating, troubleshooting, and maintaining a node requires technical proficiency. Familiarity with tools such as Linux, Docker, and Git is assumed. Users not familiar with this technology stack should consider assigning their license to an existing node operator to minimize their technical requirements.
{% endhint %}

#### Software

* **Operating System**: the following operating systems have been certified to run the node software:
  * Ubuntu 24.04.x (see [**Linux Requirements**](https://docs.docker.com/desktop/setup/install/linux/#general-system-requirements)[ **for Docker**](https://docs.docker.com/desktop/setup/install/linux/#general-system-requirements)**)**
* **Docker**:  version 28.0.1
* **Docker-Compose:** version 2.29.1

#### Hardware

The following hardware specs are required to run a COTI full node:

| Specification    | Minimum | Recommended | Professional |
| ---------------- | ------- | ----------- | ------------ |
| **vCPUs**        | 2       | 4           | 8            |
| **Memory (GiB)** | 8       | 16          | 64           |
| **Storage**      | 100 GB  | 200 GB      | 500 GB       |

In addition to the above hardware, a reliable, high-bandwidth internet connection is recommended.

### Recommended **minimum** hosted configuration

* AWS: m7a.Large (2 vCPUs, 8GB memory)
* OVH: b2-15 (4 vCPUs, 15GB memory)

_**Disclaimer**: The above configuration has been certified on Testnet; higher transaction volumes on Mainnet may require increased specifications._

### Recommended **optimal** hosted configuration

* AWS: r5n.2xlarge (8 vCPUs, 64GB memory)
* OVH: r2-120 (8 vCPUs, 120GB memory)

#### Network

* **Open Ports**: You need to open the following ports in your firewall or cloud environment to allow inbound connections:
  * 8545 - Allowing RPC requests to flow in
  * 8546 - Allowing WebSocket connection to flow in
  * 7400 - Allowing peer-2-peer connection
* **Static IP**: Requirement is to have a stable access to RPC so metering of health could be performed.

***

### Setting up Your Node Environment

COTI full nodes are run using docker. Docker provides a way for everyone to run battle-tested, reliable images, known to work with the network.

#### Prerequisites

<table data-header-hidden><thead><tr><th width="257.44818115234375"></th><th></th></tr></thead><tbody><tr><td><ol><li>Git</li></ol></td><td>See <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git"><strong>git-scm.com/book/en/v2/Getting-Started-Installing-Git</strong></a></td></tr><tr><td><ol start="2"><li>Docker</li></ol></td><td>See <a href="https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository"><strong>docs.docker.com/engine/install/ubuntu</strong></a></td></tr><tr><td><ol start="3"><li>Docker Compose</li></ol></td><td>See <a href="https://docs.docker.com/compose/install/standalone/"><strong>docs.docker.com/compose/install/standalone</strong></a></td></tr></tbody></table>

### Installation Steps

{% hint style="info" %}
Following Recommended steps are ones that as under best-practices but should be handled carefully as they might have a serious impact over the operating system.
{% endhint %}

1. **Recommended steps:**
   1.  Set host name (where `<name>`is your chosen node name)

       {% code fullWidth="false" %}
       ```bash
       sudo hostnamectl set-hostname fullnode-<name>
       ```
       {% endcode %}
   2.  Update package lists

       {% code fullWidth="false" %}
       ```bash
       sudo apt update
       sudo apt upgrade
       ```
       {% endcode %}
   3.  Reboot system

       {% code fullWidth="false" %}
       ```bash
       sudo reboot
       ```
       {% endcode %}
   4.  Update OS

       {% code fullWidth="false" %}
       ```bash
       sudo do-release-upgrade
       ```
       {% endcode %}


2. **Configure Docker**
   *   Add your user to the `docker` group:

       {% code fullWidth="false" %}
       ```bash
       sudo usermod -aG docker $( whoami )
       ```
       {% endcode %}
   *   Logout and re-login

       {% code fullWidth="false" %}
       ```bash
       logout
       ```
       {% endcode %}


3.  **Clone the COTI Full Node project**

    {% code fullWidth="false" %}
    ```bash
    git clone https://github.com/coti-io/coti-full-node.git
    ```
    {% endcode %}


4.  **Start Your Node**

    1. Navigate to the newly created "coti-full-node" directory



    {% code fullWidth="false" %}
    ```bash
    cd ./coti-full-node
    ```
    {% endcode %}



    b. Execute node start script



    {% code fullWidth="false" %}
    ```bash
    ./start_coti-fullnode.sh
    ```
    {% endcode %}

    \
    c. Once the docker-compose has started the node, liveliness check will be executed



    {% code fullWidth="false" %}
    ```bash
    ./liveliness_coti_full-node.sh
    ```
    {% endcode %}

{% hint style="info" %}
If liveliness check passed locally it means that your node is syncing with the other nodes in the network
{% endhint %}

&#x20;         Output example:

```
Initial block number: 208539
Check 1: Block number is 208540
Block number has progressed. Node is syncing.
```

5.  **To Check Node Logs**



    ```
    docker logs -f coti-fullnode
    ```

### Restarting Your Node

To restart your node follow these steps:

1.  Stop your node

    {% code fullWidth="false" %}
    ```bash
    ./stop_coti-fullnode.sh
    ```
    {% endcode %}


2.  Start your node

    {% code fullWidth="false" %}
    ```bash
    ./start_coti-full-node.sh
    ```
    {% endcode %}

### Node Configuration

If you are running a node without a license, no further configuration of the node is required. Simply ensure you are connected to the network.

### Verifying Node Functionality

* Metrics: Visit [**uptime.coti.io**](https://uptime.coti.io) to track performance and status, for Mainnet nodes.
* Node availability is crucial for the smooth operation of the network. \
  In order to rank the availability as a metrics, COTI is using existing platform and presents it publicly.\
  Node is consider available by responding to request of the `eth_blockNumber`, using this request helps making sure that node is progressing with network synchronization, hence, healthy node.

### Incentives

The economic structure of COTI nodes is designed to incentivize active participation and ensure the long-term sustainability of the network.

Licensed Full Nodes that maintain a minimum uptime of 98% are eligible to receive **Validation Rewards**, which are distributed every **103-hour Period** (epoch). This uptime requirement underscores the importance of reliability and consistent performance within the network.

For more information on incentives, visit the "Node Economy" section of the [coti-node-ecosystem-litepaper.md](coti-node-ecosystem-litepaper.md "mention").

### Maintenance & Monitoring

1. Regular Updates: Keep your node software updated to the latest version. This ensures you receive security patches and new features.
2. Resource Usage: Monitor CPU, RAM, and disk space to ensure uninterrupted operation.
3. Uptime: Use a process manager (like `systemd`) or Docker auto-restart policies to keep your node running if it crashes unexpectedly.

### Troubleshooting

#### Common Issues:

* Peer Connection Errors: Ensure your ports are open and your firewall allows inbound connections.
* High Resource Usage: Upgrade your hardware or adjust configuration settings to reduce overhead.

Where to Get Help:

* [**COTI Discord**](https://discord.coti.io/)
* [**COTI Support**](mailto:support@coti.io)

### FAQ

1. How many nodes can I run?&#x20;
   1. There’s no set limit, but each node requires its own resources. Running multiple nodes can help decentralize the network but comes with higher operational costs.
2. Can I run a node on a VPS or cloud platform?
   1. Absolutely. Just ensure the service meets the hardware, OS, and networking requirements.
3. Do I earn more rewards by running a more powerful node?
   1. Generally, consistently high uptime can lead to more consistent rewards, however, the only measure to qualify for rewards is uptime.
4. Is it mandatory to purchase a node license to run a node?&#x20;
   1. No. A node license simply allows you to earn rewards for helping decentralize the network, however, it is not necessary to run a COTI node.

### Next Steps

Congratulations on setting up your COTI Node! By running a node, you’re contributing to the security and decentralization of the COTI network.

The following related sections may provide helpful information:

* [coti-node-ecosystem-litepaper.md](coti-node-ecosystem-litepaper.md "mention")
