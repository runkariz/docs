# Events

Events are generally use when an action happens in Kariz:

> | name                   | description                        |
> |------------------------|------------------------------------|
> | **workflow_published** | when a workflow has been started   |
> | **workflow_done**      | when a workflow is finished        |

## Payloads

```json
{
  "WorkflowExecutionId": "4225a392-2f8f-43fe-ae6c-41de160d8b78",
  "UserId": "xx",
  "TpUserId": "xx",
  "TraceId": "00-109e1753a9364b210ffcfde508cd533d-c29fa557833c1d31-00",
  "EventType": "workflow_done",
  "ConfigurationName": "bol_get_all_offers"
}
```
