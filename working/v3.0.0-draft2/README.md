# Account Information API

## V3.0.0-draft2

This is v3.0.0-draft2 NZ Open Banking Account Information API technical specification. The document has been converted from Swagger 2.0 to OpenAPI 3.0, using the v2.3 final swagger 2.0 document as the basis.

The `AuthorizationParam` definition and references have been removed.  Header parameters named `Authorization` are [ignored in OpenAPI 3](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.3.md#user-content-parametername); `securitySchemes` and `security` should be used instead.

To align with FAPI 1.0 Basic, section 6.2.1, the `charset=utf-8` has been removed from the content-type of each API.  All JSON data objects are to use UTF-8.

The OpenAPI 3.0 document is [here](account-info-nz-openapi.yaml).
