# CODEX Project Context (pcc-inventory-collab)

This file gives coding agents fast, high-signal context for working in this repository.

## 1) Tech stack
- Laravel 12 (PHP 8.2+), Blade views, Eloquent ORM.
- Frontend build: Vite + Tailwind CSS + Alpine.js.
- Test stack: Pest + Laravel testing helpers.

## 2) Domain summary
This app is an inventory + production tracker with these core concepts:
- **Item**: a raw ingredient/stocked input (`items` table).
- **Unit**: measurement unit for an item.
- **Product**: finished product recipe definition.
- **Product <-> Item pivot**: `item_product` with `quantity_required` per unit produced.
- **Restock**: incoming stock batch rows (`restock` table) with `batch_code` and `batch_date`.
- **Produce**: production records that consume item batches using FIFO-like order.
- **ProduceBatchUsage**: links produced rows to specific restock rows consumed.

## 3) Important business rules
- Inventory/admin routes are behind `auth`; controllers also enforce `is_admin` checks.
- `Item.stock` is treated as a derived value and synchronized from `restock` totals.
- Producing a product:
  - validates recipe exists,
  - computes max producible quantity from stock + recipe,
  - consumes restock quantities in order (`batch_date`, `restock_date`, `id`),
  - writes `ProduceBatchUsage` records,
  - re-syncs affected `Item.stock` values.
- Deleting a production entry restores consumed restock quantities using usage rows.

## 4) Key files to inspect first
- Routes: `routes/web.php`
- Controllers:
  - `app/Http/Controllers/ItemController.php`
  - `app/Http/Controllers/ProductController.php`
  - `app/Http/Controllers/RestockController.php`
  - `app/Http/Controllers/ProduceController.php`
- Models:
  - `app/Models/Item.php`
  - `app/Models/Product.php`
  - `app/Models/Restock.php`
  - `app/Models/Produce.php`
  - `app/Models/ProduceBatchUsage.php`
- Views: `resources/views/{items,products,restock,produce}/*.blade.php`
- Schema/migrations: `database/migrations/*`

## 5) Local workflow commands
- Setup: `composer install && npm install`
- First run: `cp .env.example .env && php artisan key:generate && php artisan migrate`
- Dev servers: `composer run dev`
- Tests: `php artisan test`
- Optional style check: `./vendor/bin/pint`

## 6) Code conventions for agents
- Prefer existing patterns over introducing new architecture.
- Keep validation in controllers unless project starts moving to Form Requests.
- Wrap multi-row stock-changing operations in `DB::transaction(...)`.
- If changing restock or produce logic, verify stock synchronization behavior.
- Use eager loading (`with(...)`) for list pages to avoid N+1 queries.
- For Blade/JS UI tweaks, preserve existing visual style and Alpine approach.

## 7) Safe change checklist
Before finalizing changes touching inventory logic:
1. Confirm CRUD flow still works for affected resource.
2. Confirm `Item.stock` remains consistent after create/update/delete operations.
3. Run `php artisan test`.
4. If UI changed, capture a screenshot for review.

## 8) Notes for future docs
If business rules evolve, update this file first, then mirror brief summaries into tool-specific instruction files.
