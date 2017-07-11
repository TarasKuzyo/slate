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
    'FirstName': 'John',
    'LastName': 'Doe',
    'Address': '123 Fake Street, Springville, UT, 12345',
    'SSN': '333-22-1111',
    'DOB': '1990-01-01',
    'CreditReport': True,
    'CriminalRecordReport': False,
    'Sandbox': True
}

r = requests.post(url, headers=headers, params=payload)
```

```shell 
curl https://api.globalver.com/v1.0
    -H "Authorization: your_api_key"
    -d "FirstName=John"
    -d "LastName=Doe"
    --data-urlencode "Address=123 Fake Street, Springville, UT, 12345"
    -d "SSN=333-22-1111"
    -d "DOB=1990-01-01"
    -d "CreditReport=True"
    -d "CriminalRecordReport=False"
    -d "Sandbox=True"
```

> The above command returns JSON structured like this:

```json

{
    "OrderId": "91334", 
    "Status": "pending"
    "Cost": 0.0
}

```

This endpoint returns json data with backreference details.

### HTTP Request

`POST https://api.globalver.com/v1.0`

### Query Parameters


#### Required

Parameter      | Description
-------------- | -----------
FirstName      | The first name of the applicant.
LastName       | The last name of the applicant.
Address        | The address of the applicant.
SSN            | Social Security Number of the applicant.
DOB            | The birthdate of the applicant in yyyy-mm-dd format.

#### Optional

Parameter            | Default | Description
-------------------- | ------- | -----------
CreditReport         | True    | If True Credit Report is included
CriminalRecordReport | True    | If True Criminal Record Report is included
Sandbox              | False   | If True the sandbox URL is used, otherwise use production URL.


### Errors

Error Code | JSON Response
---------- | -------------
401 | `{"errors": "Invalid login. Invalid login credentials."}`
422 | `{"errors": {"DOB": ["Not a valid datetime."] } }`




## Background screening status check


```python

import requests

url = 'https://api.globalver.com/v1.0'

headers = {
    'Authorization': "your_api_key",
}

payload = {
    'OrderId': '91338',
    'Sandbox': True
}

r = requests.get(url, headers=headers, params=payload)
```

```shell 
curl -G https://api.globalver.com/v1.0
    -H "Authorization: your_api_key"
    -d "OrderId=91338"
    -d "Sandbox=True"
```

> The above command returns JSON structured like this:

```json
{
    "OrderId": "91334", 
    "Status": "pending", 
    "ReportUrl": "https://lightning.instascreen.net/send/interchangeview/?a=64pelqfHfjseq&b=540&c=rptview&file=1453&format=pdf",
    "Cost": 0.0
}
```

This endpoint returns json data with order status and report URL.

Field       | Description
----------- | -----------
OrderId     | The unique identifier for the background check order
Status      | Current order status (possible values are: pending, ready, error)
ReportUrl   | A direct link to the report document
Cost        | The amount charged for the report ($9.99 per report type)


<aside class="notice">
For <code>Sandbox</code> the cost is always 0.0.
</aside>

### HTTP Request

`GET https://api.globalver.com/v1.0`

### URL Parameters

#### Required

Parameter | Description
--------- | -----------
OrderId   | The unique identifier for the background check order (returned in POST request response)

#### Optional

Parameter    | Default | Description
------------ | ------- | -----------
Sandbox      | False   | If True the sandbox URL is used, otherwise use production URL.


### Errors

Error Code | JSON Response
---------- | -------------
401 | `{"errors": "Invalid login. Invalid login credentials."}`
422 | `{"errors": "Missing, invalid and/or unauthorized 'OrderId' element." }`



