# QBCore

## Supported versions

_Exact QBCore versions tested — filled in Pass 2._

## Auto-detection

_How QBCore is detected (`qb-core` resource state) — filled in Pass 2._

## Money accounts

_How `PlayerData.money.bank` and `PlayerData.money.cash` are mapped — filled in Pass 2._

## Player names

QBCore stores first/last name inside `charinfo`. The bridge handles the JSON decoding automatically via `Bridge.GetFirstName` / `Bridge.GetLastName`.

## Admin / permission detection

_How `Bridge.IsAdmin` is resolved on QBCore — filled in Pass 2._

## Known quirks

_Timing issues with async MySQL in callbacks and how they are avoided (`MySQL.update.await`) — filled in Pass 2._
