# Installation Overview

A five-step quick start. Each step has its own sub-page with full details.

## Quick start

1. [Install dependencies](requirements.md)
2. [Import the SQL file](database.md)
3. [Register the `ox_inventory` items](ox-inventory.md)
4. [Stream the props (card + TPE)](streamed-props.md)
5. (Optional) [Install the LB Phone apps](lb-phone.md)

Once done, head to [First Launch](first-launch.md) to verify everything is working.

## Folder layout after install

```
resources/
├── nash_banking/              — main resource (required)
├── nash_banking_phone/        — LB Phone app for personal banking (optional)
└── nash_businessbanking_phone/— LB Phone app for business banking (optional)
```

## Load order

{% hint style="warning" %}
Nash Banking must start **after** your framework, `ox_lib`, `oxmysql`, and `ox_inventory`.
{% endhint %}

```
ensure ox_lib
ensure oxmysql
ensure ox_inventory
ensure es_extended        # or qb-core / qbx_core
ensure nash_banking
ensure nash_banking_phone
ensure nash_businessbanking_phone
```
