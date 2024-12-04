---
description: Track Custom Events using Safary's Javascript SDK
---

# Setup events

Safary makes available after `document.readyState === 'complete'` a method under `window.safary.track`, which can be executed while defining an event type, event name and additional parameters — see complete documentation below.

To help the visualization of common events in Safary's dashboards, below we also indicate how to fill the `parameters` field for some events, such as swap, deposit and withdrawal.

***

## Track Function

```typescript
safary.track({
            eventType: string,
            eventName: string,
            parameters: object
          })
```

<table><thead><tr><th width="139">FIELD</th><th width="83">TYPE</th><th width="105">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>eventType</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>Type/Category of the event. For example: “click”, “stake”, “swap”.</td></tr><tr><td>eventName</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>Specific name of the event of the type defined. For example: “submit button”, “swap offer 1”, “buy in main page”.</td></tr><tr><td>parameters</td><td>object</td><td>Optional</td><td>A dictionary of properties for the specific event being tracked. For example, if the event was “purchase”, it might have properties like “price” and “product”.</td></tr></tbody></table>

Example to track two different **generic** events:

* Event of type "click":

```typescript
safary.track( { eventType: "click", eventName: "form submit" } 
```

* Event of type "add-to-favorites"

```typescript
safary.track( { eventType: "add-to-favorites", 
                eventName: "main offer",
                parameters: { price: "0.001", currency: "ETH" } 
             } )
```

See the last section of this page for troubleshooting.

***

## Swap Event

To track a swap event, use the `track` function with "swap" as the `eventType`, choose any name as `eventName`, and make sure to add all the <mark style="color:red;">**required**</mark> attributes in the field `parameters`, which are listed below.

<table><thead><tr><th width="176">parameters</th><th width="91">TYPE</th><th width="110">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>walletAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The wallet address being used for the swap.</td></tr><tr><td>fromAmount</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The amount being from which the swap is occurring.</td></tr><tr><td>fromCurrency</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The currency from which the swap is occurring.</td></tr><tr><td>fromAmountUSD</td><td>number</td><td>Optional</td><td>The currency from which the swap is occurring in USD. Fill if the USD amount is available during the function call.</td></tr><tr><td>contractAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The contract address related to the swap. This will be used in the future to check if the swap was actually successful using on-chain data.</td></tr></tbody></table>

Example only with required fields:

```typescript
safary.track({
            eventType: "swap",
            eventName: "swaps-main",
            parameters: { 
                walletAddress: "0x9999999999999",
                fromAmount: 0.001,
                fromCurrency: "ETH",
                contractAddress: "0x000000000000",
            }
          })
```

Example with the suggested field `fromAmountUSD`:

```typescript
safary.track({
            eventType: "swap",
            eventName: "swaps-main",
            parameters: { 
                walletAddress: "0x9999999999999",
                fromAmount: 0.001,
                fromCurrency: "ETH",
                fromAmountUSD: 1.8,
                contractAddress: "0x000000000000",
            }
          })
```

Please note that the parameters field can also include any other information you believe is useful to track. For example, in the case of swaps it might be interesting to add properties like the `toAmount`, `toAmountUSD` and `toCurrency` - see the following example with these additional information:

```typescript
safary.track({
            eventType: "swap",
            eventName: "swaps-main",
            parameters: { 
                walletAddress: "0x9999999999999",
                fromAmount: 0.001,
                fromCurrency: "ETH",
                fromAmountUSD: 1.8,
                contractAddress: "0x000000000000",
                toAmount: 0.000045,
                toCurrency: "BTC",
                toAmountUSD: 1.73,
            }
          })
```

See the last section of this page for troubleshooting.

***

## Deposit Event

To track a deposit event, use the `track` function with "deposit" as the `eventType`, choose any name as `eventName`, and make sure to add all the <mark style="color:red;">**required**</mark> attributes in the field `parameters`, which are listed below.

<table><thead><tr><th width="174">parameters</th><th width="93">TYPE</th><th width="105">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>walletAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The wallet address being used for the deposit.</td></tr><tr><td>amount</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The amount being deposited.</td></tr><tr><td>currency</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The currency of the amount being deposited.</td></tr><tr><td>amountUSD</td><td>number</td><td>Optional</td><td>The amount being deposited in USD. Fill if the USD amount is available during the function call.</td></tr><tr><td>contractAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The contract address related to the deposit. This will be used in the future to check if it was actually successful using on-chain data.</td></tr></tbody></table>

Example only with required fields:

```typescript
safary.track({
            eventType: "deposit",
            eventName: "deposit-promo-01",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                contractAddress: "0x1111111111111",
            }
          })
```

Example with the suggested field `amountUSD`:

```typescript
safary.track({
            eventType: "deposit",
            eventName: "deposit-promo-01",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
            }
          })
```

Please note that the parameters field can also include any other information you believe is useful to track. For example, in the case of deposits it might be interesting to add a property like the `transactionHash` - see the following example with this additional information:

```typescript
safary.track({
            eventType: "deposit",
            eventName: "deposit-promo-01",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
                transactionHash: "0x123123123123123",
            }
          })
```

See the last section of this page for troubleshooting.

***

## Withdrawal Event

To track a withdrawal event, use the `track` function with "withdrawal" as the `eventType`, choose any name as `eventName`, and make sure to add all the <mark style="color:red;">**required**</mark> attributes in the field `parameters`, which are listed below.

<table><thead><tr><th width="171">parameters</th><th width="93">TYPE</th><th width="100">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>walletAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The wallet address used for the withdrawal.</td></tr><tr><td>amount</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The amount being withdrawn.</td></tr><tr><td>currency</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The currency of the amount being withdrawn.</td></tr><tr><td>amountUSD</td><td>number</td><td>Optional</td><td>The amount being withdrawn in USD. Fill if the USD amount is available during the function call.</td></tr><tr><td>contractAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The contract address related to the withdrawal. This will be used in the future to check if it was actually successful using on-chain data.</td></tr></tbody></table>

Example only with required fields:

```typescript
safary.track({
            eventType: "withdrawal",
            eventName: "withdrawal-mtd-12",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                contractAddress: "0x1111111111111",
            }
          })
```

Example with the suggested field `amountUSD`:

```typescript
safary.track({
            eventType: "withdrawal",
            eventName: "withdrawal-mtd-12",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
            }
          })
```

Please note that the parameters field can also include any other information you believe is useful to track. For example, in the case of withdrawals it might be interesting to add a property like the `transactionHash` - see the following example with this additional information:

```typescript
safary.track({
            eventType: "withdrawal",
            eventName: "withdrawal-mtd-12",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
                transactionHash: "0x123123123123123",
            }
          })
```

See the last section of this page for troubleshooting.

***

## NFT Purchase Event

To track a NFT purchase event, use the `track` function with "NFT purchase" as the `eventType`, choose any name as `eventName`, and make sure to add all the <mark style="color:red;">**required**</mark> attributes in the field `parameters`, which are listed below.

<table><thead><tr><th width="175">parameters</th><th width="91">TYPE</th><th width="104">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>walletAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The wallet address used for the purchase.</td></tr><tr><td>amount</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The amount of the NFT purchase.</td></tr><tr><td>currency</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The currency of the amount of the NFT purchase.</td></tr><tr><td>amountUSD</td><td>number</td><td>Optional</td><td>The amount of the NFT purchase in USD. Fill if the USD amount is available during the function call.</td></tr><tr><td>contractAddress</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The contract address related to the NFT purchase. This will be used in the future to check if it was actually successful using on-chain data.</td></tr><tr><td>tokenId</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>The token id of the NFT referenced in the contract address.</td></tr></tbody></table>

Example only with required fields:

```typescript
safary.track({
            eventType: "NFT purchase",
            eventName: "nft-mario-crypto-99",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                contractAddress: "0x1111111111111",
                tokenId: 1,
            }
          })
```

Example with the suggested field `amountUSD`:

```typescript
safary.track({
            eventType: "NFT purchase",
            eventName: "nft-mario-crypto-99",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
                tokenId: 1,
            }
          })
```

Please note that the parameters field can also include any other information you believe is useful to track. For example, in the case of NFT purchases it might be interesting to add a property like the `transactionHash` - see the following example with this additional information:

```typescript
safary.track({
            eventType: "NFT purchase",
            eventName: "nft-mario-crypto-99",
            parameters: { 
                walletAddress: "0x9999999999999",
                amount: 0.001,
                currency: "ETH",
                amountUSD: 1.8,
                contractAddress: "0x1111111111111",
                tokenId: 1,
                transactionHash: "0x123123123123123",
            }
          })
```

See the last section of this page for troubleshooting.

***

## Form Event

The `form` event can be used to track user information. To track a form submission event, use the track function with "form" as the `eventType`, and any appropriate name as `eventName`, and make sure to add the <mark style="color:red;">**required**</mark> attributes in the field `parameters`. The `parameters` field can contain an email or a telegram username, optionally including the user's name.

| parameters | TYPE   | USE                                             | DESCRIPTION                                             |
| ---------- | ------ | ----------------------------------------------- | ------------------------------------------------------- |
| email      | string | <mark style="color:red;">**Required**</mark> \* | \*optional when telegram is set                         |
| telegram   | string | <mark style="color:red;">**Required**</mark> \* | <p>Telegram username<br>*optional when email is set</p> |
| name       | string | Optional                                        |                                                         |

Example only with required email field:

```javascript
safary.track({
  eventType: "form",
  eventName: "newsletter-signup",
  parameters: { 
    email: "user@example.com"
  }
})
```

Example only with required telegram field:

```javascript
safary.track({
  eventType: "form",
  eventName: "telegram-signup",
  parameters: { 
    telegram: "user123"
  }
})
```

Example with optional name field included:

```javascript
safary.track({
  eventType: "form",
  eventName: "newsletter-signup",
  parameters: { 
    email: "user@example.com",
    name: "John Doe"
  }
})

safary.track({
  eventType: "form",
  eventName: "telegram-signup",
  parameters: { 
    telegram: "user123",
    name: "Jane Doe"
  }
})
```

Please note that the `parameters` field can also include any other information you believe is useful to track. For form submissions, additional properties relevant to your application's needs might be included.

See the last section of this page for troubleshooting.

***

## Social Login Event

The `social_login` event can be used to track information regarding logins made using socials. To track a social login event, use the track function with "social\_login" as the `eventType`, and any appropriate name as `eventName`, and make sure to add the <mark style="color:red;">**required**</mark> attributes in the field `parameters`.&#x20;

The `parameters` field should contain at least one of the three possible fields: `twitter_username`, `lens_handle` and `farcaster_id` described below. (note these field names are **case sensitive**).

<table><thead><tr><th width="178">parameters</th><th width="91">TYPE</th><th width="199">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>twitter_username</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark> *<br>*Optional when <code>lens_handle</code> or <code>farcaster_id</code> is set</td><td>Twitter/X username.<br>For example, for <a href="https://x.com/Safaryclub">https://x.com/safaryclub</a>, send only "safaryclub".</td></tr><tr><td>lens_handle</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark> *<br>*Optional when <code>twitter_username</code> or <code>farcaster_id</code> is set</td><td>Lens handle without the namespace. <br>For example, for <a href="https://hey.xyz/u/ricardocarvalho">https://hey.xyz/u/ricardocarvalho</a><br>send only "ricardocarvalho"</td></tr><tr><td>farcaster_id</td><td>number</td><td><mark style="color:red;"><strong>Required</strong></mark> *<br>*Optional when <code>twitter_username</code> or <code>farcaster_id</code> is set</td><td>Farcaster ID (aka FID).<br>For example, for <a href="https://warpcast.com/jhv">https://warpcast.com/jhv</a> click on the three dots and then click on "About" to see the FID of 1385.</td></tr></tbody></table>

Example only with required `twitter_username` field:

```javascript
safary.track({
  eventType: "social_login",
  eventName: "login-using-twitter",
  parameters: { 
    twitter_username: "elonmusk"
  }
})
```

Example only with required `lens_handle` field:

```javascript
safary.track({
  eventType: "social_login",
  eventName: "login-using-lens",
  parameters: { 
    lens_handle: "ricardocarvalho"
  }
})
```

Example only with required `farcaster_id` field:

```javascript
safary.track({
  eventType: "social_login",
  eventName: "login-using-farcaster",
  parameters: { 
    farcaster_id: "jhv"
  }
})
```

Please note that the `parameters` field can also include any other information you believe is useful to track. For form submissions, additional properties relevant to your application's needs might be included.

See the last section of this page for troubleshooting.

***

## Example using Typescript

**Step 1:** Declaring `safary` in the window object.

```typescript
declare global {
  interface Window {
    safary?: {
      track: (args: {
        eventType: string
        eventName: string
        parameters?: { [key: string]: string | number | boolean }
      }) => void
    }
  }
}
```

**Step 2:** In your code, when some action you want to track is triggered, containing, for example, some `contextualData`, you can use `safary.track` as follows.

* To track any custom event, for example, a "click" event:

```typescript
window.safary?.track({
  eventType: 'click',
  eventName: 'signup',
  parameters: {
    button: (contextualData.get('button-id') as string) || '',
    funnel_step: (contextualData.get('page-url') as string) || '',
  },
})
```

* To track a "swap event following our standards define above, including the optional field (`fromAmountUSD`) and an additional information (`chainId`):

```typescript
window.safary?.track({
  eventType: 'swap',
  eventName: 'swap-main',
  parameters: {
    walletAddress: (contextualData.get('walletAddress') as string),
    fromAmount: (contextualData.get('amount') as number),
    fromCurrency: (contextualData.get('currency') as string),
    fromAmountUSD: (contextualData.get('amountUSD') as number),
    contractAddress: (contextualData.get('contractAddress') as string),
    chainId: (contextualData.get('chainId') as string) || '',
  },
})
```

***

## Troubleshooting

Required arguments:

* Note that `eventType` + `eventName` are required arguments for `safary.track` .
* Therefore, for example, the following would **not** work:

```tsx
safary.track( { eventName: "main offer" } )
// Gives:
// ERROR: safary.track(): eventType is undefined.
```

Required format:

* Note that the `parameters` used in the both tracking functions is required to be an **object.**
* Therefore, for example, passing a **string** as `parameters` would **not** work:

```tsx
safary.track( { eventType: "buy", eventName: "main offer", parameters: "ETH" } )
// Gives:
// ERROR: safary.track(): parameters is not an object.
```

