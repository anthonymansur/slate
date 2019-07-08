---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  # - errors

search: true
---

# Introduction

Welcome to the Slay Beauty Pass API. You can use our API to access Slay API endpoints, which can get and update information from our database.

We currently only have language bindings in shell and Javascript. You can view code examples in the dark area to the right.

If you have any questions, please feel free to contact Anthony at <anthony@slaybeautypass.com> or Daniel at <daniel@slaybeautypass.com>.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api_endpoint_here"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/your_endpoint_here";
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
  "message": "This is an error message"
}
```

All api requests give a response similar to what is shown on the right. If successful, the success property is set to true, and the items property is an array of objects.

If not successful, the success property is set to false, and the message property contains a strings with the error message.

# Customer

## Get Customer

```shell
curl "https://app.slaybeautypass.com/api/customers"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/customers";
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
    "email": "example@slaybeautypass.com",
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

This endpoint retrieves the logged in customer.

### HTTP Request

`GET https://app.slaybeautypass.com/api/customers`

## Add New Customer

This endpoint creates a customer with the email address of the authenticated user into the database.

### HTTP Request

`POST https://app.slaybeautypass.com/api/customers`

## Charge Customer

This endpoint charges the customer.

### HTTP Request

`PUT https://app.slaybeautypass.com/api/customers/charge`

### Body Parameters

| Parameter | Description                                   |
| --------- | --------------------------------------------- |
| credits   | The number of credits to charge the customer. |

## Update Customer

This endpoint updates the customer in the database.

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/customers/update"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"stripe_token":"cus_FO7hGruL0FKJPE"}'
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/customers/update";
const headers = {
  Authorization: "Bearer <id_token>"
};
const data = {
  stripe_token: "cus_FO7hGruL0FKJPE"
};

axios.put(uri, data, headers);
```

### HTTP Request

`PUT https://app.slaybeautypass.com/api/customers/update`

### Body Parameters

| Parameter | Description                                           |
| --------- | ----------------------------------------------------- |
| [Object]  | Object with the properties used to edit the customer. |

# Provider

## Get Providers

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/providers?service=mani"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/providers?service=mani";
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
    "services": [],
    "test_mode": false,
    "tier_level": "$$",
    "tags": ["Cocktail Bar", "Free Parking", "Pet Friendly"],
    "uses_platform": false,
    "platform_data": {
      "platform_id": "",
      "site_id": -1,
      "api_key": "",
      "admin_id": "",
      "admin_username": "",
      "admin_password": "",
      "access_token": "",
      "location_id": "",
      "services_mapping": [
        {
          "slug": "",
          "platform_service_id": -1
        }
      ]
    }
  }
]
```

This endpoint gets the list of providers from the database.

<aside class="notice">
A platform refers to a software booking system that a provider potentially uses to manage their appointments. If we are integrated with them, the `uses_platform` field will be set to `true`.
</aside>

### HTTP Request

`GET https://app.slaybeautypass.com/api/providers`

### Query Parameters

| Parameter | Required | Description                      |
| --------- | -------- | -------------------------------- |
| service   | false    | The service (slug) to filter by. |

# Service

## Get Services

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/services"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/services";
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
    "slug": "mani"
  },
  {
    "label": "Gel mani",
    "slug": "gel_mani"
  },
  {
    "label": "Brow wax",
    "slug": "brow_wax"
  }
]
```

This endpoint gets the list of services from the database.

### HTTP Request

`GET https://app.slaybeautypass.com/api/services`

# Openings

## Get Openings

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/openings"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/openings";
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
    "customer_id": "ObjectId(\"507f1f77bcf86cd799439011\")",
    "start_time": "2018-07-27 10:00:00.000",
    "end_time": "2018-07-27 11:00:00.000",
    "is_available": true
  }
]
```

This endpoint gets the list of openings from the database.

### HTTP Request

`GET https://app.slaybeautypass.com/api/openings`

## Get Opening Dates

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/opening_dates"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/opening_dates";
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

`GET https://app.slaybeautypass.com/api/opening_dates`

# Appointments

## Get Appointments

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/transactions"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/transactions;
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

This endpoint gets the list of appointments of the authenticated user from the database.

### HTTP Request

`GET https://app.slaybeautypass.com/api/transactions`

### Query Parameters

| Parameter     | default | Description                                                      |
| ------------- | ------- | ---------------------------------------------------------------- |
| get_denied    | false   | If set to true, also gets the appointments that have been denied |
| get_cancelled | false   | If set to true, also gets appointments that have been cancelled  |

## Book an Appointment

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/transactions"
  -X POST
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"provider_id":"507f1f77bcf86cd799439011",
      "opening_id": "507f1f77bcf86cd799439011",
      "service": "{"slug": "mani", "label": "Mani"}",
      "is_confirmed": "true"
      }'
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/transactions";
const data = {
  provider_id: "507f1f77bcf86cd799439011",
  opening_id: "507f1f77bcf86cd799439011",
  service: {
    slug: "mani",
    label: "Mani"
  },
  is_confirmed: "true"
};
const headers = {
  Authorization: "Bearer <id_token>"
};

axios.post(uri, data, headers);
```

This endpoint books an appoinment for the given parameters.

### HTTP Request

`POST https://app.slaybeautypass.com/api/transactions`

### Body Parameters

| Parameter    | Required | Description                                                       |
| ------------ | -------- | ----------------------------------------------------------------- |
| provider_id  | true     | The id of the provider that the appointment is being booked for.  |
| opening_id   | true     | The id of the opening that reflects the timing of the appointment |
| service      | true     | The service object for the requested appointment.                 |
| is_confirmed | false    | If set to true, makes the appointment confirmed automatically.    |

<aside class="notice">
  `is_confirmed` should be set to true for all appointments being made by providers using a platform.
</aside>

## Cancel Appointment

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.slaybeautypass.com/api/transactions/cancel"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"transaction_id": 507f1f77bcf86cd799439011}'
```

```javascript
const axios = require("axios");

const uri = "https://app.slaybeautypass.com/api/transactions;
const data = {
  transaction_id: "507f1f77bcf86cd799439011"
}
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.post(uri, data, headers);
```

This endpoint cancel a booked appoinment.

### HTTP Request

`PUT https://app.slaybeautypass.com/api/transactions`

### Body Parameters

| Parameter      | Required | Description                                 |
| -------------- | -------- | ------------------------------------------- |
| transaction_id | true     | The id of the booked appointment to cancel. |

<!--
### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->
