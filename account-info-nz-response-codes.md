# Account info API response codes

## HTTP response codes

The following are generic API response codes and do not necessarily reflect the status of accounts information authorisation and request processing.  API consumers **must** check the relevant fields of the account-requests representations as returned for actual processing status.

### Return & error codes

The following are the HTTP response codes for the different HTTP methods - across all Account Info API endpoints.

|Situation|HTTP Status|Notes|Returned by POST|Returned by GET|Returned by DELETE|
|:---|:---|:---|:---|:---|:---|
|Query completed successfully|200 OK||No|Yes|No|
|Normal execution. The request has succeeded.|201 Created|The operation results in the creation of a new resource.|Yes|No|No|
|Delete operation completed successfully|204 No Content||No|No|Yes|
|Account Request has malformed missing or non-compliant JSON body or URL parameters|400 Bad Request|The requested operation will not be carried out.|Yes|No|No|
|Authorization header missing or invalid token|401 Unauthorized|The operation was refused access. Re-authenticating the PSU may result in an appropriate token that can be used|Yes|Yes|Yes|
|Token has incorrect scope or a security policy was violated. Re-authenticating the PSU is unlikely to remediate the situation.|403 Forbidden|The operation was refused access.|Yes|Yes|Yes|
|The TPP tried to access the resource with a method that is not supported.|405 Method Not Allowed||Yes|Yes|Yes|
|The request contained an accept header that requested a content-type other than application/json and a character set other than UTF-8|406 Not Acceptable||Yes|Yes|Yes|
|The operation was refused as too many requests have been made within a certain timeframe. The ASPSP should include a Retry-After header in the response indicating how long the TPP must wait before retrying the operation.|429 Too Many Requests|Throttling is a NFR.|Yes|Yes|Yes|
|Something went wrong on the API gateway or micro-service|500 Internal Server Error|The operation failed.|Yes|Yes|Yes|
|The endpoint (resource) identified by the URL is currently not supported/implemented by the ASPSP|501 Not Implemented|ASPSP has not yet implemented|Yes|Yes|Yes|

An ASPSP **MAY** return other standard HTTP status codes (e.g. from gateways and other edge devices) as described in [RFC 7231 - Section 6.](https://tools.ietf.org/html/rfc7231#section-6)

#### 400 (Bad Request) v/s 404 (Not Found)

When a TPP tries to request a resource URL with an resource Id that does not exist, the ASPSP **must** respond with a 400 (Bad Request) rather than a 404 (Not Found).

E.g., if a TPP tries to GET /accounts/22289 where 22289 is not a valid AccountId, the ASPSP must respond with a 400.

When a TPP tries to request a resource URL that results in no business data being returned (e.g. a request to retrieve standing order on an account that does not have standing orders) the ASPSP **must** respond with a 200 (OK) and set the array to be empty.

If the TPP tries to access a URL for a resource that is not defined by these specifications (e.g. GET /card-accounts), the ASPSP **may** choose to respond with a 404 (Not Found).

If an ASPSP has not implemented an optional API, it **must** respond with a 404 (Not Found) for requests to that URL.

The table below illustrates some examples of expected behaviour:

|Situation|Request|Response|
|:---|:---|:---|
|TPP attempts to retrieve an account with an AccountId that does not exist|GET /accounts/1001|400 (Bad Request)|
|TPP attempts to retrieve a resource that is not defined|GET /credit-cards|404 (Not Found)|
|TPP attempts to retrieve a resource that is in the specification, but not implemented by the ASPSP.|GET /direct-debits|404 (Not Found)|
|E.g., an ASPSP has chosen not to implement the bulk direct-debit endpoint, TPP attempts to retrieve standing orders for an AccountId that does not exists|GET /accounts/1001/standing-orders|400 (Bad Request)|
|TPP attempts to retrieve standing orders for an AccountId that exists, but does not have any standing orders|GET /accounts/1000/standing-orders|200 OK<br><code>\{<br>&nbsp;&nbsp;"Data": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"StandingOrder": []<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"Links": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"Self": "/accounts/1000/standing-orders/"<br>&nbsp;&nbsp;},<br>&nbsp;&nbsp;"Meta": {<br>&nbsp;&nbsp;&nbsp;&nbsp;"TotalPages": 1<br>&nbsp;&nbsp;  }<br> }</code>|

#### 403 (Forbidden)

When a TPP tries to access a resource that it does not have permission to access, the ASPSP **must** return a 403 (Forbidden).

The situation could arise when:

* The TPP uses an access token that does not have the approporiate scope to access the requested resource.
* The TPP does not have a consent authorisation for the AccountId
    E.g., an attempt to access GET /accounts/2001 or /accounts/2001/transactions when the PSU has not selected AccountId 2001 for authorisation.
* The TPP does not have a consent authorisation with the right Persmissions to access the requested resource.
    E.g., an attempt to access GET /standing-orders when the ReadStandingOrdersBasic permission was not included in the consent authorisation.
* The TPP attempted to access a resource with an Id that it does not have access to.
    E.g., an attempt to access GET /account-requests/1001 where an account-request resource with Id 1001 belongs to another TPP.

When the TPP uses an access token that is no longer valid, the situation could potentially be remedied by asking the PSU to re-authenticate. This should be indicated by a 401 (Unauthorized) status code.

#### 429 (Too Many Requests)

When a TPP tries to access a resource too frequently the ASPSP may return a 429 (Too Many Requests).  This is a Non Functional Requirement and is down to individual ASPSPs to decide throttling limits.

This situation could arise when:

* The TPP has not implemented caching, it requests transactions for a PSU account, and constantly re-requests the same transactions
* Similarly for any of the PSU information endpoints

## Account Request status by resource

### Resources in use as part of pilot

The following table lists the resources that are in scope of the pilot, followed by any specific request processing status codes.  All other resources included in the Swagger / OpenAPI specification (but not part of the pilot) are to return HTTP response code `501 Not Implemented`.

||Resource|HTTP Method|Endpoint|Mandatory?|Idempotent|
|:---|:---|:---|:---|:---|:---|
|1|account-requests|POST|`/account-requests`|Mandatory|No|
|2|account-requests|GET|`/account-requests/{AccountRequestId}`|Mandatory||
|3|account-requests|DELETE|`/account-requests/{AccountRequestId}`|Mandatory|Yes|
|4|accounts|GET|`/accounts`|Mandatory||
|5|accounts|GET|`/accounts/{AccountId}`|Mandatory||
|6|balances|GET|`/accounts/{AccountId}/balances`|Mandatory||

### POST /account-requests

Create an Account Request
`POST /account-requests`
The API allows the AISP to ask an ASPSP to create a new **account-request** resource.

* This API effectively allows the AISP to send a copy of the consent to the ASPSP to authorise access to account and transaction information.
* For v1.0 - we have made a decision to remove functionality for an AISP to pre-select a set of accounts (i.e., the SelectedAccounts block). This is because the behaviour of the pre-selected accounts, after authorisation, is not clear form a Legal perspective at the time of writing the spec.
* The ASPSP creates the account-request resource and responds with a unique AccountRequestId to refer to the resource.
* Prior to calling the API, the AISP must have an access token issued by the ASPSP using a client credentials grant.

#### Account Request Status

The account-request resource that is created successfully must have one of the following Status code-list enumerations:

||Status|Description|
|:---|:---|:---|
|1|Rejected|The account request has been rejected.|
|2|AwaitingAuthorisation|The account request is awaiting authorisation.|

### GET /account-requests/\{AccountRequestId\}

GET an Account Request by its ID
`GET /account-requests/{AccountRequestId}`
An AISP can optionally retrieve a **account-request** resource that they have created to check its status.

Prior to calling the API, the AISP must have an access token issued by the ASPSP using a client credentials grant.

#### Account Request Status

Once the PSU authorises the account-request resource - the Status of the account-request resource will be updated with "Authorised".

The available Status code-list enumerations for the account-request resource are:

||Status|Description|
|:---|:---|:---|
|1|Rejected|The account request has been rejected.|
|2|AwaitingAuthorisation|The account request is awaiting authorisation.|
|3|Authorised|The account request has been successfully authorised.|
|4|Revoked|The account request has been revoked via the ASPSP interface.|

### DELETE /account-requests/\{AccountRequestId\}

DELETE Account Request
`DELETE /account-requests/{AccountRequestId}`
If the PSU revokes consent to data access with the AISP - the AISP should delete the **account-request** resource.

* This is done by making a call to DELETE the account-request resource.
* Prior to calling the API, the AISP must have an access token issued by the ASPSP using a client credentials grant.

### GET Authorised Accounts

GET All Authorised Accounts
`GET /accounts`
**The first step for an AISP after an account-request is authorised - is to call the GET /accounts endpoint.**

This will give the full list of accounts (the AccountId(s)) that the PSU has authorised the AISP to access. The AccountId(s) returned can then be used to retrieve other resources for an account. The selection of authorised accounts for v1.0 happens **only** at the ASPSP's interface.

The AISP will use an access token associated with the PSU issued through an authorization code grant.

### GET Resources for a Specific Account

GET Resource for Specific Account
`GET /accounts/{AccountId}`
`GET /accounts/{AccountId}/balances`

An AISP can retrieve the account information resources for the `AccountId` (which is retrieved in the call to `GET /accounts`).

The AISP will use an access token associated with the PSU issued through an authorization code grant.