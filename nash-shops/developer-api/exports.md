# Exports

## Client Exports

### openShop(shopName)
Opens the shop NUI for the specified shop.

| Parameter | Type | Description |
|-----------|------|-------------|
| shopName | string | Shop identifier from config |

### closeShop()
Closes the currently open shop NUI.

## Server Exports

### addShopItem(shopName, item)
Dynamically adds an item to a shop at runtime.

| Parameter | Type | Description |
|-----------|------|-------------|
| shopName | string | Target shop identifier |
| item | table | Item data with name and price |

### removeShopItem(shopName, itemName)
Removes an item from a shop at runtime.
