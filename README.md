# DemoGate

**A public, synthetic dbt fixture repository for LineageGate demonstrations.**

DemoGate provides a deliberately small and reviewable pull-request surface for
testing [LineageGate](https://github.com/puri-adityakumar/lineagegate). Its
committed dbt manifest represents fictional `customers` and `orders` models so a
GitHub App can receive real webhook events and publish real Checks without using
production code, warehouse rows, or customer data.

## Purpose

LineageGate needs a safe repository in which it can demonstrate a complete
governance workflow:

1. A pull request proposes a breaking manifest change.
2. LineageGate compares the base and head artifacts.
3. DataHub evidence reveals missing or affected catalog context.
4. The GitHub Check fails closed and requests remediation or approval.
5. A corrected commit creates a new, independently evaluated run.
6. Approval on the old commit becomes stale.
7. Current-commit approval allows the same Check to succeed.

This repository exists only to supply that synthetic GitHub-side input. The
LineageGate repository contains the application, tests, evidence, and final demo.

## Contents

```text
target/manifest.json   synthetic dbt manifest committed as a demo artifact
README.md              fixture scope, safety rules and usage notes
```

The fixture currently includes:

- `model.demo.customers`
- `model.demo.orders`
- a synthetic `not_null_customers_email` test
- fictional columns and dependencies used to create additive and breaking diffs

`orders.legacy_order_key` is intentionally represented in the GitHub fixture
while the showcase DataHub schema can omit it. That controlled mismatch proves
that LineageGate treats catalog drift as an evidence gap and fails closed instead
of silently assuming the change is safe.

## How to use it

Create a branch and edit only `target/manifest.json` to model the intended change.
Open a pull request and allow the installed LineageGate GitHub App to process the
event. Typical scenarios include:

- removing a column;
- changing a column type;
- adding or removing a dependency;
- removing a test;
- restoring a breaking field as remediation.

Do not place generated warehouse results, profiles, credentials, compiled SQL
from a private project, or real organization metadata in this repository.

## Verified demonstration

The correlated signed-webhook run used for the LineageGate demo is documented in:

- [Phase 7 evidence](https://github.com/puri-adityakumar/lineagegate/tree/main/docs/evidence/phase-07-webhook-e2e)
- [Evidence manifest](https://github.com/puri-adityakumar/lineagegate/blob/main/docs/evidence/phase-07-webhook-e2e/evidence-manifest.json)
- [Final 2:15 demo](https://github.com/puri-adityakumar/lineagegate/blob/main/demo/renders/lineagegate-final.mp4)

The evidence package records the breaking SHA, remediation SHA, failed and
successful Check states, stale and current approvals, DataHub readback, and
durable audit reconstruction.

## Safety boundaries

- All model names, columns, dependencies, and tests are synthetic.
- No production source code or warehouse data belongs here.
- No API key, GitHub App key, webhook secret, token, or database URL belongs here.
- `target/manifest.json` is intentionally committed; other generated dbt output
  should remain untracked unless a future scenario explicitly requires it.
- Pull requests may intentionally break the fixture and should not be treated as
  production changes.

For implementation details, local setup, security invariants, and test commands,
see the [LineageGate README](https://github.com/puri-adityakumar/lineagegate#readme).
