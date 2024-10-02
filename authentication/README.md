# Authentication

---
## 1- Get a disposable token for your user
<details>
 <summary><code>GET</code><code>https://api.demo.trykariz.com/api/KarizClientApp/GetTpUserAuthDisposableToken?tpUserId={tp_app_user_id}&redirectUrl={redirect_url}&scopes={scope_names}</code></summary>

##### Parameters

> | name         | required | type   | data type | description                                                                                        |
> |--------------|----------|--------|-----------|----------------------------------------------------------------------------------------------------|
> | scopes       | required | query  | string[]  | Available options: `shopify`, `woocommerce`                                                        |
> | tpUserId     | required | query  | string    | The user id that you want to execute workflow for                                                  |
> | redirectUrl  | optional | query  | string    | The URL which user will be redirected to after finishes the authentication flow it can be your app |

**Note: `tpUserId` will be used to refer to your user and run workflows, so use a unique id for each user.**

##### Responses

> | http code | content-type              | response                                 |
> |-----------|---------------------------|------------------------------------------|
> | `200`     | `application/json`        | See the sample response                  |
> | `400`     | `application/json`        | `{"code":"400","message":"Bad Request"}` |
> | `405`     | `text/html;charset=utf-8` | None                                     |

Sample Response:
```json
{
  "token": "token",
  "expirationUtcUnixTimeSeconds": 1727873939
}
```
##### Example cURL

> ```javascript
> curl -X 'GET' \
> 'https://api.demo.trykariz.com/api/KarizClientApp/GetTpUserAuthDisposableToken?tpUserId=1&redirectUrl=https%3A%2F%2Fmyapp.com&scopes=shopify&scopes=woocommerce' \
> -H 'accept: text/plain' \
> -H 'Authorization: Bearer {access_token}'
> ```

</details>


---

## 2- Redirect your user on our authenticator

You must redirect your user to this page, which has a parameter `token` 
that must be filled with the generated token from step 1.
https://www.demo.trykariz.com/en/tpuser/authorize?token={token_from_step_1}

Your users will see this:\
![user auth flow](..%2Fassets%2Ftp_user_auth_flow.png)
Once they have authenticated, you can run workflows for them.