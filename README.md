# nfourteen/aggregate-product-metapackage

Installs the Nfourteen Aggregate Product suite for Magento 2. Ships no code of its own - it only declares the dependency set.

> [!WARNING]
> **Beta - not for production use.** The Aggregate Product suite is in beta, and this applies to every package this metapackage installs. There are no tagged releases yet, known bugs are still being fixed, and behavior, configuration and stored data may change between commits without an upgrade path. Use it in development or staging environments only.

| Package | Module | Purpose |
| --- | --- | --- |
| `nfourteen/module-aggregate-product` | `Nfourteen_AggregateProduct` | The `aggregate` product type: sell groups of products at fixed quantities |
| `nfourteen/module-aggregate-product-graph-ql` | `Nfourteen_AggregateProductGraphQl` | GraphQL query and add-to-cart support |
| `nfourteen/module-aggregate-product-import-export` | `Nfourteen_AggregateProductImportExport` | CSV import/export of aggregate child links |
| `nfourteen/module-inventory-aggregate-product` | `Nfourteen_InventoryAggregateProduct` | MSI integration: salability, reservations, source deduction, return-to-stock |

## Requires MSI

`Nfourteen_InventoryAggregateProduct` depends on Magento Inventory, so requiring this metapackage pulls MSI into your project whether or not you were already using it. If you run legacy `Magento_CatalogInventory` stock with the MSI modules disabled, **this is not the package you want**: that configuration is untested, and while `Nfourteen_AggregateProduct` alone may work against legacy stock, this is unverified. Require the individual modules you need and plan to test them yourself.

Needs PHP >= 8.3 and Magento Open Source 2.4.x with MSI enabled.

## Installation

These packages are not on Packagist, so every Git remote needs its own `vcs` entry - all five of them, because the metapackage's `require` list says what it needs, not where each one lives:

```json
{
    "repositories": {
        "nfourteen-aggregate-product-metapackage": {
            "type": "vcs",
            "url": "git@github.com:nfourteen/aggregate-product-metapackage.git"
        },
        "nfourteen-aggregate-product": {
            "type": "vcs",
            "url": "git@github.com:nfourteen/module-aggregate-product.git"
        },
        "nfourteen-aggregate-product-graph-ql": {
            "type": "vcs",
            "url": "git@github.com:nfourteen/module-aggregate-product-graph-ql.git"
        },
        "nfourteen-aggregate-product-import-export": {
            "type": "vcs",
            "url": "git@github.com:nfourteen/module-aggregate-product-import-export.git"
        },
        "nfourteen-inventory-aggregate-product": {
            "type": "vcs",
            "url": "git@github.com:nfourteen/module-inventory-aggregate-product.git"
        }
    }
}
```

Composer falls through to Packagist for any name it cannot find in the repositories above, and none of these names are registered there, so someone else could register one. To close that off, exclude the namespace:

```json
{
    "repositories": {
        "packagist.org": {
            "type": "composer",
            "url": "https://repo.packagist.org",
            "exclude": ["nfourteen/*"]
        }
    }
}
```

Then:

```bash
composer require nfourteen/aggregate-product-metapackage
bin/magento module:enable \
    Nfourteen_AggregateProduct \
    Nfourteen_AggregateProductGraphQl \
    Nfourteen_AggregateProductImportExport \
    Nfourteen_InventoryAggregateProduct
bin/magento setup:upgrade
```

## Versioning

Versioned independently of the modules it requires. A tag is cut here when the supported combination of module versions changes.
