# Account Info API changelog

---

## V0.0.3 - 16/02/2018

* Added AccountRequestResponseModel - this model wraps the AccountRequestModel (model of account access requests) and adds the required response fields.  Previously this was done through read-only attributes, however this causes semantic error when used with 'required'.  The AccountRequestsResponseModel is syntactically equivalent, allows for required fields and removes the error condition.

## V0.0.2 - 19/12/2017

* Updated info section (contact, licence info, terms of service)
* Corrected 200 response in GET /accounts/{AccountId} - now users PaymentLinks (previously not actually included)

## V0.0.1 - 12/12/2017

* For discussion: used additional 'link' in GET /accounts and GET /accounts/{AccountId} to indicate account is valid for making payments (see PaymentLinks model in specification)
* Created models in 'definitions' section:
  * BalanceModel
  * AccountModel using AccountSchemeModel
  * AccountSchemeModel - enum that allows 'BECS Electronic Credit' - NZ bank account numbers
  * AccountRequestModel
  * Links
  * PaymentLinks
* Updated GET /accounts and GET /accounts/{AccountId} to use AccountModel and PaymentLinks
* Updated GET /accounts/{AccountId}/balances to use BalanceModel and Links (and since this is referred to in GET /balances this is also consistent
* Updated POST /account-requests request and response to use AccountRequestModel
* Updated GET /account-requests to use AccountRequestModel
* Updated request headers to set JWS signature and financial institution identifier as optional
* Left all other resources intact and unchanged (as per MVP directive) - could benefit from resource models being included in 'definitions later'
