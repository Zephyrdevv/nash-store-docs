# Client Events

Events broadcasted from the server to the client (or locally on the client).

## TPE flow

### `nash_banking:openTPE`

Opens the TPE overlay with the given amount, description and cards.

_Payload, example listener — filled in Pass 2._

### `nash_banking:tpeResult`

Notifies the buyer of the payment outcome.

_Filled in Pass 2._

### `nash_banking:buyerReceiveTPE`

Specifically fired for P2P flows — buyer receives the TPE in their hand.

_Filled in Pass 2._

### `nash_banking:sellerTransferOut`

Tells the seller to play the give animation and remove the prop.

_Filled in Pass 2._

### `nash_banking:sellerCleanup`

Tells the seller to remove the prop after an error.

_Filled in Pass 2._

## UI / balance

### `nash_banking:updateBalance`

Updates the bank / cash balance shown in the UI.

_Filled in Pass 2._

### `nash_banking:notification`

Displays an in-UI notification toast.

_Filled in Pass 2._

### `nash_banking:inspectCard`

Opens the card inspection UI with the given metadata.

_Filled in Pass 2._

## Other events

_Filled in Pass 2._
