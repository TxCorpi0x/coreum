# Coreum Offerings

## Side Chains

- Coreum uses sidechains to achieve horizontal scaling.
- Each sidechain can process a significant number of transactions per second.
- By adding more sidechains, the network's overall transaction throughput is increased. For example, the document states a single chain can handle 7,000 transactions per second, so adding one sidechain doubles this to 14,000 transactions per second.

## Fee Model

Coreum utilizes transaction fees to secure the network by compensating validators and discouraging attacks. The transaction fee is calculated using the following formula:

`fee = gasPrice * gas`

- `Gas`: Represents the computational power required for the transaction. It remains relatively constant for most transaction types.
- `GasPrice`: This parameter varies dynamically based on the current transaction load on the blockchain.

Coreum employs an automatic gas price adjustment mechanism, referred to as the "fee model," to respond to fluctuations in transaction load. The parameters of this fee model are governed by on-chain governance.

The fee model operates across several regions:

### Green Region

When there are few or no transactions, the "Short Average Block Gas" is 0, and the gasPrice is set to the "Initial Gas Price."
As the transaction load increases, the gasPrice decreases until it reaches the minimum value defined by "Maximum Discount."
The "Maximum Discount" is achieved when the "Short Average Block Gas" equals the "Long Average Block Gas."

### Violet Region

In this region, the gasPrice remains at the minimum unless the block load approaches the blockchain's capacity (defined by "EscalationStartBlockGas").

### Yellow Region

As the blockchain nears saturation, the gasPrice increases rapidly. This is designed to prevent attackers from hindering the processing of legitimate transactions.

### Blue Region

## Smart Tokens

When the blockchain's capacity ("MaxBlockGas") is reached, the gasPrice is set to a constant high level.
The gasPrice remains high until the transaction volume decreases to safer levels.

- Smart Tokens are tokens that are natively issued on the Coreum blockchain.
- They are "wrapped around" smart contracts, meaning they incorporate smart contract functionality.
- They are designed to be highly customizable and flexible.
- Smart tokens are lightweight and exist on the chain's storage and memory.
- Interacting with them doesn't require calling separate smart contract functions.
- Smart Tokens inherit characteristics and functions from a global token definition, similar to objects and classes.

### Features

- Issuance: The initial creation of the Smart Token, defining its settings, amount, features, and Access Control List (ACL).
- Minting: The creation of new tokens. Can be enabled during issuance for use cases like stablecoins. If not enabled, the total supply is fixed.
- Access Control List (ACL): Defines the relationships between accounts and allowed features, providing flexible asset administration.
- Burning: Allows token holders with permission to destroy tokens, reducing the total supply.
- Freezing: Enables the administrator to freeze a user's token balance, either partially or entirely. Can also be global, preventing all transfers.
- Whitelisting: Restricts token ownership to a specific set of verified accounts, useful for KYC/AML compliance.
- Blacklisting: Allows the issuer to prevent specific accounts from sending or receiving the token.
- Burn Rate: A portion of the transferred amount is destroyed during a token transfer.
- Send Fee: A fee charged on token transfers, paid to the issuer.
- IBC Compatibility: Smart Tokens can be transferred to and from other IBC-compatible blockchains.
- Smart Contract Integration: Smart Tokens can be integrated with smart contracts for greater customization.

## DEX

### Core Functionality

- The DEX is a built-in exchange functionality native to the Coreum blockchain.
- It's designed to facilitate low-fee, secure, and fast trading.
- It supports trading any asset issued on the Coreum blockchain, as well as the native CORE token.

### Order Book

- The DEX uses an on-demand order book model.
- This means that users can create orders for any possible trading pair.
- Each order includes details like:
  - Currency pair
  - Trade direction (buy or sell)
  - Execution price
  - Size
  - Additional conditions for execution and closing

#### On-Demand Order Book

The Coreum DEX uses an "on-demand" approach. This likely means that the order book isn't necessarily a continuously maintained, central structure in the same way. Instead:

- Orders can be created for any possible trading pair as needed.
- The system has the capability to generate or construct the relevant order information when a trade is initiated.
- This could offer greater flexibility in supporting a wider variety of less common trading pairs without needing to store a constantly updated book for each one.

### Order Matching

- To execute an order, it must be matched with an order of the opposite direction and the same or better price.
- The DEX supports:
  - Direct order matching: Where orders in the same pair are matched. This is the most straightforward type of order matching. It occurs when a buy order and a sell order for the exact same trading pair are matched.
    - Example:
      - Alice places a buy order for 100 CORE at $1.00 USD.
      - Bob places a sell order for 100 CORE at $1.00 USD.
      - The DEX directly matches Alice's and Bob's orders, and the trade is executed.
  - Indirect order matching: Where orders from intermediary pairs (using CORE as the intermediary asset) are used to fulfill an order.
    - This is a more complex type of matching that increases trading possibilities.
    - It uses an intermediary asset (in Coreum's case, likely CORE) to facilitate trades between assets that don't have direct orders.
      - Example:
        - Let's say there are orders for USD/EUR and USD/CORE, but not directly for EUR/CORE.
        - If someone wants to trade EUR for CORE, the DEX could:
          - Match the EUR with USD orders.
          - Then match the resulting USD with CORE orders.

### Order Execution

- Orders are executed only by an opposite order, either directly or indirectly.
- When orders match, they are executed at the price of the order that was created earlier.

### Order Types

- The DEX supports two main order types:
  - Market orders: These orders are fulfilled at the best available price.
  - Limit orders: These orders are fulfilled only at a specified price or better.

### Advanced Order Features

- The DEX also offers advanced order types:
  - Good Till Cancel (GTC) orders: Remain active until canceled or filled.
  - Good Till Time orders: Can be executed only within a specified time limit.
  - Immediate or Cancel (IOC) orders: Must be executed immediately, or they are canceled.
  - Fill or Kill (FOK) orders: Must be executed entirely at once; otherwise, they are canceled

## Document revision

The following items per module is not stated in [docs.coreum.dev](https://docs.coreum.dev/docs)

### FT Asset

- DEX Integration: The code includes functionality related to DEX (Decentralized Exchange) integration, specifically messages for updating DEX whitelisted denoms and unified reference amounts (`MsgUpdateDEXWhitelistedDenoms`, `MsgUpdateDEXUnifiedRefAmount`).
- Token Upgrade: The code includes messages and logic for upgrading tokens to newer versions (`MsgUpgradeTokenV1`, `DelayedTokenUpgradeV1`).
- Set Frozen: The code contains messages to set frozen state of an account (`MsgSetFrozen`). This is different from `MsgFreeze` and `MsgUnfreeze` which only modify the frozen amount.
- DEX Settings: The `MsgIssue` message includes a DEXSettings field, suggesting specific configurations for DEX integration during token issuance.

### NFT Asset

- Royalty Rate: The code includes support for a royalty rate, which can be set during class definition. This allows the issuer to receive a percentage of the trade value when the NFT is traded on a DEX. While the document mentions the potential use of the "disable sending" feature to force transfers via DEX for royalty fees, it doesn't explicitly state that a royalty rate feature exists.
- Mint Fee: The code implements a mint fee that is charged when an NFT is minted. This fee is configurable via parameters and is burned.
- DataDynamicIndexedItem: The code implements the DataDynamicIndexedItem which can be updated depending on item's editors.
- Send Authorization: The code defines custom send authorization logic, allowing for granular control over NFT transfers. This allows users to delegate specific NFT sending rights to other accounts.
- Burnt NFTs Tracking: The code tracks burnt NFTs and provides a query to retrieve them.
- Class Definition: The code defines the ClassDefinition which contains the information about the NFT class.
- URI and URIHash: The code contains URI and URIHash fields in the Class and NFT definitions.

### Deterministic Gas

- Authz Grant Overhead: The code includes logic to add an overhead to the gas cost of MsgGrant messages if the authorization type is SendAuthorization, MintAuthorization, or BurnAuthorization. This overhead is calculated based on the size of the authorization data.
- DEX Whitelisted Denoms Gas Calculation: The code calculates the gas for MsgUpdateDEXWhitelistedDenoms based on the number of denoms being whitelisted.
- Deterministic Gas for gRPC Services: The code includes a DeterministicMsgServer that wraps gRPC services and charges a deterministic amount of gas for defined message types.
- Gas Limit Override: The code overrides the gas limit for deterministic messages by multiplying the required gas by a multiplier (fuseGasMultiplier) to ensure the handler has enough gas to execute.
- Metrics and Events: The code emits events (EventGas) and reports metrics related to gas consumption, including a "deterministic gas factor" that compares the real gas used to the deterministic gas.
- Nondeterministic Message Handling: The code explicitly defines a set of messages as nondeterministic and assigns them a gas function that always returns 0 and false.
- Ante Handlers: The code uses ante handlers (AddBaseGasDecorator, ChargeFixedGasDecorator) to add base gas and charge fixed gas for transactions.
- Extension Feature Check: The code checks if a message involves a token with the extension feature enabled and, if so, marks the message as nondeterministic.
- Simulation Chain ID: The code uses a different fuseGasMultiplier for the simulation chain ID.
- Type Assertions for Extension Messages: The code uses type assertions to identify messages that might invoke asset extensions.
- MsgToMsgURL Function: The code uses the MsgToMsgURL function to convert a message to a URL-like string for identification.
- Fixed Gas and TxBaseGas: The code defines FixedGas, TxBaseGas, SigVerifyCost, TxSizeCostPerByte, FreeSignatures, and FreeBytes as parameters that influence the overall gas calculation.
- Gas Configuration: The code defines a Config struct to hold all the gas-related parameters and message-to-gas-function mappings.

### Decentralized Exchange

- Order ID Regex: The code enforces a specific regex format for order IDs. The document mentions the order ID as a unique identifier but doesn't specify the format.
- Quantity Step Validation: The code validates that the order quantity is a multiple of the quantity step for the asset. This ensures that orders are placed in valid increments.
- Parameter Management: The code includes parameter management, allowing governance to configure various DEX parameters like `DefaultUnifiedRefAmount`, `PriceTickExponent`, `QuantityStepExponent`, `MaxOrdersPerDenom`, and `OrderReserve`.
- Account Denom Orders Count: The code tracks the number of orders per account and denom to enforce the MaxOrdersPerDenom limit.
- Price and Quantity Step Calculation: The code implements the logic to calculate the price tick and quantity step based on the unified reference amount and exponents.
- Order Book Data Structure: The code defines the data structure for storing the order book, including the orders and their corresponding IDs.
- Order Cancellation by Denom: The code supports the cancellation of all orders for a specific account and denom.
- Order Sequence: The code uses a sequence number to uniquely identify each order.
- Order Reserve Denomination: The code uses a specific denomination for the order reserve, which is configurable via parameters.
- GoodTil Handling: The code implements the logic to automatically cancel orders that have reached their `GoodTilBlockHeight` or `GoodTilBlockTime`.
- Price Marshalling: The code implements custom marshalling and unmarshalling logic for the Price type to ensure proper serialization and deserialization.
- DEX Order Cancellation Feature: The code implements the logic to allow the token admin to cancel user's orders if the `dex_order_cancellation` feature is enabled.
- Block DEX Feature: The code implements the logic to block the DEX completely for the issued denom if the block_dex feature is enabled.
- Global Freeze Feature: The code implements the logic to prohibit placing an order with the corresponding base_denom and quote_denom if the `global_freeze` feature is enabled.
- Denoms to Trade With Feature: The code implements the logic to allow the asset FT to be traded only against a predefined set of denoms specified in `denoms_to_trade_with`.
- Extension Feature Handling: The code implements the logic to integrate the DEX with the smart contract extension capability of asset FT.

### FeeModel

- Governance Parameter Updates: The code includes the MsgUpdateParams message and related logic for updating the module parameters through governance proposals. The document mentions the parameters themselves but doesn't detail how they can be updated.
- gRPC Query Services: The code defines gRPC query services for retrieving the minimum gas price, recommended gas price, and module parameters. The document mentions the GetMinGasPrice keeper method but doesn't elaborate on the gRPC query layer.
- Recommended Gas Price Query: The code implements a QueryRecommendedGasPrice query that returns the recommended gas price based on the current block's gas usage and the configured parameters.
