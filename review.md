# Code Review

## Findings
- **High – Guideline Violation**: Several publicly exposed functions omit return type annotations, conflicting with `AGENTS.md`'s requirement that every function parameter and return value include explicit typing (`app/app/main.py:33-187`, `app/app/components.py:76-210`, `app/app/forms.py:7-160`, `app/app/layouts.py:3-23`). Please add concrete `-> ...` annotations (e.g., `-> air.Dialog`, `-> str`, `-> air.Form`) so downstream call sites remain type-safe.
- **Medium – Bug Risk**: The inline style on `render_try_card` uses `overflow-auto` instead of a valid CSS property (`overflow: auto;`), so browsers ignore the overflow rule and long text will bleed outside the card (`app/app/components.py:116`). Replace with `style="height: auto; max-height: 250px; overflow: auto;"` to restore the intended scrolling behaviour.
- **Low – Guideline Violation**: `db.load_template_data` and helpers accept `list[dict]` without constraining the dictionary schema, reducing readability and static checking (`app/app/db.py:87-110`). Consider defining typed `TypedDict` structures (or dedicated dataclasses) for template payloads so new entries remain consistent with `MiceCard`/`TryCard` expectations.

## Questions
- Are we expecting to support custom MICE card types beyond `M/I/C/E`? If so, the direct dictionary lookups in `MICE_COLORS`/`MICE_TOOLTIPS` need graceful fallbacks to avoid runtime `KeyError`s.

## Summary
Focus next on tightening type annotations across the view layer and correcting the Try card overflow style regression; both will bring the codebase in line with the documented standards and prevent UI drift.
