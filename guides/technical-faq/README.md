---
description: Frequently Asked Questions
---

# Technical FAQ

{% content-ref url="general.md" %}
[general.md](general.md)
{% endcontent-ref %}

{% content-ref url="metrics-and-performance.md" %}
[metrics-and-performance.md](metrics-and-performance.md)
{% endcontent-ref %}

{% content-ref url="setup.md" %}
[setup.md](setup.md)
{% endcontent-ref %}

{% content-ref url="troubleshooting.md" %}
[troubleshooting.md](troubleshooting.md)
{% endcontent-ref %}

### Does W\&B support SSO for SaaS?

Yes, W\&B supports setting up Single Sign On (SSO) for the SaaS offering via Auth0. W\&B support SSO integration with any OIDC compliant identity provider(ex: Okta, AzureAD etc.). If you have an OIDC provider, please follow the steps below:

* Create a Single Page Application (SPA) on your Identity Provider.
* Set `grant_type` to `implicit` flow.
* Set the callback URI to [`https://wandb.auth0.com/login/callback`](https://wandb.auth0.com/login/callback)
* Once you have the above setup, contact your customer success manager(CSM) and let us know the Client ID and Issuer URL associated with the application.

We'll then setup an Auth0 connection with the above details and enable SSO.&#x20;
