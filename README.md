# Adding integrations to your product 

Through our unified API you can connect your customers to new sales channels.
You can add all supported integrations to your product in minutes. 

For existing users, you can use their credentials you stored to fetch data through our platform in a unified format, removing the need for you to tranform the data or maintain the integrations.

# Data Models
Our data models are based on the Shopify data models. This means that if you already have a shopify integration. You can add all other sales channels to your application instantly.

# Onboarding Users with Kariz
Building new e-commerce integrations for your users with Kariz is easy. There are only two steps in the process:
1. Authenticate Your Users
2. Request Data from Sales Channels
---
## Step 1: [Authenticate Your Users](https://github.com/runkariz/docs/blob/main/authentication/README.md)
To authenticate users, there are two methods available:
- **Using Our Authenticator**\
Redirect users to our authenticator to securely authenticate them.
- **Using Existing User Credentials**\
Include user credentials in your API request when fetching user data.
---
## Step 2: Request Data from Sales Channels
After authenticating your users, you can request data from their sales channels.
### Supported Channels
We currently support the following sales channels:
- Shopify
- WooCommerce
- Amazon
- Bol.com
### Supported Data Models
We support the following e-commerce data models:
- [Orders](https://github.com/runkariz/docs/blob/main/order/README.md)
- [Products (including Inventory data)](https://github.com/runkariz/docs/blob/main/product/README.md)\
_Note_: All inventory information is included within the Products data.
---

## Data Models
Our data models are based on the Shopify data models to ensure consistency and ease of integration.
### Why Shopify Data Models?
Kariz uses the Shopify data models because they allow for easy adaptation to your internal data model, especially if you have already built an integration with Shopify. You can instantly add any of our new integrations to your product because the data model we provide is consistent across all channels.
### Modifications to the Shopify Data Models
We have made two minor changes to the Shopify data models:
1. **ID Fields as Strings**\
All ID fields are converted to strings to maintain consistency across different platforms.
2. **Added Sales_Channel Variable**\
A new variable called `channel_name` is added to indicate the source platform of the user data. For example, if the user data is fetched from Shopify, `channel_name = 'shopify'`.
---
## Orders Data Model
The order data from all supported channels is unified into a single data model based on the [Shopify Order API](https://shopify.dev/docs/api/admin-rest/2024-10/resources/order). For more information about how the data model works, please refer to the Shopify Order API Documentation.

---
## Products and Inventory Data Model
The product data, including inventory information, from all supported channels is unified into a single data model based on the Shopify Product API.
### Included Inventory Data
Our product model includes the inventory data for all product variants. This means that with one call, you receive:
- **All the products**
- **All their variants**
- **Their inventory**
- **Inventory levels split over multiple locations (if available for the channel)**\
For more information about the inventory data, refer to the Shopify inventory models:
- **Inventory Item: [InventoryItem API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/inventoryitem)**
- **Inventory Level: [InventoryLevel API Documentation](https://shopify.dev/docs/api/admin-rest/2024-10/resources/inventorylevel)**

---
## (Optional) Step 3: Map the Unified Data Model to Your Internal Model
After receiving the data from your users, you can map it onto your internal data model.
### If You Already Have a Shopify Integration
Mapping the data to your internal data model should be straightforward. Since the data models for orders, products, and inventory are based on Shopify's models, you can reuse your existing mappings. Keep in mind:
- We include the `channel_name` variable in the response to indicate the source platform of the data. You can use this to handle the data appropriately within your application.
- All ID fields are strings. See the Modifications to the Shopify Data Models section for more information.
### If You Don’t Have a Shopify Integration
You will need to map the Shopify data models onto your own data model. The advantage is that you only have to do this mapping once, and it will work across all supported sales channels.

---
## Supported Methods
We currently support the GET method for retrieving data.

---

