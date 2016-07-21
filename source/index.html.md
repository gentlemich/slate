---
title: API Reference

language_tabs:
  - ruby
  - shell

toc_footers:
  - <a href='https://upcall.com'>Upcall</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Upcall API! You can use the API to view and manage campaigns, numbers, and calls.

The Upcall API follows [JSON-API](http://jsonapi.org/) conventions.

# Authentication

> To authorize, pass in the `auth_token` parameter:

```ruby
RestClient.get "http://www.upcall.com/api/v1/campaigns", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN"
```

> Make sure to replace `AUTH_TOKEN` with your auth token.

Upcall uses auth tokens in the URL to allow access to the API.

All requests to the API should include the `auth_token` query parameter.

# Campaigns

You can use the campaigns endpoints to create new campaigns, as well as manage and view your existing campaigns.

## Get All Campaigns

```ruby
RestClient.get "http://www.upcall.com/api/v1/campaigns", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :language => "en",
  }
}
```

```shell
curl "http://www.upcall.com/api/v1/campaigns/123?auth_token=AUTH_TOKEN&language=en"
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
        "pitch": "Be friendly",
        "language": "en",
        "status": "pending",
        "updated_at": "2016-07-18T10:49:18.000Z",
        "created_at": "2016-07-18T10:49:18.000Z",
        "start_datetime": "2016-07-11T00:04:58.000Z",
        "end_datetime": "2016-07-12T00:04:58.000Z",
        "instructions": "Call people and ask about product satisfaction"
      }
    },
    {
      "id": "877",
      "type": "campaigns",
      "attributes": {
        "name": "My Campaign",
        "pitch": "Be friendly",
        "language": "en",
        "status": "pending",
        "updated_at": "2016-07-18T10:49:18.000Z",
        "created_at": "2016-07-18T10:49:18.000Z",
        "start_datetime": "2016-07-11T00:04:58.000Z",
        "end_datetime": "2016-07-12T00:04:58.000Z",
        "instructions": "Call people and ask about product satisfaction"
      }
    }
  ],
  "links": {
    "self": "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=11&page%5Bsize%5D=2"
  }
}

```

This endpoint retrieves all campaigns.

### HTTP Request

`GET http://www.upcall.com/api/v1/campaigns`

### Query Parameters

Parameter |  Description
--------- |  -----------
name | Filter for campaigns based on their name.
language | Filter for campaigns based on their language.
status | Filter for campaigns based on their status. Must be one of `pending`, `ready`, `paused`, `completed`, or `archived`.
from_number | Filter for campaigns based the caller ID number they will be dialed from.
min_start_datetime | Minimum start date time, eg. `2016-07-18T10:49:18.000Z`
max_start_datetime | Maximum start date time, eg. `2016-07-18T10:49:18.000Z`
min_created_datetime | Minimum created date time, eg. `2016-07-18T10:49:18.000Z`
max_created_datetime | Maximum created date time, eg. `2016-07-18T10:49:18.000Z`
min_updated_datetime | Minimum updated date time, eg. `2016-07-18T10:49:18.000Z`
max_updated_datetime | Maximum updated date time, eg. `2016-07-18T10:49:18.000Z`

## Get a Specific Campaign

```ruby
RestClient.get "http://www.upcall.com/api/v1/campaigns/123", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN"
```

> The above command returns JSON structured like this:

```json

{
  "data": {
    "id": "123",
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "pitch": "Be friendly",
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

This endpoint retrieves a specific campaign.

## Get Numbers for a Campaign

The campaign numbers endpoint retrieves numbers that are part of a campaign. It describes their current status in the campaign, in terms of whether they have been dialed successfuly, will be retried, etc.

### HTTP Request

`GET http://www.upcall.com/api/v1/campaigns/:id/numbers`

### Query Parameters

Parameter |  Description
--------- |  -----------
phone | Filter for a specific phone number.
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


```ruby
RestClient.get "http://www.upcall.com/api/v1/campaigns/1/numbers", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :company => "Safeway",
  }
}
```

```shell
curl "http://www.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN&company=Safeway"
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
    "self": "http://www.upcall.com/api/v1/campaigns/7/numbers?auth_token=3e94ac4aa8d4c2907477fa211aa1f412937910d1&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "http://www.upcall.com/api/v1/campaigns/7/numbers?auth_token=3e94ac4aa8d4c2907477fa211aa1f412937910d1&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "http://www.upcall.com/api/v1/campaigns/7/numbers?auth_token=3e94ac4aa8d4c2907477fa211aa1f412937910d1&page%5Bnumber%5D=10&page%5Bsize%5D=2"
  }
}
```

## Add a Number to a Campaign

Adds a number to the campaign. Note that this endpoint only works on numbers you have already [added to a contact list](#numbers).

### HTTP Request

`POST http://www.upcall.com/api/v1/campaigns/:id/numbers`

```ruby

number = {
  "data" => {
    "type" => "numbers",
    "attributes" => {
      "list_contact_id" => 245,
    }
  }
}

RestClient.get "http://www.upcall.com/api/v1/campaigns/1/numbers", number.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "http://www.upcall.com/api/v1/campaigns/1/numbers?auth_token=AUTH_TOKEN" \
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

## Create a  Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "My Campaign",
      "instructions" => "Call people and ask about product satisfaction",
      "pitch" => "Be friendly",
      "start_datetime" => "2016-07-11T00:04:58Z",
      "end_datetime" => "2016-07-12T00:04:58Z",
      "language" => "en",
      "status" => "pending",
      "from_number" => "+16501231234",
    }
  }
}

RestClient.post "http://www.upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "instructions": "Call people and ask about product satisfaction",
      "pitch": "Be friendly",
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
      "pitch": "Be friendly",
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

`POST http://www.upcall.com/api/v1/campaigns`

The expected data format is a JSON payload in the body.

<aside class="info">
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
status | Status of the campaign. Must be one of `pending`, `ready`, `paused`, `completed`, or `archived`.
from_number | Number used for the campaign.


## Update a  Campaign

```ruby

campaign = {
  "data" => {
    "type" => "campaigns",
    "attributes" => {
      "name" => "My Campaign",
      "instructions" => "Call people and ask about product satisfaction",
      "pitch" => "Be friendly",
      "start_datetime" => "2016-07-11T00:04:58Z",
      "end_datetime" => "2016-07-12T00:04:58Z",
      "language" => "en",
      "status" => "pending",
      "from_number" => "+16501231234",
    }
  }
}

RestClient.path "http://www.upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X PATCH "http://www.upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
-H 'Content-Type: text/json; \
-d @- << EOF
{
  "data": {
    "type": "campaigns",
    "attributes": {
      "name": "My Campaign",
      "instructions": "Call people and ask about product satisfaction",
      "pitch": "Be friendly",
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
      "pitch": "Be friendly",
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

This endpoint updates an existing campaign.

### HTTP Request

`PATCH http://www.upcall.com/api/v1/campaigns`

The expected data format is a JSON payload in the body.

Attribute |  Description
--------- |  -----------
name | Name of the campaign.
instructions | Instructions for the caller.
pitch | Pitch for the campaign.
start_datetime | Start time for the campaign, eg. `2016-07-18T10:59:30.000Z`
end_datetime | End time for the campaign, eg. `2016-07-18T10:59:30.000Z`
language | Language of the campaign.
status | Status of the campaign. Must be one of `pending`, `ready`, `paused`, `completed`, or `archived`.
from_number | Number used for the campaign.

# Numbers

You can use the numbers endpoints to add numbers to your campaign 

