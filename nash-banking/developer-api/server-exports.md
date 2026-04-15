# Server Exports

Exports callable from any server-side script via `exports.nash_banking:<name>(...)`.

## `CardPayment(source, amount, description)`

Triggers an animated TPE on the player's screen and charges their bank account when they confirm with a card.

_Signature, return values, example — filled in Pass 2._

## `HasActiveCard(source)`

Returns `true` if the player has at least one active (non-frozen) card.

_Signature, return values, example — filled in Pass 2._

## `InspectCard(source, metadata)`

Opens the card inspection UI on the client from a metadata table (used by the physical card item).

_Signature, return values, example — filled in Pass 2._

## Other exports

_List of any additional server exports — filled in Pass 2._
