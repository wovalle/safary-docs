---
description: Safary script guarantees privacy and security in Web3.
---

# Install Safary script (hosted)

### **Table of Contents**

1. [How to integrate your website with Safary script (hosted version)](install-safary-script-hosted.md#how-to-integrate-your-website-with-safary-script-self-hosted-version)
   1. [(Optional) If your site has a Content Security Policy (CSP)](install-safary-script-hosted.md#optional-if-your-site-has-a-content-security-policy-csp)

***

### How to integrate your website with Safary script (hosted version)

To integrate your website with Safary, you need to update the front-end (HTML code) of the website you want to track.&#x20;

1.  First, once you have signed up, go to your Safary's home page ([https://app.safary.club/](https://app.safary.club/)). Under "**Integrate Safary snippet"** if you click on Hosted Tag, you should see something like:

    <figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Example of Safary's home page with the tracking script code</p></figcaption></figure>
2. Now click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left of the code to copy the contents of the tracking script
3. Go to your front-end's HTML code, and simply paste (CTRL+V or CMD+V) the script's code within the `<head> … </head>` tags of the pages you want to have tracking enabled.
   * For example, we suggest adding at least to both your landing page and your "app" page, which would have a "connect wallet" functionality.

That's it.  Your Safary tracking script code will look something like this in your page:

{% code overflow="wrap" %}
```html
<script>var script=document.createElement('script');script.src="https://tag.safary.club/stag-X.X.X.js";script.async=true;script.setAttribute('data-name','safary-sdk');script.setAttribute('data-product-id','prd_A123456789');script.integrity="sha256-XXXXXXXXXXXXXXXX";script.crossOrigin="anonymous";var target=document.head||document.body;target.appendChild(script);</script>
```
{% endcode %}

***

### **OPTIONAL - If your site has a Content Security Policy (CSP)**

If your site has Content Security Policy (CSP) enabled, you need to add the hash of the code above.

1. In Safary's home page ([https://app.safary.club/](https://app.safary.club/)),  you will see under "**Integrate Safary snippet"**, a small section with the title:
   * "If you have a Content Security Policy (CSP) in your service - optional"
2.  Click on that title to expand it, and you will see something like:

    <figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Example of Safary's home page with the content security policy (CSP) for the tracking script</p></figcaption></figure>
3. Now  you can click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left to copy the contents of the policy.
4. Finally, you need to add the directives in the place you implement CSP, which can vary.
   1. For example, if you use Node.js with the Helmet package in your backend to setup CSP, the code above is exactly the one to be pasted in your backend.
   2. On the other hand, if your CSP is setup using a \<meta> tag in the front-end to include the policy, you can still use the hash in the \<meta> tag. For example, using the above:
      * {% code overflow="wrap" %}
        ```
        <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'tag.safary.club' connect-src 'self' 'tag.safary.club'">
        ```
        {% endcode %}
