# Order Configurations
To interact with your users' orders, use the following configurations to fetch their orders.
The order data from all the different channels is matched to a single unified data model based on the Shopify order data model. For more information about how the data model works, refer to the [Shopify Order API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/order).\
Kariz uses the Shopify data model because it allows for easy adaptation to your internal data model, especially if you have already built an integration with Shopify. You can instantly add any of our new integrations to your product because the data model we provide is consistent across all channels.\
**Modifications to the Shopify Data Model**:\
We have made two minor changes to the Shopify order data model:\
_ID Fields as Strings_: All ID fields are converted to strings to maintain consistency across different platforms.\
_Added `channel_name` Variable_: A new variable called `channel_name` is added to indicate the source platform of the user data. For example, if the user data is fetched from Shopify, `channel_name = 'shopify'`.
### `shopify_get_orders` : Retrieve a list of orders for **Shopify**
##### Channels:
In the configuration, the scopes that are being used are called **channels**.
- `shopify`
##### Available parameters:
> | name    | required                                | data type | description                            |
> |---------|-----------------------------------------|-----------|----------------------------------------|
> | baseUrl | *required(if user is not authenticated) | string    | Shop URL (ex: shop_name.myshopify.com) |
> | token   | *required(if user is not authenticated) | string    | OAuth 2.0 token                        |
Sample Json:
```json
{
  "params": {
    "shopify": [
      {
        "key": "baseUrl",
        "value": "shop_name.myshopify.com"
      },
      {
        "key": "token",
        "value": "oauth token"
      }
    ]
  }
}
```

---
### `woocommerce_get_orders` : Retrieve a list of orders for **WooCommerce**
##### Channels:
In the configuration, the scopes that are being used are called **channels**.
- `woocommerce`
##### Available parameters:
> | name    | required                                | data type | description                                              |
> |---------|-----------------------------------------|-----------|----------------------------------------------------------|
> | baseUrl | *required(if user is not authenticated) | string    | Full shop URL (ex: https://domain.com/wordpress/wp-json) |
> | token   | *required(if user is not authenticated) | string    | Basic Auth token (base64)                                |
Sample Json:
```json
{
  "params": {
    "woocommerce": [
      {
        "key": "baseUrl",
        "value": "https://domain.com/wordpress"
      },
      {
        "key": "token",
        "value": "base64 token"
      }
    ]
  }
}
```

---

#### Retrieve a list of orders

<details>
 <summary><code>POST</code> <code>https://api.demo.trykariz.com/api/KarizClientApp/RunSync?configName={config_name}&tpUserId={tp_app_user_id}</code></summary>

##### Parameters

> | name       | required    | type   | data type | description                                       |
> |------------|-------------|--------|-----------|---------------------------------------------------|
> | configName | required    | query  | string    | [Workflow configuration name](#Configurations)    |
> | tpUserId   | required    | query  | string    | The user id that you want to execute workflow for |

##### Body
Expected contract: [generic_order_param_contract.json](generic_order_param_contract.json)\
**Sample JSON:**
```json
{
  "params": {
    "shopify": [
      {
        "key": "baseUrl",
        "value": "shop_name.myshopify.com"
      },
      {
        "key": "token",
        "value": "oauth token"
      }
    ],
    "global": [
      {
        "key": "disable_validations",
        "value": "true"
      }
    ]
  },
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