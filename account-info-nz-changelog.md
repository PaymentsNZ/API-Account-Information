# Account Info API changelog

---

## V0.2.1 - 20/07/2018

* Altered `Reference` and `TransactionReference` fields to use BECSRemittance (alignment with Payment API)
  * This affects Transactions, Beneficiaries and StandOrders

## v0.2.0 - 15/06/2018

* Version bump to v0.2.0 indicating re-inclusion of OpenID Connect into authorisation flow
* Basepath changed to /open-banking-nz/v0.2
* Minor correction to enums where 'BECSElectronicCredit' was erroneously 'BECS Electronic Credit' (enum values should not have spaces)

## V0.1.2 - 6/06/2018

Changing URL to open-banking-nz

## V0.1.1 - 7/05/2018

* Refactored the following to models and referenced from API paths:
  * Transaction (TransactionModel)
  * Beneficiary (BeneficiaryModel)
  * Standing Orders (StandingOrderModel)
  * Products (ProductModel)
  * Direct Debits (DirectDebitModel)
* Added 501 response code to specification for endpoints not in MVP scope
* Added account number format description of 2-4-7-2 hyphen-delimited as recommended by Payments NZ working group and accepted by standards sub-group on 4/05/2018.  This has been added to both Debtor and Creditor account identifiers in OpenAPI specification
* Creditor Account updated to reflect BECS format accepted (StandingOrderModel, BeneficiaryModel)
* **Open Issue** reference fields in Standing Orders and Beneficiaries are single field, 35 max length.  Should these be updated to match BECSRemittance of Payment API?

## v0.1.0 - 23/03/2018

* Changed base path to /open-banking/v0.1
* Version bump specification to v0.1.0 to reflect MINOR change to stabilise for pilot

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
