---
title: Python payment processor API

language_tabs:
  - python
  - shell

includes:
  - account
  - errors
search: true
---

# Introduction

This API is designed to be both simple and "correct" from a technical perspective. I would like to stick to a strong RESTful principle with endpoints named after models and methods being CRUD (create, read, update, destroy).

For testing purposes and speeding up development we highly recommend [Postman](https://www.getpostman.com/apps). It enables you to store all variables, headers in one place and make a request in one
click.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). 

# Authentication

All endpoints will require authentication with an **api_key** (associated with a project). Think 
about project as about unique unit, all other parts are around it. In our case project
is our current ICO.

At first you should receive your project api key for performing
authorize requests. Simply run this commands.

```python

python manage.py shell # in root of project
>> from payment_processor_api.models import Project
>> Project.objects.create()
>> Project.objects.create().api_key.secret_access_key
>> 72tyrha545ccaa7d1c483782783914911fe52df3 # save this to .env file
```

# Project

## Get All requirements


After recieving **api_key** now we can make authorized requests.
At first you should make a POST request on link '/project' and in return
you receive json dictionary with all requirements.

### HTTP Request

`POST http://localhost:8000/project/defaults?project_key={{project_key}}`

```shell
curl "http://localhost:8000/project/defaults?project_key={{project_key}}"
  -H "PROJECTKEY: 72tyrha545ccaa7d1c483782783914911fe52df3"
```

> The above command returns JSON structured like this:

```json
{
  "default_confirmation_level": 6,
  "default_callback": "hell.comm",
  "default_deposit_behavior": {"recipients": [
    {
      "receival_address": "0x1000000000000000000000000000000000000000",
      "withdrawal_percent": 52
    },
    {
      "receival_address": "0x1220000000000000000000000000000000000000",
      "withdrawal_percent": 48
    }
    ]}
}
```


### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
callback_url | "" | 
confirmation_level | 1 - minimum , 10 - maximum | 
recipient_list |[]|Format: [{address: percent}, {address: percent}]. Percents must sum to 100.
