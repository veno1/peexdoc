---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby
  - javascript

toc_footers:
  
  - <a href='http://www.peexit.com'>Documentation Powered by peexit</a>

search: true

code_clipboard: true
---

# Peex Inbound API Documentation

This document contains instructions about the Peex platform Rest API (sandbox). The inbound is 
intended to send transactions to Cameroon.

## Security

To access to these resources, you will need to use an access token (SECRETKEY). These secret key 
must be set in your http request header. Your secret key will be send by mail for the production 
environment. For now, you use the below key. 
**"header-key":** "SECRETKEY", 
"value": **"37528871-1f6c-43e9-a74a-0ad1f4baba76"** 
Remind: Just set it as an http header when callings the API.

## Resources

 For now, the platform exposes two resources : 


- **clients model:**


      - **"name"**
      - **"email"** 
      - **"adress"** 
      - **"web_site"** 
      - **"description"**
      - **"callback_url"** 
      - **"sold"** 
      - **"orange_fees"**
      - **"mtn_fees"** 
      - **"is_activated"**
      - **"created_at"**
      - **"updated_at"**
      


- **Transaction model**


       - **"id"**
       - **"amount"**
       - **"status":**
       - **"from_currency"**
       - **"to_currency"**
       - **"created_at"**
       - **"updated_at"**
       - **"payment_proof"**
       - **"identifier_by"**
       - **"track_id"**
       - **"type"**


## Retrieve partner information



```ruby

require "uri"
require "json"
require "net/http"

url = URI("https://dev-backend.peexit.com/api/v3/clients/me")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["ACCESS-TOKEN"] = "{{token}}"
request["SECRETKEY"] = "ea5ebe38-c3cf-4655-999a-eb0c63805a76"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body

```

```javascript

var myHeaders = new Headers();
myHeaders.append("ACCESS-TOKEN", "{{token}}");
myHeaders.append("SECRETKEY", "ea5ebe38-c3cf-4655-999a-eb0c63805a76");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  

var requestOptions = {
  method: 'GET',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://dev-backend.peexit.com/api/v3/clients/me", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```


> This is the response of the request


```json
{
    "name": "Peex",
    "email": "infos@peexit.com",
    "adress": "Bonamoussadi",
    "web_site": "peexit.com",
    "description": "Peers Exchange",
    "callback_url": "http://api.peex-callback/",
    "solde": 1341.0,
    "orange_fees": 500.0,
    "mtn_fees": 500.0,
    "is_activated": true,
    "created_at": "2022-03-23T13:35:38.773Z",
    "updated_at": "2022-07-22T16:44:31.154Z"
}
```

This allow you to retrieve the information about you in our system, you will be able to see your balance, 
and fees that is applicable for billings.

- **url:** “{{base_url}}clients/me”
- **method:** GET
- **params:**
  - Nothing, just provide the secret key in the request header
- **Response:**
  - Status 200: the result will contain information about you
  - Status 401: this means you’re not authorized to make requests in the server may be you 
have not provided a correct secret key, or your secret key is wrong



## Payment Request

```javascript

var myHeaders = new Headers();
myHeaders.append("ACCESS-TOKEN", "{{token}}");
myHeaders.append("SECRETKEY", "ea5ebe38-c3cf-4655-999a-eb0c63805a76");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "track_id": "TEST202103019",
  "amount": 10,
  "aml_cft": 1,
  "fxrate": 1,
  "from_currency": "XAF",
  "to_currency": "XAF",
  "transaction_type": "mobile",
  "user_id": 375,
  "client_id": 1,
  "mobile_phone": "+237675287689",
  "sender_first_name": "signe",
  "sender_last_name": "paul",
  "sender_email": "raoulsigne@yahoo.fr",
  "sender_mobile_phone": "+237675287689",
  "sender_country": "CM",
  "to_country": "CM",
  "purpose": "Familial maintenance",
  "fund_origin": "Salary"
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch(" https://dev-backend.peexit.com/api/v3/clients/request_payment", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

```ruby

require "uri"
require "json"
require "net/http"

url = URI(" https://dev-backend.peexit.com/api/v3/clients/request_payment")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["ACCESS-TOKEN"] = "{{token}}"
request["SECRETKEY"] = "ea5ebe38-c3cf-4655-999a-eb0c63805a76"
request["Content-Type"] = "application/json"
request.body = JSON.dump({
  "track_id": "TEST202103019",
  "amount": 10,
  "aml_cft": 1,
  "fxrate": 1,
  "from_currency": "XAF",
  "to_currency": "XAF",
  "transaction_type": "mobile",
  "user_id": 375,
  "client_id": 1,
  "mobile_phone": "+237675287689",
  "sender_first_name": "signe",
  "sender_last_name": "paul",
  "sender_email": "raoulsigne@yahoo.fr",
  "sender_mobile_phone": "+237675287689",
  "sender_country": "CM",
  "to_country": "CM",
  "purpose": "Familial maintenance",
  "fund_origin": "Salary"
})

response = http.request(request)
puts response.read_body
```

> this is the response  to the request

```json
{
    "request": {
        "id": 2124,
        "amount": 10,
        "status": "pending",
        "from_currency": "XAF",
        "to_currency": "XAF",
        "created_at": "2022-07-27T12:49:36.066Z",
        "updated_at": "2022-07-27T12:49:38.873Z",
        "payment_proof": null,
        "identifier_by": "+237675287689",
        "track_id": "TEST202103019"
    }
}
```


To request a payment to a peex user by sending his email or mobile_phone


- **url:** {{base_url}}clients/request_payment 
- **method:** POST 
- **params:**

   - **email:** the user email to whom the payment will be done (required if no 
mobile_phone) 
   - **mobile_phone:** the user mobile_phone to whom the payment will be done
(required if no mobile_phone) 
   - **amount:** the amount to be send (required) 
   - **from_currency :** the origins currency (required) 
   - **to_currency:** the destination currency ( optional default xaf )  fxrate : the rate will be use to convert from from_currency to to_currency
   - **aml_cft :** a key that confirm the validity of the transaction , without it the 
transaction will not be created 
   - **sender_first_name :** the sender first name (required for the first transaction 
of this sender) 
   - **sender_last_name :** the sender last name (required for the first transaction of 
this sender) 
   - **sender_email :** the sender email (not required) 
   - **sender_mobile_phone:** the sender mobile phone (always required) 
   - **sender_country:** Country Code in which the money is sent. ISO Alpha 2
standard
  - **first_name:** the beneficiary first name (required) 
  - **last_name:** the beneficiary last name (required)
  - **to_country:** the beneficiary country in ISO Alpha 2 standard (required)
  - **purpose:** the reason of this transaction (required for conformity)
  - **fund_origin:** the fund origin (required for conformity)
- **response:** 
   - **status 200:** the transaction objet with the status 
   - **status 404:** the given user is not found in peex platform 
   - **status 422:** the aml_cft params is not set to true


## Get All requests of a client 

```ruby

require "uri"
require "json"
require "net/http"

url = URI(" https://dev-backend.peexit.com/api/v3/clients/all_requests?track_id=TEST2020102301")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["SECRETKEY"] = "ea5ebe38-c3cf-4655-999a-eb0c63805a76"
request["Content-Type"] = "application/json"
request.body = JSON.dump({
 
})

response = http.request(request)
puts response.read_body
```

```javascript

var myHeaders = new Headers();
myHeaders.append("SECRETKEY", "ea5ebe38-c3cf-4655-999a-eb0c63805a76");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
 
});

var requestOptions = {
  method: 'GET',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch(" https://dev-backend.peexit.com/api/v3/clients/all_requests?track_id=TEST2020102301", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

> this is the response you will get from the request

```json

[
    {
        "id": 214,
        "amount": 1000000000,
        "status": "rejected",
        "from_currency": "XAF",
        "to_currency": "XAF",
        "created_at": "2020-10-23T09:54:06.141Z",
        "updated_at": "2020-10-27T10:54:42.758Z",
        "payment_proof": "AMOUNT_EXCEED_THRESHOLD",
        "identifier_by": "+237691295354",
        "track_id": "TEST2020102301",
        "type": "mobile"
    }
]
```


Return all transactions of the client (determine from your secretkey) 


- **url:** “{{base_url}}clients/all_requests” /:track_id" 
   
- **method:** GET 

- **params:**  Track_id  and secret key are absolutely needed 
 
- **response:**


     -  **status 200:** the transaction objet with the status
     -  **status 422:** the transaction does not belong to your 
     -  **status 404:** there is not transaction for the given id



## Formatting


We define here the different format of params required by resources.

- **email:** string on email format. **ex:** testuser@gmail.com 
- **mobile_phone:** string, complete phone number with country code. **ex:** 
+237678923563 (without spaces nor hyphens). 
- **amount: double,** the amount which would be transferred to a user. **ex:** 6789.23
- **from_currency:** string, the currency in which the amount is send. It is a country 
currency code. **ex:** USD (Uppercase). 
- **to_currency:** string, the currency in which the amount will be paid. It is a country 
currency code. **ex:** XAF (Uppercase). 
- **fxrate:** double, is the conversion rate from from_currency to to_currency. **ex:** 655.34 
- **aml_cft:** boolean, is a value that certified the informations send through the request. ex: true 
(in the case of payment request, the action will be considered only if aml_cft is set to true). 
- **status:** string, is informs about a payment request status. Possible value are **(new, pending, 
paid, failed).** **ex:** new (lowercase).


## Possible transaction values

|Id| Status| Meaning|
|---|------|---------|
|1 |new |Transaction reached the PEEX Server|
|2 |pending| The transaction has been paid, awaiting the confirmation identifier|
|3 |mw_balance_pending| There is no enough fund in the peex account to pay this transaction|
|4 |balance_pending| The account does not have enough funds to pay the transaction|
|5 |max_amount |The transaction amount is greater than the maximum allowed|
|6 |paid| Transaction has been PAID and CONFIRM|
|7 |failed| Automatic payment failed, reason is in the PAYMENT_PROOF field|
|8 |canceled |Transaction has been canceled manually, reason is in the PAYMENT_PROOF field|
|9 |rejected| this transaction cannot be processed due to insufficient funds|