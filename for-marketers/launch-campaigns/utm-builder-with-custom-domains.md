# UTM builder with custom domains

{% hint style="info" %}
This feature is enabled for PRO customers
{% endhint %}

{% hint style="warning" %}
This feature is in alpha
{% endhint %}

Safary supports adding custom domains to your Safary campaigns. By default, when you create a campaign in Safary, we provide a link with the `safary.link/campaign_id` domain. PRO customers have the ability to use their own custom domain as the campaign link, so instead of sharing your `safary.link/campaign_id` campaign links, they can use their own domains, such as `example.com/campaign_id`.

In order for you to be able to configure custom domains, you need to add a `CNAME` DNS Record in your domain registrar or nameserver. This DNS Record will forward the requests made to the domain you designated for campaigns to the Safary servers so we can reroute them. The DNS Record must have the following details:

Safary cannot provide details for all of the domain registrars (usually ), but the process should be the same in all of them: log in to your domain registrar, go to DNS Administration, click create DNS Record, and add a record with the following details:

> Type: CNAME
>
> Target: redir.safary.link

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Example of how to configure a DNS Record in cloudflare.</p></figcaption></figure>



Once the DNS Record has been configured, contact Safary's support so we can enable this feature for your project.
