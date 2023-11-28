### Add federated users {#add-user-sso}

{% list tabs %}

- {{ org-name }}

   1. [Log in]({{ link-passport-login }}) to the organization administrator account.
   1. Go to [{{ org-full-name }}]({{ link-org-main }}).
   1. In the left-hand panel, select [{{ ui-key.yacloud_org.pages.users }}]({{ link-org-users }}) ![icon-users](../../_assets/organization/icon-users.svg).
   1. In the top-right corner, click ![icon-users](../../_assets/datalens/arrow-down.svg) → **{{ ui-key.yacloud_org.page.users.action.add-federated-users }}**.
   1. Select the identity federation to add users from.
   1. List the name IDs of users, separating them with line breaks.
   1. Click **Add**. This will give the users access to the organization.

- CLI

   {% include [cli-install](../cli-install.md) %}

   {% include [default-catalogue](../default-catalogue.md) %}

   1. View a description of the add user command:

      ```bash
      yc organization-manager federation saml add-user-accounts --help
      ```

   1. Add users by listing their name IDs separated by a comma:

      ```bash
      yc organization-manager federation saml add-user-accounts \
         --name <federation_name> \
         --name-ids <list_of_user_name_IDs>
      ```

- API

   Use the [addUserAccounts](../../iam/api-ref/Federation/addUserAccounts.md) REST API method for the [Federation](../../iam/api-ref/Federation/index.md) resource or the [FederationService/AddUserAccounts](../../iam/api-ref/grpc/federation_service.md#AddUserAccounts) gRPC API call and provide the following in the request:

   * Federation ID in the `federationId` parameter.
   * List of user name IDs in the `nameIds` parameter.

{% endlist %}
