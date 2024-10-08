# Authentication

There are two ways to authenticate your users with Kariz:
- 1- **Using Existing User Credentials**
- 2- **Using Our Authenticator**\
Both methods are supported by the API and are documented below.
We recommend using our authenticator when onboarding new users or adding users to new channels.

We recommend Using Existing User Credentials when onboarding existing users on existing channels. Using this method, you will need to add their credentials in each API request you make to us.

---
## 1- Using Existing User Credentials
If you prefer to authenticate users using existing credentials (e.g., when you have stored their credentials in your application), you can include the required parameters in the request body each time you run a method. This way, users do not need to authenticate again through our authenticator.

You only need to set the required parameters in the request body.

Structure:
```json
{
  "params": {
    "{channel_name or workflow environment variable name}": [
      {
        "key": "{variable}",
        "value": "{value}"
      }
    ]
  }
}

```

Sample Json:
```json
{
  "params": {
    "woocommerce": [
      {
        "key": "baseUrl",
        "value": "https://localhost:7443/wordpress"
      },
      {
        "key": "token",
        "value": "base64"
      }
    ],
    "global": [
      {
        "key": "disable_validation",
        "value": "true"
      }
    ]
  },
  "fields": "id,name"
}

```
### Explanation:
- **params**: Contains the parameters required for each channel or workflow.
  - **woocommerce**: The name of the channel.
    - **key/value pairs**: Credentials or settings needed for the channel.
  - **global**: Global parameters applicable to all channels.
- **fields**: Specifies which fields to retrieve or operate on.

---
## 2- Using Kariz Authenticator
We recommend using our authenticator when onboarding new users or adding users to new channels. This method creates a user in our database, and their credentials are automatically refreshed. Once you have sent them the authentication link and they have authenticated, you can always fetch their user data.
This method consists of two steps:
- 1- Create an Authentication Link for the User
- 2- Redirect the User Using the Authentication Link

### 2-1: Create an Authentication Link for the User
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

## 2-2: Redirect the User Using the Authentication Link
After obtaining the token, redirect your user to the following URL to complete the authentication process:
https://www.demo.trykariz.com/en/tpuser/authorize?token={token_from_step_1}

**User Experience**:\
Your users will see an authentication screen where they can authorize access to the specified applications. Once they have authenticated, you can run workflows on their behalf.

![user auth flow](..%2Fassets%2Ftp_user_auth_flow.png)
Once they have authenticated, you can run workflows for them.