---
description: Upload a CSV file with wallet addresses to be included into Safary's users.
---

# Upload wallets

In the **Users dashboard** you can click on the following button to upload wallets to Safary:

<figure><img src="../../.gitbook/assets/image.png" alt="" width="181"><figcaption><p>Button to upload a CSV with wallets</p></figcaption></figure>

To **monitor** the upload you can to "Settings" -> "File Uploads" or [https://app.safary.club/home/settings/uploads](https://app.safary.club/home/settings/uploads). Nonetheless, you will receive an **e-mail** when the data pipeline finishes processing the uploaded data.

### Data Format

This upload expects a **CSV** file with the following fields, <mark style="color:red;">**separated by comma**</mark>.

<table><thead><tr><th width="159">FIELD</th><th width="83">TYPE</th><th width="101">USE</th><th>DESCRIPTION</th></tr></thead><tbody><tr><td>wallet_address</td><td>string</td><td><mark style="color:red;"><strong>Required</strong></mark></td><td>Wallet address to be imported. Example: "0x00000011111111010101001111111111"</td></tr><tr><td>username</td><td>string</td><td>Optional</td><td>Name to be associated with the User that has the wallet address.</td></tr><tr><td>twitter_link</td><td>string</td><td>Optional</td><td>URL of the twitter profile of the User that has the wallet address. Example: "<a href="https://x.com/elonmusk">https://x.com/elonmusk</a>"<br><strong>Must start with "http://" or "https://".</strong></td></tr><tr><td>email</td><td>string</td><td>Optional</td><td>E-mail of the User that has the wallet address.<br><strong>Must be a valid e-mail.</strong></td></tr></tbody></table>

Note that the first line of the CSV file to be uploaded must contain the names of the fields.&#x20;

* For example, if you want to only upload wallet addresses and twitter profiles, the first line would be: "wallet\_address,twitter\_link".

Finally, note that when Safary processes the uploaded wallets, they either become new Users (if they have never been captured by Safary) or are associated with existing Users.

### CSV File Example

Here's an example of a CSV file that would be uploaded successfully. Note that you do not need to add all the fields, only `wallet_address` is mandatory.

{% file src="../../.gitbook/assets/example_file_to_upload.csv" %}
Example of CSV file with all fields that can be uploaded&#x20;
{% endfile %}
