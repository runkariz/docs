# Product Configurations

The product data from all the different channels is matched to a single unified data model based on the Shopify product
data model. For more information about how the data model works, refer to
the [Shopify Product API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/product).\
Additionally, the inventory data for all product variants is included in our product model. This means that with one
call, you get:\
All the products.\
All their variants.\
Their inventory.\
Their inventory levels split over multiple locations (if available for the channel).\
For more information about the inventory data, refer to the Shopify inventory models:\
Inventory
Item: [InventoryItem API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/inventoryitem)\
Inventory
Level: [InventoryLevel API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/inventorylevel)\
**Modifications to the Shopify Data Model**:\
We have made two minor changes to the Shopify product data model:\
_ID Fields as Strings_: All ID fields are converted to strings to maintain consistency across different platforms.\
_Added `channel_name` Variable_: A new variable called `channel_name` is added to indicate the source platform of the
user data. For example, if the user data is fetched from Shopify, `channel_name = 'shopify'`.

### `shopify_get_products` : Retrieve a list of products for **Shopify** (including Inventory Items and Levels)

#### Delivery method(s):

- [x] **JSON response (sync)**
- [x] **Webhooks (async)**

##### Channels (scopes):

- [x] `shopify`

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

### `woocommerce_get_products` : Retrieve a list of products for **WooCommerce**

#### Delivery method(s):

- [x] **JSON response (sync)**
- [x] **Webhooks (async)**

##### Channels (scopes):

- [x] `woocommerce`

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

### `bol_get_all_offers` : Retrieve all offers for **Bol**

#### Delivery method(s):

- [ ] JSON response (sync)
- [x] **Webhooks (async)**

The delivery method for this configuration utilizes webhooks.
This means you need to include a webhook URL in your parameters as outlined below.
Kariz will stream the results to the provided webhook.
Currently, the batch size is set to **50** records per transmission, but this will be adjustable in the future.

**Note: For testing webhooks follow this
link: [Testing Webhooks](https://docs.github.com/en/webhooks/testing-and-troubleshooting-webhooks/testing-webhooks)**

##### Channels (scopes):

- [x] `bol`

##### Expected response: 202, 400, 403, 405

The [generic_product_param_contract.json](generic_product_param_contract.json) will be delivered **without the operation
wrap**.
###### Body
```json
[
  {
    "id": "5954461335827",
    "channel_name": "bol",
    "name": "#1004"
  },
  {
    "id": "5954461335827",
    "channel_name": "bol",
    "name": "#1004"
  }
]
```
###### Headers

> | name                           | description                                                                             |
> |--------------------------------|-----------------------------------------------------------------------------------------|
> | **X-Kariz-User-Id**            | Kariz user id which is preforming the operation                                         |
> | **X-Kariz-TP-User-Id**         | The `tpUserId` that you provide when running the configuration                          |
> | **X-Kariz-Configuration-Name** | The `configName` Name of the configuration                                              |
> | **X-Kariz-Execution-Id**       | Execution id                                                                            |
> | **X-Kariz-Hmac-SHA256**        | HMAC used to validate the request's origin (requires your client_id for verification).  |

You can use these headers that you receive on given webhook to process incoming data.


##### Available parameters:

> | name              | required                                | data type                     | description                                   |
> |-------------------|-----------------------------------------|-------------------------------|-----------------------------------------------|
> | token             | *required(if user is not authenticated) | string                        | Basic Auth token (base64)                     |
> | delivery_webhook  | *required                               | object (see the sample below) | The result will be delivered on the given URL |
Sample Json:

```json
{
  "params": {
    "bol": [
      {
        "key": "token",
        "value": "base64 token"
      },
      {
        "key": "delivery_webhook",
        "value": {
          "url": "https://smee.io/HhzhkxlPzMwo0GC",
          "method": "post",
          "headers": {
            "header_1": "1",
            "header_2": "2"
          },
          "params": {
            "param_1": "1",
            "param_2": "2"
          }
        }
      }
    ]
  }
}
```

---

#### Retrieve a list of products

<details>
 <summary><code>POST</code> <code>https://api.demo.trykariz.com/api/KarizClientApp/RunSync?configName={config_name}&tpUserId={tp_app_user_id}</code></summary>

##### Parameters

> | name                   | required | type   | data type   | description                                                                                     |
> |------------------------|----------|--------|-------------|-------------------------------------------------------------------------------------------------|
> | configName             | required | query  | string      | [Workflow configuration name](#Configurations)                                                  |
> | tpUserId               | required | query  | string      | The user id that you want to execute workflow for                                               |
> | completionCallbackUrl  | optional | query  | string(URL) | When the workflow is finished it calls given webhook with [workflow_done](../event/README.md)   |

##### Body

Expected contract: [generic_product_param_contract.json](generic_product_param_contract.json)\
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

> | http code | content-type              | response                                                       |
> |-----------|---------------------------|----------------------------------------------------------------|
> | `200`     | `application/json`        | [generic_product_contract.json](generic_product_contract.json) |
> | `202`     | `application/json`        | see the sample below                                           |
> | `400`     | `application/json`        | `{"code":"400","message":"Bad Request"}`                       |
> | `405`     | `text/html;charset=utf-8` | None                                                           |

**Sample Response (200):**

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

Note: The root properties (`Output`, `Errors`) are the operation wrap, `Output.result` contains [generic_product_contract.json](generic_product_contract.json)

**Sample Response (202):**

```json
{
  "workflowExecutionId": "4225a392-2f8f-43fe-ae6c-41de160d8b78",
  "userId": "xx",
  "tpUserId": "xx",
  "traceId": "00-109e1753a9364b210ffcfde508cd533d-c29fa557833c1d31-00",
  "eventType": "workflow_published",
  "configurationName": "bol_get_all_offers"
}
```

##### Example cURL

> ```javascript
>  curl -X 'POST' \ 
> 'https://api.demo.trykariz.com/api/KarizClientApp/RunSync?configName=shopify_get_products&tpUserId=1' \
> -H 'accept: text/plain' \
> -H 'Authorization: Bearer {access_token}' \
> -H 'Content-Type: application/json' \
> -d '{
> "limit": 1
> }'
> ```

</details>