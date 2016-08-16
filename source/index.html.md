---
title: API Reference

language_tabs:
- ruby
- shell

toc_footers:
- <a href='https://www.upcall.com'>< back to Upcall</a>

includes:
- errors

search: true
---

# Introduction

Welcome to the Upcall API beta! You can use the API to view and manage campaigns, numbers, calls and credits.

The Upcall API follows [JSON-API](http://jsonapi.org/) conventions.

## Getting started
1. Create an Upcall account and get your [API token](https://www.upcall.com/en/dashboard/api)
2. Add credit to your [account](https://www.upcall.com/en/companies/payment) and check out our [pricing](https://www.upcall.com/en/pricing)
3. Verify your outbound phone number (callerID), the `from_number` in a campaign.
4. Implement the API
5. You are done!

## Howto
### Call numbers with a script
1. [Create a campaign](#create-a-campaign)
2. [Add numbers](#add-a-number-to-a-campaign)
3. The numbers will be called within 24 hours (Mon-Fri).
4. [Verify the status of the calls and get results](#get-numbers-for-a-campaign)

# Authentication

> To authorize, pass in the `token` in the header:

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/campaigns
```

> Make sure to replace `YOUR_TOKEN` with your auth token.

Upcall uses auth tokens in the header to allow access to the API.

All requests to the API should include the `token` parameter.

# Campaigns

You can use the campaigns endpoints to create new campaigns, as well as manage and view your existing campaigns.

## Get All Campaigns

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns", {
  :params => {
    :language => "en",
  }
}, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/campaigns/123
```

> The above command returns JSON structured like this:

```json

{
  "data": [
  {
    "id": "876",
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "language": "en",
      "questions": [],
      "status": "pending",
      "kind": "alert_users",
      "always_open": true,
      "updated_at": "2016-07-18T10:49:18.000Z",
      "created_at": "2016-07-18T10:49:18.000Z",
      "start_date": "2016-07-11",
      "end_date": "2016-07-12",
      "instructions": "Call people and ask about product satisfaction",
      "from_number": "+12024561111"
    }
  },
  {
    "id": "877",
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "language": "en",
      "kind": "alert_users",
      "always_open": true,
      "questions": [
      {
        "id": 84,
        "question": "What is your favorite color?",
        "response_type": "multiple_radio",
        "explanations": "We'd like to know their favorite color in order to match the choices next time they're coming.",
        "has_comment": "true",
        "fields": [
        	"Blue",
        	"Green"
        ]
      }
      ],
      "status": "pending",
      "updated_at": "2016-07-18T10:49:18.000Z",
      "created_at": "2016-07-18T10:49:18.000Z",
      "start_date": "2016-07-11",
      "end_date": "2016-07-12",
      "instructions": "Call people and ask about product satisfaction",
      "from_number": "+12024561111"
    }
  }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/campaigns?page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/campaigns?page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/campaigns?page%5Bnumber%5D=11&page%5Bsize%5D=2"
  }
}

```

This endpoint retrieves all campaigns.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns`

### Query Parameters

Parameter |  Description
--------- |  -----------
name | Filter for campaigns based on their name.
language | Filter for campaigns based on their language. eg. `en` for English.
status | Filter for campaigns based on their status. Must be one of `pending`, `ready`, `paused`, or `completed`.
from_number | Filter for campaigns based the caller ID number they will be dialed from.
min_start_date | Minimum start date time, eg. `2016-07-18`
max_start_date | Maximum start date time, eg. `2016-07-18`
min_created_datetime | Minimum created date time, eg. `2016-07-18T10:49:18.000Z`
max_created_datetime | Maximum created date time, eg. `2016-07-18T10:49:18.000Z`
min_updated_datetime | Minimum updated date time, eg. `2016-07-18T10:49:18.000Z`
max_updated_datetime | Maximum updated date time, eg. `2016-07-18T10:49:18.000Z`

## Get a Specific Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/campaigns/1
```

> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "1",
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "language": "en",
      "questions": [
      {
        "id": 24,
        "question": "What is your favorite color?",
        "response_type": "multiple_checkbox",
        "explanations": "We'd like to know their favorite color in order to match the choices next time they're coming.",
        "has_comment": "true",
        "fields": [
            "red",
            "green",
            "blue",
            "black"
          ]
      }
      ],
      "status": "pending",
      "kind": "alert_users",
      "always_open": true,
      "updated_at": "2016-07-18T10:59:30.000Z",
      "created_at": "2016-07-18T10:59:30.000Z",
      "start_date": "2016-07-11",
      "end_date": "2016-07-12",
      "instructions": "Call people and ask about product satisfaction",
      "from_number": "+16501234567"
    }
  }
}
```

This endpoint retrieves a specific campaign.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns/:id`

## Create a Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "Call gym customers",
      "instructions" => "Call people and ask about them about their gym habits",
      "pitch" => "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "questions" => [
      {
        "question" => "What days are you going to the gym?",
        "response_type" => "multiple_checkbox",
        "explanations" => "Make sure to count all activities",
        "comment" => false,
        "fields" => [
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday"
        ]
      }
      ],
      "start_date" => "2016-07-11",
      "end_date" => "2016-07-12",
      "language" => "en",
      "kind" => "alert_users",
      "always_open": true,
      "from_number" => "+16501231234",
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/campaigns", campaign.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/campaigns \
-H "Authorization: Token token=YOUR_TOKEN"
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name" => "Call gym customers",
      "instructions" => "Call people and ask about them about their gym habits",
      "pitch" => "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "questions" => [
      {
        "question" => "What days are you going to the gym?",
        "response_type" => "multiple_checkbox",
        "explanations" => "Make sure to count all activities",
        "comment" => false,
        "fields" => [
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday"
        ]
      }
      ],
      "start_date" => "2016-07-11",
      "end_date" => "2016-07-12",
      "language" => "en",
      "kind" => "alert_users",
      "always_open" => true,
      "from_number" => "+16501231234",
    }
  }
}
EOF

```

> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "123",
    "type": "campaigns",
    "attributes": {
      "name": "Call gym customers",
      "instructions": "Call people and ask about them about their gym habits",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "questions": [
      {
        "question": "What days are you going to the gym?",
        "response_type": "multiple_checkbox",
        "explanations": "Make sure to count all activities",
        "comment": false,
        "fields": [
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday"
        ]
      }
      ],
      "start_date": "2016-07-11",
      "end_date": "2016-07-12",
      "language": "en",
      "kind": "alert_users",
      "always_open": true,
      "from_number": "+16501231234",
    }
  }
}
```

This endpoint creates a new campaign. The campaign will start (and be marked as `ready` automatically) when all required fields are present, numbers are added and there is enough credit on your account. It is also possible to create a campaign on Upcall.com and finish it using the API.


### HTTP Request

`POST https://api.upcall.com/api/v1/campaigns`

The expected data format is a JSON payload in the body.

<aside class="notice">
  A note about <code>from_number</code>: when creating a campaign, you must use a number that has been confirmed. These are available in your Upcall dashboard.
</aside>

Attribute | Required | Description
--------- | -------- | -----------
name | Yes | Name of the campaign.
from_number | Yes | Number used for the campaign.
language | Yes | Language of the campaign. eg. `en` for English. (English is the only supported language for now)
kind | Yes | Type of campaign. Must be one of `lead_gen`, `feedback`, `verify_info`, `inform_product`, `alert_users`, `inquire_biz`, `appt_setting`, `mystery_shopping`, `confirm_attendance`, `other`, `customer_support`, `market_research`
always_open | Yes | Keep the campaign open even when all numbers have been called. Must be one of `true` or `false`
instructions | Yes | Instructions for the Upcaller.
pitch | Yes | Introductory pitch read by the Upcaller while on the phone.
questions | No | Structured questions/data to read during the call.
question.question | No | Question to ask
question.response_type | No | Type of expected answer. Must be one of `multiple_radio` (radio buttons), `multiple_checkbox` (checkboxes), `freeform` (text box), `calendar` (calendar), `stars` (5-star rater)
question.fields | No | Fields for the multiple choice questions (radio and checkboxes).
question.explanations | No | Extra explanations for the Upcaller
question.comment | No | Add a comment box under each question for the Upcaller to comment. Must be one of `true`, `false`.
start_date | No | Start time for the campaign, eg. `2016-07-18`
end_date | No | End time for the campaign, eg. `2016-07-18`

## Update a Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "My Campaign",
      "instructions" => "Call people and ask about product satisfaction",
      "pitch" => "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "start_date" => "2016-07-11",
      "end_date" => "2016-07-12",
      "language" => "en",
      "always_open" => true,
      "kind": "alert_users",
      "from_number" => "+16501231234",
    }
  }
}

RestClient.patch "https://api.upcall.com/api/v1/campaigns/1", campaign.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X PATCH https://api.upcall.com/api/v1/campaigns/1 \
-H 'Content-Type: text/json; \
-H "Authorization: Token token=YOUR_TOKEN"
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "instructions": "Call people and ask about product satisfaction",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I am calling on behalf of [[company]]. How are you today?",
      "start_date": "2016-07-11",
      "end_date": "2016-07-12",
      "language": "en",
      "always_open": true,
      "kind": "alert_users",
      "from_number": "+16501231234"
    }
  }
}
EOF

```
This endpoint updates an existing campaign.


### HTTP Request

`PATCH https://api.upcall.com/api/v1/campaigns/:id`

The expected data format is a JSON payload in the body.

<aside class="notice">
  A note about <code>from_number</code>: when creating a campaign, you must use a number that has been confirmed. These area available in your Upcall dashboard.
</aside>

Attribute | Required | Description
--------- | -------- | -----------
name | Yes | Name of the campaign.
from_number | Yes | Number used for the campaign.
language | Yes | Language of the campaign. eg. `en` for English. (English is the only supported language for now)
kind | Yes | Type of campaign. Must be one of `lead_gen`, `feedback`, `verify_info`, `inform_product`, `alert_users`, `inquire_biz`, `appt_setting`, `mystery_shopping`, `confirm_attendance`, `other`, `customer_support`, `market_research`
always_open | Yes | Keep the campaign open even when all numbers have been called. Must be one of `true` or `false`
instructions | Yes | Instructions for the Upcaller.
pitch | Yes | Introductory pitch read by the Upcaller while on the phone.
questions | No | Structured questions/data to read during the call.
question.question | No | Question to ask
question.response_type | No | Type of expected answer. Must be one of `multiple_radio` (radio buttons), `multiple_checkbox` (checkboxes), `freeform` (text box), `calendar` (calendar), `stars` (5-star rater)
question.fields | No | Fields for the multiple choice questions (radio and checkboxes).
question.explanations | No | Extra explanations for the Upcaller
question.comment | No | Add a comment box under each question for the Upcaller to comment. Must be one of `true`, `false`.
start_date | No | Start time for the campaign, eg. `2016-07-18`
end_date | No | End time for the campaign, eg. `2016-07-18`

## Delete a Campaign

```ruby

RestClient.delete "https://api.upcall.com/api/v1/campaigns/54", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X DELETE "https://api.upcall.com/api/v1/campaigns/54" \
-H "Authorization: Token token=YOUR_TOKEN" \
-d @- << EOF
EOF

```

> The above command returns JSON structured like this:

```json

```

This endpoint deletes an existing campaign.

### HTTP Request

`DELETE https://api.upcall.com/api/v1/campaigns/:id`

## Get Numbers for a Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1/numbers", {
  :params => {
    :company => "Safeway",
  }
}, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/campaigns/1/numbers?company=Safeway
```

> The above command returns JSON structured like this:

```json

{
  "data": [
  {
    "id": "102",
    "type": "campaign_numbers",
    "attributes": {
      "status": "completed",
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "calls": [
      {
        "id": 2,
        "status": "completed",
        "note": "Great conversation!",
        "duration": 67,
        "cost": "0.58",
        "cost_currency": "USD",
        "datetime_called": "2016-07-21T11:20:53.000Z",
        "recording_urls": [
        "https://recordings.upcall.com/ajn383fb2u.mp3"
        ],
        "caller": {
          "id": 511,
          "name": "Maria Lopez",
          "profile_url": "http://www.upcall.com/en/callers/511"
        }
      }
      ],
      "questions": [
      {
        "id": 28,
        "response_type": "multiple_radio",
        "question": "What is your favorite color?",
        "explanations": "",
        "result": "0",
        "fields": [
        "blue"
        ],
        "comment": "Turquoise to be precise"
      }
      ]
    }
  },
  {
    "id": "103",
    "type": "campaign_numbers",
    "attributes": {
      "status": "untried",
      "first_name": "Janie",
      "last_name": "Smith",
      "title": "Director",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "janie@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Janie Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "calls": [],
      "questions": []
    }
  }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=10&page%5Bsize%5D=2"
  }
}
```

The campaign numbers endpoint retrieves numbers that are part of a campaign. It describes their current status in the campaign, in terms of whether they have been dialed successfuly, will be retried, etc.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns/:id/numbers`

### Query Parameters

Parameter |  Description
--------- |  -----------
status | Filter for status. Must be one of `untried`, `incompleted`, `dnc`, `failed`, `completed`, `voicemail`.
phone | Filter for a specific phone number. The `+` sign before the number should be omitted.
first_name | Filter for first name.
last_name | Filter for last name.
title | Filter for job title.
company | Filter for company.
email | Filter for e-mail address.
recipient | Filter for address recipient.
line1 | Filter for address line 1.
line2 | Filter for address line 2.
city | Filter for city.
region | Filter for region (equivalent to state in the United States).
postal_code | Filter for a postal code (equivalent to zip code in the United States).
country | Filter for a country.

## Get a Specific Number for a Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1/numbers/2", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/campaigns/1/numbers/2?company=Safeway
```

> The above command returns JSON structured like this:

```json

{
  "data": [
  {
    "id": "2",
    "type": "campaign_numbers",
    "attributes": {
      "status": "completed",
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "calls": [
      {
        "id": 2,
        "status": "completed",
        "note": "Great conversation!",
        "duration": 67,
        "cost": "0.58",
        "cost_currency": "USD",
        "datetime_called": "2016-07-21T11:20:53.000Z",
        "recording_urls": [
        "https://recordings.upcall.com/ajn383fb2u.mp3"
        ],
        "caller": {
          "id": 511,
          "name": "Maria Lopez",
          "profile_url": "http://www.upcall.com/en/callers/511"
        }
      }
      ],
      "questions": [
      {
        "id": 28,
        "response_type": "multiple_radio",
        "question": "What is your favorite color?",
        "explanations": "",
        "result": "0",
        "fields": [
        "blue"
        ],
        "comment": "Turquoise to be precise"
      }
      ]
    }
  }
  }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/campaigns/1/numbers?page%5Bnumber%5D=10&page%5Bsize%5D=2"
  }
}
```

The campaign number endpoint retrieves a number that is part of a campaign. It describes their current status in the campaign, in terms of whether they have been dialed successfuly, will be retried, etc.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns/:id/numbers/:id`


## Add a Number to a Campaign

```ruby

number = {
  "data" => {
    "type" => "numbers",
    "attributes" => {
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "custom_fields": {
        "color": "grey",
        "client_id", 12322
      }
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/campaigns/1/numbers", number.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/campaigns/1/numbers" \
-H "Authorization: Token token=YOUR_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaign_numbers",
    "attributes": {
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "custom_fields": {
        "color": "grey",
        "client_id", 12322
      }
    }
  }
}
```

Adds a number to a campaign. As long as the campaign is marked as `ready`, the number will be called within 24 hours (during business hours).

### HTTP Request

`POST https://api.upcall.com/api/v1/campaigns/:id/numbers`


> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "142",
    "type": "campaign_numbers",
    "attributes": {
      "status": "active",
      "first_name": "Sally",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sally@safeway.com",
      "phone": "+16501231234",
      "extension": null,
      "address": {
        "recipient": "Sally Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      },
      "custom_fields": {
        "color": "grey",
        "client_id", 12322
      }
    }
  }
}
```

## Update a Number in a Campaign

```ruby

number = {
  "data" => {
    "type" => "numbers",
    "attributes" => {
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      }
    }
  }
}

RestClient.patch "https://api.upcall.com/api/v1/campaigns/1/numbers/2", number.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X PATCH "https://api.upcall.com/api/v1/campaigns/1/numbers/2" \
-H "Authorization: Token token=YOUR_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaign_numbers",
    "attributes": {
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      }
    }
  }
}
```

Adds a number to the campaign. 

### HTTP Request

`POST https://api.upcall.com/api/v1/campaigns/:id/numbers`


> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "2",
    "type": "campaign_numbers",
    "attributes": {
      "status": "active",
      "first_name": "Joey",
      "last_name": "Smith",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "joey@safeway.com",
      "phone": "+16507991144",
      "extension": null,
      "address": {
        "recipient": "Joey Smith",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      }
    }
  }
}
```

## Delete a Number from a Campaign

```ruby

RestClient.delete "https://api.upcall.com/api/v1/campaigns/1/numbers/2", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/campaigns/1/numbers/2" \
-H "Authorization: Token token=YOUR_TOKEN" \
-H 'Content-Type: text/json;
```

Delete a number from the campaign. 

### HTTP Request

`DELETE https://api.upcall.com/api/v1/campaigns/:id/numbers/:number_id`


> The above command does not return anything

# Contacts

You can use the contacts endpoints to list, add and modify contacts in your contact lists. A contact list can be reused across campaigns and is a good way to organize numbers.

## Get All Contacts

```ruby
RestClient.get "https://api.upcall.com/api/v1/contacts", {
  :params => {
    :title => "Director",
  }
}, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/contacts/1?title=Director
```

> The above command returns JSON structured like this:

```json

{
  "data": [
  {
    "id": "281",
    "type": "contact",
    "attributes": {
      "list_id": 31,
      "first_name": "John",
      "last_name": "Smith",
      "title": "Director",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      }
    }
  },
  {
    "id": "282",
    "type": "contact",
    "attributes": {
      "list_id": 31,
      "first_name": "Name1",
      "last_name": "Contact",
      "title": "Director",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Fremont",
        "region": "California",
        "postal_code": "94536",
        "country": "United States"
      }
    }
  }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/contacts?page%5Bnumber%5D=1&page%5Bsize%5D=2&title=Director",
    "next": "https://api.upcall.com/api/v1/contacts?page%5Bnumber%5D=2&page%5Bsize%5D=2&title=Director",
    "last": "https://api.upcall.com/api/v1/contacts?page%5Bnumber%5D=3&page%5Bsize%5D=2&title=Director"
  }
}
```

This endpoint manipulates contacts in the contacts lists.

### HTTP Request

`GET https://api.upcall.com/api/v1/contacts`

### Query Parameters

Parameter | Description
--------- |  -----------
status | Filter for status. Must be one of `active` or `disabled`.
phone | Filter for a specific phone number.
extension | Filter for a specific phone extension.
first_name | Filter for first name.
last_name | Filter for last name.
title | Filter for job title.
company | Filter for company.
email | Filter for e-mail address.
recipient | Filter for address recipient.
line1 | Filter for address line 1.
line2 | Filter for address line 2.
city | Filter for city.
region | Filter for region (equivalent to state in the United States).
postal_code | Filter for a postal code (equivalent to zip code in the United States).
country | Filter for a country.


## Get a Specific Contact

```ruby
RestClient.get "https://api.upcall.com/api/v1/contacts/1", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/contacts/1
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "306",
    "type": "contact",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Mountain View",
        "region": "California",
        "postal_code": "94043",
        "country": "United States"
      }
    }
  }
}
```

This endpoint retrieves a specific contact.

### HTTP Request

`GET https://api.upcall.com/api/v1/contacts/:id`

## Create a Contact

```ruby

number = {
  "data" =>
  {
    "type" => "contacts",
    "attributes" =>
    {
      "campaign_id" => 1
      "first_name" => "Sam",
      "last_name" => "Contact",
      "title" => "Manager",
      "company" => "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email" => "sam@safeway.com",
      "status" => "active",
      "phone" => "+16507991234",
      "extension" => "123",
      "address" =>
      {
        "recipient" => "Sam Contact",
        "line1" => "100 Main Street",
        "line2" => "Building 500",
        "city" => "Mountain View",
        "region" => "California",
        "postal_code" => "94043",
        "country" => "United States"
      },
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/contacts", number.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/contacts" \
-H "Authorization: Token token=YOUR_TOKEN"
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "contacts",
    "attributes": {
      "campaign_id": 1
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Mountain View",
        "region": "California",
        "postal_code": "94043",
        "country": "United States"
      }
    }
  }
}
EOF

```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "contacts",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Mountain View",
        "region": "California",
        "postal_code": "94043",
        "country": "United States"
      }
    }
  }
}
```

This endpoint adds a new contact to an existing contact list.

<aside class="notice">
  Be aware that by adding a contact to a list that is actively being used in an open campaign will also add a number to this campaign.
</aside>


### HTTP Request

`POST https://api.upcall.com/api/v1/contacts`

The expected data format is a JSON payload in the body.

Attribute | Required | Description
--------- | -------- | -----------
list_id | Yes | The contact list, parent of this contact
phone | Yes | Phone number of the contact.
extension | No | Phone extension of the contact.
first_name | No | First name of the contact.
last_name | No | Last name of the contact.
title | No | Job title of the contact.
company | No | Company of the contact.
email | No | Email address of the contact.
address.recipient | No | Address recipient of the contact.
address.line1 | No | Address line 1 of the contact.
address.line2 | No | Address line 2 of the contact.
address.city | No | City of the contact.
address.region | No | Region of the contact (equivalent to state in the United States).
address.postal_code | No | Postal code of the contact (equivalent to zip code in the United States).
address.country | No | Country of the contact.

## Update a Contact

```ruby

number = {
  "data" =>
  {
    "type" => "contacts",
    "attributes" =>
    {
      "title" => "New Title",
      "company" => "New Company",
    }
  }
}

RestClient.patch "https://api.upcall.com/api/v1/contacts/1", number.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/contacts/1" \
-H "Authorization: Token token=YOUR_TOKEN"
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "contacts",
    "attributes": {
      "title": "New Title",
      "company": "New Company",
    }
  }
}
EOF

```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "contact",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "New Title",
      "info": "Some random info about the contact, visible to the Upcallers.",
      "company": "New Company",
      "email": "sam@safeway.com",
      "status": "active",
      "phone": "+16507991234",
      "extension": "123",
      "address": {
        "recipient": "Sam Contact",
        "line1": "100 Main Street",
        "line2": "Building 500",
        "city": "Mountain View",
        "region": "California",
        "postal_code": "94043",
        "country": "United States"
      }
    }
  }
}
```

This endpoint updates an existing contact.


### HTTP Request

`PATCH https://api.upcall.com/api/v1/contacts/:id`

The expected data format is a JSON payload in the body.

Attribute | Required | Description
--------- | -------- | -----------
list_id | Yes | The contact list, parent of this contact
phone | Yes | Phone number of the contact.
extension | No | Phone extension of the contact.
first_name | No | First name of the contact.
last_name | No | Last name of the contact.
title | No | Job title of the contact.
company | No | Company of the contact.
email | No | Email address of the contact.
address.recipient | No | Address recipient of the contact.
address.line1 | No | Address line 1 of the contact.
address.line2 | No | Address line 2 of the contact.
address.city | No | City of the contact.
address.region | No | Region of the contact (equivalent to state in the United States).
address.postal_code | No | Postal code of the contact (equivalent to zip code in the United States).
address.country | No | Country of the contact.

## Delete a Contact

```ruby

RestClient.delete "https://api.upcall.com/api/v1/contacts/1", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X DELETE "https://api.upcall.com/api/v1/contacts/1" \
-H "Authorization: Token token=YOUR_TOKEN" \
-H 'Content-Type: text/json; \
EOF

```

> The above command returns no response:

This endpoint updates an existing contact.


### HTTP Request

`DELETE https://api.upcall.com/api/v1/contacts/:id`

# Calls

## Get All Calls

```ruby
RestClient.get "https://api.upcall.com/api/v1/calls", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/calls
```

> The above command returns JSON structured like this:

```json
{
  "data": [
  {
    "id": "506",
    "type": "call",
    "attributes": {
      "note": "abc",
      "status": "completed",
      "duration": 30,
      "cost": "0.0",
      "campaign_number_id": 2470,
      "from": "+16507991234",
      "to": "+16508881100",
      "transcript": null,
      "datetime_called": "2016-07-21T10:45:36.000Z",
      "cost_currency": "EUR",
      "recording_urls": [
      "http://www.site.com/recording/1",
      "http://www.site.com/recording/2",
      "http://www.site.com/recording/3"
      ],
      "caller": {
        "id": 100,
        "name": "Billy Caller",
        "profile_url": "https://api.upcall.com/callers/100"
      }
    }
  },
  {
    "id": "507",
    "type": "call",
    "attributes": {
      "note": "abc",
      "status": "busy",
      "duration": 30,
      "cost": "0.0",
      "campaign_number_id": 2471,
      "from": "+16507991234",
      "to": "+16508881101",
      "transcript": null,
      "datetime_called": "2016-07-21T10:45:36.000Z",
      "cost_currency": "EUR",
      "recording_urls": [
      "http://www.site.com/recording/1",
      "http://www.site.com/recording/2",
      "http://www.site.com/recording/3"
      ],
      "caller": {
        "id": 100,
        "name": "Billy Caller",
        "profile_url": "https://api.upcall.com/callers/100"
      }
    }
  }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/calls?page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/calls?page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/calls?page%5Bnumber%5D=10&page%5Bsize%5D=2"
  }
}
```

The calls endpoint retrieves all calls.

### HTTP Request

`GET https://api.upcall.com/api/v1/calls`

### Query Parameters

Parameter |  Description
--------- |  -----------
status | Filter for status. Must be one of `completed`, `completed_fu_call`, `completed_fu_email`, `completed_fu_inperson`, `call_back_later`, `busy`, `no_answer`, `voicemail`, `hungup`, `not_interested`, `dnc`, `no_service`, `technical_issue`, `wrong_number`, `untried`.
to_number_id | Filter for calls to a specific number, by its ID.
campaign_id | Filter for a specific campaign, by its ID.
min_duration | Filter for minimum duration.
max_duration | Filter for maximum duration.
min_cost | Filter for minimum cost.
max_cost | Filter for maximum cost.

## Get a Specific Call

```ruby
RestClient.get "https://api.upcall.com/api/v1/calls/1", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/calls/1
```

> The above command returns JSON structured like this:

```json

{
  "id": "1",
  "type": "call",
  "attributes": {
    "note": "abc",
    "status": "busy",
    "duration": 30,
    "cost": "0.0",
    "campaign_number_id": 2471,
    "from": "+16507991234",
    "to": "+16508881101",
    "transcript": null,
    "datetime_called": "2016-07-21T10:45:36.000Z",
    "cost_currency": "EUR",
    "recording_urls": [
    "http://www.site.com/recording/1",
    "http://www.site.com/recording/2",
    "http://www.site.com/recording/3"
    ],
    "caller": {
      "id": 100,
      "name": "Billy Caller",
      "profile_url": "https://api.upcall.com/callers/100"
    }
  }
}

```

This endpoint retrieves a specific call.

### HTTP Request

`GET https://api.upcall.com/api/v1/calls/:id`


# Credits

You can use the credits endpoints to add new credits to your account, as well as to check your current balance.

<aside class="notice">
  You should first link a credit card with your account by going to your dashboard on Upcall.com
</aside>

## Get Current Balance

```ruby
RestClient.get "https://api.upcall.com/api/v1/credits", {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -H "Authorization: Token token=YOUR_TOKEN" https://api.upcall.com/api/v1/credits
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "credits",
    "attributes": {
      "currency": "USD",
      "pending_balance": 20
    }
  }
}
```

This endpoint retrieves the current credit balance on your account.

### HTTP Request

`GET https://api.upcall.com/api/v1/credits`

## Add Credit to your account

```ruby

credit = {
  "data" => {
    "type" => "credits",
    "attributes" => {
      "amount" => 10.00,
      "currency" => "USD",
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/credits", credit.to_json, {:Authorization => 'Token token=YOUR_TOKEN'}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/credits" \
-H "Authorization: Token token=YOUR_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "credits",
    "attributes": {
      "amount" => 10.00,
      "currency" => "USD",
    }
  }
}
EOF
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "credits",
    "attributes": {
      "currency": "USD",
      "credit_balance": 30
    }
  }
}
```

This endpoint adds a specified amount to your credit account.

<aside class="notice">
  Credits added in this manner will be charged to your credit card.
</aside>

### HTTP Request

`POST https://api.upcall.com/api/v1/credits`

Attribute | Required | Description
--------- | -------- | -----------
amount | Yes | Amount of money to add to the account. eg. `10`
currency | Yes | Currency unit for the amount. eg. `usd`
