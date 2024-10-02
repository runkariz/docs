# Configurations

- `shopify_get_orders` : Retrieve a list of orders for **Shopify**
- `woocommerce_get_orders` : Retrieve a list of orders for **WooCommerce**
- ------------------------------------------------------------------------------------------

#### Retrieve a list of orders

<details>
 <summary><code>POST</code> <code>https://api.demo.trykariz.com/api/KarizClientApp/RunSync?configName={config_name}&tpUserId={tp_app_user_id}</code></summary>

##### Parameters

> | name       | required    | type   | data type | description                                       |
> |------------|-------------|--------|-----------|---------------------------------------------------|
> | configName | required    | query  | string    | Workflow configuration name                       |
> | tpUserId   | required    | query  | string    | The user id that you want to execute workflow for |

##### Body
Expected contract: [generic_order_param_contract.json](generic_order_param_contract.json)\
**Sample JSON:**
```json
{
  "api_version": "1",
  "attribution_app_id": "1",
  "created_at_max": "1948-06-08T07:16:01.0Z",
  "created_at_min": "1948-06-08T07:16:01.0Z",
  "fields": "id,name",
  "financial_status": "paid",
  "fulfillment_status": "shipped",
  "ids": "1,2",
  "limit": 10,
  "processed_at_max": "1948-06-08T07:16:01.0Z",
  "processed_at_min": "1948-06-08T07:16:01.0Z",
  "since_id": "1",
  "status": "any",
  "updated_at_max": "1918-04-30T21:13:27.0Z",
  "updated_at_min": "1939-08-05T01:34:33.0Z"
}
```

##### Responses

> | http code | content-type              | response                                                     |
> |-----------|---------------------------|--------------------------------------------------------------|
> | `200`     | `application/json`        | [generic_order_contract.json](generic_order_contract.json)   |
> | `400`     | `application/json`        | `{"code":"400","message":"Bad Request"}`                     |
> | `405`     | `text/html;charset=utf-8` | None                                                         |

Sample Response:
```json
{
  "Errors": {},
  "Output": {
    "step": "transform",
    "isSuccess": true,
    "schemaValidationErrors": null,
    "restErrors": {},
    "operationErrors": null,
    "unexpectedErrors": null,
    "result": [
      {
        "id": "5954461335827",
        "channel_name": "shopify",
        "name": "#1004"
      },
      {
        "id": "5954461335827",
        "channel_name": "shopify",
        "name": "#1004"
      }
    ]
  }
}
```
##### Example cURL

> ```javascript
>  curl -X 'POST' \ 
> 'https://api.demo.trykariz.com/api/KarizClientApp/RunSync?configName=shopify_get_orders&tpUserId=1' \
> -H 'accept: text/plain' \
> -H 'Authorization: Bearer {access_token}' \
> -H 'Content-Type: application/json' \
> -d '{
> "limit": 1
> }'
> ```

</details>