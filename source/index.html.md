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
RestClient.get "http://upcall.com/api/v1/campaigns", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN"
```

> Make sure to replace `AUTH_TOKEN` with your auth token.

Upcall uses auth tokens in the URL to allow access to the API.

All requests to the API should include the `auth_token` query parameter.

# Campaigns

## Get All Campaigns

```ruby
RestClient.get "http://upcall.com/api/v1/campaigns", {
  :params => {
    :auth_token => "AUTH_TOKEN",
    :language => "en",
  }
}
```

```shell
curl "http://upcall.com/api/v1/campaigns/123?auth_token=AUTH_TOKEN&language=en"
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
    "self": "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN&page%5Bnumber%5D=11&page%5Bsize%5D=2"
  }
}

```

This endpoint retrieves all campaigns.

### HTTP Request

`GET http://upcall.com/api/v1/campaigns`

### Query Parameters

Parameter |  Description
--------- |  -----------
name | Filter for campaigns based on their name.
language | Filter for campaigns based on their language.
status | Filter for campaigns based on their status. Must be one of `pending`, `ready`, `paused`, `completed`, or `archived`.
min_start_datetime | Minimum start date time, eg. `2016-07-18T10:49:18.000Z`
max_start_datetime | Maximum start date time, eg. `2016-07-18T10:49:18.000Z`
min_created_datetime | Minimum created date time, eg. `2016-07-18T10:49:18.000Z`
max_created_datetime | Maximum created date time, eg. `2016-07-18T10:49:18.000Z`
min_updated_datetime | Minimum updated date time, eg. `2016-07-18T10:49:18.000Z`
max_updated_datetime | Maximum updated date time, eg. `2016-07-18T10:49:18.000Z`

## Get a Specific Campaign

```ruby
RestClient.get "http://upcall.com/api/v1/campaigns/123", {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN"
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

RestClient.post "http://upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X POST "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
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

`POST http://upcall.com/api/v1/campaigns`

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

RestClient.path "http://upcall.com/api/v1/campaigns", campaign.to_json {
  :params => { :auth_token => "AUTH_TOKEN" }
}
```

```shell
curl -X PATCH "http://upcall.com/api/v1/campaigns?auth_token=AUTH_TOKEN" \
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

`PATCH http://upcall.com/api/v1/campaigns`

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
