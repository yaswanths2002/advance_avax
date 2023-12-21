## Advance_avax

# DeFi Empire Smart Contract

## Overview
The overview of the Project to deploy a subnet evm on avalanche network and deploy the contract

This smart contract allows players to collect, build, and earn rewards for their participation in the game's activities.

- It has two main contracts, `ERC20` token contract and the `Vault` contract

- The ERC20-Token contract, `ERC20.sol`, was deployed on the `demosubnet` network at the contract address `0x4Ac1d98D9cEF99EC6546dEd4Bd550b0b287aaD6D` and
- The Vault contract, `Vault.sol` was deployed on the `demosubnet` network at the contract address`0xA4cD3b0Eb6E5Ab5d8CE4065BcCD70040ADAB1F00`.

## Deploying my EVM subnet using the Avalanche CLI

- First we need to install avalanche CLI on our local system to do that you can follow the avalanche documentation https://docs.avax.network/tooling/cli-guides/install-avalanche-cli

- First we need to create a dubnet by running below command 
```bash
  avalanche subnet create <subnet_name>
```
- Next wee will type the details and choose the things according to our requirements
- By running the below command we will be able to see the created subnets
```bash
  avalanche subnet list
```
- And then we will type the below command to deploy the subnet and here I am deploying it locally
```bash
  avalanche subnet deploy <subnet_name>
```
## Adding subnet to MetaMask
- We can add the subnet to the metamask by toggling on the netwok and click on the `Add Network` and click on `Add Network Manually` and enter the details of the subnet we have created before.

## Contract Details
- **Network:** demosubnet
- **Chain ID** 1234
- **Currency Symbol** YS

## Vault Contract Functionalities
### Deposit

- The `deposit()` function allows users to deposit ERC-20 tokens into the vault.

- This function calculates the number of shares to mint based on the deposited amount and the current total supply of shares.

```solidity
function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

```

### Withdraw

- The `withdraw` function allows users to withdraw their tokens from the vault.
- This function calculates the amount to withdraw based on the number of shares burned and the current total supply of shares.

```solidity
function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
```

### ERC-20 Interface

The ERC20 interface provides the functions signatures basic token operations like checking balances, transferring tokens, and approving token transfers.

```solidity
interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}
```

## Usage Guidelines

1. **Token Approval:**
   Before depositing, ensure that you have approved the contract to spend your ERC20 tokens. Use the ERC20 `approve` function to grant the necessary permission.

```solidity
function approve(address spender, uint amount) external returns (bool);
```

2. **Deposit:**
   Call the `deposit` function with the desired amount of tokens to mint corresponding shares.

3. **Withdraw:**
   Call the `withdraw` function with the number of shares to burn and receive the proportional amount of tokens.

4. **Monitor Balances:**
   Keep track of your token balances and shares to manage deposits and withdrawals effectively, by calling the `balanceOf()`

   ```solidity
   function balanceOf(address account) external view returns (uint);
   ```

### Author : Yaswanth
