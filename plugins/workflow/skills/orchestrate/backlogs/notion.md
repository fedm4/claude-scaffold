# Backlog tool: Notion

Notion-specific mechanics for the `orchestrate` skill. The board URL and the
docs page live in the project's `CLAUDE.md`.

## Reading an item
Fetch the item with `notion-fetch`, then its comments with
`notion-get-comments` (`include_all_blocks: true`, `include_resolved: true`)
in the same batch. A comment can change scope or carry a decision that isn't
in the item body — never skip it.

## Database and properties
Fetch the database first to get its data source id and the exact property
names before creating or querying items — don't guess them. Create new items
under that data source. Status values are usually `Not started` /
`In progress` / `Done`; confirm against the database instead of assuming.

## Splitting a plan into items
Number split items in order (`1.1`, `1.2`, `2.1`, …) and set their Priority.
Design decisions for the whole plan go in the parent/epic item, not
repeated across the split items.

## Closing an item
Two calls, in this order:
1. `notion-update-page` with `insert_content`: the conclusion (what was
   done, check result, PR link).
2. `notion-update-page` with `update_properties`: Status → `Done`.

Never flip the status before the conclusion is written.

## Reports
A report or audit is a new child page under the docs page named in
`CLAUDE.md`, titled with the date. The backlog item gets a short conclusion
that links to it — never the full report inline in the item.
