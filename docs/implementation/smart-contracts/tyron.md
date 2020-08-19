# The ```tyron``` smart-contract

Zilliqa powers smart-contracts written in the Scilla programming language.

1. The tyron smart-contract gets deployed with the following immutable fields:
    1. tyron_init: the address of the TyronInit smart contract
    2. contract_owner: the address of the user who is the owner of the contract

2. Call the ContractInit transition that sets the:
    1. operation_cost
    2. foundation_address
    3. client_commission

> These parameters can only change through the ContractInit transition, which calls the Initialize transition of the TyronInit contract

:bulb: future development: give client the option of offering discounts

3. To create a DID, the Sidetree protocol requires the suffix_data (Base64URL encoded).
    1. Call the DidCreate transition
