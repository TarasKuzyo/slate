---
title: API Reference

language_tabs:
  - python

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>


search: true
---

# Introduction

This example API documentation page was created with [Slate](https://github.com/tripit/slate).

# Authentication

> To authorize, use this code:


```python
import requests

url = 'https://still-ravine-49399.herokuapp.com/backgroundcheck/api/v1.0'

headers = {
    'username': "uname",
    'password': "paswd",
}

r = requests.post(url, headers=headers, params=payload)
```

> Make sure to replace `username` and `password` with your credentials.


User credentials are expected to be included in all API requests to the server in a header that looks like the following:

```curl
username: uname
password: paswd
```

# Background Screening

## Submit Background Screening Request


```python

import requests

url = 'https://still-ravine-49399.herokuapp.com/backgroundcheck/api/v1.0'

headers = {
    'username': "admin",
    'password': "admin",
}

payload = {
    'first_name': 'John',
    'last_name': 'Doe',
    'address': '123 Fake Street, Springville, UT, 12345',
    'reference_id': '1234567890',
    'ssn': '333-22-1111',
    'DOB': '1990-01-01',
    'screening_type': 'all',
    'sandbox': True
}

r = requests.post(url, headers=headers, params=payload)
```

> The above command returns JSON structured like this:

```json

{
    "order_id": "91334", 
    "reference_id": "1234567890", 
    "status": "pending"
}

```

This endpoint returns json data with backreference details.

### HTTP Request

`POST https://still-ravine-49399.herokuapp.com/backgroundcheck/api/v1.0`

### Query Parameters


#### Required

Parameter      | Description
-------------- | -----------
first_name     | The first name of the applicant.
last_name      | The last name of the applicant.
address        | The address of the applicant.
ssn            | Social Security Number of the applicant.
screening_type | Type of background check (all|credit|criminal)

#### Optional

Parameter    | Default | Description
------------ | ------- | -----------
reference_id | random 32 character string | Provides for the client to transmit any sort of file identifier, reference data, or other information of its choosing.
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

    url = 'https://still-ravine-49399.herokuapp.com/backgroundcheck/api/v1.0'

    headers = {
        'username': "admin",
        'password': "admin",
    }

    payload = {
        'order_id': '91338',
        'reference_id': '1234567890',
        'sandbox': True
    }

    r = requests.get(url, headers=headers, params=payload)
```

> The above command returns JSON structured like this:

```json
{
    "order_id": "91334", 
    "reference_id": "111222333", 
    "status": "pending", 
    "report_url": "https://example.com?file=91335&format=pdf"
}
```

This endpoint returns json data with order status and report URL.



### HTTP Request

`GET https://still-ravine-49399.herokuapp.com/backgroundcheck/api/v1.0`

### URL Parameters

#### Required

Parameter | Description
--------- | -----------
order_id  | The unique identifier for the background check order (returned in POST request response)

#### Optional

Parameter    | Default | Description
------------ | ------- | -----------
reference_id | None | Provides for the client to transmit any sort of file identifier, reference data, or other information of its choosing. 
sandbox      | False | If True the sandbox URL is used, otherwise use production URL.


### Errors

Error Code | JSON Response
---------- | -------------
401 | `{"errors": "Invalid login. Invalid login credentials."}`
422 | `{"errors": "Missing, invalid and/or unauthorized 'OrderId' element." }`



