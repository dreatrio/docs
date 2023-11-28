{% list tabs %}

- CLI

{% include [cli-install](../cli-install.md) %}

1. Assign a [role](../../iam/concepts/access-control/roles.md) for the group:

   ```bash
   yc organization-manager organization add-access-binding \
   --subject=group:<group_ID> \
   --role=<role_ID> \
   <organization_ID>
   ```

1. Make sure the requested rights are granted:

   ```bash
   yc organization-manager organization list-access-bindings <organization_ID>
   ```

   A response contains a list of all roles assigned to users and groups in the organization:

   ```
   +------------------------------------------+--------------+----------------------+
   |                 ROLE ID                  | SUBJECT TYPE |      SUBJECT ID      |
   +------------------------------------------+--------------+----------------------+
   | organization-manager.admin               | userAccount  | ajev1p2345lj******** |
   | organization-manager.organizations.owner | userAccount  | ajev1p2345lj******** |
   | editor                                   | group        | ajev1p2345lj******** |
   | viewer                                   | group        | ajev1p2345lj******** |
   +------------------------------------------+--------------+----------------------+
   ```

- Terraform

   {% include [terraform-install](../terraform-install.md) %}

   1. Add the resource parameters to the configuration file and specify the required role and group:

      ```
      resource "yandex_organizationmanager_organization_iam_member" "users-editors" {
        organization_id = "<cloud_ID>"
        role            = "<role_ID>"
        member          = "group:<group_ID>"
      }
      ```

      Where:

      * `organization_id`: [Cloud ID](../../resource-manager/operations/cloud/get-id.md). This is a required parameter.
      * `role`: [Role](../../iam/concepts/access-control/roles.md) being assigned. This is a required parameter.
      * `member`: Group the role is assigned to. It should be specified in `group:<group_ ID>` format. This is a required parameter.

      For more information about the `yandex_organizationmanager_organization_iam_member` resource parameters, see the [provider documentation]({{ tf-provider-resources-link }}/organizationmanager_organization_iam_member).


   1. Create resources:

      {% include [terraform-validate-plan-apply](../../_tutorials/terraform-validate-plan-apply.md) %}

      All the resources you need will then be created in the specified folder. You can check the new resource using the [management console]({{ link-console-main }}) or this [CLI](../../cli/quickstart.md) command:

      ```bash
      yc resource-manager folder list-access-bindings <folder_name>|<folder_ID>
      ```

{% endlist %}