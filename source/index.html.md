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
