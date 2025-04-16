## Document revision

The following items per module is not stated in [docs.coreum.dev](https://docs.coreum.dev/docs)

### FT Asset

- DEX Integration: The code includes functionality related to DEX (Decentralized Exchange) integration, specifically messages for updating DEX whitelisted denoms and unified reference amounts (`MsgUpdateDEXWhitelistedDenoms`, `MsgUpdateDEXUnifiedRefAmount`). (add to doc)
- Token Upgrade: The code includes messages and logic for upgrading tokens to newer versions (`MsgUpgradeTokenV1`, `DelayedTokenUpgradeV1`).(remove from code)
- Set Frozen: The code contains messages to set frozen state of an account (`MsgSetFrozen`). This is different from `MsgFreeze` and `MsgUnfreeze` which only modify the frozen amount. (Add comment to the proto definition in code.◊)
- DEX Settings: The `MsgIssue` message includes a DEXSettings field, suggesting specific configurations for DEX integration during token issuance. (Add DEX section in ftasset the same as Readme)

### NFT Asset

- Mint Fee: The code implements a mint fee that is charged when an NFT is minted. This fee is configurable via parameters and is burned (add a section to docs, currently is zero but it may change by go proposals).

### Deterministic Gas

- DEX Whitelisted Denoms Gas Calculation: The code calculates the gas for `MsgUpdateDEXWhitelistedDenoms` based on the number of denoms being whitelisted (It is present in repo, it should be reflected in the docs).
- Deterministic Gas for gRPC Services: The code includes a DeterministicMsgServer that wraps gRPC services and charges a deterministic amount of gas for defined message types (Mehdi double check).
- Fixed Gas and TxBaseGas: The code defines FixedGas, TxBaseGas, SigVerifyCost, TxSizeCostPerByte, FreeSignatures, and FreeBytes as parameters that influence the overall gas calculation (To be included in the docs, parameter only).

### Decentralized Exchange

- Order ID Regex: The code enforces a specific regex format for order IDs. The document mentions the order ID as a unique identifier but doesn't specify the format (Add to the docs).
- Quantity Step Validation: The code validates that the order quantity is a multiple of the quantity step for the asset. This ensures that orders are placed in valid increments.
- Price and Quantity Step Calculation: The code implements the logic to calculate the price tick and quantity step based on the unified reference amount and exponents (Add to docs according to price-quantity.md).
- Parameter Management: The code includes parameter management, allowing governance to configure various DEX parameters like `DefaultUnifiedRefAmount`, `PriceTickExponent`, `QuantityStepExponent`, `MaxOrdersPerDenom`, and `OrderReserve` (Params to be added to docs).
- Account Denom Orders Count: The code tracks the number of orders per account and denom to enforce the MaxOrdersPerDenom limit (Example values and link explorer).
- Order Cancellation by Denom: The code supports the cancellation of all orders for a specific account and denom (denom is missing in the doc, it should be added).
- Order Sequence: The code uses a sequence number to uniquely identify each order (Add to docs).

### All Modules Must have

1. TO be mentioned in the docs:  If it is not clear in docs, please look into the protos and repo docs. it should be a section for each of the modules at the beginning of the document. 
    1. Look at tx.proto for messages
    2. Look at params.proto for parameters.
    3. Look at query.proto for query specifications.