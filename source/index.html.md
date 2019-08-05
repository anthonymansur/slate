---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  # - errors

search: true
---

# Introduction

Welcome to the Slay Beauty Pass Public API. You can use our API to access Slay API endpoints, which can get and update information from our database.

We currently only have language bindings in shell and Javascript. You can view code examples in the dark area to the right.

If you have any questions, please feel free to contact Anthony at <anthony@slaybeautypass.com> or Daniel at <daniel@slaybeautypass.com>.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api_endpoint_here"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/your_endpoint_here";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> Make sure to replace `<id_token>` with the JWT.

Slay uses JSON Web Tokens (JWT) to allow access to our API. In order to get authenticated, you will need to log in through our Auth0 platform. Our Auth0 domain is `goslay.auth0.com`, our client ID is `QDErU26iSka57ucbwucwjJGEz69cD307`.

Once authenticated via Auth0, pass the jwt token to the HTTP header like the following:

`Authorization: Bearer <id_token>`

<aside class="notice">
You must replace <code>&lt;id_token&gt;</code> with your jwt token provided by Auth0.
</aside>

Every subsequent api call will now contain information of the authenticated user, such as the email address used to log in.

# Errors

> Sample success JSON structured like this:

```json
{
  "success": true,
  "items": []
}
```

> Sample error JSON structured like this:

```json
{
  "success": false,
  "message": "This is an error message."
}
```

All api requests give a response similar to what is shown on the right unless specified otherwise. If successful, the success property is set to true, and the items property is an array of objects.

If not successful, the success property is set to false, and the message property contains a strings with the error message.

# Customer

## Get Customer

```shell
curl "https://app-stage.slaybeautypass.com/api/customers"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/customers";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@slaybeautypass.com",
    "phone": "5555555555",
    "birthdate": "1999-01-31",
    "stripe_customer_id": "cus_FO7hGruL0FKJPE",
    "credits": 45,
    "personal_referrer_code": "JOHN-DVJ3",
    "personal_referrals": [],
    "promo_code_on_purchase": "SLAYEVERYDAY",
    "paid_invoices": [],
    "test_mode": false,
    "is_guest": false,
    "platform_client_id": []
  }
]
```

This endpoint retrieves the logged in customer from the database. If the customer doesn't exist, an empty array will be sent.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/customers`

## Add New Customer

```shell

curl "https://app-stage.slaybeautypass.com/api/customers"
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"first_name":"John", "last_name":"Doe", "phone":"5555555555", "promo_code_on_purchase":"SLAYEVERYDAY"}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/customers";

const data = {
  first_name: "John",
  last_name: "Doe",
  phone: "5555555555",
  promo_code_on_purchase: "SLAYEVERYDAY"
};

const headers = {
  Authorization: "Bearer <id_token>"
};

axios.post(uri, data, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@slaybeautypass.com",
    "phone": "5555555555",
    "birthdate": "",
    "credits": 0,
    "personal_referrer_code": "JOHN-DVJ3",
    "personal_referrals": [],
    "promo_code_on_purchase": "SLAYEVERYDAY",
    "paid_invoices": [],
    "test_mode": false,
    "is_guest": false
  }
]
```

This endpoint creates a customer with the email address of the authenticated user into the database.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/customers`

### Body Parameters

| Parameter              | Required |
| ---------------------- | -------- |
| first_name             | yes      |
| last_name              | yes      |
| phone                  | yes      |
| birthdate              | no       |
| promo_code_on_purchase | no       |
| is_guest               | no       |
| test_mode              | no       |


## Charge Customer

This endpoint charges the customer and returns the amount of money charged in USD.

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/customers/charge`

### Body Parameters

| Parameter | Description                                   |
| --------- | --------------------------------------------- |
| credits   | The number of credits to charge the customer. |

## Update Customer

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/customers/update"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"stripe_token":"cus_FO7hGruL0FKJPE"}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/customers/update";
const headers = {
  Authorization: "Bearer <id_token>"
};
const data = {
  stripe_token: "cus_FO7hGruL0FKJPE"
};

axios.put(uri, data, headers);
```

This endpoint updates the customer in the database.

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/customers/update`

### Body Parameters

| Parameter              |
| ---------------------- |
| first_name             |
| last_name              |
| birthdate              |
| phone                  |
| promo_code_on_purchase |


# Provider

## Get Providers

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/providers?service=mani&user_latitude=35.232352&user_longitude=-80.846744"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri =
  "https://app-stage.slaybeautypass.com/api/providers?service=mani&user_latitude=35.232352&user_longitude=-80.846744";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "salon_name": "Salon",
    "is_active": true,
    "is_instant_booking": true,
    "slug": "salon",
    "token": "3142",
    "stars": 4.5,
    "phone": "5555555555",
    "email": "salon@slaybeautypass.com",
    "address": "123 Main Street, City, State, 12345",
    "latitude": 42.3601,
    "longitude": 71.0589,
    "yelp_page": "yelp.com",
    "neighborhood": "south end",
    "metro_area": "Boston",
    "photo_url": "",
    "services": {
      "massage": {
        "label": "Massage",
        "credits": 40,
        "retail_price": 60,
        "payout": 10
      }
    },
    "services_meta": "Mani, Pedi, Massage",
    "time_zone": "America/New_York", // refer to moment-timezone
    "test_mode": false,
    "price_range": "$$",
    "tags": "Cocktail Bar, Free Parking, Pet Friendly",
    "uses_platform": false,
    // next two fields are only available if user_latitude & user_longitude are provided
    "distance_to_user": 5020,
    "distance_to_user_in_miles": 3.1
  }
  ...
]
```

This endpoint gets the list of providers from the database. If the user's coordinates are provided, the endpoint will sort the providers from closest to farthest.

<aside class="notice">
A platform refers to a software booking system that a provider potentially uses to manage their appointments. If we are integrated with them, the `uses_platform` field will be set to `true`.
</aside>

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/providers`

### Query Parameters

| Parameter      | Required | Description                                                    |
| -------------- | -------- | -------------------------------------------------------------- |
| service        | false    | The service (slug) to filter by.                               |
| user_latitude  | false    | The latitude of the user (if set, must also provide longitude) |
| user_longitude | false    | The longitude of the user (if set, must also provide latitude) |

# Service

## Get Services

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/services"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/services";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "label": "Mani",
    "slug": "mani",
    "default_time": 30
  },
  {
    "label": "Massage",
    "slug": "massage",
    "default_time": 60
  }
  ...
]
```

This endpoint gets the list of active services from the database.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/services`

# Openings

## Get Openings

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/openings?start_time=019-06-01T12:00:00-04:00&end_time=2019-06-01T12:30:00-04:00"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri =
  "https://app-stage.slaybeautypass.com/api/openings?start_time=019-06-01T12:00:00-04:00&end_time=2019-06-01T12:30:00-04:00";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "provider_id": "ObjectId(\"507f1f77bcf86cd799439011\")",
    "start_time": "2019-06-01T12:00:00-04:00",
    "end_time": "2019-06-01T12:30:00-04:00",
    "service_type": "mani",
    "employee": {
      "id": 100324,
      "gender": "female",
      "first_name": "Sarah",
      "last_name": "Brooks"
    }
  }
]
```

> The `employee` field is only seet for providers using a platform.

This endpoint gets the list of openings from the database.

For providers using a platform, there are many instances where an opening is associated with multiple employees. Unless specifically requested by the salon, the server outputs a random employee from the list of employees that meet the employee filter criteria every time the endpoint is called.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/openings`

### Query Parameters

| Parameter       | Required | Description                                                                                                   |
| --------------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| provider_id     | true     | The id to filter the openings by.                                                                             |
| service         | true     | The service (slug) to get the openings by.                                                                    |
| start_time      | false    | If set, only get openings that start at or after start_time. Otherwise, default is set to an hour from now.   |
| end_time        | false    | If set, only get openings that end at or before end_time. Otherwise, default is set to 11:59 PM of start_time |
| employee_id     | false    | If set, only get openings from the specified employee                                                         |
| employee_gender | false    | If set (to "male" or "female"), only get openings from employees with the specified gender.                   |

<aside class="notice">
  `employee_id` and `employee_gender` should only be used for providers using a platform. 
</aside>

## Get Opening Dates

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/opening_dates"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/opening_dates";
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
["2019-07-07T07:00:00.000Z", "2019-07-08T07:00:00.000Z", "2019-07-09T07:00:00.000Z"]
```

This endpoint gets the list of dates, up to 2 weeks starting from today, that have at least one opening for that given day.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/opening_dates`

# Appointments

## Get Appointments

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/transactions"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/transactions;
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "customer_id": "ObjectId(\"507f1f77bcf86cd799439011\")",
    "provider_id": "ObjectId(\"507f1f77bcf86cd799439011\")",
    "opening_id": "ObjectId(\"507f1f77bcf86cd799439011\")",
    "start_time": "2019-06-01T12:00:00-04:00",
    "end_time": "2019-06-01T12:30:00-04:00",
    "employee": {
      "id": 100324,
      "first_name": "Sarah",
      "last_name": "Brooks",
      "gender": "female"
    },
    "credits_used": 15,
    "service_name": "mani",
    "is_cancelled": false,
    "is_cancelled_confirmed": false,
    "status": "confirmed", // can also be "denied" or "pending"
    "number_of_times_called": 0,
    "platform_appointment_id": 0
  }
]
```

> Note: `platform_appointment_id` and `employee` are only available if a provider is integrated with a platform. `opening_id` is only available if a provider is not integrated with a platform.

This endpoint gets the list of appointments (in ascending order of the `start_time`) of the authenticated user from the database.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/transactions`

### Query Parameters

| Parameter     | default | Description                                                      |
| ------------- | ------- | ---------------------------------------------------------------- |
| get_denied    | false   | If set to true, also gets the appointments that have been denied |
| get_cancelled | false   | If set to true, also gets appointments that have been cancelled  |

## Book an Appointment

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/transactions"
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"provider_id":"507f1f77bcf86cd799439011",
      "opening": '{"_id": "ObjectId('507f1f77bcf86cd799439011')", "start_time": "2019-06-01T12:00:00-04:00", "end_time": "2019-06-01T12:30:00-04:00", "employee": {"id": 100324, "gender": "female", "first_name": "Sarah", "last_name":"Brooks"}}',
      "service": '{"slug": "mani", "label": "Mani"}',
      "is_confirmed": "true"
      }'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/transactions";
const data = {
  provider_id: "507f1f77bcf86cd799439011",
  opening: {
    _id: ObjectId("507f1f77bcf86cd799439011"),
    start_time: "2019-06-01T12:00:00-04:00",
    end_time: "2019-06-01T12:30:00-04:00",
    employee: {
      id: 100324,
      gender: "female",
      first_name: "Sarah",
      last_name: "Brooks"
    }
  },
  service: {
    slug: "mani",
    label: "Mani"
  }
};
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.post(uri, data, headers);
```

> Note: employee field should only be set for providers that use a platform.

This endpoint books an appoinment for the given parameters.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/transactions`

### Body Parameters

| Parameter                   | Required | Description                                                                  |
| --------------------------- | -------- | ---------------------------------------------------------------------------- |
| provider_id                 | true     | The id of the provider that the appointment is being booked for.             |
| opening                     | true     | The opening object that reflects the timing of the appointment.              |
| opening.\_id                | depends  | The id of the opening object (required if provider does not use a platform). |
| opening.start_time          | true     | The start time of the appointment.                                           |
| opening.end_time            | false    | The end time of the appointment.                                             |
| opening.employee            | false    | The employee information.                                                    |
| opening.employee.id         | true     | The id of the employee.                                                      |
| opening.employee.gender     | true     | The gender of the employee ("male" or "female").                             |
| opening.employee.first_name | true     | The first name of the employee.                                              |
| opening.employee.last_name  | true     | The last name of the employee.                                               |
| service                     | true     | The service object for the requested appointment.                            |
| service.slug                | true     | The service identifier.                                                      |
| service.label               | true     | The service name.                                                            |

## Cancel Appointment

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/transactions/cancel"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"transaction_id": 507f1f77bcf86cd799439011}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/transactions;
const data = {
  transaction_id: "507f1f77bcf86cd799439011"
}
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.post(uri, data, headers);
```

This endpoint cancels a booked appoinment and refunds the user the credits used to book the appointment. If the cancellation is within 12 hours, charges the user \$20.

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/transactions`

### Body Parameters

| Parameter      | Required | Description                                 |
| -------------- | -------- | ------------------------------------------- |
| transaction_id | true     | The id of the booked appointment to cancel. |

# Subscriptions

## Get all Subscriptions Tiers

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/levels"
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/levels;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, data, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "name": "Essentials",
    "slug": "essentials",
    "emoji": "",
    "servicesAmount": {
      "min": 1,
      "max": 2
    },
    "credits": 20,
    "price": 30,
    "hasSalesTax": false,
    "sales_tax_fee": 0,
    "credit_card_fee": 1.2049433573635433,
    "price_after_fees": 31.204943357363543,
    "plan_id": "plan_FPKJhDDIPntG71"
  },
  ...
]
```

This endpoint is used to get all the subscription tiers currently offered.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/levels`

## Get Subscription

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/subscriptions"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/subscriptions;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "name": "Flawless",
    "slug": "flawless",
    "emoji": "",
    "servicesAmount": {
      "min": 5,
      "max": 7
    },
    "credits": 60,
    "price": 90,
    "hasSalesTax": false,
    "sales_tax_fee": 0,
    "credit_card_fee": 2.9969104016477814,
    "price_after_fees": 92.99691040164778,
    "plan_id": "plan_FPKKSr3GeDLEeK",
    "created": "2019-07-13T13:38:26-04:00",
    "current_period_start": "2019-08-13T13:38:26-04:00",
    "current_period_end": "2019-08-13T13:38:26-04:00"
  }
]
```

This endpoint is used to get the subscription tier of the logged in user.

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/subscriptions`

## Subscribing to a Beauty Pass

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/subscriptions"
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"token": "tok_1EuT6v2eZvKYlo2CHki1KX4", "subscription": "flawless"}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/subscriptions;
const data = {
  token: "tok_1EuT6v2eZvKYlo2CHki1KX4",
  subscription: "flawless"
}
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.post(uri, data, headers);
```

This endpoint is used to subscribe a user to a beauty pass subscriscription.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/subscriptions`

### Body Parameters

| Parameter    | Required | Description                                            |
| ------------ | -------- | ------------------------------------------------------ |
| subscription | true     | The slug of the subscription the user is enrolling in. |

## Changing into a different Beauty Pass

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/subscriptions"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"subscription": "glam"}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/subscriptions;
const data = {
  plan_id: "glam"
}
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.put(uri, data, headers);
```

This endpoint is used to change the subscription of the user.

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/subscriptions`

### Body Parameters

| Parameter    | Required | Description                                                    |
| ------------ | -------- | -------------------------------------------------------------- |
| subscription | true     | The slug of the subscription the user is enrolling in          |


## Get Charges

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/charges"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/charges;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
    {
      "id": "ch_1CEPtWKyYYKKyjlMUd2fYjgA",
      "object": "charge",
      "amount": 11602,
      "amount_refunded": 0,
      "application": null,
      "application_fee": null,
      "application_fee_amount": null,
      "balance_transaction": "txn_1F359pKyYYKKyjlM6CoSQlRS",
      "billing_details": {
        "address": {
          "city": null,
          "country": null,
          "line1": null,
          "line2": null,
          "postal_code": "33069",
          "state": null
        },
        "email": null,
        "name": "Slay Web Application",
        "phone": null
      },
      "captured": true,
      "created": 1523141546,
      "currency": "usd",
      "customer": "cus_CdhIgTIGkSLmqD",
      "description": null,
      "destination": null,
      "dispute": null,
      "failure_code": null,
      "failure_message": null,
      "fraud_details": {},
      "invoice": "in_1CEPtWKyYYKKyjlMOU2tjexd",
      "livemode": false,
      "metadata": {},
      "on_behalf_of": null,
      "order": null,
      "outcome": {
        "network_status": "approved_by_network",
        "reason": null,
        "risk_level": "normal",
        "seller_message": "Payment complete.",
        "type": "authorized"
      },
      "paid": true,
      "payment_intent": null,
      "payment_method": "card_1CEPtUKyYYKKyjlMGXpm64kl",
      "payment_method_details": {
        "card": {
          "brand": "visa",
          "checks": {
            "address_line1_check": null,
            "address_postal_code_check": "pass",
            "cvc_check": "pass"
          },
          "country": "US",
          "exp_month": 1,
          "exp_year": 2041,
          "fingerprint": "pA0S4U5EGxGJykun",
          "funding": "credit",
          "last4": "4242",
          "three_d_secure": null,
          "wallet": null
        },
        "type": "card"
      },
      "receipt_email": null,
      "receipt_number": null,
      "receipt_url": "https://pay.stripe.com/receipts/acct_1BvF4HKyYYKKyjlM/ch_1CEPtWKyYYKKyjlMUd2fYjgA/rcpt_EGvdwOtBTelnB8Cxzf1p5x64iip4Fzo",
      "refunded": false,
      "refunds": {
        "object": "list",
        "data": [],
        "has_more": false,
        "total_count": 0,
        "url": "/v1/charges/ch_1CEPtWKyYYKKyjlMUd2fYjgA/refunds"
      },
      "review": null,
      "shipping": null,
      "source": {
        "id": "card_1CEPtUKyYYKKyjlMGXpm64kl",
        "object": "card",
        "address_city": null,
        "address_country": null,
        "address_line1": null,
        "address_line1_check": null,
        "address_line2": null,
        "address_state": null,
        "address_zip": "33069",
        "address_zip_check": "pass",
        "brand": "Visa",
        "country": "US",
        "customer": "cus_CdhIgTIGkSLmqD",
        "cvc_check": "pass",
        "dynamic_last4": null,
        "exp_month": 1,
        "exp_year": 2041,
        "fingerprint": "pA0S4U5EGxGJykun",
        "funding": "credit",
        "last4": "4242",
        "metadata": {},
        "name": "Slay Web Application",
        "tokenization_method": null
      },
      "source_transfer": null,
      "statement_descriptor": null,
      "status": "succeeded",
      "transfer_data": null,
      "transfer_group": null
    },
    ...
  ]
```
This endpoint is used to get all the charges of a user (limit 100).

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/charges`


## Get Invoices

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/invoices"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/invoices;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, headers);
```

> The above command returns JSON structured like this:

```json
[
    {
      "id": "in_1CEPtWKyYYKKyjlMOU2tjexd",
      "object": "invoice",
      "account_country": "US",
      "account_name": "Slay",
      "amount_due": 11602,
      "amount_paid": 11602,
      "amount_remaining": 0,
      "application_fee_amount": null,
      "attempt_count": 1,
      "attempted": true,
      "auto_advance": false,
      "billing": "charge_automatically",
      "billing_reason": null,
      "charge": "ch_1CEPtWKyYYKKyjlMUd2fYjgA",
      "collection_method": "charge_automatically",
      "created": 1523141546,
      "currency": "usd",
      "custom_fields": null,
      "customer": "cus_FZJQXo0fY7oQm7",
      "customer_address": null,
      "customer_email": "daniel@goslay.com",
      "customer_name": null,
      "customer_phone": null,
      "customer_shipping": null,
      "customer_tax_exempt": "none",
      "customer_tax_ids": [],
      "default_payment_method": null,
      "default_source": null,
      "default_tax_rates": [],
      "description": null,
      "discount": null,
      "due_date": null,
      "ending_balance": 0,
      "footer": null,
      "hosted_invoice_url": "https://pay.stripe.com/invoice/invst_9jRWzn3Uw4n6awIppFsCLBnmSv",
      "invoice_pdf": "https://pay.stripe.com/invoice/invst_9jRWzn3Uw4n6awIppFsCLBnmSv/pdf",
      "lines": {
        "data": [
          {
            "id": "sli_2f26e060e57d81",
            "object": "line_item",
            "amount": 11602,
            "currency": "usd",
            "description": "1 × Slay Test Pricing Plans (at $116.02 / month)",
            "discountable": true,
            "livemode": false,
            "metadata": {},
            "period": {
              "end": 1528411946,
              "start": 1525733546
            },
            "plan": {
              "id": "plan_CdhBXNvCQBGDWG",
              "object": "plan",
              "active": true,
              "aggregate_usage": null,
              "amount": 11602,
              "billing_scheme": "per_unit",
              "created": 1523141175,
              "currency": "usd",
              "interval": "month",
              "interval_count": 1,
              "livemode": false,
              "metadata": {},
              "nickname": "Deprecated – Fierce",
              "product": "prod_CdhAyv6SbKnYyI",
              "tiers": null,
              "tiers_mode": null,
              "transform_usage": null,
              "trial_period_days": null,
              "usage_type": "licensed"
            },
            "proration": false,
            "quantity": 1,
            "subscription": "sub_CdhIYBYaCucjdU",
            "subscription_item": "si_CdhIxHl7AKbmS5",
            "tax_amounts": [],
            "tax_rates": [],
            "type": "subscription"
          }
        ],
        "has_more": false,
        "object": "list",
        "url": "/v1/invoices/in_1CEPtWKyYYKKyjlMOU2tjexd/lines"
      },
      "livemode": false,
      "metadata": {},
      "next_payment_attempt": null,
      "number": "B642C00-0001",
      "paid": true,
      "payment_intent": null,
      "period_end": 1523141546,
      "period_start": 1523141546,
      "post_payment_credit_notes_amount": 0,
      "pre_payment_credit_notes_amount": 0,
      "receipt_number": null,
      "starting_balance": 0,
      "statement_descriptor": null,
      "status": "paid",
      "status_transitions": {
        "finalized_at": 1523141546,
        "marked_uncollectible_at": null,
        "paid_at": 1523141546,
        "voided_at": null
      },
      "subscription": "sub_CdhIYBYaCucjdU",
      "subtotal": 11602,
      "tax": null,
      "tax_percent": null,
      "total": 11602,
      "total_tax_amounts": [],
      "webhooks_delivered_at": 1523141547
    },
    ...
  ]
```
This endpoint is used to get all the invoices of a user (limit 10).

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/invoices`

# Platforms

## Get Employees

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/platforms/technicians"
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/platforms/technicians;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, data, headers);
```

> The above command returns JSON structured like this:

```json
[
  {
    "firstName": "Sarah",
    "lastName": "Brooks",
    "gender": "female",
    "id": 100324
  },
  {
    "firstName": "John",
    "lastName": "Doe",
    "gender": "Male",
    "id": 100465
  }
]
```

This endpoint is used to get the employees of a specified provider that uses a platform

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/platforms/technicians`

### Body Parameters

| Parameter   | Required | Description                                                                              |
| ----------- | -------- | ---------------------------------------------------------------------------------------- |
| provider_id | true     | The id of the provider to get the employees from                                         |
| service     | false    | The service slug to filter employees that have an availability for the specified service |

<!--
### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->
