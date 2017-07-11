---
title: API Reference

language_tabs:
  - python
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>


search: true
---

# Introduction

This example API documentation page was created with [Slate](https://github.com/lord/slate).

# Authentication


> To authorize, use this code:


```python
import requests

url = 'https://api.globalver.com/v1.0'

headers = {
    'Authorization': "your_api_key",
}

r = requests.post(url, headers=headers)
```

```shell
# With shell, you can just pass the correct header with each request
curl https://api.globalver.com/v1.0
  -H "Authorization: your_api_key"
```

> Make sure to replace `your_api_key` with your actual API key.

Globalver uses API keys to allow access to the API. You can register a new API key at our [developer portal](http://example.com/developers).

The API key is expected to be included in all API requests to the server in a header that looks like the following:

`Authorization: your_api_key`

<aside class="notice">
You must replace <code>your_api_key</code> with your personal API key.
</aside>


# Background Screening

## Submit Background Screening Request


```python

import requests

url = 'https://api.globalver.com/v1.0'

headers = {
    'Authorization': "your_api_key",
}

payload = {
    'first_name': 'John',
    'last_name': 'Doe',
    'address': '123 Fake Street, Springville, UT, 12345',
    'ssn': '333-22-1111',
    'DOB': '1990-01-01',
    'screening_type': 'all',
    'sandbox': True
}

r = requests.post(url, headers=headers, params=payload)
```

```shell 
curl https://api.globalver.com/v1.0
    -H "Authorization: your_api_key"
```

> The above command returns JSON structured like this:

```json

{
    "order_id": "91334", 
    "status": "pending"
    "cost": 0.0
}

```

This endpoint returns json data with backreference details.

### HTTP Request

`POST https://api.globalver.com/v1.0`

### Query Parameters


#### Required

Parameter      | Description
-------------- | -----------
first_name     | The first name of the applicant.
last_name      | The last name of the applicant.
address        | The address of the applicant.
ssn            | Social Security Number of the applicant.
screening_type | Type of background check (all, credit, criminal)

#### Optional

Parameter    | Default | Description
------------ | ------- | -----------
DOB          | None    | The birthdate of the applicant.
sandbox      | False   | If True the sandbox URL is used, otherwise use production URL.


<aside class="notice">
<code>DOB</code> field is required if <code>screening_type</code> is either <code>all</code> or <code>criminal</code>.
</aside>

### Errors

Error Code | JSON Response
---------- | -------------
401 | `{"errors": "Invalid login. Invalid login credentials."}`
422 | `{"errors": {"screening_type": ["Not a valid choice."] } }`
422 | `{"errors": {"DOB": ["Argument is required for criminal background check."] } }`




## Background screening status check


```python

import requests

url = 'https://api.globalver.com/v1.0'

headers = {
    'Authorization': "your_api_key",
}

payload = {
    'order_id': '91338',
    'sandbox': True
}

r = requests.get(url, headers=headers, params=payload)
```

```shell 
curl https://api.globalver.com/v1.0
    -H "Authorization: your_api_key"
```

> The above command returns JSON structured like this:

```json
{
    "order_id": "91334", 
    "status": "pending", 
    "report_url": "https://example.com?file=91335&format=pdf",
    "cost": 0.0
}
```

This endpoint returns json data with order status and report URL.



### HTTP Request

`GET https://api.globalver.com/v1.0`

### URL Parameters

#### Required

Parameter | Description
--------- | -----------
order_id  | The unique identifier for the background check order (returned in POST request response)

#### Optional

Parameter    | Default | Description
------------ | ------- | -----------
sandbox      | False   | If True the sandbox URL is used, otherwise use production URL.


### Errors

Error Code | JSON Response
---------- | -------------
401 | `{"errors": "Invalid login. Invalid login credentials."}`
422 | `{"errors": "Missing, invalid and/or unauthorized 'OrderId' element." }`



