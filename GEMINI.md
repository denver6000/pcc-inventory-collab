# GEMINI Project Instructions

For full project context, read `CODEX.md` first.

## Quick orientation
- Framework: Laravel 12 + Blade + Eloquent.
- Domain: Items, Products (recipes), Restocks (batches), Produce runs, and ProduceBatchUsage consumption links.
- Critical invariant: `items.stock` should reflect aggregated restock quantities after produce/restock operations.

## Editing priorities
- Follow existing route/controller/model structure.
- Keep stock-changing writes inside transactions.
- Prefer consistency with existing validations and naming.

## Useful commands
- `composer run dev`
- `php artisan migrate`
- `php artisan test`
- `./vendor/bin/pint`
