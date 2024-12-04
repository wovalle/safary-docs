---
description: >-
  In this page we show how you can integrate Safary's script with the option of
  disabling the location tracking.
---

# Disable Location Tracking

## Hosted Tag

If you want to use our hosted script, please follow the steps below with special care in the fourth step:

1.  First, once you have signed up, go to your Safary's home page ([https://app.safary.club/](https://app.safary.club/)).  Under "**Integrate Safary snippet"**, if you click on "Hosted tag" you should see something like:

    <figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
2. Now click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left of the code to copy the contents of the tracking script.
3. Go to your front-end's HTML code, and simply paste (CTRL+V or CMD+V) the script's code within the `<head> … </head>` tags of the pages you want to have tracking enabled.&#x20;
   * When pasting the code, you should see something like:

{% code overflow="wrap" %}
```html
<script>var script=document.createElement('script');script.src="https://tag.safary.club/stag-X.X.X.js";script.async=true;script.setAttribute('data-name','safary-sdk');script.setAttribute('data-product-id','prd_PiKqKcREQM');script.integrity="sha256-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";script.crossOrigin="anonymous";var target=document.head||document.body;target.appendChild(script);</script>
```
{% endcode %}

4. Now to <mark style="color:red;">**disable location tracking**</mark>, add a parameter to specify that behavior:
   * Add `script.setAttribute('data-location', 'disabled');` right after the part containing `script=document.createElement('script');`
   * It will look like this after adding it:

{% code overflow="wrap" %}
```markup
<script>var script=document.createElement('script');script.setAttribute('data-location','disabled');script.src="https://tag.safary.club/stag-X.X.X.js";script.async=true;script.setAttribute('data-name','safary-sdk');script.setAttribute('data-product-id','prd_PiKqKcREQM');script.integrity="sha256-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";script.crossOrigin="anonymous";var target=document.head||document.body;target.appendChild(script);</script>
```
{% endcode %}

5. That's it.  You can save the code in your HTML and rest assured that location data will not be tracked.

***

## Self-Hosted Tag

If you want to use self-hosted script, please follow the steps below with special care in the fourth step:

1.  First, once you have signed up, go to your Safary's home page ([https://app.safary.club/](https://app.safary.club/)). Under "**Integrate Safary snippet"**, if you click on "Selfhosted tag" you should see something like:

    <figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
2. Now click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left of the code to copy the contents of the tracking script.
3. Go to your front-end's HTML code, and simply paste (CTRL+V or CMD+V) the script's code within the `<head> … </head>` tags of the pages you want to have tracking enabled.&#x20;
   * When pasting the code, you should see something like:

{% code overflow="wrap" %}
```html
<script product="prd_1234567890" referrerpolicy="origin" data-name="safary-sdk" async>(()=>{"use strict";function e(e){return e&&"undefined"!=typeof Symbol&&e.constructor===Symbol?"symbol":typeof e}function t(t,n,r,a){if((n||void
...
</script>
```
{% endcode %}

4. Now to <mark style="color:red;">**disable location tracking**</mark>, add a parameter to specify that behavior:
   * Add `data-location="disabled"` right after the part containing `product="prd_"`
   * It will look like this after adding it:

{% code overflow="wrap" %}
```markup
<script product="prd_1234567890" data-location="disabled" referrerpolicy="origin" data-name="safary-sdk" async>(()=>{"use strict";function e(e){return e&&"undefined"!=typeof Symbol&&e.constructor===Symbol?"symbol":typeof e}function t(t,n,r,a){if((n||void
...
</script>
```
{% endcode %}

5. That's it.  You can save the code in your HTML and rest assured that location data will not be tracked.
