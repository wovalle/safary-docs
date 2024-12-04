---
description: Pro Feature, BETA.
---

# Custom domains

Safary now provides the additional feature of tracking campaigns using a `safary.link` link. In the ever-evolving domain of web3, security stands at the forefront of critical aspects to consider. There exist greater trust and credibility in a link belonging to your domain, compared to a third-party link. In line with these considerations, we've added the option to use your own custom domain with Safary Campaigns.

The usage of custom domains follows these steps:

* The user creates a CNAME record pointing towards `redir.safary.link`
* Safary then enables the use of the registered custom domain
* Campaign links can now be directly accessed through the customer's domain

For instance, if you're setting the CNAME record and your domain is `campaign.example.com`, it will look like this:

```
CNAME campaign.example.com redir.safary.link
```

This indicates that `campaign.example.com` now points to `redir.safary.link`, and can be used in your Safary Campaigns.
