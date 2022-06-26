# Account Info API change log

---

## v2.3.0 - 19/06/2022

- No new changes to Swagger for v2.3.0

## v2.2.0 - 19/06/2022

- No new changes to Swagger for v2.2.0 - as changes for the version are on scoping mandatory endpoint functionality

## v2.1.0 - 01/12/2020

- New error code QueryParam.Invalid
- Updated scope definition for accounts that are accessible via the API
- Removed ReadPAN permission as per Technical Decision - 025 - ReadPAN Permission Code Changes
- Added MaskedCardNumber as new SchemeName enumeration for the account identification object

## v2.0.1 - 17/07/2020

- Patch update to Swagger specification to include documented query parameters.

## v2.0.0 - 20/03/2020

Incorporate changes in preparation for v2.0.0 release, including:

- Version bump to v2.0.0 and URL to v2.0
- x-fapi-customer-last-logged-time updated to x-fapi-auth-date (per latest FAPI spec). Aligned definition of date type (per OBIE v3.1.2 spec)
- Content-Type aligned with Accept header rewording; updated Content-Type to Mandatory (align with OBIE v3.1.2)
- Added x-customer-user-agent (agreed for NZ v1.0 spec, but not documented)
- Added x-merchant-ip-address (agreed for NZ v1.0 spec, but not documented)
- 415 (Unsupported Media Type) for unsupported request payloads (per OBIE v3.1.2) - was not documented in v1.0 spec
- 503 (Service Unavailable) for deprecated resources (per OBIE v3.1.2)
- ErrorResponseStructure (align with v3.1.2 OBIE but reworded)
- Updated DeliveryAddress class in NZRisk1 to OBPostalAddress8 to align with Technical Decision - 009 - Standardising Address Objects
- Agreed error codes as per Technical Decision - 003 - Error Codes
- References to "account request" to "account access consent"
- Updated third_party_client_credential scope to accounts based on Technical Decision - 021 - OAuth

## v1.0.0 - 26/02/2019

Incorporate changes in preparation for v1.0.0 release, including:

- Version bump to v1.0.0 and URL to v1.0
- Removed `501 Not Implemented` from `/account-requests/{AccountRequestId}` and `/accounts/{AccountId}/transactions`
- Update `PaymentLinks` example to use `v1.0` in path
- Added new risk HTTP headers `x-merchant-ip-address` and `x-customer-user-agent`
- Added new fields `Risk` object as per the Risk Fields page in confluence documentation
- Remove 'QtrDay' in Standing Order Frequency
- Update `ReadPartyPSU` to `ReadPartyAuthUser`
- Removed `OB: reference`
- Replaced UK actor names (ASPSP, TPP, PSU) with NZ names (API Provider, Third Party, Customer)
- Replaced `EquivalentAmount` with `CurrencyExchange`

## V0.3.0 - 02/11/2018

Incorporate upstream V2.0 changes.

- Basepath changed to /open-banking-nz/v0.3
- Added tags to group endpoints
- Refactored Meta to definitions and referenced
- Fixed balances reference and description
- Refactored parameters and objects similar to upstream
- Added Offers, Party, Statements, Scheduled Payments endpoints
  - Scheduled Payments has minor changes to use BECSRemittance, BECSElectronicCredit
- Updated TransactionModel to reflect **undocumented** changes to Transactions structure
  - Not listed in [Key changes](https://openbanking.atlassian.net/wiki/spaces/DZ/pages/127009221/Read+Write+Data+API+Specification+-+v2.0.0)
- Updated AccountModel to reflect **undocumented** changes
- Updated Account Requests to reflect **undocumented** changes
- Updated Beneficiaries to reflect **undocumented** changes
- Incorporated **extensive** ProductModel changes
- Updated StandingOrders to reflect **undocumented** changes
- Added 501 response to new endpoints (reflecting not mandatory)

## V0.2.2 - 26/07/2018

Removed IBAN and Sort Code from AccountSchemeModel - not relevant to NZ market.

## V0.2.1 - 20/07/2018

- Altered `Reference` and `TransactionReference` fields to use BECSRemittance (alignment with Payment API)
  - This affects Transactions, Beneficiaries and StandOrders

## v0.2.0 - 15/06/2018

- Version bump to v0.2.0 indicating re-inclusion of OpenID Connect into authorisation flow
- Basepath changed to /open-banking-nz/v0.2
- Minor correction to enums where 'BECSElectronicCredit' was erroneously 'BECS Electronic Credit' (enum values should not have spaces)

## V0.1.2 - 6/06/2018

Changing URL to open-banking-nz

## V0.1.1 - 7/05/2018

- Refactored the following to models and referenced from API paths:
  - Transaction (TransactionModel)
  - Beneficiary (BeneficiaryModel)
  - Standing Orders (StandingOrderModel)
  - Products (ProductModel)
  - Direct Debits (DirectDebitModel)
- Added 501 response code to specification for endpoints not in MVP scope
- Added account number format description of 2-4-7-2 hyphen-delimited as recommended by Payments NZ working group and accepted by standards sub-group on 4/05/2018. This has been added to both Debtor and Creditor account identifiers in OpenAPI specification
- Creditor Account updated to reflect BECS format accepted (StandingOrderModel, BeneficiaryModel)
- **Open Issue** reference fields in Standing Orders and Beneficiaries are single field, 35 max length. Should these be updated to match BECSRemittance of Payment API?

## v0.1.0 - 23/03/2018

- Changed base path to /open-banking/v0.1
- Version bump specification to v0.1.0 to reflect MINOR change to stabilise for pilot

## V0.0.3 - 16/02/2018

- Added AccountRequestResponseModel - this model wraps the AccountRequestModel (model of account access requests) and adds the required response fields. Previously this was done through read-only attributes, however this causes semantic error when used with 'required'. The AccountRequestsResponseModel is syntactically equivalent, allows for required fields and removes the error condition.

## V0.0.2 - 19/12/2017

- Updated info section (contact, licence info, terms of service)
- Corrected 200 response in GET /accounts/{AccountId} - now users PaymentLinks (previously not actually included)

## V0.0.1 - 12/12/2017

- For discussion: used additional 'link' in GET /accounts and GET /accounts/{AccountId} to indicate account is valid for making payments (see PaymentLinks model in specification)
- Created models in 'definitions' section:
  - BalanceModel
  - AccountModel using AccountSchemeModel
  - AccountSchemeModel - enum that allows 'BECS Electronic Credit' - NZ bank account numbers
  - AccountRequestModel
  - Links
  - PaymentLinks
- Updated GET /accounts and GET /accounts/{AccountId} to use AccountModel and PaymentLinks
- Updated GET /accounts/{AccountId}/balances to use BalanceModel and Links (and since this is referred to in GET /balances this is also consistent
- Updated POST /account-requests request and response to use AccountRequestModel
- Updated GET /account-requests to use AccountRequestModel
- Updated request headers to set JWS signature and financial institution identifier as optional
- Left all other resources intact and unchanged (as per MVP directive) - could benefit from resource models being included in 'definitions later'
