# Networks

COTI test networks provide developers access to a free testing environment for COTI v2 network services. Testnets simulate the exact development environment as you would expect for Mainnet. This includes transaction fees, available services, etc.

#### Network Technical Details

When utilizing the COTI network environments, it is vital to understand the technical details that influence their operation. Here are some key parameters you will encounter:

* **Base Gas Fee**: The base gas fee is set at 5,000,000 wei. This fee is fundamental for executing transactions on the network and is essential for interacting with smart contracts effectively.
* **Block Gas Limit**: Each block has a gas limit of 120,000,000. This limit dictates the total amount of gas that can be used for all transactions within a single block, ensuring that blocks stay within a manageable size for processing.
* **Block Generation Time**: Blocks are generated every 5 seconds. This rapid block generation contributes to the network's efficiency and ensures that transactions are processed swiftly, providing developers and users with timely updates.

Understanding these parameters is crucial for developers and users interacting with the COTI environments, as they directly impact transaction execution and network performance.

{% hint style="info" %}
**NOTE**:&#x20;

* The **COTI Testnet** is currently operating in review mode. As such, it may experience unplanned resets. Remember to back up your work regularly and avoid storing any sensitive or confidential data.
{% endhint %}

<table><thead><tr><th width="143">Networks</th><th>Description</th></tr></thead><tbody><tr><td><strong>Devnet</strong></td><td><p>Code that is under development by the COTI core team and likely to be used in an upcoming release. Designed to give developers  exposure to features soon to be released. </p><p>Updates to the Devnet are made frequently, No guarantee by COTI to keep the consistency of the blocks and transactions.<br>----------------------------------------------------</p><p><mark style="color:red;"><strong>Devnet was sunset on March 2nd, 2025.</strong></mark></p></td></tr><tr><td><strong>Testnet</strong></td><td><p>Testnet runs the same code as the COTI v2 Mainnet, designed to provide a pre-production environment for developers about to move to Mainnet.<br>----------------------------------------------------</p><ul><li>Network name: COTI Testnet</li><li>RPC URL: <a href="https://testnet.coti.io/rpc">https://testnet.coti.io/rpc</a></li><li>WS URL: <a href="wss://testnet.coti.io/ws">wss://testnet.coti.io/ws</a></li><li>Chain ID: 7082400</li><li>Currency symbol: COTI</li><li>Block explorer URL: <a href="https://testnet.cotiscan.io">https://testnet.cotiscan.io</a></li><li>Testnet status page: <a href="https://uptime.coti.io/">https://uptime.coti.io</a></li></ul></td></tr><tr><td><strong>Mainnet</strong></td><td><p>The COTI Mainnet is where applications are run in production, with transaction fees paid in COTI. Transactions are submitted to the COTI Mainnet by any dApp.</p><p><br>Availability date: TBD</p></td></tr></tbody></table>
