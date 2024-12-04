---
description: Safary script guarantees privacy and security in Web3.
---

# Install Safary script (self hosted)

### **Table of Contents**

1. [How to integrate your website with Safary script (self-hosted version)](install-safary-script-self-hosted.md#how-to-integrate-your-website-with-safary-script-self-hosted-version)
   1. [(Optional) If your site has a Content Security Policy (CSP)](install-safary-script-self-hosted.md#optional-if-your-site-has-a-content-security-policy-csp)
2. [Latest security updates (05/01/2024)](install-safary-script-self-hosted.md#latest-security-updates-05-01-2024)
3. [Source code of Safary script](install-safary-script-self-hosted.md#source-code-of-safary-script)

***

### How to integrate your website with Safary script (self-hosted version)

To integrate your website with Safary, you need to update the front-end (HTML code) of the website you want to track.&#x20;

To prioritize your site's security, Safary suggests adding our script's code in your site (i.e. self hosting it) instead of pointing to another domain to serve the code - this way you will **not** need to allow cross-domain scripts in your page.

1.  First, once you have signed up, go to your Safary's home page ([https://app.safary.club/](https://app.safary.club/)) and you will see under "**Integrate Safary snippet"** some code starting with `<script product="prd_`

    * For example, you should see something like:

    <figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Example of Safary's home page with the tracking script code</p></figcaption></figure>
2. Now click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left of the code to copy the contents of the tracking script
3. Go to your front-end's HTML code, and simply paste (CTRL+V or CMD+V) the script's code within the `<head> … </head>` tags of the pages you want to have tracking enabled.
   * For example, we suggest adding at least to both your landing page and your "app" page, which would have a "connect wallet" functionality.

That's it.  Your Safary tracking script code will look something like this in your page:

{% code overflow="wrap" %}
```html
<script product="prd_1234567890" referrerpolicy="origin" data-name="safary-sdk" async>(()=>{"use strict";function e(e){return e&&"undefined"!=typeof Symbol&&e.constructor===Symbol?"symbol":typeof e}function t(t,n,r,a){if((n||void
...
</script>
```
{% endcode %}

***

### **OPTIONAL - If your site has a Content Security Policy (CSP)**

If your site has Content Security Policy (CSP) enabled, you need to add the hash of the code above.

1. In Safary's home page ([https://app.safary.club/](https://app.safary.club/)),  you will see under "**Integrate Safary snippet"**, a small section with the title:
   * "If you have a Content Security Policy (CSP) in your service - optional"
2.  Click on that title to expand it, and you will see something like:

    <figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Example of Safary's home page with the content security policy (CSP) for the tracking script</p></figcaption></figure>
3. Now  you can click on the copy icon ![](<../../.gitbook/assets/image (5).png>) on the left to copy the contents of the policy.
4. Finally, you need to add the directives in the place you implement CSP, which can vary.
   1. For example, if you use Node.js with the Helmet package in your backend to setup CSP, the code above is exactly the one to be pasted in your backend.
   2. On the other hand, if your CSP is setup using a \<meta> tag in the front-end to include the policy, you can still use the hash in the \<meta> tag. For example, using the hash above:
      * {% code overflow="wrap" %}
        ```
        <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'sha256-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'" connect-src 'self' 'tag.safary.club'">
        ```
        {% endcode %}

***

## **Latest security updates (05/01/2024)**

Safary prioritizes security within the tracking script's infrastructure and has been implementing a number of security features. Below we give a few examples of what we have added (and we will continue to reinforce our security and add more features as we move forward).

* Our tag.safary.club domain has DNSSEC enabled with an authentication chain of trust and digitally signed DNS records.
* We enforce HSTS (HTTP Strict Transport Security) to protect visitors by ensuring that their browsers always connect to our domain over HTTPS.
* Our web server automatically redirects visitors from HTTP to HTTPS on the same domain.
* Our servers supports only secure TLS versions and also only the most up-to-date secure ciphers with enforcement of cipher order.
* We include security headers in order to activate browser mechanisms to protect visitors against attacks involving, for example, cross-site scripting (XSS) or framing.
* Our server is behind a firewall that explicitly blocks any path, body size, address or input that is different from the expected. The firewall also enforces important rules managed by AWS.
* Our web server supports secure parameters for Diffie-Hellman key exchange and a secure hash function for key exchange. Moreover, we do not allow for client-initiated renegotiation.
* We do not support HTTP nor TLS compression.
* The trust chain of our website's certificate is complete and signed by a trusted root certificate authority.
* All IP addresses of our web server have a route announcement that is matched by the published route authorisation (RPKI), which protects against various unintentional or malicious route configuration errors.
* Our script sanitizes every string used in the front-end, avoiding code injection and related attacks.

***

### Source code of Safary script

With transparency, security and privacy in mind, the complete code of our Safary script will be made public, as our script will be open sourced in the next coming weeks.

In the meantime, below we share the source code of our Safary script in a single Typescript file, currently in version 0.1.13.

* Click the arrow ( > ) in the box to expand the code:

<details>

<summary>Safary Session Manager - Typescript code in a single file</summary>

````
type InternalTrackingObject = {
    'trk-type': string;
    'trk-name': string;
    'trk-param': Record<string, unknown> | undefined;
};

type TrackParameters = {
    eventType: string;
    eventName: string;
    parameters?: Record<string, unknown>;
};

type walletObject = {
    address: string;
    event: string;
    type: string | null;
    chainId: string | number | null;
};

type InfoObject = {
    si: string;
    pls: string;
    sd: Record<string, string>;
    plsd: Record<string, string>;
    u: string;
    r: string;
    tag?: string;
    fulltag?: Record<string, string>;
    wa?: Array<walletObject>;
    evt: string;
    'evt-trk'?: InternalTrackingObject | null;
    v: string;
    time: string;
};

export function assertType<T>(
    value: unknown,
    required: boolean,
    type: 'string' | 'number' | 'boolean' | 'object' | 'undefined' | 'symbol',
    varName?: string,
): asserts value is T {
    if ((required || typeof value !== 'undefined') && typeof value !== type) {
        throw new Error(
            `Assertion failed: Expected ${
                varName ?? 'value'
            } to be of type ${type} but received ${typeof value}`
        );
    }
}

export function assertUndefined<T>(
    value: unknown,
    varName?: string,
): asserts value is T {
    if (typeof value === 'undefined') {
        throw new Error(
            `Assertion failed: Expected ${
                varName ?? 'value'
            } to exist but received undefined`
        );
    }
}

export class SafaryManager {
    private readonly localItemName = '____sfry_anonymous'; // local item name
    private sessionId: string = 'none'; // session id
    private sessionData: Record<string, string> = {
        sessionId: 'none',
    };
    private previousLocalStorageData: Record<string, string> = {
        sessionId: 'n',
    };

    private currentWallets: Array<walletObject> = new Array<walletObject>();
    private currentListenerWallets: Array<walletObject> = new Array<walletObject>();

    private SAFARY_BACKEND_ORIGIN =
        process.env.SAFARY_BACKEND_ORIGIN ?? 'https://tag.safary.club';

    private SAFARY_SCRIPT_VERSION =
        process.env.npm_package_version ?? 'v0.0.0-dev';

    private SAFARY_TAG;
    private PRODUCT_ID;

    private timeScriptLoaded = new Date().toISOString();

    private pooling_active = false;

    constructor(currentStagScript: HTMLOrSVGScriptElement | null = null) {
        this.SAFARY_TAG = currentStagScript;
        this.PRODUCT_ID = this.getProductID();
    }

    public async setup() {
        try {
            await this.setupSession();
            this.setupEthereumListeners();
            this.setupPhantomSolanaListeners();
            this.setupAptosAndAtomListeners();
            this.setupWalletConnect();
            this.setupOKXWalletListeners();
            
            // Wait for page load to see if all extensions have been loaded
            window.onload = function() {
                const lsEvent = new CustomEvent('fullPageFinishedLoadingSafary');
                window.dispatchEvent(lsEvent);
            };

            // Listen to the event
            window.addEventListener(
                'fullPageFinishedLoadingSafary',
                async () => {
                    this.setupStarknetListeners();
                    this.setupBitgetWalletListeners();
                    this.setupUniSatWalletListeners();
                }
            );
            
            // Attach tracking functions to window.safary
            this.setupTrackingFunctions();
            
        } catch (e) {
            console.error('Error during Safary tag setup.');
        }
    }

    public getProductID() {
        let product_id_tag = this.SAFARY_TAG;

        if (!product_id_tag) {
            product_id_tag = window.document.querySelector(
                'script[data-name="safary-sdk"]'
            );
        }

        if (product_id_tag) {
            let product_id = product_id_tag?.getAttribute('data-product-id');

            if (!product_id) {
                const script_src = product_id_tag?.getAttribute('src');
                if (script_src) {
                    const script_src_url = new URL(script_src);
                    product_id = script_src_url.searchParams.get('id');
                }
            }
            if (product_id && SafaryManager.isValidProductID(product_id)) {
                return product_id;
            }
        }
        console.error(
            "ERROR: No valid product ID was found. Please contact Safary's support."
        );
        return;
        
    }

    public getSessionID() {
        return this.sessionId;
    }

    public getSessionData() {
        return this.sessionData;
    }

    public static isValidProductID(productID: string) {
        // it must be a string
        if (typeof productID !== 'string') {
            console.error(
                'ERROR: safary.isValidProductID(): the product ID must be a string.'
            );
            return false;
        }
        // it must have 14 characters
        if (productID.length !== 14) {
            console.error(
                'ERROR: safary.isValidProductID(): the product ID must have 14 characters.'
            );
            return false;
        }
        // it must start with 'prd_'
        if (!productID.startsWith('prd_')) {
            console.error(
                'ERROR: safary.isValidProductID(): the product ID must start with "prd_".'
            );
            return false;
        }
        // from character 4 to 14, it must be alphanumeric
        const regex = /^[a-z0-9]+$/i;
        const result = regex.test(productID.substring(4));
        if (!result) {
            console.error(
                'ERROR: safary.isValidProductID(): the product ID must be alphanumeric.'
            );
            return false;
        }
        return true;
    }

    public static isValidSessionID(sessionID: string) {
        // it must be a string
        if (typeof sessionID !== 'string') {
            console.error(
                'ERROR: safary.isValidSessionID(): the session ID must be a string.'
            );
            return false;
        }
        // it must have 36 characters
        if (sessionID.length !== 36) {
            console.error(
                'ERROR: safary.isValidSessionID(): the session ID must have 36 characters.'
            );
            return false;
        }
        // it must be a valid UUID
        const regex =
            /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i;
        const result = regex.test(sessionID);
        if (!result) {
            console.error(
                'ERROR: safary.isValidSessionID(): the session ID must be a valid UUID.'
            );
            return false;
        }
        return true;
    }

    /**
     * Sets up the session.
     *
     * @remarks
     * This method should only be used internally.
     *
     * @returns A promise that resolves once the session is set up.
     */
    private async setupSession() {
        // Reads session info from Local Storage
        const sls = window.localStorage.getItem(this.localItemName);

        if (typeof sls !== 'undefined' && sls !== null && sls.length > 0) {
            this.previousLocalStorageData = sls.startsWith('{')
                ? JSON.parse(sls)
                : { sessionId: sls };

            const localSessionID = this.previousLocalStorageData['sessionId'];

            // If the session ID is valid, use it
            if (SafaryManager.isValidSessionID(localSessionID)) {
                this.sessionId = localSessionID;
                this.sessionData = { sessionId: this.sessionId };
                // Does not need to update Local Storage
            }
        }

        // If no valid session ID was found, get a new one
        if (this.sessionId === 'none') {
            await this.getNewSession();
            // Update Local Storage
            window.localStorage.setItem(
                this.localItemName,
                JSON.stringify(this.sessionData)
            );
        }
    }

    /**
     * Retrieves a new session from the server.
     * This method should only be used internally.
     * @returns {Promise<void>} A promise that resolves when the session is retrieved successfully.
     */
    public async getNewSession() {
        await fetch(
            `${this.SAFARY_BACKEND_ORIGIN}/session.json?id=${this.PRODUCT_ID}`
        )
            // Make sure we treat it as json, to avoid potential problems!
            .then((response) => {
                // check to see if there is json
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then((session) => {
                if (
                    !SafaryManager.isValidSessionID(session.sessionId as string)
                ) {
                    throw new Error('Invalid session id');
                } else {
                    this.sessionData = session;
                    this.sessionId = session.sessionId as string;
                }
            });
    }

    public async visit() {
        try {
            // Process Wallet and Send Data
            await this.processWallet();
            const infoObject = this.getInfoObject('vt'); // Script Event type ('vt' = visit, 'wl' = wallet)
            await this.sendVisitData(infoObject);
        } catch (error) {
            console.error('Error during Safary tag execution.');
        }
    }

    private setupTrackingFunctions() {
        // Create safary in window
        if (typeof window.safary === 'undefined') {
            window.safary = window.safary || {};
            window.safary.track = this.track.bind(this);
            window.safary.trackSwap = this.trackSwaps.bind(this);
            window.safary.trackDeposit = this.trackDeposit.bind(this);
            window.safary.trackWithdrawal = this.trackWithdrawal.bind(this);
            window.safary.trackNFTPurchase = this.trackNFTPurchase.bind(this);
            window.safary.products = [this.PRODUCT_ID];
        } else {
            if (!window.safary.products.includes(this.PRODUCT_ID)) {
                window.safary.products.push(this.PRODUCT_ID);
            }
        }
    }

    public async track(params: TrackParameters) {
        try {
            assertType(params, true, 'object', 'params');
            assertUndefined(params.eventType, 'eventType');
            assertUndefined(params.eventName, 'eventName');
            assertType(params.parameters, false, 'object', 'parameters');
        } catch (e) {
            console.error(
                'ERROR: safary.track(): there were some validation errors.'
            );
            return;
        }

        let infoObject = this.getInfoObject('trk', {
            'trk-type': params.eventType,
            'trk-name': params.eventName,
            'trk-param': params.parameters,
        });

        for(let product_id of window.safary.products){
            if(SafaryManager.isValidProductID(product_id)){
                infoObject['tag'] = product_id;
                infoObject['fulltag'] = undefined;
                await this.sendVisitData(infoObject);
            }
        }
    }

    // Get Info
    private getInfoObject(
        eventType: 'vt' | 'wl' | 'trk' = 'vt',
        trackingObject?: InternalTrackingObject
    ): InfoObject {
        const validUrl = this.isEmptyOrValidStart(window.location.href, false);
        const validReferrer = this.isEmptyOrValidStart(window.document.referrer, true);
        
        const tagAttr: Record<string, string> = {};
        if (this.SAFARY_TAG) {
            Array.from(this.SAFARY_TAG.attributes).forEach(attr => {
                tagAttr[attr.name] = attr.value;
            });
        }

        if(validUrl && validReferrer){
            return {
                si: this.sessionId,
                pls: this.previousLocalStorageData['sessionId'],
                sd: this.sessionData,
                plsd: this.previousLocalStorageData,
                u: window.location.href,
                r: window.document.referrer,
                tag: this.PRODUCT_ID,
                fulltag: Object.keys(tagAttr).length > 0 ? tagAttr : undefined,
                wa:
                    this.currentWallets.length > 0
                        ? this.currentWallets
                        : undefined,
                evt: eventType, // Script Event type ('vt' = visit, 'wl' = wallet, 'trk' = tracking)
                'evt-trk': trackingObject ?? null,
                v: this.SAFARY_SCRIPT_VERSION,
                time:
                    eventType == 'vt'
                        ? this.timeScriptLoaded
                        : new Date().toISOString(),
            };
        }
        else {
            console.error('ERROR in Safary tag: Invalid URL or Referrer.');
            throw new Error(`Invalid URL or Referrer. URL: ${window.location.href} Referrer: ${window.document.referrer}`);
        }
    }

    // Validate URLs
    public isEmptyOrValidStart(s: string, canBeEmpty: boolean = false) {
        if (typeof s === 'undefined' || s === null) {
            return canBeEmpty;
        }
        if(typeof s !== 'string'){
            return false;
        }
        if(canBeEmpty && s.length === 0){
            return true;
        }
        return /^[A-Za-z0-9]/.test(s);
    }

    // Send the data to Safary's backend for processing
    private async sendVisitData(infoObject: InfoObject) {
        if (typeof infoObject.tag === 'undefined') {
            console.error(
                "ERROR: No valid product ID was found. Please contact Safary's support."
            );
            return;
        }
        const backend_url = `${this.SAFARY_BACKEND_ORIGIN}/sfry/?id=${infoObject.tag}`;
        return fetch(backend_url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
            },
            body: JSON.stringify({ v: infoObject }),
        });
    }

    // Set wallet type
    private setWalletType(provider: any) {
        const mapping = {
            isBraveWallet: 'Brave',
            isBitKeep: 'Bitget',
            isPhantom: 'Phantom',
            isCoinbaseWallet: 'Coinbase',
            isMetaMask: 'MetaMask',
        };

        // here only because typescript doesn't allow typing for in loops
        let key: keyof typeof mapping;

        for (key in mapping) {
            if (provider[key]) {
                return mapping[key];
            }
        }

        return 'Unknown';
    }

    // Process Ethereum Provider
    private async addWalletFromProvider(provider: any) {
        try {
            if (
                provider &&
                provider?.selectedAddress
            ) {
                const provider_chainId = provider?.chainId ?? await provider?.request({
                    method: 'eth_chainId',
                }); 
                return [
                    provider?.selectedAddress,
                    this.setWalletType(provider),
                    provider_chainId,
                    'eth_sfry_all',
                ];
            }
        } catch (e) {
            console.error('Error in Safary tag when adding Wallet From Provider.');
        }
        return [];
    }

    private getEthereumProvider() {
        if (typeof window.phantom === 'undefined') {
            return window.ethereum;
        } else {
            if (
                typeof window.ethereum !== 'undefined' &&
                typeof window.ethereum.detected !== 'undefined' &&
                window.ethereum.detected.length > 0
            ) {
                return window.ethereum.detected[0];
            } else {
                return window.phantom.ethereum;
            }
        }
    }

    // Process Wallet + Wallet Type + Wallet Chain
    private async processWallet() {
        if (typeof window.ethereum !== 'undefined') {
            const provider = this.getEthereumProvider();
            if(provider){
                // Accounts Before: previously authorized wallets: Not a different 'evt', as the user is not effectively signing up
                const ac_bf: string[] = await provider.request({
                    method: 'eth_accounts',
                });

                if (typeof ac_bf !== 'undefined' && ac_bf.length > 0) {
                    const chainId = await provider.request({
                        method: 'eth_chainId',
                    });
                    ac_bf.forEach((element) => {
                        this.currentWallets.push({
                            address: element,
                            event: 'eth_accounts',
                            type: null,
                            chainId: chainId,
                        });
                    });
                }
            }

            // Ethereum
            const walletFromETH = await this.processEthereum();
            if (walletFromETH.length > 0) {
                walletFromETH.forEach((element) => {
                    if (element.length > 0) {
                        this.currentWallets.push({
                            address: element[0],
                            event: 'eth_accounts__eth',
                            type: element[1],
                            chainId: element[2],
                        });
                    }
                });
            }
        }

        // OKX Wallet - ALL
        if (typeof window.okxwallet !== 'undefined') {
            // Accounts Before: previously authorized wallets: Not a different 'evt', as the user is not effectively signing up
            const okxWallets: {chainId: string, address: string}[] = await this.getOKXwallets();

            if (typeof okxWallets !== 'undefined' && okxWallets.length > 0) {
                okxWallets.forEach((element) => {
                    this.currentWallets.push({
                        address: element.address,
                        event: 'accounts',
                        type: 'OKX Wallet',
                        chainId: element.chainId,
                    });
                });
            }
        }

        // BitKeep
        if (typeof window.bitkeep !== 'undefined'){
            const bitgetWallets: {address: string, network: string}[] = [];

            // Bitcoin via Unisat
            if(typeof window.bitkeep.unisat !== 'undefined') {      
                while(typeof window.bitkeep.unisat._initialized === 'undefined' || !window.bitkeep.unisat._initialized){
                    await new Promise(r => setTimeout(r, 5));
                }
                const btkpWallets: {address: string, network: string}[] = window.bitkeep.unisat.identities;
                bitgetWallets.push(...btkpWallets);
            }
            // Ton
            if(typeof window.bitkeep.ton !== 'undefined') {       
                while(typeof window.bitkeep.ton._initialized === 'undefined' || !window.bitkeep.ton._initialized){
                    await new Promise(r => setTimeout(r, 5));
                }
                const btkpWallets: {address: string, network: string}[] = window.bitkeep.ton.identities;
                const chainId = window.bitkeep.ton.chainId;
                btkpWallets.forEach((element) => {
                    bitgetWallets.push({address: element.address, network: `Ton ${chainId}`});
                });
            }
            // Solana
            if(typeof window.bitkeep.solana !== 'undefined') {
                while(typeof window.bitkeep.solana._initialized === 'undefined' || !window.bitkeep.solana._initialized){
                    await new Promise(r => setTimeout(r, 5));
                }
                const solAddress = window.bitkeep.solana.publicKey?.toString();
                const solNetwork = window.bitkeep.solana.network;
                if(typeof solAddress !== 'undefined' && solAddress.trim() !== ''){
                    bitgetWallets.push({address: solAddress, network: `Solana ${solNetwork}`});
                }
            }
            // Aptos
            if(typeof window.bitkeep.aptos !== 'undefined') {
                while(typeof window.bitkeep.aptos._initialized === 'undefined' || !window.bitkeep.aptos._initialized){
                    await new Promise(r => setTimeout(r, 5));
                }
                const aptosWallets: {address: string, network: string} = window.bitkeep.aptos._state;
                if(typeof aptosWallets !== 'undefined' && aptosWallets.address.trim() !== ''){
                    bitgetWallets.push({address: aptosWallets.address, network: `Aptos ${aptosWallets.network}`});
                }
            }
            // Starknet
            if(typeof window.starknet_bitkeep !== 'undefined') {
                while(typeof window.starknet_bitkeep._initialized === 'undefined' || !window.starknet_bitkeep._initialized){
                    await new Promise(r => setTimeout(r, 5));
                }
                const starknetWallets: {address: string, chainId: string} = window.starknet_bitkeep._state;
                if(typeof starknetWallets !== 'undefined' && starknetWallets.address.trim() !== ''){   
                    const starknetChainDict: Record<string, string> = this.getStarknetchainDict();
                    const chainId = starknetChainDict[starknetWallets.chainId || '']

                    bitgetWallets.push({address: starknetWallets.address, network: chainId});
                }
            }

            if (typeof bitgetWallets !== 'undefined' && bitgetWallets.length > 0) {
                bitgetWallets.forEach((element) => {
                    this.currentWallets.push({
                        address: element.address,
                        event: 'accounts',
                        type: 'Bitget',
                        chainId: element.network,
                    });
                });
            }
        }

        // Unisat standalone for Bitcoin
        if (typeof window.unisat !== 'undefined' && 
            (typeof window.unisat.isBitKeep === 'undefined' || 
            !window.unisat.isBitKeep)) {
            // Accounts Before: previously authorized wallets: Not a different 'evt', as the user is not effectively signing up
            const unisatWallets: string[] = await window.unisat.getAccounts();
            const unisatNetwork = await window.unisat.getNetwork() ?? 'Unknown';

            if (typeof unisatWallets !== 'undefined' && unisatWallets.length > 0) {
                unisatWallets.forEach((element) => {
                    this.currentWallets.push({
                        address: element,
                        event: 'accounts',
                        type: 'UniSat',
                        chainId: unisatNetwork,
                    });
                });
            }
        }

        // Wallet Connect
        const walletFromWC = await this.handleWalletConnect({ key: 'SAFARY_VISIT', value: '' });
        if (walletFromWC.length > 0) {
            walletFromWC.forEach((element) => {
                if (element.address !== null) {
                    this.currentWallets.push(element);
                }
            });
        }
    }


    public setupOKXWalletListeners() {
        // Check if okxwallet is on the window object
        if (typeof window.okxwallet === 'undefined') {
            window.__defineSetter__('okxwallet', this.setupOKXWalletListeners);
            return;
        }

        // OKX Wallet
        window.okxwallet.on('connectWallet', async () => {
            const okxWallets: {chainId: string, address: string}[] = await this.getOKXwallets();
            // Add all wallets returned by event
            const wallets: Array<walletObject> = new Array<walletObject>();
            
            if (typeof okxWallets !== 'undefined' && okxWallets.length > 0) {
                okxWallets.forEach((element) => {
                    wallets.push({
                        address: element.address,
                        event: 'accountsChanged',
                        type: 'OKX Wallet',
                        chainId: element.chainId,
                    });
                });
            }
            
            // if wa is not empty, send data
            this.sendWalletData(wallets);
        });        
    }

    public setupBitgetWalletListeners() {
        // Check if bitkeep is on the window object
        if (typeof window.bitkeep === 'undefined') {
            window.__defineSetter__('bitkeep', this.setupBitgetWalletListeners);
            return;
        }        

        // Bitcoin via Unisat
        if(typeof window.bitkeep.unisat !== 'undefined') {
            // accountsChanged
            window.bitkeep.unisat.on('accountsChanged', async (accounts: string[]) => {
                const wallets: Array<walletObject> = new Array<walletObject>();
                const chainId = await window.bitkeep.unisat.network;
                accounts.forEach((element) => {
                    wallets.push({
                        address: element,
                        event: 'accountsChanged',
                        type: 'Bitget',
                        chainId: chainId,
                    });
                });
                this.sendWalletData(wallets);
            });

            // networkChanged
            window.bitkeep.unisat.on('networkChanged', async (network: string) => {
                const wallets: Array<walletObject> = new Array<walletObject>();
                wallets.push({
                    address: 'Unknown', // To avoid being removed during data processing
                    event: 'accountsChanged',
                    type: 'Bitget',
                    chainId: network,
                });
                this.sendWalletData(wallets);
            });

        }

        // Ton
        if(typeof window.bitkeep.ton !== 'undefined') {            
            // accountsChanged
            window.bitkeep.ton.on('accountsChanged', async (accounts: string[]) => {
                const wallets: Array<walletObject> = new Array<walletObject>();
                const chainId = window.bitkeep.ton.chainId;
                accounts.forEach((element) => {
                    wallets.push({
                        address: element,
                        event: 'accountsChanged',
                        type: 'Bitget',
                        chainId: `Ton ${chainId}`,
                    });
                });
                this.sendWalletData(wallets);
            });

            // chainChanged
            window.bitkeep.ton.on('chainChanged', async (chainId: string) => {
                const wallets: Array<walletObject> = new Array<walletObject>();
                wallets.push({
                    address: 'Unknown', // To avoid being removed during data processing
                    event: 'chainChanged',
                    type: 'Bitget',
                    chainId: `Ton ${chainId}`,
                });
                this.sendWalletData(wallets);
            });
        }

        // Solana
        if(typeof window.bitkeep.solana !== 'undefined') {
            // connect
            window.bitkeep.solana.on('connect', async (publicKey: string) => {
                const wallets: Array<walletObject> = new Array<walletObject>();
                const network = window.bitkeep.solana.network;
                wallets.push({
                    address: publicKey.toString(),
                    event: 'accountsChanged',
                    type: 'Bitget',
                    chainId: `Solana ${network}`,
                });
                this.sendWalletData(wallets);
            });
        }

        // Aptos
        if(typeof window.bitkeep.aptos !== 'undefined') {            
            // accountChanged
            window.bitkeep.aptos.onAccountChange(async (account: {address: string}) => {
                var wallets: Array<walletObject> = new Array<walletObject>();
                const network = await window.bitkeep.aptos.network();
                wallets.push({
                    address: account.address,
                    event: 'accountsChanged',
                    type: 'Bitget',
                    chainId: `Aptos ${network}`,
                });
                this.sendWalletData(wallets);
            });

            // networkChange
            window.bitkeep.aptos.onNetworkChange((network: string) => {
                var wallets: Array<walletObject> = new Array<walletObject>();
                wallets.push({
                    address: 'Unknown', // To avoid being removed during data processing
                    event: 'chainChanged',
                    type: 'Bitget',
                    chainId: `Aptos ${network}`,
                });
                this.sendWalletData(wallets);
            });            
        }

        // Starknet
        if(typeof window.starknet_bitkeep !== 'undefined') {
            const starknetChainDict: Record<string, string> = this.getStarknetchainDict();

            // accountsChanged
            window.starknet_bitkeep.on('accountsChanged', async (ac: string[]) => {
                var out_wallets: Array<walletObject> = new Array<walletObject>();
                const chainId = starknetChainDict[window.starknet_bitkeep.chainId || '']
                ac.forEach(async (element) => {
                    out_wallets.push({
                        address: element,
                        event: 'accountsChanged',
                        type: 'Bitget',
                        chainId: chainId,
                    });
                });
                this.sendWalletData(out_wallets);
            });

            // networkChanged
            window.starknet_argentX.on('networkChanged', async (ac: string[]) => {                
                var out_wallets: Array<walletObject> = new Array<walletObject>();
                ac.forEach(async (element) => {
                    out_wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'chainChanged',
                        type: 'Bitget',
                        chainId: starknetChainDict[element || ''],
                    });
                });
                this.sendWalletData(out_wallets);
            });
        }      
    }

    // Unisat standalone for Bitcoin
    public setupUniSatWalletListeners() {
        // Check if unisat is on the window object
        if (typeof window.unisat === 'undefined') {
            window.__defineSetter__('unisat', this.setupUniSatWalletListeners);
            return;
        }
        else {
            if(typeof window.unisat.isBitKeep === 'undefined' || !window.unisat.isBitKeep){
                // accountsChanged
                window.unisat.on('accountsChanged', async (accounts: string[]) => {
                    const wallets: Array<walletObject> = new Array<walletObject>();
                    const unisatNetwork = await window.unisat.getNetwork() ?? 'Unknown';
                    accounts.forEach((element) => {
                        wallets.push({
                            address: element,
                            event: 'accountsChanged',
                            type: 'UniSat',
                            chainId: unisatNetwork,
                        });
                    });
                    this.sendWalletData(wallets);
                });

                // networkChanged
                window.unisat.on('networkChanged', async (network: string) => {
                    const wallets: Array<walletObject> = new Array<walletObject>();
                    wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'accountsChanged',
                        type: 'UniSat',
                        chainId: network,
                    });
                    this.sendWalletData(wallets);
                });
            }
        }
    }

    public async getOKXwallets(){
        const defaultAddress = window.okxwallet.selectedAddress;
        const defaultChainId = window.okxwallet.chainId;
        let uniqueList = [];
        let seenAddresses = new Set();
        // if not null
        if(typeof defaultAddress !== 'undefined' && defaultAddress !== null && defaultAddress.trim() !== ''){
            uniqueList.push({
                chainId: defaultChainId,
                address: defaultAddress
            });
            seenAddresses.add(defaultAddress);
        }
        const allWallets = await window.okxwallet.requestWallets();
        if(allWallets.length === 0){
            return uniqueList;
        }
        const originalArray = allWallets[0].address;
        
        for (let element of originalArray) {
            if (element.address.trim() !== '' && !seenAddresses.has(element.address)) {
                uniqueList.push({
                    chainId: element.chainId,
                    address: element.address
                });
                seenAddresses.add(element.address);
            }
        }
        
        return uniqueList;
    }


    private async processEthereum() {
        const ethWallets = [];

        // Get wallet in the window.ethereum.providers
        if (typeof window.ethereum.providers !== 'undefined') {
            // For each value, key in providers
            window.ethereum.providers.forEach(
                async (value: { selectedAddress: any; chainId: any }) => {
                    ethWallets.push(await this.addWalletFromProvider(value));
                }
            );
        }
        // This is set by Coinbase, if we go to window.ethereum in this case, we get MetaMask as wallet type
        if (typeof window.ethereum.selectedProvider !== 'undefined') {
            ethWallets.push(
                await this.addWalletFromProvider(
                    window.ethereum.selectedProvider
                )
            );
        } else {
            ethWallets.push(
                await this.addWalletFromProvider(this.getEthereumProvider())
            );
        }

        return ethWallets;
    }

    public setupEthereumListeners() {
        // Check if ethereum is on the window object
        if (typeof window.ethereum === 'undefined') {
            window.__defineSetter__('ethereum', this.setupEthereumListeners);
            return;
        }
        // Listen for accounts changed
        window.ethereum.on('accountsChanged', async (ac: string[]) => {
            // Add all wallets returned by event to eth_wallets
            var eth_wallets: Array<walletObject> = new Array<walletObject>();
            if (!Array.isArray(ac)) {
                ac = [ac];
            }
            const chainId = await window.ethereum.request({
                method: 'eth_chainId',
            })
            ac.forEach(async (element) => {
                eth_wallets.push({
                    address: element,
                    event: 'accountsChanged',
                    type: null,
                    chainId: chainId,
                });
            });

            // Ethereum: To get type and chain
            const walletFromETH = await this.processEthereum();
            walletFromETH.forEach((element) => {
                if (element[0] !== null && element[0] !== undefined) {
                    eth_wallets.push({
                        address: element[0],
                        event: 'eth_accounts__eth_listener',
                        type: element[1],
                        chainId: element[2],
                    });
                }
            });
            // if wa is not empty, send data
            this.sendWalletData(eth_wallets);
        });
        // Listen for chain changed
        window.ethereum.on('chainChanged', async (ac: string[]) => {
            // Add all wallets returned by event to eth_wallets
            var eth_wallets: Array<walletObject> = new Array<walletObject>();
            if (!Array.isArray(ac)) {
                ac = [ac];
            }
            ac.forEach(async (element) => {
                eth_wallets.push({
                    address: 'Unknown', // To avoid being removed during data processing
                    event: 'chainChanged',
                    type: null,
                    chainId: element,
                });
            });
            // if wa is not empty, send data
            this.sendWalletData(eth_wallets);
        });
    }


    public setupPhantomSolanaListeners() {
        // Check if ethereum is on the window object
        if (typeof window.phantom === 'undefined') {
            window.__defineSetter__('phantom', this.setupPhantomSolanaListeners);
            return;
        }

        // Phantom - SOLANA
        window.phantom.solana.on('connect', async (body: any) => {
            // Add all wallets returned by event to eth_wallets
            var sol_wallets: Array<walletObject> = new Array<walletObject>();
            // Add one wallet
            sol_wallets.push({
                address: body.toString(),
                event: 'accountsChanged',
                type: 'Phantom',
                chainId: 'Solana',
            });
            
            // if wa is not empty, send data
            this.sendWalletData(sol_wallets);
        });        
    }

    private is_atomscan(wallet_or_chain: string) {
        var chn = ['agoric', 'aioz', 'akash', 'archway', 'mantle', 'axelar', 'band', 
        'bze', 'bcna', 'bitsong', 'bostrom', 'canto', 'swth', 'c4e', 'cheqd',
        'chihuahua', 'comdex', 'cosmos', 'cre', 'cro', 'cudos', 'decentr', 
        'desmos', 'emoney', 'echelon', 'ethos', 'evmos', 'fetch', 'firma',
        'genesis', 'gravity', 'idep', 'inj', 'iaa', 'ixo', 'juno', 'kava', 
        'ki', 'darc', 'kujira', 'lamb', 'like', 'lum', 'lumen', 'panacea', 
        'meme', 'migaloo', 'omniflix', 'orai', 'osmo', 'pasg', 'persistence', 
        'plq', 'point', 'pb', 'rebus', 'regen', 'rizon', 'secret', 'sent',
        'shentu', 'sif', 'somm', 'stafi', 'stars', 'star', 'stride', 'tori', 
        'terra', 'umee', 'und', 'vdl']

        if (!wallet_or_chain) {
            return false;
        }

        // Check if wallet starts with any of the values in chn        
        return chn.some(c => wallet_or_chain.startsWith(c));
    }

    public setupAptosAndAtomListeners() {
        window.addEventListener("message", (event: any) => {
            // Petra
            if(event?.data?.type === 'PetraApiResponse'){
                var atom_wallets: Array<walletObject> = new Array<walletObject>();

                if(event?.data?.result?.chainId){
                    // chainChanged
                    atom_wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'chainChanged',
                        type: 'Petra',
                        chainId: `Aptos ${event?.data?.result?.name}`,
                    });
                }
                else if(event?.data?.result?.address){
                    // new connection
                    atom_wallets.push({
                        address: event?.data?.result?.address,
                        event: 'accountsChanged',
                        type: 'Petra',
                        chainId: null,
                    });
                }
                // if wa is not empty, send data
                this.sendWalletData(atom_wallets);
            }

            // Keplr - chainId
            else if(event?.data?.type === 'proxy-request' && event?.data?.method === 'getKey'){
                var chainId = event?.data?.args[0]
                if(this.is_atomscan(chainId)){
                    var atom_wallets: Array<walletObject> = new Array<walletObject>();
                    // chainChanged
                    atom_wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'chainChanged',
                        type: 'Keplr',
                        chainId: chainId,
                    });
                    // if wa is not empty, send data
                    this.sendWalletData(atom_wallets);
                }
            }

            // Keplr - wallet
            else if(event?.data?.type === 'proxy-request-response'){
                var wallet = event?.data?.result?.return?.bech32Address
                if(this.is_atomscan(wallet)){
                    var atom_wallets: Array<walletObject> = new Array<walletObject>();
                    // new connection
                    atom_wallets.push({
                        address: wallet,
                        event: 'accountsChanged',
                        type: 'Keplr',
                        chainId: null,
                    });
                    // if wa is not empty, send data
                    this.sendWalletData(atom_wallets);
                }
            }

            // Leap - chainId
            else if(event?.data?.target === 'leap:content' && event?.data?.data?.type === 'enable-access'){
                var chainId = event?.data?.data?.chainIds[0]
                if(this.is_atomscan(chainId)){
                    var atom_wallets: Array<walletObject> = new Array<walletObject>();
                    // chainChanged
                    atom_wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'chainChanged',
                        type: 'Leap',
                        chainId: chainId,
                    });
                    // if wa is not empty, send data
                    this.sendWalletData(atom_wallets);
                }
            }

            // Leap - wallet
            else if(event?.data?.target === 'leap:inpage' && event.data?.data?.name === 'onGET-KEY'){
                var wallet = event?.data?.data?.payload?.key?.bech32Address
                if(this.is_atomscan(wallet)){
                    var atom_wallets: Array<walletObject> = new Array<walletObject>();
                    // new connection
                    atom_wallets.push({
                        address: wallet,
                        event: 'accountsChanged',
                        type: 'Leap',
                        chainId: null,
                    });
                    // if wa is not empty, send data
                    this.sendWalletData(atom_wallets);
                }
            }
        });
        
        // Check if aptos is on the window object
        if (typeof window.aptos !== 'undefined') {
            window.aptos.onAccountChange((newAccount: { address: string }) => {
                var aptos_wallets: Array<walletObject> = new Array<walletObject>();
                // Changed account
                aptos_wallets.push({
                    address: newAccount?.address,
                    event: 'accountsChanged',
                    type: 'Petra',
                    chainId: null,
                });
                // if wa is not empty, send data
                this.sendWalletData(aptos_wallets);
            });
            window.aptos.onNetworkChange((newNetwork: { name: string }) => {
                var aptos_wallets: Array<walletObject> = new Array<walletObject>();
                // Changed network
                aptos_wallets.push({
                    address: 'Unknown', // To avoid being removed during data processing
                    event: 'accountsChanged',
                    type: 'Petra',
                    chainId: `Aptos ${newNetwork?.name}`,
                });
                // if wa is not empty, send data
                this.sendWalletData(aptos_wallets);
            });
        }
    }

    public setupStarknetListeners() {
        const starknetChainDict: Record<string, string> = this.getStarknetchainDict();

        // Argent X
        if (typeof window.starknet_argentX !== 'undefined') {
            // Listen for accounts changed
            window.starknet_argentX.on('accountsChanged', async (ac: string[]) => {
                // Add all wallets returned by event to out_wallets
                var out_wallets: Array<walletObject> = new Array<walletObject>();
                if (!Array.isArray(ac)) {
                    ac = [ac];
                }
                const walletType = window.starknet_argentX.name;
                const chainId = starknetChainDict[window.starknet_argentX.chainId || '']
                ac.forEach(async (element) => {
                    out_wallets.push({
                        address: element,
                        event: 'accountsChanged',
                        type: walletType,
                        chainId: chainId,
                    });
                });
                // if wa is not empty, send data
                this.sendWalletData(out_wallets);
            });

            // Listen for chain changed
            window.starknet_argentX.on('networkChanged', async (ac: string[]) => {
                // Add all wallets returned by event to eth_wallets
                var out_wallets: Array<walletObject> = new Array<walletObject>();
                if (!Array.isArray(ac)) {
                    ac = [ac];
                }
                const walletType = window.starknet_argentX.name;
                ac.forEach(async (element) => {
                    out_wallets.push({
                        address: 'Unknown', // To avoid being removed during data processing
                        event: 'chainChanged',
                        type: walletType,
                        chainId: starknetChainDict[element || ''],
                    });
                });
                // if wa is not empty, send data
                this.sendWalletData(out_wallets);
            });
        }

        // Braavos
        if (typeof window.starknet_braavos !== 'undefined') {
            
            Object.defineProperty(window.starknet_braavos, 'selectedAddress', {
                get() {
                    return this._selectedAddress;
                },
                set(newValue) {
                    // If newValue is not empty, send data
                    if (newValue !== '' && typeof newValue !== 'undefined' && newValue !== null) {
                        const chainId = starknetChainDict[window.starknet_braavos.account.provider.chainId || '']
                        const walletObject: walletObject = {
                            address: newValue,
                            event: 'accountsChanged',
                            type: 'Braavos',
                            chainId: chainId,
                        };

                        // Dispatch the event
                        const lsEvent = new CustomEvent('customConnectedWalletSafary', {
                            detail: { wallet: walletObject },
                        });
                        window.dispatchEvent(lsEvent);
                    }
                    
                    // Update the actual value
                    this._selectedAddress = newValue;
                },
                configurable: true, // Allow the property to be reconfigured if needed
                enumerable: true    // Make the property enumerable
            });

            // Listen to the event: might reuse later
            window.addEventListener(
                'customConnectedWalletSafary',
                async (event: any) => {
                    await this.sendWalletData([event.detail.wallet]);
                }
            );            
        }
    }

    private setupWalletConnect() {
        try {
            const originalSetItem = window.localStorage.setItem;
            // Overwrite the setItem function
            window.localStorage.setItem = function (
                key: string,
                value: string
            ) {
                try {
                    // Call the original setItem method
                    originalSetItem.call(this, key, value);

                    // Dispatch the event
                    const lsEvent = new CustomEvent('localStorageSetItem', {
                        detail: { key: key, value: value },
                    });
                    window.dispatchEvent(lsEvent);
                } catch (e) {
                    console.error('Error in Safary tag when saving Local Storage.');
                }
            };

            // Listen to the event
            window.addEventListener(
                'localStorageSetItem',
                async (event: any) => {
                    if (['wagmi.store'].includes(event.detail.key)) {
                        const wm = this.handleWagmi(event.detail.value);
                        await this.sendWalletData(wm);
                    }
                    else if (['hashconnectData'].includes(event.detail.key)) {
                        const li = this.handleHashConnect(event.detail.value);
                        await this.sendWalletData(li);
                    }
                    else if (['li.fi-wallets'].includes(event.detail.key)) {
                        const li = this.handleLiFi(event.detail.value);
                        await this.sendWalletData(li);
                    }
                    else if (event.detail.key.startsWith('dynamic_authenticated_user')){
                        const dyn = this.handleDynamic(event.detail.value);
                        await this.sendWalletData(dyn);
                    }
                    else if (['privy:connections'].includes(event.detail.key)) {
                        const privy = this.handlePrivy(event.detail.value);
                        await this.sendWalletData(privy);
                    }
                    else {
                        const wc = await this.handleWalletConnect(event.detail, true);
                        await this.sendWalletData(wc);
                    }
                }
            );
        } catch (e) {
            console.error('Error in Safary tag when setting up Wallet Connect.');
        }
    }

    /**
     *  Check if the object is in the list
     *
     * @param {walletObject} newObject - Object to check
     * @param {Array<walletObject>} list - List of objects
     *
     * @returns {boolean} - True if the object is in the list, false otherwise
     */
    public static isObjectInList(newObject: walletObject, list: Array<walletObject>) {
        
        return list.some(obj => 
            obj.address === newObject.address && 
            obj.event === newObject.event &&
            obj.type === newObject.type &&
            obj.chainId === newObject.chainId
        );
    }

    /**
     * Filter the array of wallet objects to remove the ones that are already in the current list
     * 
     * @param {walletObject} newArray - Array of wallet objects to filter
     * @param {Array<walletObject>} existingList - List of wallet objects to compare against
     * 
     * @returns {Array<walletObject>} - Array of wallet objects that are not in the current list
     */
    public static filterExistingObjects(newArray: Array<walletObject>, existingList: Array<walletObject>) {
        return newArray.filter(newObj => !SafaryManager.isObjectInList(newObj, existingList));
    }

    private async sendWalletData(wa: Array<walletObject>) {
        if (wa.length > 0) {
            const filteredArray = SafaryManager.filterExistingObjects(wa, this.currentListenerWallets);
            if (filteredArray.length > 0) {
                this.currentListenerWallets = this.currentListenerWallets.concat(filteredArray);

                const infoObject = this.getInfoObject('wl');
                infoObject['wa'] = filteredArray;
                await this.sendVisitData(infoObject);
            }
        }
    }

    public handleWagmi(value: string): Array<walletObject> {
        try {
            const wstore = JSON.parse(value);
            if(wstore && wstore?.state?.data){
                const wdata = wstore?.state?.data;
                if(wdata && wdata.account && wdata.account.length > 0){
                    let wtype: string | null = null;
                    try {
                        wtype = JSON.parse(window.localStorage.getItem('wagmi.wallet') || '');
                    }
                    catch (e) {
                        wtype = null;
                    }
                    return [{
                        address: wdata.account,
                        event: 'wagmi_wallets',
                        type: wtype,
                        chainId: wdata?.chain?.id,
                    }];
                }
            }
        }
        catch (e) {
            return [];
        }
        return [];
    }

    public handleHashConnect(value: string): Array<walletObject> {
        const hederaChainDict: Record<string, number> = this.getHederachainDict();
        var hash_wallets: Array<walletObject> = new Array<walletObject>();
        try {
            const hash_paired_data = JSON.parse(value);
            if(hash_paired_data && hash_paired_data?.pairingData && hash_paired_data?.pairingData?.length > 0){
                const wdataList = hash_paired_data?.pairingData;
                for (let wdata of wdataList) {
                    const network = wdata?.network;
                    const chainId = hederaChainDict['hedera-' + network]
                    if(wdata && wdata?.accountIds && wdata?.accountIds?.length > 0){
                        for(let account of wdata.accountIds){
                            hash_wallets.push({
                                address: account,
                                event: 'hashconnect_wallets',
                                type: 'HashPack',
                                chainId: chainId,
                            });
                        }
                    }
                }
            }
        }
        catch (e) {
            return hash_wallets;
        }
        return hash_wallets;
    }

    public handleLiFi(value: string): Array<walletObject> {
        try {
            const wallets = JSON.parse(value);
            if(wallets && wallets.length > 0){
                const wdata = wallets[wallets.length-1];
                if(wdata && wdata?.address && wdata?.address.length > 0){
                    return [{
                        address: wdata.address,
                        event: 'lifi_wallets',
                        type: wdata.name,
                        chainId: null,
                    }];
                }
            }
        }
        catch (e) {
            return [];
        }
        return [];
    }

    public handleDynamic(value: string): Array<walletObject> {
        var dyn_wallets: Array<walletObject> = new Array<walletObject>();
        try {
            const dyn_auth_user = JSON.parse(value);
            if(dyn_auth_user && dyn_auth_user?.verifiedCredentials && dyn_auth_user?.verifiedCredentials?.length > 0){
                const wdataList = dyn_auth_user?.verifiedCredentials;
                for (let wdata of wdataList) {
                    if(wdata && wdata?.address && wdata?.address?.length > 0){
                        dyn_wallets.push({
                            address: wdata.address,
                            event: 'dynamic_authenticated_user',
                            type: wdata.walletName,
                            chainId: wdata.chain,
                        });
                    }
                }
            }
        }
        catch (e) {
            return dyn_wallets;
        }
        return dyn_wallets;
    }

    public handlePrivy(value: string): Array<walletObject> {
        var privy_wallets: Array<walletObject> = new Array<walletObject>();
        try {
            const connections = JSON.parse(value);
            if(connections && connections?.length > 0){
                const wdataList = connections;
                for (let wdata of wdataList) {
                    if(wdata && wdata?.address && wdata?.address?.length > 0){
                        privy_wallets.push({
                            address: wdata.address,
                            event: 'privy_connection',
                            type: wdata.walletClientType !== 'unknown' ? wdata.walletClientType : wdata.connectorType,
                            chainId: wdata?.chainId, // Not exposed, but they have it internally and might expose one day.
                        });
                    }
                }
            }
        }
        catch (e) {
            return privy_wallets;
        }
        return privy_wallets;
    }

    private async handleWalletConnect(data: { key: any; value: any }, 
                                      is_listener_call: boolean = false): 
                                      Promise<Array<walletObject>> {
        const key = data.key;

        if (
            [
                'wc@2:client:0.3//session',
                'WALLETCONNECT_DEEPLINK_CHOICE',
                'wc@2:ethereum_provider:/chainId',
                'WCM_RECENT_WALLET_DATA',
                'WCM_VERSION',
                'SAFARY_VISIT',
                'SAFARY_POOLING',
            ].includes(key)
        ) {
            // Try reading WalletConnect from Local Storage
            let session_value_ls = window.localStorage.getItem(
                'wc@2:client:0.3//session'
            );

            if (session_value_ls && session_value_ls?.length > 0) {
                return await this.processWalletConnect(
                    is_listener_call,
                    session_value_ls
                );
            } else {
                // Else, Try reading WalletConnect from IndexedDB
                try {
                    const session_value_idb = await this.getValueFromIndexedDB(
                        'wc@2:client:0.3:session'
                    );

                    if (session_value_idb && session_value_idb?.length > 0) {
                        const wc_deeplink = await this.getValueFromIndexedDB(
                            'WALLETCONNECT_DEEPLINK_CHOICE'
                        );
                        const wc_chainid = await this.getValueFromIndexedDB(
                            'wc@2:ethereum_provider:chainId'
                        );
                        return await this.processWalletConnect(
                            is_listener_call,
                            session_value_idb,
                            wc_deeplink,
                            wc_chainid
                        );
                    } else {
                        throw new Error(
                            'Error Reading Wallet Connect from IndexedDB.'
                        );
                    }
                } catch (e) {
                    // Else, Setup Pooling
                    if(this.pooling_active === false){
                        const intervalId = window.setInterval(() => {
                            if(this.pooling_active){
                                const lsEvent = new CustomEvent('localStorageSetItem', {
                                    detail: { key: 'SAFARY_POOLING', value: '' },
                                });
                                window.dispatchEvent(lsEvent);
                            }
                        }, 10000);

                        // Stop the pooling after 60 seconds
                        window.setTimeout(() => {
                            window.clearInterval(intervalId);
                            this.pooling_active = false;
                        }, 60000);

                        this.pooling_active = true;
                    }
                }
                
            }
        }
        return [];
    }

    private getHederachainDict(): Record<string, number> {
        return {
            "hedera-mainnet": 295,
            "hedera-testnet": 296,
            "hedera-previewnet": 297,
            "hedera-localnet": 298
        };
    }

    private getStarknetchainDict(): Record<string, string> {
        return {
            'SN_MAIN': 'Starknet Mainnet',
            '0x534e5f4d41494e': 'Starknet Mainnet',
            '23448594291968334': 'Starknet Mainnet',
            
            '393402133025997798000961': 'Starknet Sepolia',
            '0x534e5f5345504f4c4941': 'Starknet Sepolia',
            'SN_SEPOLIA': 'Starknet Sepolia',

            '1536727068981429685321': 'Starknet Goerli',
            '0x534e5f474f45524c49': 'Starknet Goerli',
            'SN_GOERLI': 'Starknet Goerli',

            '393402129659245999442226': 'Starknet Goerli2',
            '0x534e5f474f45524c4932': 'Starknet Goerli2',
            'SN_GOERLI2': 'Starknet Goerli2',

            '': 'Unknown'
        };
    }
    
    private async getValueFromIndexedDB(
        key: string,
        dbName: string = 'WALLET_CONNECT_V2_INDEXED_DB',
        objectStoreName: string = 'keyvaluestorage'
    ): Promise<any> {
        let newSfryWcIdbAttempt = false;
        return new Promise((resolve, reject) => {
            // Open the database: DO NOT specify version
            // (it will get latest, and make sure onupgradeneeded is called only when DB does not exist)
            const request = window.indexedDB.open(dbName);

            request.onerror = (_event) => {
                if(!newSfryWcIdbAttempt){
                    console.error('Safary tag error: IndexedDB Database error.');
                    reject(request.error);
                }
                else {
                    // Not an error, but a new DB creation was aborted
                    resolve(null);
                }
            };

            request.onupgradeneeded = () => {
                // The DB did not exist, we don't want to create it, so abort
                newSfryWcIdbAttempt = true;
                request?.transaction?.abort();
                resolve(null);
            };

            request.onsuccess = (_event) => {
                const db = request.result;

                try {
                    // Start a transaction and get the object store
                    const transaction = db.transaction(
                        [objectStoreName],
                        'readonly'
                    );
                    const objectStore = transaction.objectStore(objectStoreName);

                    // Get the value by key
                    const getRequest = objectStore.get(key);

                    getRequest.onerror = (_event) => {
                        console.error('Safary tag error: IndexedDB Error getting value.');
                        reject(getRequest.error);
                    };

                    getRequest.onsuccess = (_event) => {
                        resolve(getRequest.result); // Returns the value associated with the key
                    };
                } catch (e) {
                    resolve(null);
                }
            };
        });
    }

    // Wallet Connect: get data and send
    private async processWalletConnect(
        is_listener_call: boolean = false,
        session_value: string | null = window.localStorage.getItem(
            'wc@2:client:0.3//session'
        ),
        wc_deeplink: string | null = window.localStorage.getItem(
            'WALLETCONNECT_DEEPLINK_CHOICE'
        ),
        wc_chainid: string | null = window.localStorage.getItem(
            'wc@2:ethereum_provider:/chainId'
        )
    ): Promise<Array<walletObject>> {
        var wc_wallet: Array<walletObject> = new Array<walletObject>();

        try {
            if(session_value){
                const session = JSON.parse(session_value || '')[0];
                const namespaces = session.namespaces;
                // Get first value under namespaces
                const firstNamespace = namespaces[Object.keys(namespaces)[0]];
                const full_wa = firstNamespace.accounts[0].split(':');
                const wa: string = full_wa.pop();

                // Merge string in array full_wa
                const chainShortName: string = full_wa.join('-');
                const hederaChainDict: Record<string, number> = this.getHederachainDict();
                const wch = chainShortName.startsWith('hedera')
                    ? hederaChainDict[chainShortName]
                    : wc_chainid;

                let wty = this.getWalletTypeForWalletConnect(wc_deeplink, session)

                // Create object to send to Safary
                wc_wallet.push({
                    address: wa,
                    event: is_listener_call
                        ? 'eth_sfry_wc_listener'
                        : 'eth_sfry_wc',
                    type: wty,
                    chainId: wch,
                });

                this.pooling_active = false;
            }
        } catch (e) {
            console.error('Safary tag error when processing Wallet Connect.', e);
        }
        return wc_wallet;
    }

    private getWalletTypeForWalletConnect(wc_deeplink: string | null, session: any){
        let wty = undefined;
        try {
            if (wc_deeplink && wc_deeplink.trim().length > 0) {
                wty = JSON.parse(wc_deeplink)?.name;
            }
            if (!wty || wty.trim().length === 0) {
                wty = session?.peer?.metadata?.name || 'WalletConnect';
            }
        } catch (error) {
            //
        }
        return wty ?? 'WalletConnect';
    }

    public async trackSwaps(params?: {
        eventName?: string;
        fromAmount: number;
        fromCurrency: string;
        fromAmountUSD?: number;
        contractAddress: string;
        parameters?: Record<string, unknown>;
    }) {
        try {
            assertType(params, true, 'object', 'params');
            assertUndefined(params?.fromAmount, 'fromAmount');
            assertUndefined(params?.fromCurrency, 'fromCurrency');
            assertUndefined(params?.contractAddress, 'contractAddress');
            assertType(params?.parameters, false, 'object', 'parameters');
        } catch (e) {
            console.error(
                'ERROR: safary.trackSwaps(): there were some validation errors.'
            );
            return;
        }

        return this.track({
            eventType: 'swap',
            eventName: params?.eventName ?? 'Swaps',
            parameters: {
                default__fromAmount: params?.fromAmount,
                default__fromCurrency: params?.fromCurrency,
                default__contractAddress: params?.contractAddress,
                default__fromAmountUSD: params?.fromAmountUSD,
            },
        });
    }

    public async trackDeposit(params?: {
        eventName?: string;
        amount: number;
        currency: string;
        amountUSD?: number;
        contractAddress: string;
        parameters?: Record<string, unknown>;
    }) {
        try {
            assertType(params, true, 'object', 'params');
            assertUndefined(params?.amount, 'fromAmount');
            assertUndefined(params?.currency, 'fromCurrency');
            assertUndefined(params?.contractAddress, 'contractAddress');
            assertType(params?.parameters, false, 'object', 'parameters');
        } catch (e) {
            console.error(
                'ERROR: safary.trackDeposit(): there were some validation errors.'
            );
            return;
        }

        return this.track({
            eventType: 'deposit',
            eventName: params?.eventName ?? 'Deposits',
            parameters: {
                default__fromAmount: params?.amount,
                default__fromCurrency: params?.currency,
                default__contractAddress: params?.contractAddress,
                default__fromAmountUSD: params?.amountUSD,
            },
        });
    }

    public async trackWithdrawal(params?: {
        eventName?: string;
        amount: number;
        currency: string;
        amountUSD?: number;
        contractAddress: string;
        parameters?: Record<string, unknown>;
    }) {
        try {
            assertType(params, true, 'object', 'params');
            assertUndefined(params?.amount, 'fromAmount');
            assertUndefined(params?.currency, 'fromCurrency');
            assertUndefined(params?.contractAddress, 'contractAddress');
            assertType(params?.parameters, false, 'object', 'parameters');
        } catch (e) {
            console.error(
                'ERROR: safary.trackWithdrawal(): there were some validation errors.'
            );
            return;
        }

        return this.track({
            eventType: 'withdrawal',
            eventName: params?.eventName ?? 'Withdrawals',
            parameters: {
                default__fromAmount: params?.amount,
                default__fromCurrency: params?.currency,
                default__contractAddress: params?.contractAddress,
                default__fromAmountUSD: params?.amountUSD,
            },
        });
    }

    public async trackNFTPurchase(params?: {
        eventName?: string;
        amount: number;
        currency: string;
        amountUSD?: number;
        contractAddress: string;
        tokenId: number;
        parameters?: Record<string, unknown>;
    }) {
        try {
            assertType(params, true, 'object', 'params');
            assertUndefined(params?.amount, 'fromAmount');
            assertUndefined(params?.currency, 'fromCurrency');
            assertUndefined(params?.contractAddress, 'contractAddress');
            assertUndefined(params?.tokenId, 'tokenId');
            assertType(params?.parameters, false, 'object', 'parameters');
        } catch (e) {
            console.error(
                'ERROR: safary.trackNFTPurchase(): there were some validation errors.'
            );
            return;
        }

        return this.track({
            eventType: 'NFT Purchase',
            eventName: params?.eventName ?? 'NFT Purchases',
            parameters: {
                default__fromAmount: params?.amount,
                default__fromCurrency: params?.currency,
                default__fromAmountUSD: params?.amountUSD,
                default__contractAddress: params?.contractAddress,
                default__tokenId: params?.tokenId,
            },
        });
    }
}

```
````



</details>

