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

Welcome to the Upcall API! You can use the API to view and manage campaigns, numbers, and calls.

The Upcall API follows [JSON-API](http://jsonapi.org/) conventions.

## How to get started
1. Create an Upcall account and get your [API token](https://www.upcall.com/en/account/api)
2. Add credit to your [account](https://www.upcall.com/en/account) and check out our [pricing](https://www.upcall.com/en/pricing)
3. Verify your outbound phone number (callerID)
4. Implement the API
5. You are done!

## Example
1. Create a campaign
2. Add numbers

# Authentication

> To authorize, pass in the `auth_token` parameter:

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN"
```

> Make sure to replace `AUTH_TOKEN` with your auth token.

Upcall uses auth tokens in the URL to allow access to the API.

All requests to the API should include the `auth_token` query parameter.

# Campaigns

You can use the campaigns endpoints to create new campaigns, as well as manage and view your existing campaigns.

## Get All Campaigns

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :language => "en",
  }
}
```

```shell
curl "https://api.upcall.com/api/v1/campaigns/123?auth_token=AUTH_TOKEN&language=en"
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
        "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
        "language": "en",
        "questions": [],
        "status": "pending",
        "updated_at": "2016-07-18T10:49:18.000Z",
        "created_at": "2016-07-18T10:49:18.000Z",
        "start_datetime": "2016-07-11T00:04:58.000Z",
        "end_datetime": "2016-07-12T00:04:58.000Z",
        "instructions": "Call people and ask about product satisfaction",
        "from_number": "+12024561111"
      }
    },
    {
      "id": "877",
      "type": "campaigns",
      "attributes": {
        "name": "My Campaign",
        "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
        "language": "en",
        "questions": [
	      {
	      	"question": "What is your favorite color?",
	      	"response_type": "freeform",
	      	"explanations": "We'd like to know their favorite color in order to match the choices next time they're coming.",
	      	"has_comment": "true"
	      }
	     ],
        "status": "pending",
        "updated_at": "2016-07-18T10:49:18.000Z",
        "created_at": "2016-07-18T10:49:18.000Z",
        "start_datetime": "2016-07-11T00:04:58.000Z",
        "end_datetime": "2016-07-12T00:04:58.000Z",
        "instructions": "Call people and ask about product satisfaction",
        "from_number": "+12024561111"
      }
    }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=11&page%5Bsize%5D=2"
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
language | Filter for campaigns based on their language.
status | Filter for campaigns based on their status. Must be one of `pending`, `ready`, `paused`, or `completed`.
from_number | Filter for campaigns based the caller ID number they will be dialed from.
min_start_datetime | Minimum start date time, eg. `2016-07-18T10:49:18.000Z`
max_start_datetime | Maximum start date time, eg. `2016-07-18T10:49:18.000Z`
min_created_datetime | Minimum created date time, eg. `2016-07-18T10:49:18.000Z`
max_created_datetime | Maximum created date time, eg. `2016-07-18T10:49:18.000Z`
min_updated_datetime | Minimum updated date time, eg. `2016-07-18T10:49:18.000Z`
max_updated_datetime | Maximum updated date time, eg. `2016-07-18T10:49:18.000Z`

## Get a Specific Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "https://api.upcall.com/api/v1/campaigns/1?auth_token=AUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "1",
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "language": "en",
      "questions": [
      	{
      		"question": "What is your favorite color?",
      		"response_type": "freeform",
      		"explanations": "We'd like to know their favorite color in order to match the choices next time they're coming.",
      		"has_comment": "true"
      	}
      ],
      "status": "pending",
      "updated_at": "2016-07-18T10:59:30.000Z",
      "created_at": "2016-07-18T10:59:30.000Z",
      "start_datetime": "2016-07-11T00:04:58.000Z",
      "end_datetime": "2016-07-12T00:04:58.000Z",
      "instructions": "Call people and ask about product satisfaction",
      "from_number": "+16501234567"
    }
  }
}
```

This endpoint retrieves a specific campaign.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns/:id`

## Get Numbers for a Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1/numbers", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :company => "Safeway",
  }
}
```

```shell
curl "https://api.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN&company=Safeway"
```

> The above command returns JSON structured like this:

```json

{
  "data": [
    {
      "id": "102",
      "type": "campaign_numbers",
      "attributes": {
        "status": "active",
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
    },
    {
      "id": "103",
      "type": "campaign_numbers",
      "attributes": {
        "status": "active",
        "first_name": "Janie",
        "last_name": "Smith",
        "title": "Director",
        "company": "Safeway",
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
        }
      }
    }
  ],
  "links": {
    "self": "https://api.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=10&page%5Bsize%5D=2"
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


## Add a Number to a Campaign

```ruby

number = {
  "data" => {
    "type" => "numbers",
    "attributes" => {
      "list_contact_id" => 245,
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/campaigns/1/numbers", number.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "list_contact_id": 245,
    }
  }
}
```

Adds a number to the campaign. Note that this endpoint only works on numbers you have already [added to a contact list](#numbers).

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
      }
    }
  }
}
```

## Get Calls for a Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/campaigns/1/numbers/calls", {
  :params => {
    :auth_token => "AUTH_TOKEN",
  }
}
```

```shell
curl "https://api.upcall.com/api/v1/campaigns/1/numbers/calls?auth_token=AUTH_TOKEN"
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
    "self": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=10&page%5Bsize%5D=2"
  }
}
```

The campaign calls endpoint retrieves all calls for a campaign.

### HTTP Request

`GET https://api.upcall.com/api/v1/campaigns/:id/numbers/calls`

### Query Parameters

Parameter |  Description
--------- |  -----------
status | Filter for status. Must be one of `completed`, `completed_fu_call`, `completed_fu_email`, `completed_fu_inperson`, `call_back_later`, `busy`, `no_answer`, `voicemail`, `hungup`, `not_interested`, `dnc`, `no_service`, `technical_issue`, `wrong_number`, `untried`.
to_number_id | Filter for calls to a specific number, by it's ID.
min_duration | Filter for minimum duration.
max_duration | Filter for maximum duration.
min_cost | Filter for minimum cost.
max_cost | Filter for maximum cost.


## Create a Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "My Campaign",
      "instructions" => "Call people and ask about product satisfaction",
      "pitch" => "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "questions" => [
      	{
      	"question" => "When are you available for an appointment?",
      	"response_type" => "freeform",
      	"explanations" => "",
      	"comment" => false
      	}
      ],
      "start_datetime" => "2016-07-11T00:04:58Z",
      "end_datetime" => "2016-07-12T00:04:58Z",
      "language" => "en",
      "status" => "pending",
      "from_number" => "+16501231234",
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "instructions": "Call people and ask about product satisfaction",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "start_datetime": "2016-07-11T00:04:58Z",
      "end_datetime": "2016-07-12T00:04:58Z",
      "language": "en",
      "status": "pending",
      "from_number": "+16501231234"
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
      "name": "My Campaign",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "questions": [],
      "language": "en",
      "status": "pending",
      "updated_at": "2016-07-18T10:59:30.000Z",
      "created_at": "2016-07-18T10:59:30.000Z",
      "start_datetime": "2016-07-11T00:04:58.000Z",
      "end_datetime": "2016-07-12T00:04:58.000Z",
      "instructions": "Call people and ask about product satisfaction"
    }
  }
}
```

This endpoint creates a new campaign.


### HTTP Request

`POST https://api.upcall.com/api/v1/campaigns`

The expected data format is a JSON payload in the body.

<aside class="notice">
A note about <code>from_number</code>: when creating a campaign, you must use a number that has been confirmed. These are available in your Upcall dashboard.
</aside>

Attribute |  Description
--------- |  -----------
name | Name of the campaign.
instructions | Instructions for the caller.
pitch | Pitch for the campaign.
start_datetime | Start time for the campaign, eg. `2016-07-18T10:59:30.000Z`
end_datetime | End time for the campaign, eg. `2016-07-18T10:59:30.000Z`
language | Language of the campaign.
status | Status of the campaign. Must be one of `pending`, `ready`, `paused`, or `completed`.
from_number | Number used for the campaign.

## Update a Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "My Campaign",
      "instructions" => "Call people and ask about product satisfaction",
      "pitch" => "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "start_datetime" => "2016-07-11T00:04:58Z",
      "end_datetime" => "2016-07-12T00:04:58Z",
      "language" => "en",
      "status" => "pending",
      "from_number" => "+16501231234",
    }
  }
}

RestClient.patch "https://api.upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X PATCH "https://api.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "instructions": "Call people and ask about product satisfaction",
      "pitch": "Hi [[prospect_name]], My name is [[caller_name]] and I'm calling on behalf of [[company]]. How are you today?",
      "start_datetime": "2016-07-11T00:04:58Z",
      "end_datetime": "2016-07-12T00:04:58Z",
      "language": "en",
      "status": "pending",
      "from_number": "+16501231234"
    }
  }
}
EOF

```
This endpoint updates an existing campaign.


### HTTP Request

`PATCH https://api.upcall.com/api/v1/campaigns`

The expected data format is a JSON payload in the body.

<aside class="notice">
A note about <code>from_number</code>: when creating a campaign, you must use a number that has been confirmed. These area available in your Upcall dashboard.
</aside>

Attribute |  Description
--------- |  -----------
name | Name of the campaign.
instructions | Instructions for the caller.
pitch | Pitch for the campaign.
start_datetime | Start time for the campaign, eg. `2016-07-18T10:59:30.000Z`
end_datetime | End time for the campaign, eg. `2016-07-18T10:59:30.000Z`
language | Language of the campaign.
status | Status of the campaign. Must be one of `pending`, `ready`, `paused`, or `completed`.
from_number | Number used for the campaign.

## Delete a Campaign

```ruby

RestClient.delete "https://api.upcall.com/api/v1/campaigns/:id", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X DELETE "https://api.upcall.com/api/v1/campaigns/:id?auth_token=AUTH_TOKEN" \
-d @- << EOF

EOF

```

> The above command returns JSON structured like this:

```json

```

This endpoint deletes an existing campaign.

### HTTP Request

`DELETE https://api.upcall.com/api/v1/campaigns/:id`

# Numbers

You can use the numbers endpoints to add numbers to your contact lists. Each campaign has one contact list associated with it. Once you have added a number to the contact list for the campaing, you will need to [add it the campaign](#add-a-number-to-a-campaign) itself to indicate you would like callers to dial the number.

## Get All Numbers

```ruby
RestClient.get "https://api.upcall.com/api/v1/numbers", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :title => "Director",
  }
}
```

```shell
curl "https://api.upcall.com/api/v1/numbers/1?auth_token=AUTH_TOKEN&title=Director"
```

> The above command returns JSON structured like this:

```json

{
  "data": [
    {
      "id": "281",
      "type": "number",
      "attributes": {
        "list_id": 31,
        "first_name": "Name0",
        "last_name": "Contact",
        "title": "Director",
        "company": "Safeway",
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
      "type": "number",
      "attributes": {
        "list_id": 31,
        "first_name": "Name1",
        "last_name": "Contact",
        "title": "Director",
        "company": "Safeway",
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
    "self": "https://api.upcall.com/api/v1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2&title=Director",
    "next": "https://api.upcall.com/api/v1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2&title=Director",
    "last": "https://api.upcall.com/api/v1/numbers?auth_token=AUTH_TOKEN&page%5Bnumber%5D=3&page%5Bsize%5D=2&title=Director"
  }
}
```

This endpoint retrieves all numbers.

### HTTP Request

`GET https://api.upcall.com/api/v1/numbers`

### Query Parameters

Parameter |  Description
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


## Get a Specific Number

```ruby
RestClient.get "https://api.upcall.com/api/v1/numbers/1", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "https://api.upcall.com/api/v1/numbers/1?auth_token=AUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "306",
    "type": "number",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
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

This endpoint retrieves a specific number.

## Create a Number

```ruby

number = {
  "data" =>
  {
    "type" => "numbers",
    "attributes" =>
    {
      "campaign_id" => 1
      "first_name" => "Sam",
      "last_name" => "Contact",
      "title" => "Manager",
      "company" => "Safeway",
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

RestClient.post "https://api.upcall.com/api/v1/numbers", number.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/numbers?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "numbers",
    "attributes": {
      "campaign_id": 1
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
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
    "type": "number",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "Manager",
      "company": "Safeway",
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

This endpoint creates a new number and adds it to the contact list for a campaign.

<aside class="notice">
This endpoint will add the number to a contact list for a specific campaign. After adding a number to the contact list, you will need to <a href="#add-a-number-to-a-campaign">add it the campaign</a> itself to indicate you would like callers to dial the number.
</aside>


### HTTP Request

`POST https://api.upcall.com/api/v1/numbers`

The expected data format is a JSON payload in the body.

Attribute |  Description
--------- |  -----------
campaign_id | The number will be added to the contact list of this campaign.
phone | Phone number of the contact.
extension | Phone extension of the contact.
first_name | First name of the contact.
last_name | Last name of the contact.
title | Job title of the contact.
company | Company of the contact.
email | Email address of the contact.
status | Contact status, must be one of `active` or `disabled`.
address.recipient | Address recipient of the contact.
address.line1 | Address line 1 of the contact.
address.line2 | Address line 2 of the contact.
address.city | City of the contact.
address.region | Region of the contact (equivalent to state in the United States).
address.postal_code | Postal code of the contact (equivalent to zip code in the United States).
address.country | Country of the contact.

## Update a Number

```ruby

number = {
  "data" =>
  {
    "type" => "numbers",
    "attributes" =>
    {
      "title" => "New Title",
      "company" => "New Company",
    }
  }
}

RestClient.patch "https://api.upcall.com/api/v1/numbers/1", number.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/numbers/1?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "numbers",
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
    "type": "number",
    "attributes": {
      "list_id": 1,
      "first_name": "Sam",
      "last_name": "Contact",
      "title": "New Title",
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

This endpoint updates an existing number.


### HTTP Request

`PATCH https://api.upcall.com/api/v1/numbers/:id`

The expected data format is a JSON payload in the body.

Attribute |  Description
--------- |  -----------
campaign_id | The number will be added to the contact list of this campaign.
phone | Phone number of the contact.
extension | Phone extension of the contact.
first_name | First name of the contact.
last_name | Last name of the contact.
title | Job title of the contact.
company | Company of the contact.
email | Email address of the contact.
status | Contact status, must be one of `active` or `disabled`.
address.recipient | Address recipient of the contact.
address.line1 | Address line 1 of the contact.
address.line2 | Address line 2 of the contact.
address.city | City of the contact.
address.region | Region of the contact (equivalent to state in the United States).
address.postal_code | Postal code of the contact (equivalent to zip code in the United States).
address.country | Country of the contact.

# Calls

## Get All Calls

```ruby
RestClient.get "https://api.upcall.com/api/v1/calls", {
  :params => {
    :auth_token => "AUTH_TOKEN",
  }
}
```

```shell
curl "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN"
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
    "self": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.upcall.com/api/v1/calls?auth_token=AUTH_TOKEN&page%5Bnumber%5D=10&page%5Bsize%5D=2"
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

## Get a Specific Campaign

```ruby
RestClient.get "https://api.upcall.com/api/v1/calls/1", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "https://api.upcall.com/api/v1/calls/1?auth_token=AUTH_TOKEN"
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


# Credits

You can use the credits endpoints to add new credits to your account, as well as to check your current balance.

## Get Current Balance

```ruby
RestClient.get "https://api.upcall.com/api/v1/credits", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "https://api.upcall.com/api/v1/credits?auth_token=AUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "credits",
    "attributes": {
      "currency": "USD",
      "credit_balance": 20
    }
  }
}
```

This endpoint retrieves the current credit balance on your account.

### HTTP Request

`GET https://api.upcall.com/api/v1/credits`

## Add Credit to your account

```ruby

campaign = {
  "data" => {
    "type" => "credits",
    "attributes" => {
      "amount" => 10.00,
      "currency" => "USD",
    }
  }
}

RestClient.post "https://api.upcall.com/api/v1/credits", credit.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "https://api.upcall.com/api/v1/credits?auth_token=AUTH_TOKEN" \
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

This endpoint adds a specified amount to your current credit balance.

<aside class="notice">
Credits added in this manner will be billed to your account.
</aside>

### HTTP Request

`POST https://api.upcall.com/api/v1/credits`

Attribute |  Description
--------- |  -----------
amount | Amount of money to spend.
currency | Currency unit for the amount.
