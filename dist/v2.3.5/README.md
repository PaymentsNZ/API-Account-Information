# Account Information API

## V2.3.5

This is v2.3.5 NZ Open Banking Account Information API technical specification.

The Swagger 2.0 document is [here](account-info-nz-swagger.yaml).

### Changes

account-info-nz-swagger.yaml
- Relaxed Amount pattern from `^\d{1,13}\.\d{1,5}$` to `^\d{1,13}(\.\d{1,5})?$` so the decimal portion is optional
- Refactored the inline `{Amount, Currency}` object (used in 16 places) into reusable definitions:
  - `ActiveCurrencyAndAmount_SimpleType` — the amount string
  - `ActiveOrHistoricCurrency` — the ISO 4217 currency code
  - `ActiveOrHistoricCurrencyAndAmount` — the composed object
- Replaced standalone ISO 4217 currency code fields (account `Currency`, `SourceCurrency`, `TargetCurrency`, `UnitCurrency`) with references to `ActiveOrHistoricCurrency`
