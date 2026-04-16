# How to create a business

Nash Banking supports two kinds of businesses: **personal** (owned by a player, independent from jobs) and **job-linked** (tied to a framework job so all employees of that job inherit access).

## Personal business

Anyone can create one if you allow it.

1. Player runs `/bank`, opens the **Business** tab.
2. Clicks **Create business**, types a name.
3. `Config.Business.CreationCost` (default 50 000€) is debited from the player's bank account.
4. The player is now the boss. They can add employees via identifier from the dashboard.

The business is stored with `job_name = NULL` — no framework job attached.

## Job-linked business (admin only)

Use this when you want an existing framework job (e.g. `police`, `mechanic`, `burger_shot`) to share a business balance.

1. Run `/bankadmin` → **Businesses** tab → **Create business**.
2. Pick a boss (search a player), a business name, and a **Job**. The dropdown is populated from `QBCore.Shared.Jobs` / `ESX.GetJobs()`.
3. All online players whose job matches get access automatically at their matching grade — boss grade → `boss` role, manager/lead grade → `manager`, others → `employee`.

When TPE income with `business_id = X` arrives and `X.job_name` is set, the funds are also written to the framework's society account (ESX `society_<job>`) / management bank (QBCore).

## Adding employees & roles

From the business dashboard, the boss can:

1. **Add** — type an identifier (license:… or citizenid) and a role.
2. **Remove** — click a row → **Remove**.
3. **Change role** — not exposed in the UI. Use SQL:

    ```sql
    UPDATE nash_business_employees
    SET role = 'manager'
    WHERE business_id = 42 AND identifier = 'license:abc…';
    ```

Roles are enforced against `Config.Business.AccessLevel`:

| Role | Level |
|---|---|
| `boss` | 3 |
| `manager` | 2 |
| `employee` | 1 |

`AccessLevel = 'manager'` means bosses + managers can open the dashboard; employees cannot.

## Transferring ownership

Two SQL updates:

```sql
-- 1. Update the owner
UPDATE nash_businesses SET owner_identifier = 'NEW_BOSS_ID' WHERE id = 42;

-- 2. Swap roles
UPDATE nash_business_employees SET role = 'employee' WHERE business_id = 42 AND identifier = 'OLD_BOSS_ID';
UPDATE nash_business_employees SET role = 'boss'     WHERE business_id = 42 AND identifier = 'NEW_BOSS_ID';
```

If the new boss was not an employee yet, insert them first:

```sql
INSERT INTO nash_business_employees (business_id, identifier, role)
VALUES (42, 'NEW_BOSS_ID', 'boss');
```

## Deleting a business

From `/bankadmin` → **Businesses** → delete. Cascade deletes:

- `nash_business_employees` (via FK `ON DELETE CASCADE`)
- `nash_business_transactions` (same)

The business balance is forfeited — there's no refund. If you want to preserve it, move it to the boss first via the dashboard's **Withdraw all** flow.
