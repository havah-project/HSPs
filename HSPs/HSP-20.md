| HSP  | Title                | Author        | Discussions-to | Status | Type             | Category  | Created |
|------|----------------------|---------------|----------------|--------|------------------|-----------|---------|
| 20   | HAVAH Token Standard | KyoungNam Kim |                | Final  | Standards Track  | Contract  |         |

# Simple Summary
A standard interface for tokens on HAVAH network.

# Abstract
This draft HSP describes a token standard interface to provide basic functionality to transfer tokens. We inspired by [ERC20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md).

# Motivation
A token standard interface allows any tokens on HAVAH to be re-used by other third parties, from wallets to decentralized exchanges.

# Specification
## Methods
### name
Returns the name of the token. e.g. `MySampleToken`.

```Java
@External(readonly=true)
public String name()
```

### symbol
Returns the symbol of the token. e.g. `MST`.
```Java
@External(readonly=true)
public String symbol()
```

### decimals
Returns the number of decimals the token uses. e.g. `18`.

```Java
@External(readonly=true)
public BigInteger decimals()
```

### totalSupply
Returns the total token supply.

```Java
@External(readonly=true)
public BigInteger totalSupply()
```

### balanceOf
Returns the account balance of another account with address `_owner`.

```Java
@External(readonly=true)
public BigInteger balanceOf(Address _owner)
```

### transfer
Transfers `_value` amount of tokens to address `_to`, and MUST fire the `Transfer` event. This function SHOULD throw if the `self.msg.sender` account balance does not have enough tokens to spend.

```Java
@External
public boolean transfer(Address _to, BigInteger _value)
```

### transferFrom
Transfers `_value` amount of tokens from address `_from` to address `_to`, and MUST fire the `Transfer` event.

The transferFrom method is used for a withdraw workflow, allowing contracts to transfer tokens on your behalf. This can be used for example to allow a contract to transfer tokens on your behalf and/or to charge fees in sub-currencies. The function SHOULD throw unless the `_from` account has deliberately authorized the sender of the message via some mechanism.

*NOTE:* Transfers of 0 values MUST be treated as normal transfers and fire the `Transfer` event.

```Java
@External
public boolean transferFrom(Address _from, Address _to, BigInteger _value)
```

### approve
Allows `_spender` to withdraw from your account multiple times, up to the `_value` amount. If this function is called again it overwrites the current allowance with `_value`.

*NOTE:* To prevent attack vectors like the one described here and discussed here, clients SHOULD make sure to create user interfaces in such a way that they set the allowance first to 0 before setting it to another value for the same spender. THOUGH The contract itself shouldn’t enforce it, to allow backwards compatibility with contracts deployed before

```Java
@External
public boolean approve(Address _spender, BigInteger _value)
```

### allowance
Returns the amount which `_spender` is still allowed to withdraw from `_owner`.

```Java
@External(readonly = true)
public BigInteger allowance(Address _owner, Address _spender)
```

## Eventlogs
### Transfer
Must trigger on any successful token transfers.

```Java
@EventLog(indexed=2)
public void Transfer(Address _from, Address _to, BigInteger _value)
```

### Approval
Must trigger on any successful call to approve(address _spender, uint256 _value).

```Java
@EventLog(indexed=2)
public void Approval(Address _owner, Address _spender, BigInteger _value)
```

# Implementation
- [smart-contract-examples/hsp20-token at master · havah-project/smart-contract-examples](https://github.com/havah-project/smart-contract-examples/tree/master/hsp20-token)
- [havah-project/sclib-token: A Havah Library for Standard Tokens](https://github.com/havah-project/sclib-token) 

# References
- [EIPs/eip-20.md at master · ethereum/EIPs](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)

# Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0).