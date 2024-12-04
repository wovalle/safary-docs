---
description: Pro Feature, BETA.
---

# Safary API

Welcome to the first public release of Safary API. This REST API allows you to access diverse resources from [https://api.safary.club](https://api.safary.club/). With a range of methods and endpoints at your disposal, you can manipulate and retrieve useful data with ease. It should be noted, however, that this is a beta release. As we continue to develop and refine our API, changes may occur, so your patience and understanding are most appreciated.

### Versioning

Our API uses a versioning system to ensure its evolutivity. Each version is pathed; for example, the current version is `v1`. Hence, all methods are found behind the main URL as follows: `https://api.safary.club/v1/{method-name}`. This structure allows us to evolve the API while maintaining backwards compatibility.

### Authentication

Authentication to our API is performed using API keys. To create your keys, you must first log in to your Safary account. Then, navigate to your settings and find the subheading labeled 'API keys'.

Include your API key in your API calls using the `Authorization: Bearer {Your API key}` HTTP header. Here is an example using `curl`:

```sh
curl -X GET 'https://api.safary.club/v1/{method-name}' \
-H 'Authorization: Bearer {your-api-key}'
```

Note: Replace the `{method-name}` placeholder with the actual method name (eg: `websites`, `wallets`) you want to access and replace `{your-api-key}` with your actual API key.

### Pagination

All list endpoints in our API are paginated. List endpoints accept two pagination parameters: `page` and `limit`. Currently, the API can return a maximum of 100 elements at a time. However, this could change as we continue to develop and enhance the API.

### Entities

### Website

<table><thead><tr><th>Field name</th><th>Data type</th><th>Comment</th><th data-hidden>Remarks</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td></td><td></td></tr><tr><td><code>name</code></td><td>string / null</td><td></td><td></td></tr><tr><td><code>app_url</code></td><td>string / null</td><td></td><td></td></tr><tr><td><code>archived</code></td><td>boolean</td><td></td><td></td></tr><tr><td><code>industry</code></td><td>string / null</td><td></td><td></td></tr><tr><td><code>website_url</code></td><td>string</td><td></td><td></td></tr><tr><td><code>created_at</code></td><td>string</td><td><a href="https://www.iso.org/iso-8601-date-and-time-format.html">ISO8601</a> Date String</td><td></td></tr><tr><td><code>updated_at</code></td><td>Date</td><td><a href="https://www.iso.org/iso-8601-date-and-time-format.html">ISO8601</a> Date String</td><td></td></tr></tbody></table>

### Wallet

<table><thead><tr><th>Field name</th><th>Data type</th><th>Comment</th><th data-hidden>Remarks</th></tr></thead><tbody><tr><td><code>address</code></td><td>string</td><td></td><td></td></tr><tr><td><code>balance</code></td><td>number</td><td>In USD</td><td></td></tr><tr><td><code>country</code></td><td>string</td><td><a href="https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes">ISO3166</a> Country Code</td><td></td></tr><tr><td><code>source</code></td><td>string</td><td></td><td></td></tr><tr><td><code>connection_date</code></td><td>string</td><td><a href="https://www.iso.org/iso-8601-date-and-time-format.html">ISO8601</a> Date String</td><td></td></tr></tbody></table>

### Event

| Field         | Type   | Comment                                                                       |
| ------------- | ------ | ----------------------------------------------------------------------------- |
| id            | string |                                                                               |
| product\_id   | string |                                                                               |
| session\_id   | string |                                                                               |
| date          | string | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
| type          | string |                                                                               |
| name          | string |                                                                               |
| json\_payload | string | JSON string                                                                   |
| amount\_usd   | number | Nullable, only available for specific event types.                            |

### Campaign

| Property Name  | Property Data Type | Description                                                                                                                                                                              |
| -------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id             | string             |                                                                                                                                                                                          |
| kind           | string             | Identifies the type of campaign. Possible values: 'slise', 'hypelab', 'blockchain\_ads', 'layer3', 'guildxyz', 'questn', 'zealy', 'link', 'galxe', 'intract', 'farcaster\_frames\_link'. |
| source         | string             |                                                                                                                                                                                          |
| name           | string             |                                                                                                                                                                                          |
| medium         | string             |                                                                                                                                                                                          |
| content        | string             |                                                                                                                                                                                          |
| target\_url    | string             |                                                                                                                                                                                          |
| code           | string             |                                                                                                                                                                                          |
| created\_at    | date               | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String                                                                                                            |
| updated\_at    | date               | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String                                                                                                            |
| external\_link | string             |                                                                                                                                                                                          |

### Wallet Activity

| kind (activity variants) | Field        | Type   | Comment                                                                       |
| ------------------------ | ------------ | ------ | ----------------------------------------------------------------------------- |
| visit                    | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | url          | string |                                                                               |
| wallet\_connected        | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | address      | string |                                                                               |
|                          | type         | string |                                                                               |
|                          | chain        | string |                                                                               |
| chain\_changed           | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | chain        | string |                                                                               |
| swap                     | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | event        | string |                                                                               |
|                          | currencyFrom | string |                                                                               |
|                          | currencyTo   | string |                                                                               |
|                          | amountUSD    | number |                                                                               |
| deposit                  | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | event        | string |                                                                               |
|                          | currency     | string |                                                                               |
|                          | amountUSD    | number |                                                                               |
| withdrawal               | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | event        | string |                                                                               |
|                          | currency     | string |                                                                               |
|                          | amountUSD    | number |                                                                               |
| event                    | activity     | string |                                                                               |
|                          | date         | date   | [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) Date String |
|                          | event        | string |                                                                               |
| other                    | activity     | string |                                                                               |
|                          | date         | date   |                                                                               |

#### Websites

The `/websites` endpoint provides access to website-related resources.

* **GET /websites**: Returns a list of websites associated with your project. The list is paginated and supports the parameters `page` and `limit`.
* **GET /websites/{websiteId}**: This method returns the details for a single website. Replace `{websiteId}` with the ID of the specific website you are interested in.

#### Events

The `/events` endpoint provides access to custom events related to specific websites.

* **GET /events**: This endpoint returns a list of custom events tied to a website. The list is paginated and supports the parameters `page` and `limit`. When using this endpoint, make sure to format your URL and endpoint like this: `/events?websiteId={your_website_id}`. Replace `{your_website_id}` with your specific website's ID&#x20;
* **GET /events/:eventId**: This method returns details of a specific event related to a website. Include a query param for the specific website. method returns the details for a single website. When using this endpoint, make sure to format your URL and endpoint like this: `/events/:eventId?websiteId={your_website_id}`. Replace `:eventId` with the specific event id you want to check and  `{your_website_id}` with your specific website's ID&#x20;

#### Wallets

The `/wallets` endpoint provides information about crypto wallets associated to your service. This endpoint requires you to pass a `websiteId` as a query parameter to identify the specific website for which you wish to retrieve the wallets.

* **GET /wallets**: Returns a list of wallets associated to your service. The list is paginated and supports the parameters `page` and `limit`. When using this endpoint, make sure to format your URL and endpoint like this: `/wallets?websiteId={your_website_id}`. Replace `{your_website_id}` with your specific website's ID
* **GET /wallets/:address**: Returns the details of an specific wallet associated with your website. When using this endpoint, make sure to format your URL and endpoint like this: `/wallets/:address?websiteId={your_website_id}`. Replace `:address` with the specific wallet address tou want to check and `{your_website_id}` with your specific website's ID&#x20;
* **GET /wallets/:address/related**: Returns other wallets associated with the user of the original wallet. The list is paginated and supports the parameters `page` and `limit`. When using this endpoint, make sure to format your URL and endpoint like this: `/wallets/:address/related?websiteId={your_website_id}`. Replace `:address` with the specific wallet address tou want to check and `{your_website_id}` with your specific website's ID&#x20;
* **GET /wallets/:address/activity**: Returns all of the activity related to an specific wallet. The list is paginated and supports the parameters `page` and `limit`. When using this endpoint, make sure to format your URL and endpoint like this: `/wallets/:address/activity?websiteId={your_website_id}`. Replace `:address` with the specific wallet address tou want to check and `{your_website_id}` with your specific website's ID&#x20;

#### Campaigns

The `/campaigns` endpoint provides information and actions related to marketing campaigns in your product. This endpoint requires you to pass a `websiteId` as a query parameter to identify the specific website for which you wish to retrieve or manipulate campaign data.

**Endpoints:**

* **GET /campaigns**: Returns a list of campaigns. The list is paginated and supports the parameters `page` and `limit`. When using this endpoint, make sure to format your URL like this: `/campaigns?websiteId={your_website_id}`. Replace `{your_website_id}` with your specific website's ID.
* **GET /campaigns/:id**: Returns the details of a specific campaign. When using this endpoint, make sure to format your URL like this: `/campaigns/:campaignId?websiteId={your_website_id}`. Replace `:campaignId` with the specific campaign ID you want to retrieve and `{your_website_id}` with your specific website's ID.
* **POST /campaigns**: Creates a new campaign. You need to pass the details in the request body according to the campaign schema below. When using this endpoint, format your URL like this: `/campaigns?websiteId={your_website_id}`.
* **DELETE /campaigns/:id**: Deletes a specific campaign by its ID. When using this endpoint, format your URL like this: `/campaigns/:campaignId?websiteId={your_website_id}`. Replace `:campaignId` with the specific campaign ID you want to delete and `{your_website_id}` with your specific website's ID.

**Create Campaign Request Body Schema**

Below is a table describing the parameters required in the request body for creating different types of campaigns.

| Field     | Type   | Description                                                                                                                                                                              | Required |
| --------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| kind      | string | Identifies the type of campaign. Possible values: 'slise', 'hypelab', 'blockchain\_ads', 'layer3', 'guildxyz', 'questn', 'zealy', 'link', 'galxe', 'intract', 'farcaster\_frames\_link'. | Yes      |
| source    | string | Source of the campaign (1-100 chars).                                                                                                                                                    | Yes      |
| name      | string | Name of the campaign (1-100 chars).                                                                                                                                                      | Yes      |
| targetUrl | string | URL targeted by the campaign. Must be a valid URL.                                                                                                                                       | Yes      |
| medium    | string | (Optional) Medium of the campaign. Only required for 'link' kind.                                                                                                                        | No       |
| content   | string | (Optional) Content of the campaign. Only required for 'link' kind.                                                                                                                       | No       |
