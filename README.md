# Orbit Outfitters — Sample Schema for Oracle APEX

The sample tables, data, and code used in the Oracle APEX tutorials on
[vinish.dev](https://vinish.dev). Install it once and every query, trigger, and
PL/SQL snippet in those articles runs against real data.

Orbit Outfitters is a fictional outdoor equipment retailer: customers, products,
orders, order lines, suppliers, warehouses, stores, and employees, with about
2,300 orders of realistic sample data.

## Requirements

- Oracle Database 19c or later, with Oracle APEX installed.
- Oracle AI Database 26ai for the `BOOLEAN` columns. On 19c, change the
  `boolean` columns in `orbit/01_tables.sql` to `varchar2(1)` with a check
  constraint.
- A schema to own the objects, here called `ORBIT`, with `CREATE SESSION`,
  `CREATE TABLE`, `CREATE VIEW`, `CREATE SEQUENCE`, `CREATE PROCEDURE`, and a
  tablespace quota.

## Install

Run everything as the owning schema, from SQLcl, SQL Developer, or
SQL Workshop:

```sql
@orbit/install.sql
```

That runs the scripts below in order and takes about a minute, most of it
loading the sample data.

| Script | Creates |
|---|---|
| `orbit/01_tables.sql` | The `ORB_*` tables, constraints, and indexes |
| `orbit/02_logic.sql` | Triggers, the `ORB_SALES` package, and the views |
| `orbit/03_data.sql` | The sample data |
| `orbit/06_auth.sql` | `ORB_APP_USERS` and the `ORB_AUTH` package for custom authentication |
| `orbit/07_duality.sql` | The JSON duality view over orders |

Three scripts are optional and are not part of `install.sql`:

| Script | Creates |
|---|---|
| `orbit/04_stores.sql` | The `ORB_STORES` table, for the interactive grid and map tutorials |
| `orbit/05_product_images.sql` | Loads the product images in `orbit/data/product-images` |
| `orbit/uninstall.sql` | Drops everything |

`orbit/06_auth.sql` hashes passwords with PBKDF2, so the owning schema needs
`execute` on `sys.dbms_crypto`:

```sql
grant execute on sys.dbms_crypto to orbit;
```

## What is in the schema

| Table | Holds |
|---|---|
| `ORB_CUSTOMERS` | Customers, their type, loyalty tier, credit limit, and sales rep |
| `ORB_ORDERS` | Order headers with status, channel, discount, and totals |
| `ORB_ORDER_ITEMS` | Order lines, priced and totaled by triggers |
| `ORB_PRODUCTS` | Products with prices, stock, images, and reorder levels |
| `ORB_CATEGORIES` | Product categories, as a hierarchy |
| `ORB_SUPPLIERS`, `ORB_WAREHOUSES` | Suppliers and warehouses |
| `ORB_EMPLOYEES` | Sales representatives |
| `ORB_STORES` | Retail stores, with coordinates for the map tutorial |

Views such as `ORB_ORDERS_V`, `ORB_PRODUCTS_V`, and `ORB_SALES_BY_MONTH_V` join
these together for reports and charts, and the `ORB_SALES` package holds the
business rules: approving, submitting, and cancelling orders, recalculating
totals, and the customer lifetime value.

## Examples

`examples/` holds the exact snippet used in each article, named by the chapter
it belongs to, so you can copy it straight into APEX:

- `examples/ch16-top-customers.sql` and the other `chNN-*.sql` queries
- `examples/ch18-flagships.js`, `ch22-donut-init.js`, and the other JavaScript
- `examples/ch16-tier-badge.css` and `examples/static/orbit.css`
- `examples/static/orbit.js`, the shared namespace loaded as a static
  application file
- `examples/data/price-update.csv` for the data loading tutorial

## License

Sample data and code, free to use and adapt. The data is fictional: any
resemblance to a real company is coincidental.
