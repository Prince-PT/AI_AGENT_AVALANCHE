# On-Chain AI Agent for Twitter NFT Minting

## Overview
This project demonstrates an innovative integration of AI and blockchain technology, creating a system where users can receive NFTs by simply tweeting requests. The AI agent monitors Twitter, processes natural language inputs, and triggers secure on-chain NFT minting through Chainlink Functions on the Avalanche blockchain.

## Key Components
- **Eliza AI Agent**: Listens to Twitter for user requests and natural language inputs
- **Chainlink Functions**: Bridges off-chain data with on-chain execution in a decentralized manner
- **Avalanche Fuji Testnet**: The blockchain network where NFTs are minted and transferred
- **Supabase Database**: Stores and verifies gift code information
- **IPFS**: Hosts NFT metadata for decentralized storage

## Features
- 🐦 **Twitter Integration**: Users interact through simple tweets
- 🤖 **AI Processing**: Natural language understanding of user requests
- 🔗 **Decentralized Oracle**: Chainlink Functions securely connects off-chain and on-chain worlds
- 🖼️ **Dynamic NFT Minting**: Personalized NFTs created based on user requests
- 🔒 **Secure Redemption**: Each gift code can only be used once
- ⚙️ **Access Control**: Only authorized addresses can trigger key functions

## Smart Contract Details
**Contract Name**: GetGift.sol  
**Blockchain**: Avalanche Fuji Testnet  
**Standards**: ERC721 with URI Storage extension  
**Key Dependencies**:
- Chainlink Functions Client
- OpenZeppelin ERC721 implementation
- Chainlink Functions Request library

**Storage**:
- Tracks redeemed gift codes
- Maps gift codes to NFT metadata URIs
- Maintains allowlist of authorized addresses
- Stores request IDs for Chainlink callbacks

## Key Functions

### `sendRequest`
```solidity
function sendRequest(
    uint8 donHostedSecretsSlotID,
    uint64 donHostedSecretsVersion,
    string[] memory args,
    uint64 subscriptionId,
    address userAddr
) external onlyAllowList returns (bytes32 requestId)
```
- Initiates Chainlink Functions request
- Verifies gift code hasn't been redeemed
- Stores user address and gift code for callback
- Requires allowlist permission

### `fulfillRequest`
```solidity
function fulfillRequest(
    bytes32 requestId,
    bytes memory response,
    bytes memory err
) internal override
```
- Chainlink Functions callback handler
- Verifies response matches request ID
- Mints NFT if gift code is valid
- Marks gift code as redeemed

### `safeMint`
```solidity
function safeMint(address to, string memory uri) internal
```
- Internal function to mint NFTs
- Auto-increments token IDs
- Sets token metadata URI

### `addToAllowList`
```solidity
function addToAllowList(address addrToAdd) external onlyAllowList
```
- Adds addresses to authorization allowlist
- Restricted to existing allowlist members

### `addGift`
```solidity
function addGift(string memory giftName, string memory _tokenUri) public onlyAllowList
```
- Adds new gift code options
- Maps gift names to IPFS metadata URIs
- Requires allowlist permission

## Configuration Constants
```solidity
// Chainlink Functions configuration
address public constant ROUTER_ADDR = 0xA9d587a00A31A52Ed70D6026794a8FC5E2F5dCb0;
bytes32 public constant DON_ID = 0x66756e2d6176616c616e6368652d66756a692d31000000000000000000000000;
uint32 public constant CALLBACK_GAS_LIMIT = 300_000;

// Default gift codes and metadata
string constant ITEM_1_METADATA = "ipfs://QmaGqBNqHazCjSMNMuDk6VrgjNLMQKNZqaab1vfMHAwkoj";
string constant ITEM_2_METADATA = "ipfs://QmVfdGGhP6vqic84WJQ1LHxenSh9dd5ZaLCzHU5oAEnvDr";
string constant ITEM_3_METADATA = "ipfs://QmVfdGGhP6vqic84WJQ1LHxenSh9dd5ZaLCzHU5oAEnvDr";
```
