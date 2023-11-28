---
sourcePath: en/tracker/api-ref/concepts/queues/change-trigger.md
---
# Updating a trigger using an API request

Use this request to update a [trigger](../../user/trigger.md).

## Request format {#query}

Before making a request, [get permission to access the API](../access.md).

To update a trigger, use an HTTP `PATCH` request. Request parameters are passed in the request body in JSON format.

```json
PATCH /{{ ver }}/queues/<queue-id>/triggers/<trigger-id>?version=<trigger-current-version>
Host: {{ host }}
Authorization: OAuth <OAuth_token>
{{ org-id }}

{
    "name": "<trigger_name>",
    "actions": [<trigger_action_parameters>]
}
```

{% include [headings](../../../_includes/tracker/api/headings.md) %}

{% cut "Resource" %}

| Parameter | Description | Data type |
| ----- | ----- | ----- |
| \<queue-id\> | Queue ID or key. The queue key is case-sensitive. | String or number |
| \<trigger-id\> | Trigger ID | Number |
| \<trigger-current-version\> | Current trigger version | Number |

{% note info %}

You can find the current trigger version in a response to a [GET request to the trigger settings](get-trigger.md).

{% endnote %}

{% endcut %}

{% cut "Request body parameters" %}

**Additional parameters**

| Parameter | Description | Data type |
| ----- | ----- | ----- |
| name | Trigger name | String |
| actions | Array with [trigger action objects](change-trigger-actions.md) | Array of objects |
| conditions | Array with [trigger conditions](#conditions) | Array of objects |
| active | Trigger status. Acceptable values include:<ul><li>`true`: Active</li><li>`false`: Inactive</li></ul> | Logical |
| before | ID of the trigger before which to place this trigger | Number |

**Trigger conditions** {#conditions}

Depending on whether all or at least one condition is to be met, you can set the **conditions** field to either an array of elementary trigger condition objects (in this case, the trigger will fire if all the conditions are met) or an array with objects including both the array of elementary trigger condition objects and the type indicating how the conditions should be logically combined:

* Elementary [trigger condition object](change-trigger-conditions.md)
* Object indicating the type for combining trigger conditions:

   | Parameter | Description | Data type |
   | ----- | ----- | ----- |
   | type | Type for logically combining trigger conditions. Acceptable values include:<ul><li>`Or`: Logical OR (`Any`)</li><li>`And`: Logical AND (`All`)</li></ul> | String |
   | conditions | Array with elementary [trigger condition objects](change-trigger-conditions.md) | Array of objects |

{% endcut %}

Examples:

1. [Updating a trigger that fires if at least one of the conditions is met](#or-condition).
1. [Updating a trigger that fires if all the conditions are met](#and-condition).
1. [Disabling a trigger](#deactivation).
1. [Moving a trigger in the list and activating it](#move-before).

Example 1: Updating trigger conditions. The trigger must fire if at least one of the conditions is met. {#or-condition}
> - The HTTP `PATCH` method is used.
> - The trigger for the DESIGN queue is being updated.
> - The trigger ID is 16 and its current version is 1.
> - The trigger condition is being updated: the comment text matches the <q>Need info</q> or <q>Needs info</q> phrase.
> - Trigger action: the issue changes its status to <q>Need info</q>.
> ```json
> PATCH /{{ ver }}/queues/DESIGN/triggers/16?version=1
> Host: {{ host }}
> Authorization: OAuth <OAuth token>
> {{ org-id }}
> {
>   "actions": [
>       {
>           "type": "Transition",
>           "status": {
>               "key": "needInfo"
>               }
>       }
>   ],
>   "conditions": [
>        {
>            "type": "Or",
>            "conditions": [
>                { "type": "CommentFullyMatchCondition", "word": "Need info"},
>                { "type": "CommentFullyMatchCondition", "word": "Need info"}
>            ]
>        }
>    ]
> }
> ```

Example 2: Updating trigger conditions. The trigger must fire if all the conditions are met. {#and-condition}
> - The HTTP `PATCH` method is used.
> - The trigger for the DESIGN queue is being updated.
> - The trigger ID is 16 and its current version is 2.
> - The trigger condition is being updated: the comment text matches the <q>Need info</q> phrase and the issue status is `In progress`.
> - The trigger action remains unchanged: the issue changes its status to <q>Need info</q>.
> ```json
> PATCH /{{ ver }}/queues/DESIGN/triggers/16?version=2
> Host: {{ host }}
> Authorization: OAuth <OAuth token>
> {{ org-id }}
> {
>   "actions": [
>       {
>           "type": "Transition",
>           "status": {
>               "key": "needInfo"
>               }
>       }
>   ],
>   "conditions": [
>        { "type": "CommentFullyMatchCondition", "word": "Need info"},
>        { "type": "FieldEquals", "field":"status", "value":"inProgress" }
>    ]
> }
> ```

Example 3: Disabling a trigger. {#deactivation}
> - The HTTP `PATCH` method is used.
> - The trigger for the DESIGN queue is being updated.
> - The trigger ID is 16 and its current version is 3.
> - The trigger is being deactivated.
> ```json
> PATCH /{{ ver }}/queues/DESIGN/triggers/16?version=3
> Host: {{ host }}
> Authorization: OAuth <OAuth token>
> {{ org-id }}
> {
>   "active": false,
> }
> ```

Example 4: Moving a trigger in the list and activating it. {#move-before}
> - The HTTP `PATCH` method is used.
> - The trigger for the DESIGN queue is being updated.
> - The trigger ID is 16 and its current version is 4.
> - The trigger is being moved and placed before the trigger with the `6` ID and then activated.
> ```json
> PATCH /{{ ver }}/queues/DESIGN/triggers/16?version=4
> Host: {{ host }}
> Authorization: OAuth <OAuth token>
> {{ org-id }}
> {
>   "before": 6,
>   "active": true,
> }
> ```

## Response format {#answer}

{% list tabs %}

- The request is executed successfully

   {% include [answer-200](../../../_includes/tracker/api/answer-200.md) %}

   The request body contains information about the created trigger in JSON format.

   ```json
   {
      "id": 16,
      "self": "{{ host }}/{{ ver }}/queues/DESIGN/triggers/16",
      "queue": {
         "self": "{{ host }}/{{ ver }}/queues/DESIGN",
         "id": "26",
         "key": "DESIGN",
         "display": "Design"
      },
      "name": "TriggerName",
      "order": "0.0002",
      "actions": [
         {
            "type": "Transition",
            "id": 2,
            "status": {
               "self": "{{ host }}/{{ ver }}/statuses/2",
               "id": "2",
               "key": "needInfo",
               "display": "Need info"
            }
         }
      ],
      "conditions": [
         {
            "type": "Or",
            "conditions": [
               {
                  "type": "CommentFullyMatchCondition",
                  "word": "Need info",
                  "ignoreCase": true,
                  "removeMarkup": true,
                  "noMatchBefore": false
               },
               {
                  "type": "CommentFullyMatchCondition",
                  "word": "Need info",
                  "ignoreCase": true,
                  "removeMarkup": true,
                  "noMatchBefore": false
               }
            ]
         }
         ],
         "version": 2,
         "active": true
      }

   ```

   {% cut "Response parameters" %}

   | Parameter | Description | Data type |
   | ----- | ----- | ----- |
   | id | Trigger ID | String |
   | self | Links to trigger | String |
   | queue | Queue to create the trigger | Objects |
   | name | Trigger name | String |
   | order | Trigger weight. This parameter affects the order of trigger display in the interface. | String |
   | [actions](#actions) | Array with trigger actions | Array of objects |
   | [conditions](#conditions) | Array with trigger conditions | Array of objects |
   | version | Trigger version. Each trigger update increases the version number. | Number |
   | active | Trigger status | Logical |

   **Object fields** `queue` {#queue}

   {% include [queue](../../../_includes/tracker/api/queue.md) %}

   {% endcut %}

- The request failed

   If the request is processed incorrectly, the API returns a message with error details:

   {% include [answer-error-400](../../../_includes/tracker/api/answer-error-400.md) %}

   {% include [answer-error-403](../../../_includes/tracker/api/answer-error-403.md) %}

   {% include [answer-error-404](../../../_includes/tracker/api/answer-error-404.md) %}

   {% include [answer-error-409](../../../_includes/tracker/api/answer-error-409.md) %}

   {% include [answer-error-412](../../../_includes/tracker/api/answer-error-412.md) %}

   {% include [answer-error-422](../../../_includes/tracker/api/answer-error-422.md) %}

   {% include [answer-error-500](../../../_includes/tracker/api/answer-error-500.md) %}

   {% include [answer-error-503](../../../_includes/tracker/api/answer-error-503.md) %}

{% endlist %}