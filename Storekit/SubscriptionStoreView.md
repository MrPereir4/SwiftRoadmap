# SubscriptionStoreView
Provides a complete subscription management flow in a single line of code.

```swift
import SwiftUI
import StoreKit

struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1")
    }
}
```

It only needs the subscription group id to work. And it ll completely handle the subscription flow, including loading and purchasing products.

## Customizing list os subscription
You can provide a list os products identifiers for every subscription you want to include.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(productIDs: ["123456789", "987654321"])
    }
}
```
> **Note:** SubscriptionStoreView only handles the subscriptions in the same group.

## Visible relationships
You can show the user the relationshipt it have for each of the subscription options. By default, it displays all avaliable actions, but you can controle the availability of the actions by using the visibleRelationships parameter.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all)
    }
}
```

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .current)
    }
}
```

### Possible values for visibleRelationships
+ .all: Shows all possible subscriptions available, regardless of the user's current subscription status.
+ .upgrade: Displays only the subscription options that are upgrades relative to the user’s current subscription.
+ .crossgrade: Shows only the subscriptions considered a crossgrade, which are changes within the same level of service but may include variations in duration or other terms.

## Styling
By default SubscriptionStoreView uses the app icon and the title of the subcription group as additionall content to display above the list of subcriptions.

But you can quickly provide a super custom view to vbe displayed at the top.

```swit
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all) {
            MyMarketingContentView()
        }
    }
}
```

You can customize the background of the SubscriptionStoreView by using the brand-new containerBackground view modifier. It allows us to place a SwiftUI as a background of the SubscriptionStoreView.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all) {
            MyMarketingContentView()
                .containerBackground(Color.blue, for: .subscriptionStoreHeader)
                .containerBackground(Color.red, for: .subscriptionStoreFullHeight)
        }
    }
}
```

Another styling option SubscriptionStoreView provides us is the control style. By default, it uses pickers, but we can easily change it to use buttons instead by using the subscriptionStoreControlStyle view modifier.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all)
            .subscriptionStoreControlStyle(.buttons)
    }
}
```

We also have the subscriptionStoreButtonLabel view modifier allowing us to customize store buttons. When you use pickers in your subscription store, the subscriptionStoreButtonLabel view modifier affects only a single button that calls to action.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all)
            .subscriptionStoreButtonLabel(.multiline)
    }
}
```

** Use Cases
To handle the user interaction with the options for subscription available, we can use the _onInAppPurchaseCompletion_. This returns a object _Product_ and _Result_ right after the users confirm the subscription purchase.

```swift
struct ContentView: View {
    var body: some View {
        SubscriptionStoreView(groupID: "598392E1", visibleRelationships: .all)
            .onInAppPurchaseCompletion { product, result in
                //Handles the purchase completion
            }
    }
}

```

References:

https://swiftwithmajid.com/2023/08/23/mastering-storekit2-subscriptionstoreview-in-swiftui/
https://developer.apple.com/documentation/storekit/subscriptionstoreview/4248752-init

