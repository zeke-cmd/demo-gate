# demo-gate

Public synthetic fixture repository for LineageGate webhook demonstrations.

The default branch is the trusted baseline. It contains generated dbt manifest
fixtures for `customers` and `orders`; no production source, credentials, or
warehouse rows belong in this repository.

`orders.legacy_order_key` is intentionally absent from the showcase DataHub
schema so the demo can prove that catalog drift fails closed instead of being
treated as safe evidence.
