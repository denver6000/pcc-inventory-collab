# Copilot Instructions

Use `CODEX.md` as the primary project context and source of truth for architecture, business rules, and workflow commands.

## Repository guidance
- This is a Laravel 12 inventory + production app.
- Preserve existing controller-driven patterns and Blade UI approach.
- Be careful with stock math and transactional integrity when changing:
  - `app/Http/Controllers/RestockController.php`
  - `app/Http/Controllers/ProduceController.php`
- Prefer minimal, targeted changes.

## Required checks before suggesting completion
1. Run tests with `php artisan test`.
2. If inventory flow changed, validate stock synchronization paths.
3. If frontend changed, include a screenshot in review notes.
