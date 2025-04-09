## Document revision

The following items per module is not stated in [docs.coreum.dev](https://docs.coreum.dev/docs)

### FT Asset

- DEX Integration: The code includes functionality related to DEX (Decentralized Exchange) integration, specifically messages for updating DEX whitelisted denoms and unified reference amounts (`MsgUpdateDEXWhitelistedDenoms`, `MsgUpdateDEXUnifiedRefAmount`).
- Token Upgrade: The code includes messages and logic for upgrading tokens to newer versions (`MsgUpgradeTokenV1`, `DelayedTokenUpgradeV1`).
- Set Frozen: The code contains messages to set frozen state of an account (`MsgSetFrozen`). This is different from `MsgFreeze` and `MsgUnfreeze` which only modify the frozen amount.
- DEX Settings: The `MsgIssue` message includes a DEXSettings field, suggesting specific configurations for DEX integration during token issuance.

### NFT Asset

- Mint Fee: The code implements a mint fee that is charged when an NFT is minted. This fee is configurable via parameters and is burned.
- Send Authorization: The code defines custom send authorization logic, allowing for granular control over NFT transfers. This allows users to delegate specific NFT sending rights to other accounts.
- Burnt NFTs Tracking: The code tracks burnt NFTs and provides a query to retrieve them.
- Class Definition: The code defines the ClassDefinition which contains the information about the NFT class.
- URI and URIHash: The code contains URI and URIHash fields in the Class and NFT definitions.

### Deterministic Gas

- Authz Grant Overhead: The code includes logic to add an overhead to the gas cost of MsgGrant messages if the authorization type is SendAuthorization, MintAuthorization, or BurnAuthorization. This overhead is calculated based on the size of the authorization data.
- DEX Whitelisted Denoms Gas Calculation: The code calculates the gas for MsgUpdateDEXWhitelistedDenoms based on the number of denoms being whitelisted.
- Deterministic Gas for gRPC Services: The code includes a DeterministicMsgServer that wraps gRPC services and charges a deterministic amount of gas for defined message types.
- Gas Limit Override: The code overrides the gas limit for deterministic messages by multiplying the required gas by a multiplier (fuseGasMultiplier) to ensure the handler has enough gas to execute.
- Nondeterministic Message Handling: The code explicitly defines a set of messages as nondeterministic and assigns them a gas function that always returns 0 and false.
- Ante Handlers: The code uses ante handlers (AddBaseGasDecorator, ChargeFixedGasDecorator) to add base gas and charge fixed gas for transactions.
- Extension Feature Check: The code checks if a message involves a token with the extension feature enabled and, if so, marks the message as nondeterministic.
- Fixed Gas and TxBaseGas: The code defines FixedGas, TxBaseGas, SigVerifyCost, TxSizeCostPerByte, FreeSignatures, and FreeBytes as parameters that influence the overall gas calculation.
- Gas Configuration: The code defines a Config struct to hold all the gas-related parameters and message-to-gas-function mappings.

### Decentralized Exchange

- Order ID Regex: The code enforces a specific regex format for order IDs. The document mentions the order ID as a unique identifier but doesn't specify the format.
- Quantity Step Validation: The code validates that the order quantity is a multiple of the quantity step for the asset. This ensures that orders are placed in valid increments.
- Parameter Management: The code includes parameter management, allowing governance to configure various DEX parameters like `DefaultUnifiedRefAmount`, `PriceTickExponent`, `QuantityStepExponent`, `MaxOrdersPerDenom`, and `OrderReserve`.
- Account Denom Orders Count: The code tracks the number of orders per account and denom to enforce the MaxOrdersPerDenom limit.
- Price and Quantity Step Calculation: The code implements the logic to calculate the price tick and quantity step based on the unified reference amount and exponents.
- Order Cancellation by Denom: The code supports the cancellation of all orders for a specific account and denom.
- Order Sequence: The code uses a sequence number to uniquely identify each order.

### FeeModel

- Governance Parameter Updates: The code includes the MsgUpdateParams message and related logic for updating the module parameters through governance proposals. The document mentions the parameters themselves but doesn't detail how they can be updated.
- gRPC Query Services: The code defines gRPC query services for retrieving the minimum gas price, recommended gas price, and module parameters. The document mentions the GetMinGasPrice keeper method but doesn't elaborate on the gRPC query layer.
- Recommended Gas Price Query: The code implements a QueryRecommendedGasPrice query that returns the recommended gas price based on the current block's gas usage and the configured parameters.
