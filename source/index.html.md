---
title: Proxy Cart Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://github.com/CaliStyle/trailpack-proxy-engine'>Proxy Engine</a>

includes:
  - errors

search: true
---

# Introduction
Welcome to the Proxy Cart for Proxy Engine Docs! [Proxy Engine](https://github.com/CaliStyle/trailpack-proxy-engine) is an Open Source Node.js Framework created by the team at [Cali Style Technologies](https://cali-style.com).

## What is Proxy Cart?
Proxy Cart is the eCommerce component for [Proxy Engine](https://github.com/calistyle/trailpack-proxy-engine). Connect your own [Merchant Processor, Shipping Provider, Fulfillment Service, Tax Provider](https://github.com/calistyle/trailpack-proxy-generics), and import your products. Attach it to Proxy Engine and you have an REST API based eCommerce solution!

# Dependencies
## Proxy Engine
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-engine](https://github.com/calistyle/trailpack-proxy-engine) | [![Build status][ci-proxyengine-image]][ci-proxyengine-url] |

## Proxy Permissions
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-permissions](https://github.com/calistyle/trailpack-proxy-permissions) | [![Build status][ci-proxypermissions-image]][ci-proxypermissions-url] |

## Proxy Passport
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-passport](https://github.com/calistyle/trailpack-proxy-passport) | [![Build status][ci-proxypassport-image]][ci-proxypassport-url] |

## Proxy Notifications
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-notifications](https://github.com/calistyle/trailpack-proxy-notifications) | [![Build status][ci-proxynotifications-image]][ci-proxynotifications-url] |

## Supported ORMs
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-sequelize](https://github.com/calistyle/trailpack-proxy-sequelize) | [![Build status][ci-sequelize-image]][ci-sequelize-url] |

## Supported Webserver
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-express](https://github.com/trailsjs/trailpack-express) | [![Build status][ci-express-image]][ci-express-url] |

# Installation
To install Proxy Generics:

<code>
$ npm install trailpack-proxy-generics
</code>

If you are using Trail's yo generator:

<code>
$ yo trails:trailpack trailpack-proxy-generics
</code>

# Configuration
To configure Proxy Cart is simple, there are only 3 files to create/modify:

## config/main.js
> config/main.js

```javascript
module.exports = {
  packs: [
    // ... other trailpacks
    require('trailpack-proxy-cart')
    // ... other proxy-packs
  ]
}
```
Like all Trails' trailpacks, it must be included in the `config/main.js` file.


## config/web.js
> config/web.js
```javascript
module.exports = {
  /**
   * Middlewares to load (in order)
   * Add the Proxy Cart middleware after the passport middleware
   */
  middlewares: {

    //middlewares loading order
    order: [
      // ... other middleware
      'session',
      'passportInit', // <-- Handles Passport
      'passportSession', // <-- Handles Passport Session
      'proxyCartInit', // <-- Handles Cart Middleware start
      'proxyCartSession', // <-- Handles Session
      'proxyCartSessionCart', // <-- Handles Cart Session
      'proxyCartSessionCustomer', // <-- Handles Customer Session
      // ... trailing middleware
    ]
  }
}
```

## config/proxyCart.js
> config/proxyCart.js

```javascript
module.exports = {
  // The default Shop address (Nexus)
    nexus: {
      name: '<name>',
      host: '<host>',
      email: '<email>',
      address: {
        address_1: '<address 1>',
        address_2: '',
        address_3: '',
        company: '<company>',
        city: '<city>',
        province: '<full name of state/province>',
        country: '<full name of country>',
        postal_code: '<postal code>'
      }
    },
    // Allow certain events
    allow: {
      // Allows a product to be destroyed, Recommended false
      destroy_product: false,
      // Allows a product variant to be destroyed, Recommended false
      destroy_variant: false
    },
    // The default currency used
    default_currency: 'USD',
    // The countries to load by default
    default_countries: ['USA'],
    // Emails to send
    emails: {
      orderCreated: false,
      orderUpdated: false,
      orderPaid: false,
      orderFulfilled: false,
      orderRefunded: false,
      orderCancelled: false,
      sourceExpired: false,
      sourceWillExpire: false,
      sourceUpdated: false,
      subscriptionCreated: false,
      subscriptionUpdated: false,
      subscriptionActivated: false,
      subscriptionDeactivated: false,
      subscriptionCancelled: false,
      subscriptionWillRenew: false,
      subscriptionRenewed: false,
      transactionFailed: false
    },
    // Orders
    orders: {
      // Restock default for refunded order items
      refund_restock: false,
      // The default function for an automatic order payment: manual, immediate
      payment_kind: 'immediate',
      // the default function for transaction kind: authorize, sale
      transaction_kind: 'authorize',
      // The default function for an automatic order fulfillment: manual, immediate
      fulfillment_kind: 'manual',
      // The amount of times a Order will retry failed transactions
      retry_attempts: 5,
      // The amount of days before a Order will cancel from failed transactions
      grace_period_days: 5
    },
    // Subscriptions
    subscriptions: {
      // The amount of times a Subscription will retry failed transactions
      retry_attempts: 5,
      // The amount of days before a Subscription will cancel from failed transactions
      grace_period_days: 5
    },
    // Transactions
    transactions: {
      // The amount of times a Transaction will retry failed
      retry_attempts: 5,
      // The amount of days before a Transaction will cancel from failed
      authorization_exp_days: 5
    }
}
```
Also, Proxy Cart takes it's own config in the `config/proxyCart.js` file.


### Config Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------

# Definitions
## Shops
A shop represents a physical location that sells a product. When taxes and shipping are calculated per product, they are calculated by the nearest shop to the destination of an order. This means that the same product can be sold in multiple stores and shipped from different locations which may effect shipping and tax rates. They also track on hand inventory and inventory lead time.

## Products
A Product is a Physical or Digital item.

## Product Variants
A Product Variant is a variation of a product denoted by a unique Stock Keeping Unit (SKU)

## Product Association
A Product Association is a product that is associated to another product beyond the levels of a collection or tag.

## Product Images
A Product Image is an image that is associated directly with a product and sometimes a product variant.

## Product Reviews
A Product Review is input from a customer with a history of purchasing a product.

## Metadata
A Metadata is additional information about a product, customer, or review that is not constrained by the parent model.

## Collections
A Collection is a grouping of like items, such as products, customers, and shipping zones and can apply collection discounts, shipping overrides, and tax overrides.

## Tags
A Tag is a searching marker for a customer, product, or order.

## Customers
A Customer represents an account of one more users or guests.

## Users
A User is a registered user account with an username/email and password with given permission roles. Multiple users can share a single Customer account.

## Accounts
An Account is a 3rd party payment provider that the customer belongs to such as Stripe or Authorize.net, each customer can have multiple accounts

## Sources
A Source is a payment method used by the customer at checkout that belongs to a customer and a 3rd party Account.

## Carts
A Cart is a bucket that holds products and data until an order is placed.

## Orders
An Order is a bucket that holds products and data and the transactions of purchases and fulfillment.

## Fulfillment
A Fulfillment is a shipping transaction for an order.

## Fulfillment Event
A Fulfillment Event is the progress of a Fulfillment.

## Transactions
A Transaction is a representation of a purchasing event.

## Refunds
A Refund represents a transaction that has been completely or partially refunded.

## Gift Cards
A Gift Card is an alternate payment method that acts as a customer source.

## Discounts
A Discount is a value or percent off of a product, shipping cost, or total order applied by meeting criteria.

## Coupon
A redeemable discount that has a code.

## Subscriptions
A Subscription is the reoccurring of an order based on time period.

## Shipping Zones
A Shipping Zone is a geographical area that may override shipping and tax costs.

## Shipping Restrictions
A Shipping Restriction is a geographical restriction on the shipping of certain products.

## Vendors
Vendors are companies that distribute a product. In the case of drop shipping, the taxes and shipping are calculated from the vendor address to the customer.

## Events
Proxy Cart publishes many subscribable events using Proxy Engine's pub/sub.


[ci-sequelize-image]: https://img.shields.io/travis/trailsjs/trailpack-sequelize/master.svg?style=flat-square
[ci-sequelize-url]: https://travis-ci.org/trailsjs/trailpack-sequelize

[ci-proxyengine-image]: https://img.shields.io/circleci/project/github/CaliStyle/trailpack-proxy-engine/nmaster.svg
[ci-proxyengine-url]: https://circleci.com/gh/CaliStyle/trailpack-proxy-engine/tree/master

[ci-proxypassport-image]: https://img.shields.io/circleci/project/github/CaliStyle/trailpack-proxy-passport/nmaster.svg
[ci-proxypassport-url]: https://circleci.com/gh/CaliStyle/trailpack-proxy-passport/tree/master

[ci-proxypermissions-image]: https://img.shields.io/circleci/project/github/CaliStyle/trailpack-proxy-permissions/nmaster.svg
[ci-proxypermissions-url]: https://circleci.com/gh/CaliStyle/trailpack-proxy-permissions/tree/master

[ci-proxynotifications-image]: https://img.shields.io/circleci/project/github/CaliStyle/trailpack-proxy-notifications/nmaster.svg
[ci-proxynotifications-url]: https://circleci.com/gh/CaliStyle/trailpack-proxy-notifications/tree/master

[ci-express-image]: https://img.shields.io/travis/trailsjs/trailpack-express/master.svg?style=flat-square
[ci-express-url]: https://travis-ci.org/trailsjs/trailpack-express
