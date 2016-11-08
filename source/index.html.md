---
title: Life.io User Management API Reference

language_tabs:
  - shell
  - ruby

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Life.io User Management API Reference

Welcome to the Life.io User Management API. Instructions for authentication and all available endpoints are listed below.

# Authentication

All requests to the User Management API require an API Key and a User Management Admin Token.

The API Key and User Management Admin Token can be obtained from the rails console accessing the appropriate database via the following commands:

`ApiKey.create(client_type: "user_management")`

`UserManagementAdminToken.create`

This User Management API was designed with the assumption that the API Key and User Management Admin Token would be stored as environment variables within the User Management Dashboard and passed back to the server with each request.


# Application Users

## Get confirmed_at timestamp

```ruby
include HTTParty

BASE_URL = "https://hq.life.io/api/user_management/v1"

@options = {
  headers: {
    "Authorization": @user_management_admin_token
  },
  body: {
    "api_key": @api_key,
    "application_user": {
      "email": CGI::escape(user.email)
    }
  }
}

HTTParty.get("#{BASE_URL}/application_users", @options)

```

```shell
curl "https://hq.life.io/api/user_management/v1/application_users"
  -X GET
  -d "api_key=your_api_key
      &application_user[email]=test@test.com"
  -H "Authorization: Token token=your_user_management_admin_token"
```

> The above command returns JSON structured like this:

```json
{
  "confirmed_at": 2016-11-08 12:14:56 -060
}

```

This endpoint retrieves a specific user's confirmed_at timestamp.

### HTTP Request

`GET https://hq.life.io/api/user_management/v1/application_users`

### Query Parameters

Parameter | Description
--------- | -----------
email | The application user's email address is used to find the specific user. ID is not accepted.

<aside class="notice">
Note — the email address must be wrapped in a CGI::escape().
</aside>

## Delete a User

```ruby
include HTTParty

BASE_URL = "https://hq.life.io/api/user_management/v1"

@options = {
  headers: {
    "Authorization": @user_management_admin_token
  },
  body: {
    "api_key": @api_key,
    "application_user": {
      "email": CGI::escape(user.email)
    }
  }
}

HTTParty.delete("#{BASE_URL}/application_users", @options)
```

```shell
curl "https://hq.life.io/api/user_management/v1/application_users"
  -X DELETE
  -d "api_key=your_api_key
      &application_user[email]=test@test.com"
  -H "Authorization: Token token=your_user_management_admin_token"
```

> The above command returns JSON structured like this:

```json
{
  "message": "User deleted"
}
```

This endpoint deletes a user.

### HTTP Request

`DELETE "https://hq.life.io/api/user_management/v1/application_users"`

### URL Parameters

Parameter | Description
--------- | -----------
email | The application user's email address is used to find the specific user. ID is not accepted.


# Branches

## Create a Branch

```ruby
include HTTParty

BASE_URL = "https://hq.life.io/api/user_management/v1"

@options = {
  headers: {
    "Authorization": @user_management_admin_token
  },
  body: {
    "api_key": @api_key,
    "branch": {
      "name": "your_branch_name",
      "company_id": company_id,
      "country": "your_branch_country",
      "state": "your_branch_state",
      "city": "your_branch_city"
    }
  }
}

HTTParty.post("#{BASE_URL}/branches", @options)

```

```shell
curl "https://hq.life.io/api/user_management/v1/branches"
  -X POST
  -d "api_key=your_api_key
      &branch[name]=your_branch_name
      &branch[company_id]=company_id
      &branch[country]=your_branch_country
      &branch[state]=your_branch_state
      &branch[city]=your_branch_city"
  -H "Authorization: Token token=your_user_management_admin_token"
```

> The above command returns JSON structured like this:

```json
{
  "shared_id": 1,
  "registration_token": "ABC12DEFafcdeg123ab456="
}

```

This endpoint creates a branch.

### HTTP Request

`POST https://hq.life.io/api/user_management/v1/branches`

### Query Parameters

Parameter | Description
--------- | -----------
name | Required. Name of new branch.
company_id | Required. Shared_id of the parent company of new branch.
country | Required. Country where new branch is located.
state | Required. State where new branch is located.
city | Required. City where new branch is located.


<aside class="notice">
Note — the company_id should be the shared_id returned when creating the parent company of this branch. If you have not created the parent company of this branch yet, you must do this step first.
</aside>


# Companies

## Create a Company

```ruby
include HTTParty

BASE_URL = "https://hq.life.io/api/user_management/v1"

@options = {
  headers: {
    "Authorization": @user_management_admin_token
  },
  body: {
    "api_key": @api_key,
    "company": {
      "name": "your_company_name"
      "parent_organization_name": "parent_org_name"
    }
  }
}

HTTParty.post("#{BASE_URL}/companies", @options)

```

```shell
curl "https://hq.life.io/api/user_management/v1/branches"
  -X POST
  -d "api_key=your_api_key
      &company[name]=your_company_name
      &company[parent_organization_name]=parent_org_name"
  -H "Authorization: Token token=your_user_management_admin_token"
```

> The above command returns JSON structured like this:

```json
{
  "shared_id": 1,
  "subdomain": "hq"
}

```

This endpoint creates a company.

### HTTP Request

`POST https://hq.life.io/api/user_management/v1/companies`

### Query Parameters

Parameter | Description
--------- | -----------
name | Required. Name of new company.
parent_organization_name | Required. Name of the parent organization of the new company.


<aside class="notice">
Note — Double check the spelling and accuracy of the parent organization name. This name will be used to create the relationship between the new company and its parent org.
</aside>
