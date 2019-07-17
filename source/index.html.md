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
  "message": "This is an error message"
}
```

All api requests give a response similar to what is shown on the right. If successful, the success property is set to true, and the items property is an array of objects.

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

`GET https://app-stage.slaybeautypass.com/api/customers`

## Add New Customer

This endpoint creates a customer with the email address of the authenticated user into the database.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/customers`

## Charge Customer

This endpoint charges the customer.

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/customers/charge`

### Body Parameters

| Parameter | Description                                   |
| --------- | --------------------------------------------- |
| credits   | The number of credits to charge the customer. |

## Update Customer

This endpoint updates the customer in the database.

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

### HTTP Request

`PUT https://app-stage.slaybeautypass.com/api/customers/update`

### Body Parameters

| Parameter | Description                                           |
| --------- | ----------------------------------------------------- |
| [Object]  | Object with the properties used to edit the customer. |

# Provider

## Get Providers

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/providers?service=mani&is_available=true&user_latitude=35.232352&user_longitude=-80.846744"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri =
  "https://app-stage.slaybeautypass.com/api/providers?service=mani&is_available=true&user_latitude=35.232352&user_longitude=-80.846744";
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
    "test_mode": false,
    "price_range": "$$",
    "tags": "Cocktail Bar, Free Parking, Pet Friendly",
    "uses_platform": false,
    // next two fields are only available if user_latitude & user_longitude are provided
    "distance_to_user": 5020,
    "distance_to_user_in_miles": 3.1
  }
]
```

This endpoint gets the list of providers from the database. If the user's coordinates are provided, the endpoint will sort the providers from closest to farthest.

<aside class="notice">
A platform refers to a software booking system that a provider potentially uses to manage their appointments. If we are integrated with them, the `uses_platform` field will be set to `true`.
</aside>

### HTTP Request

`GET https://app-stage.slaybeautypass.com/api/providers`

### Query Parameters

| Parameter      | Required | Description                                                                                      |
| -------------- | -------- | ------------------------------------------------------------------------------------------------ |
| service        | false    | The service (slug) to filter by.                                                                 |
| is_available   | false    | If set, only get providers with an active opening (filtered by service if service param is set.) |
| user_latitude  | false    | The latitude of the user (if set, must also provide longitude)                                   |
| user_longitude | false    | The longitude of the user (if set, must also provide latitude)                                   |

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
    "label": "Gel mani",
    "slug": "gel_mani",
    "default_time": 30
  },
  {
    "label": "Massage",
    "slug": "massage",
    "default_time": 60
  }
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
  `employee_id` and `employee_gender` should only be used for providers using a platform. Also, for providers using a platform, end_time is overwritten to 11:59 PM of the start date.
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
    "credits_used": 15,
    "service_name": "mani",
    "is_cancelled": false,
    "is_cancelled_confirmed": false,
    "status": "confirmed", // can also be "denied" or "pending"
    "number_of_times_called": 0,
    "platform_appointment_id": 0,
    "technician_first_name": "Sarah",
    "technician_last_name": "Brooks"
  }
]
```

> Note: platform_appointment_id, technician_first_name, and technician_last_name are only available if a provider is integrated with a platform.

This endpoint gets the list of appointments of the authenticated user from the database.

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
  },
  is_confirmed: "true"
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

| Parameter    | Required | Description                                                      |
| ------------ | -------- | ---------------------------------------------------------------- |
| provider_id  | true     | The id of the provider that the appointment is being booked for. |
| opening      | true     | The opening object that reflects the timing of the appointment   |
| service      | true     | The service object for the requested appointment.                |
| is_confirmed | false    | If set to true, makes the appointment confirmed automatically.   |

<aside class="notice">
  `is_confirmed` should be set to true for all appointments being made by providers using a platform.
</aside>

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

This endpoint cancel a booked appoinment.

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
  {
    "name": "Glow",
    "slug": "glow",
    "emoji": "",
    "servicesAmount": {
      "min": 2,
      "max": 3
    },
    "credits": 30,
    "price": 45,
    "hasSalesTax": false,
    "sales_tax_fee": 0,
    "credit_card_fee": 1.6529351184345984,
    "price_after_fees": 46.6529351184346,
    "plan_id": "plan_FPKK0QGkAfBOVK"
  }
]
```

This endpoint is used to get all the subscription tiers currently offered.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/levels`

## Get Subscription

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/subscriptions"
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/subscriptions;

const headers = {
  Authorization: "Bearer <id_token>"
}

axios.get(uri, data, headers);
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

`POST https://app-stage.slaybeautypass.com/api/subscriptions`

## Subscribing to a Beauty Pass

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://app-stage.slaybeautypass.com/api/subscriptions"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: Bearer <id_token>"
  -d '{"token": tok_1EuT6v2eZvKYlo2CHki1KX4, "plan_id": plan_DFpIzZ7mpBqUj0}'
```

```javascript
const axios = require("axios");

const uri = "https://app-stage.slaybeautypass.com/api/subscriptions;
const data = {
  token: "tok_1EuT6v2eZvKYlo2CHki1KX4",
  plan_id: "plan_DFpIzZ7mpBqUj0"
}
const headers = {
  Authorization: "Bearer <id_token>"
}

axios.post(uri, data, headers);
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

This endpoint is used to subscribe a user to a beauty pass subscriscription.

### HTTP Request

`POST https://app-stage.slaybeautypass.com/api/subscriptions`

### Body Parameters

| Parameter | Required | Description                                                    |
| --------- | -------- | -------------------------------------------------------------- |
| token     | true     | The stripe token corresponding to the user's debit/credit card |
| plan_id   | true     | The id of the plan the user is subscribing to                  |

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
