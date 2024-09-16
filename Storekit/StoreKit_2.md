# StoreKit 2 (API)

## Products
A product represents a purchasable item that your app offers to users through in-app purchases. These products are defined and configures in App Store Connect and can include:
- **Consumables:** Items that can be purchased multiple times and are depleted with use, such as in-game currecny or extra lives.
- **Non-Consumables:** One-time purchases that provide permanent access to a feature or content, like premium upgrade or unlocking full version of an app.
- **Auto-Renewable Subscriptions:** Recurring subscription that automatically renew after a set period unless canceled by the user, such as monthly streaming services or news subscription.
- **Non-Renewing Subscriptions:**: Time-limited access to content or services that do not automatically renew, requiring the user to repurchase after expiration.

In StoreKit 2, the Product structure provides detailed information about each product, such as its price, localized title, description, and availability.

### How products is used in StoreKit 2

1 - Fetching Products: Use the Product.products(for:) async method to retrieve a list of products based on their identifiers.

```Swift
let productIds = ["com.example.app.consumable", "com.example.app.subscription"]
let products = try await Product.products(for: productIds)
```

2 - Displaying Product Information: Access properties like price, displayName, and description to present product details to the user.
```Swift
for product in products {
    print("Product Name: \(product.displayName)")
    print("Price: \(product.price)")
    print("Description: \(product.description)")
}
```
3 - Initiating Purchases: Call the purchase() method on a Product instance to start the purchase process.

```Swift
let result = try await product.purchase()
```

4 - Handling Purchase Results: Process the outcome of the purchase, whether it's successful, pending, or failed, and update the app's state accordingly.


### Request
### Purchase
#### Purchase options
- Quantity
- Promotional offer
- App account token

### Product type
### Subscription info
#### isElegibleForIntroOffer
### BackingValue



## Purchases
## Transaction info

## Transaction history
## Subscription status


References:


