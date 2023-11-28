## Federated users {#user-sso}

If you did not enable the **Automatically create users** option when [setting up a federation](../../organization/concepts/add-federation.md#federation-usage), you will have to add federated users to your organization manually.

To do this, you need to know the user name IDs returned by the Identity Provider (IdP) server together with the successful authentication response. This will usually be the user's email address. If you do not know what the server returns as the Name ID, contact the administrator who configured authentication for your federation.
### Add federated users {#add-user-sso}

{% include notitle [user-sso](add-user-sso.md) %}
