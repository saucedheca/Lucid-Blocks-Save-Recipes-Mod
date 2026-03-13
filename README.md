# Description
Automatically records every fusion (Apotheosis) the player performs and
lets them browse the saved recipes in-game by pressing R.

When the player fuses items, the mod hooks into the Fuser's
metadata_changed signal — which fires synchronously before the source
inventory is cleared — to snapshot the ingredients and re-derive the
result. Lame fusions (where the result is one of the input items) are
skipped. Duplicate recipes (same result + same multiset of ingredients)
are never stored twice. All discovered recipes are persisted to disk and
reloaded the next time the game starts.

Press R while in-game to open or close the saved recipes panel.

# Features
- Automatic recipe capture: every non-lame fusion is recorded the
  moment it completes, without any player action.
- Deduplication: a recipe is only stored once even if the same
  combination is fused multiple times.
- Persistent storage: recipes are saved to user://save_recipes.json
  and survive game restarts.
- Results panel (R key): a scrollable list of unique result items,
  each shown as an icon + name button. Result items that appear in
  multiple saved recipes are listed only once.
- Per-result recipe list: clicking a result opens a second panel
  showing every saved recipe that produces it. Each recipe is
  displayed as a row of read-only ingredient slots (icon, name, ×count)
  matching the style of the Fusion Browser.
- Back button: returns from the per-result view to the full results list.

# Save file format
user://save_recipes.json  —  JSON array of recipe objects:
  [
    {
      "result_id":    <int>,
      "result_count": <int>,
      "ingredients":  [ { "id": <int>, "count": <int> }, ... ]
    },
    ...
  ]
Ingredients within each recipe are sorted by id (ascending).

# Notes
- If the JSON save file is corrupt or missing it is silently ignored and
  an empty recipe list is started.
