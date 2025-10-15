# DeAI API - Reference

## Overview

REST API for accessing blockchain token holder information and wallet performance data. Built with Node.js and Express, it provides real-time insights into token holdings, portfolio analysis, and trading performance on the Ethereum blockchain (currently).

- **Base URL:** `https://api.decenterai.dev`
- **API Version:** v2
- **Authentication:** API Key required (X-API-Key header)

## Table of Contents

- [Authentication](#authentication)
- [API Tiers](#api-tiers)
- [API Endpoints](#api-endpoints)
- [API Key Management](#api-key-management)
- [Error Handling](#error-handling)
- [Code Examples](#code-examples)

---

# Authentication

All API requests require authentication using an API key passed in the request header:

```
X-API-Key: your_api_key_here
```

## Getting an API Key

Contact support to obtain an API key with the appropriate tier for your needs. **Contact us:** https://t.me/DeCenterAIDev

---

# API Tiers

The API offers three tiers with different limits:

| Tier | Credits | Requests/Second | Duration | Price |
|------|---------|----------------|----------|-------|
| Standard | 10,000 | 2 req/sec | 30 days | 299 USD |
| Pro | 50,000 | 5 req/sec | 30 days | 499 USD |
| Enterprise | Contact us | Contact us | 30 days | Contact for pricing |

---

# API Endpoints

## 1. Get Token Information
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

## 2. Get Top Token Holders
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
  "decimals": "integer",
  "holdersCount": "integer",
  "holders": [
    {
      "ens_domain_name": "string | null",
      "hash": "string (ethereum address)",
      "is_contract": "boolean",
      "name": "string | null",
      "value": "string",
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
  "totalSupply": "1000000000000000000000000000",
  "decimals": 18,
  "holdersCount": 500,
  "holders": [
    {
      "ens_domain_name": null,
      "hash": "0x3cc936b795a188f0e246cbb2d74c5bd190aecf18",
      "is_contract": false,
      "name": null,
      "value": "114009450197447230000000000",
      "address": "0x3cc936b795a188f0e246cbb2d74c5bd190aecf18",
      "balance": "114009450197447230000000000",
      "percentage": "11.40",
      "label": null
    },
    {
      "ens_domain_name": null,
      "hash": "0xc882b111a75c0c657fc507c04fbfcd2cc984f071",
      "is_contract": false,
      "name": null,
      "value": "104867217527337500000000000",
      "address": "0xc882b111a75c0c657fc507c04fbfcd2cc984f071",
      "balance": "104867217527337500000000000",
      "percentage": "10.49",
      "label": null
    },
    {
      "ens_domain_name": null,
      "hash": "0x0d0707963952f2fba59dd06f2b425ace40b492fe",
      "is_contract": true,
      "name": "Gate.io",
      "value": "42568598594038900000000000",
      "address": "0x0d0707963952f2fba59dd06f2b425ace40b492fe",
      "balance": "42568598594038900000000000",
      "percentage": "4.26",
      "label": "Gate.io"
    }
  ]
}
```

## 3. Get Token Holder Balance Changes
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

## 4. Get Average Entry Analysis
Analyze the average entry prices for the top holders of a specific token.

**Endpoint:** `GET /api/token/avg-entry/{contractAddress}`

**Parameters:**

- `contractAddress` (path, required): Ethereum contract address (0x...)

**Response Schema:**

```json
{
  "tokenSymbol": "string",
  "tokenName": "string",
  "decimals": "integer",
  "currentPrice": "number",
  "summary": {
    "totalHolders": "integer",
    "totalHoldersWithData": "integer",
    "avgEntryPrice": "number",
    "medianEntryPrice": "number",
    "lowestEntryPrice": "number",
    "highestEntryPrice": "number",
    "q1EntryPrice": "number",
    "q3EntryPrice": "number",
    "totalHoldingsValue": "number",
    "averageHoldingSize": "number",
    "profitLossMetrics": {
      "holdersInProfit": "integer",
      "holdersInLoss": "integer",
      "holdersAtBreakeven": "integer",
      "profitPercentage": "number",
      "lossPercentage": "number",
      "breakEvenPercentage": "number"
    }
  },
  "holders": [
    {
      "address": "string (ethereum address)",
      "label": "string | null",
      "balance": "string",
      "avgEntryPrice": "number | null",
      "transactionCount": "integer",
      "totalBuys": "integer",
      "totalBuyVolume": "number",
      "totalTokensBought": "number",
      "firstPurchaseDate": "string (ISO 8601) | null",
      "lastPurchaseDate": "string (ISO 8601) | null"
    }
  ]
}
```

**Example Request:**

```bash
curl -X GET "https://api.decenterai.dev/api/token/avg-entry/0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**

```json
{
  "tokenSymbol": "DeAI",
  "tokenName": "DeCenter AI",
  "decimals": 18,
  "currentPrice": 0.0328,
  "summary": {
    "totalHolders": 100,
    "totalHoldersWithData": 85,
    "avgEntryPrice": 0.0245,
    "medianEntryPrice": 0.0238,
    "lowestEntryPrice": 0.0112,
    "highestEntryPrice": 0.0456,
    "q1EntryPrice": 0.0198,
    "q3EntryPrice": 0.0289,
    "totalHoldingsValue": 1250000.50,
    "averageHoldingSize": 45678.90,
    "profitLossMetrics": {
      "holdersInProfit": 68,
      "holdersInLoss": 15,
      "holdersAtBreakeven": 2,
      "profitPercentage": 80.00,
      "lossPercentage": 17.65,
      "breakEvenPercentage": 2.35
    }
  },
  "holders": [
    {
      "address": "0x1234567890123456789012345678901234567890",
      "label": "Whale Investor",
      "balance": "5000000000000000000000",
      "avgEntryPrice": 0.0215,
      "transactionCount": 15,
      "totalBuys": 8,
      "totalBuyVolume": 10750.50,
      "totalTokensBought": 500000,
      "firstPurchaseDate": "2024-01-15T10:30:00.000Z",
      "lastPurchaseDate": "2024-06-20T14:45:00.000Z"
    }
  ]
}
```

## 5. Get Two-Token Overlap
Analyze the overlap between holders of two different tokens.

**Endpoint:** `GET /api/token/two-token-overlap`

**Parameters:**

- `token1` (query, required): First Ethereum contract address (0x...)
- `token2` (query, required): Second Ethereum contract address (0x...)

**Response Schema:**

```json
{
  "token1": {
    "address": "string (ethereum address)",
    "symbol": "string",
    "name": "string",
    "totalHolders": "integer"
  },
  "token2": {
    "address": "string (ethereum address)",
    "symbol": "string",
    "name": "string",
    "totalHolders": "integer"
  },
  "overlappingHolders": [
    {
      "address": "string (ethereum address)",
      "token1Balance": "string",
      "token2Balance": "string",
      "token1Percentage": "number",
      "token2Percentage": "number",
      "rank1": "integer",
      "rank2": "integer"
    }
  ],
  "overlapPercentage": "number",
  "totalOverlapping": "integer"
}
```

**Example Request:**

```bash
curl -X GET "https://api.decenterai.dev/api/token/two-token-overlap?token1=0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6&token2=0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9" \
  -H "X-API-Key: your_api_key_here"
```

**Response:**

```json
{
  "token1": {
    "address": "0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6",
    "symbol": "DeAI",
    "name": "DeCenter AI",
    "totalHolders": 500
  },
  "token2": {
    "address": "0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9",
    "symbol": "KEKIUS",
    "name": "KEKIUS",
    "totalHolders": 450
  },
  "overlappingHolders": [
    {
      "address": "0x1234567890123456789012345678901234567890",
      "token1Balance": "5000000000000000000000",
      "token2Balance": "3000000000000000000000",
      "token1Percentage": 2.5,
      "token2Percentage": 1.8,
      "rank1": 10,
      "rank2": 15
    }
  ],
  "overlapPercentage": 15.56,
  "totalOverlapping": 70
}
```

## 6. Get Portfolio Summary
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

---

# API Key Management

## Get API Key Information
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

---

# Error Handling

## Common HTTP Status Codes

| Status | Description |
|--------|-------------|
| 200 | Success |
| 400 | Bad Request - Invalid input |
| 401 | Unauthorized - Missing or invalid API key |
| 403 | Forbidden - Insufficient credits or permissions |
| 404 | Not Found - Resource doesn't exist |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |

---

# Code Examples

## JavaScript/Node.js

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

// Get average entry analysis
async function getAvgEntryAnalysis(contractAddress) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/avg-entry/${contractAddress}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}

// Get two-token overlap
async function getTwoTokenOverlap(token1Address, token2Address) {
  try {
    const response = await axios.get(`${baseUrl}/api/token/two-token-overlap`, {
      headers: {
        'X-API-Key': apiKey
      },
      params: {
        token1: token1Address,
        token2: token2Address
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

getAvgEntryAnalysis('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6')
  .then(data => console.log('Avg Entry Analysis:', data));

getTwoTokenOverlap('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6', '0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9')
  .then(data => console.log('Two-Token Overlap:', data));

getPortfolio('0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62')
  .then(data => console.log('Portfolio:', data));

getApiKeyInfo()
  .then(data => console.log('API Key Info:', data));
```

## Python

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

def get_avg_entry_analysis(contract_address):
    headers = {'X-API-Key': API_KEY}
    response = requests.get(f'{BASE_URL}/api/token/avg-entry/{contract_address}', headers=headers)
    return response.json()

def get_two_token_overlap(token1_address, token2_address):
    headers = {'X-API-Key': API_KEY}
    params = {'token1': token1_address, 'token2': token2_address}
    response = requests.get(f'{BASE_URL}/api/token/two-token-overlap', headers=headers, params=params)
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

avg_entry = get_avg_entry_analysis('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6')
print('Avg Entry Analysis:', avg_entry)

overlap = get_two_token_overlap('0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6', '0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9')
print('Two-Token Overlap:', overlap)

portfolio = get_portfolio('0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62')
print('Portfolio:', portfolio)

api_key_info = get_api_key_info()
print('API Key Info:', api_key_info)
```

## cURL

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

# Get average entry analysis
curl -X GET "https://api.decenterai.dev/api/token/avg-entry/0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6" \
  -H "X-API-Key: your_api_key_here"

# Get two-token overlap
curl -X GET "https://api.decenterai.dev/api/token/two-token-overlap?token1=0x781dB9A4D8Ae055571e8796DD4423bc13CeE5dD6&token2=0x26E550AC11B26f78A04489d5F20f24E3559f7Dd9" \
  -H "X-API-Key: your_api_key_here"

# Get portfolio data
curl -X GET "https://api.decenterai.dev/api/token/portfolio/0xe2834a9f8f20D74aaaA33Cff3d6314F34ca5be62" \
  -H "X-API-Key: your_api_key_here"

# Check API key status
curl -X GET "https://api.decenterai.dev/api/keys/info" \
  -H "X-API-Key: your_api_key_here"
```

---

# Response Caching

The API implements caching with a 10-minute duration to improve performance. Cached responses are returned when available, ensuring fast response times.

---

## Note

This API is currently in beta. New endpoints will be added regularly, and existing endpoints will receive ongoing improvements and optimizations.
