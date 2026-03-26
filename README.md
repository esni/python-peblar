## Summary

Make `CustomerUpdatePackagePubKey` optional in `PeblarSystemInformation`.

Some white-label Peblar devices (for example ChargePoint-branded variants) do not include this field in `/api/v1/system/information`. The current strict model crashes deserialization with a `mashumaro.exceptions.MissingField` error.

## Changes

- Change `customer_update_package_public_key` from required `str` to optional `str | None` with `default=None`.
- Add regression test that:
  - loads a realistic `system_information` payload fixture,
  - removes `CustomerUpdatePackagePubKey`,
  - verifies parsing still succeeds and value becomes `None`.
- Add fixture file used by that regression test.

## Why

This keeps backward compatibility for devices that return the field, while making parsing tolerant for white-label firmware variants that omit it.

## Validation

- Ran: `pytest -o addopts= tests/test_peblar.py -q`
- Result: `5 passed`
