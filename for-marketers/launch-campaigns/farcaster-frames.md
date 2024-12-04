---
description: Generate custom campaign links for a Farcaster frames flow
---

# Farcaster Frames

This page explains how to create a campaign for Farcaster Frames and the modifications you have to apply to your frames to make sure Safary collects the visitors' interactions with your frames.

***

## 1) Create a new campaign for Farcaster Frames on Safary

Click on the "Launch campaigns" menu, then under "Farcaster Frames" click on "+ New Campaign".

You will be asked to insert:

* **Campaign name:** Add a meaningful name that the campaign will be assigned to in Safary's dashboards.
* **First frame URL:** Enter the full URL of your first frame. This is the link you typically paste into some client like Warpcast, and it gives the first frame that a user will see.

After filling those two fields and clicking "Next →", you will receive a link similar to:

* [https://safary.link/aEdB5sS9LSO/MyCampaignName?frameUrl=](https://safary.link/gDtA2jH6OLOO/Test?frameUrl=)

In the section below we explain how to use this link in your frames.

***

## 2) Frames setup

### 2.1) Add meaningful titles to the frames

We will use the safary.link generated in the previous step to make sure Safary collects the interactions happening on your frames.

However, before that, to make sure your data is easy to understand, your frames need to have meaningful titles.

*   Frames have the meta property `og:title` defines the title of each individual frame. For example:

    ```python
    <meta property="og:title" content="My Frames - Welcome"/>
    ```

    If you're using [frog.fm](https://frog.fm/) to build frames, [see here](https://frog.fm/reference/frog-frame-response#title) how to add a title.

{% hint style="danger" %}
Make sure to add a different title to each frame you want to consider different!
{% endhint %}

{% hint style="info" %}
We consider a frame to be unique if they have the same title and image url.
{% endhint %}

* Therefore, if you want to see a specific frame's interactions separate from other frames, make sure it has a unique title and image url.
* Similarly, if you want two different frames to be seen as the same, add the same title and image url to both of them.

***

### 2.2) Add the safary.link of your campaign to target and post urls

To allow Safary to collect interactions within your frame, every URL and/or path that is added to a target or post\_url property needs to start with the safary.link provided in the campaign creation.

For example, consider you followed the first step in this page to create a campaign and received the following link as a result:

* Example: [https://safary.link/aEdB5sS9/CmpgnName?frameUrl=](https://safary.link/gDtA2jH6OLOO/Test?frameUrl=)

If your **frame** has a `post_url`property, you should add the safary.link above before the URL in the property. For example

*   Before adding safary.link:&#x20;

    <pre class="language-html" data-overflow="wrap"><code class="lang-html"><strong>&#x3C;meta property="fc:frame:post_url" 
    </strong><strong>content="https://my.frame/next" />
    </strong></code></pre>
*   After adding safary.link:&#x20;

    <pre class="language-html" data-overflow="wrap"><code class="lang-html"><strong>&#x3C;meta property="fc:frame:post_url" 
    </strong><strong>content="<a data-footnote-ref href="#user-content-fn-1">https://safary.link/aEdB5sS9/CmpgnName?frameUrl=</a>https://my.frame/next" />
    </strong></code></pre>

For **button** actions with `target` or `post_url`, the approach is the same. For example:

*   Before adding safary.link:&#x20;

    {% code overflow="wrap" %}
    ```html
    <meta property="fc:frame:button:1" content="Next →" />
    <meta property="fc:frame:button:1:action" content="post" />
    <meta property="fc:frame:button:1:target" 
    content="https://my.frame/next" />
    ```
    {% endcode %}
*   After adding safary.link:&#x20;

    <pre class="language-html" data-overflow="wrap"><code class="lang-html">&#x3C;meta property="fc:frame:button:1" content="Next →" />
    &#x3C;meta property="fc:frame:button:1:action" content="post" />
    &#x3C;meta property="fc:frame:button:1:target" 
    content="<a data-footnote-ref href="#user-content-fn-2">https://safary.link/aEdB5sS9/CmpgnName?frameUrl=</a>https://my.frame/next" />
    </code></pre>

{% hint style="success" %}
Add the safary.link to all `target` and `post_url` of buttons with actions of any type.
{% endhint %}

### **What to do if the frame has a transaction?**

If the button action type is `tx` (transaction), you can still add a safary.link, and we will collect only the wallet address.

{% hint style="info" %}
Add a `post_url` in the frame with a transaction (and also add a safary.link to it). \
This property defines the next frame to go after the transaction.\
Adding it and including a safary.link makes sure we collect the **transaction confirmation**.
{% endhint %}

### What if the button action type is a link?

Please also add the safary.link before that. For example:

*   Before adding safary.link:&#x20;

    {% code overflow="wrap" %}
    ```html
    <meta property="fc:frame:button:1" content="Go to web" />
    <meta property="fc:frame:button:1:action" content="link" />
    <meta property="fc:frame:button:1:target" 
    content="https://external.site/" />
    ```
    {% endcode %}
*   After adding safary.link:&#x20;

    <pre class="language-html" data-overflow="wrap"><code class="lang-html">&#x3C;meta property="fc:frame:button:1" content="Go to web" />
    &#x3C;meta property="fc:frame:button:1:action" content="link" />
    &#x3C;meta property="fc:frame:button:1:target" 
    content="<a data-footnote-ref href="#user-content-fn-3">https://safary.link/aEdB5sS9/CmpgnName?frameUrl=</a>https://external.site" />
    </code></pre>

In this case we collect the click action.

### What if the frame shows an error to the user?

If the action in a frame does not return a successful next frame (or next action) for any reason (such as timeout or server error), we collect the error and show in our analytics.

This way you will know if users are having issues and where exactly that is happening.



[^1]: This was added!

[^2]: This was added!

[^3]: This was added!
