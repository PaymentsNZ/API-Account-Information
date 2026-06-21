# Account Information API

## V3.0.3

This is v3.0.3 NZ Open Banking Account Information API technical specification.

The OpenAPI 3.0 document is [here](account-info-nz-openapi.yaml).

### Changes

- Version number updated to reflect confluence update (Risk object UML correction)
- Relaxed Amount pattern from `^\d{1,13}\.\d{1,5}$` to `^\d{1,13}(\.\d{1,5})?$` so the decimal portion is optional
- Refactored the inline `{Amount, Currency}` object (used in 16 places) into reusable components:
  - `ActiveCurrencyAndAmount_SimpleType` — the amount string
  - `ActiveOrHistoricCurrency` — the ISO 4217 currency code
  - `ActiveOrHistoricCurrencyAndAmount` — the composed object
- Replaced standalone ISO 4217 currency code fields (account `Currency`, `SourceCurrency`, `TargetCurrency`, `UnitCurrency`) with references to `ActiveOrHistoricCurrency`
