# DeAI API - Reference

## Overview

REST API for accessing blockchain token holder information and wallet performance data. Built with Node.js and Express, it provides real-time insights into token holdings, portfolio analysis, and trading performance on the Ethereum blockchain (currently).

**Base URL:** `https://api.decenterai.dev`  
**API Version:** v1  
**Authentication:** API Key required (X-API-Key header)

---

## Table of Contents

1. [Authentication](#authentication)
2. [API Tiers](#api-tiers)
3. [API Endpoints](#api-endpoints)
4. [API Key Management](#api-key-management)
5. [Error Handling](#error-handling)
6. [Code Examples](#code-examples)

---

## Authentication

All API requests require authentication using an API key passed in the request header:

```http
X-API-Key: your_api_key_here
```

### Getting an API Key

Contact support to obtain an API key with the appropriate tier for your needs.
Contact us: https://t.me/DeCenterAIDev

---

## API Tiers

The API offers three tiers with different limits:

| Tier | Credits | Requests/Second | Duration | Price |
|------|---------|-----------------|----------|-------|
| **Standard** | 10,000 | 2 req/sec | 30 days | 199 USD |
| **Pro** | 50,000 | 5 req/sec | 30 days | 399 USD |
| **Enterprise** | Contact us | Contact us | 30 days | Contact for pricing |


## API Endpoints

### 1. Get Token Information

Retrieve detailed information about a specific token.

**Endpoint:** `GET /api/token/token-info/{contractAddress}`

**Parameters:**
- `contractAddress` (path, required): Ethereum contract address (0x...)

**Response Schema:**
```json
{
  "address": "string (ethereum address)",
  "decimals": "integer",
  "name": "string",
  "symbol": "string", 
  "logoUrl": "string (url) | null",
  "buyFee": "number",
  "sellFee": "number"
}
```

**Example Request:**
```bash
curl -X GET "https://api.decenterai.dev/api/token/token-info/0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**
```json
{
  "address": "0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6",
  "decimals": 18,
  "name": "DeCenter AI",
  "symbol": "DeAI",
  "logoUrl": "https://coin-images.coingecko.com/coins/images/55693/large/DeAI_cg.png?1747039131",
  "buyFee": 5,
  "sellFee": 5
}
```

### 2. Get Top Token Holders

Retrieve the top holders of a specific token with their balances, percentages and label.

**Endpoint:** `GET /api/token/top-holders/{contractAddress}`

**Parameters:**
- `contractAddress` (path, required): Ethereum contract address (0x...)

**Response Schema:**
```json
{
  "tokenName": "string",
  "tokenSymbol": "string", 
  "totalSupply": "string",
  "holdersCount": "integer",
  "holders": [
    {
      "address": "string (ethereum address)",
      "balance": "string",
      "percentage": "string", 
      "label": "string | null"
    }
  ]
}
```

**Example Request:**
```bash
curl -X GET "https://api.decenterai.dev/api/token/top-holders/0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**
```json
{
  "tokenName": "KEKIUS",
  "tokenSymbol": "KEKIUS",
  "totalSupply": "1000000000",
  "holdersCount": 26638,
  "holders": [
    {
      "address": "0x3cc936b795a188f0e246cbb2d74c5bd190aecf18",
      "balance": "114009450.19744723",
      "percentage": "11.40",
      "label": null
    },
    {
      "address": "0xc882b111a75c0c657fc507c04fbfcd2cc984f071",
      "balance": "104867217.5273375",
      "percentage": "10.49",
      "label": null
    },
    {
      "address": "0x0d0707963952f2fba59dd06f2b425ace40b492fe",
      "balance": "42568598.5940389",
      "percentage": "4.26",
      "label": "Gate.io"
    }
  ]
}
```

### 3. Get Token Holder Balance Changes

Track balance changes for token holders over the last 7 days.

**Endpoint:** `GET /api/token/token-holder-balance-changes/{contractAddress}`

**Parameters:**
- `contractAddress` (path, required): Ethereum contract address (0x...)

**Response Schema:**
```json
{
  "tokenAddress": "string (ethereum address)",
  "tokenHolderBalanceChanges": [
    {
      "address": "string (ethereum address)",
      "balanceStart": "number",
      "balanceEnd": "number",
      "label": "string | null",
      "change": "number"
    }
  ]
}
```

**Example Request:**
```bash
curl -X GET "https://api.decenterai.dev/api/token/token-holder-balance-changes/0xf816507E690f5Aa4E29d164885EB5fa7a5627860" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**
```json
{
  "tokenAddress": "0xf816507e690f5aa4e29d164885eb5fa7a5627860",
  "tokenHolderBalanceChanges": [
    {
      "address": "0x9642b23ed1e01df1092b92641051881a322f5d4e",
      "balanceStart": 13991207769.78152,
      "balanceEnd": 19480069275.248146,
      "label": null,
      "change": 5488861505.466625
    },
    {
      "address": "0x4e755a5ab12923cb5348561ca07af2a9ec08fd4b",
      "balanceStart": 4815567967.425969,
      "balanceEnd": 315567967.425969,
      "label": null,
      "change": -4500000000
    }
  ]
}
```

### 4. Get Portfolio Summary

Retrieve comprehensive portfolio data for a wallet address.

**Endpoint:** `GET /api/token/portfolio/{walletAddress}`

**Parameters:**
- `walletAddress` (path, required): Ethereum wallet address (0x...)

**Response Schema:**
```json
{
  "totalPortfolioValue": "number",
  "totalPortfolioValueChange": "number", 
  "totalPortfolioValueChangePercentage": "number",
  "tokenBalances": [
    {
      "amount": "number",
      "valueUSD": "number | null",
      "token": {
        "address": "string (ethereum address) | null",
        "decimals": "integer",
        "name": "string",
        "symbol": "string",
        "logoUrl": "string (url) | null"
      }
    }
  ]
}
```

**Example Request:**
```bash
curl -X GET "https://api.decenterai.dev/api/token/portfolio/0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**
```json
{
  "totalPortfolioValue": 32558.188663937264,
  "totalPortfolioValueChange": 8007.940769704637,
  "totalPortfolioValueChangePercentage": 32.618573972061085,
  "tokenBalances": [
    {
      "amount": 987685.0955167826,
      "valueUSD": 32409.00486927626,
      "token": {
        "address": "0xaA7D24c3E14491aBaC746a98751A4883E9b70843",
        "decimals": 18,
        "name": "Guru",
        "symbol": "GURU",
        "logoUrl": "https://coin-images.coingecko.com/coins/images/52806/large/TokenLogo.png?1734359498"
      }
    },
    {
      "amount": 0.0556942734172829,
      "valueUSD": 149.18379466100234,
      "token": {
        "address": null,
        "decimals": 18,
        "name": "Ethereum",
        "symbol": "ETH",
        "logoUrl": "https://token-icons.s3.amazonaws.com/eth.png"
      }
    }
  ]
}
```

## API Key Management

### Get API Key Information

Check your current API key status, credits, and tier information.

**Endpoint:** `GET /api/keys/info`

**Headers:**
- `X-API-Key`: Your API key

**Response Schema:**
```json
{
  "tier": "string (standard|pro|enterprise)",
  "credits": "integer", 
  "requestsPerSecond": "integer",
  "expiresAt": "string (ISO 8601 datetime)",
  "lastPaymentDate": "string (ISO 8601 datetime)",
  "active": "boolean"
}
```

**Example Request:**
```bash
curl -X GET "https://api.decenterai.dev/api/keys/info" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**
```json
{
  "tier": "pro",
  "credits": 45230,
  "requestsPerSecond": 5,
  "expiresAt": "2025-07-19T10:30:00.000Z",
  "lastPaymentDate": "2025-06-19T10:30:00.000Z",
  "active": true
}
```
## Error Handling

### Common HTTP Status Codes

| Status | Description |
|--------|-------------|
| `200` | Success |
| `400` | Bad Request - Invalid input |
| `401` | Unauthorized - Missing or invalid API key |
| `403` | Forbidden - Insufficient credits or permissions |
| `404` | Not Found - Resource doesn't exist |
| `429` | Too Many Requests - Rate limit exceeded |
| `500` | Internal Server Error |

---

## Code Examples

### JavaScript/Node.js

```javascript
const axios = require('axios');

const apiKey = 'your_api_key_here';
const baseUrl = 'https://api.decenterai.dev';

// Get token information
async function getTokenInfo(contractAddress) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/token-info/${contractAddress}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Get top holders
async function getTopHolders(contractAddress) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/top-holders/${contractAddress}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Get token holder balance changes
async function getTokenHolderBalanceChanges(contractAddress) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/token-holder-balance-changes/${contractAddress}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Get portfolio summary
async function getPortfolio(walletAddress) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/portfolio/${walletAddress}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Get API key information
async function getApiKeyInfo() {
  try {
    const response = await axios.get(`${baseUrl}/api/keys/info`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Usage examples
getTokenInfo('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6')
  .then(data => console.log('Token Info:', data));

getTopHolders('0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9')
  .then(data => console.log('Top Holders:', data));

getTokenHolderBalanceChanges('0xf816507E690f5Aa4E29d164885EB5fa7a5627860')
  .then(data => console.log('Balance Changes:', data));

getPortfolio('0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62')
  .then(data => console.log('Portfolio:', data));

getApiKeyInfo()
  .then(data => console.log('API Key Info:', data));
```

### Python

```python
import requests

API_KEY = 'your_api_key_here'
BASE_URL = 'https://api.decenterai.dev'

def get_token_info(contract_address):
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/token/token-info/{contract_address}', headers=headers)
    return response.json()

def get_top_holders(contract_address):
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/token/top-holders/{contract_address}', headers=headers)
    return response.json()

def get_token_holder_balance_changes(contract_address):
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/token/token-holder-balance-changes/{contract_address}', headers=headers)
    return response.json()

def get_portfolio(wallet_address):
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/token/portfolio/{wallet_address}', headers=headers)
    return response.json()

def get_api_key_info():
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/keys/info', headers=headers)
    return response.json()

# Usage examples
token_info = get_token_info('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6')
print('Token Info:', token_info)

top_holders = get_top_holders('0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9')
print('Top Holders:', top_holders)

balance_changes = get_token_holder_balance_changes('0xf816507E690f5Aa4E29d164885EB5fa7a5627860')
print('Balance Changes:', balance_changes)

portfolio = get_portfolio('0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62')
print('Portfolio:', portfolio)

api_key_info = get_api_key_info()
print('API Key Info:', api_key_info)
```

### cURL

```bash
# Get token information
curl -X GET "https://api.decenterai.dev/api/token/token-info/0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6" \
  -H "X-API-Key: your_api_key_here"

# Get top holders
curl -X GET "https://api.decenterai.dev/api/token/top-holders/0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9" \
  -H "X-API-Key: your_api_key_here"

# Get token holder balance changes
curl -X GET "https://api.decenterai.dev/api/token/token-holder-balance-changes/0xf816507E690f5Aa4E29d164885EB5fa7a5627860" \
  -H "X-API-Key: your_api_key_here"

# Get portfolio data
curl -X GET "https://api.decenterai.dev/api/token/portfolio/0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62" \
  -H "X-API-Key: your_api_key_here"

# Check API key status
curl -X GET "https://api.decenterai.dev/api/keys/info" \
  -H "X-API-Key: your_api_key_here"
```

---

### Response Caching

The API implements caching with a 10-minute duration to improve performance. Cached responses are returned when available, ensuring fast response times.



### Note

 This API is currently in beta. New endpoints will be added regularly, and existing endpoints will receive ongoing improvements and optimizations.
