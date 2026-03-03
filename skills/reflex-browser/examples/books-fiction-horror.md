# Example: Fiction then Horror (Capture-First + Summary-Driven)

## User Prompt

`$reflex-browser Go to https://books.toscrape.com/index.html - fiction - give a summary of the first 4 books and then go to horror and give a summary of those first 4 books`

## Execution Rules Applied

1. One logical session reused end-to-end.
2. Capture full JSON first for each command, then parse from the captured payload.
3. Use `summary --intent` for navigation selector discovery after each navigation step.
4. Close the session at the end.

## Command Sequence (Bash)

```bash
source skills/reflex-browser/scripts/capture_json.sh

R_START=$(rb_capture start)
S=$(rb_jq "$R_START" '.session // .response.data.session')

R_OPEN=$(rb_capture open "https://books.toscrape.com/index.html" --session "$S")

R_SUM_HOME=$(rb_capture summary 20 --intent "fiction_10 category link" --session "$S")
FSEL=$(rb_pick_selector "$R_SUM_HOME" "fiction_10")
R_CLICK_F=$(rb_capture click "$FSEL" --session "$S")

for i in 1 2 3 4; do
  TITLE=$(rb_jq "$(rb_capture attribute "css=section > div > ol.row > li:nth-of-type(${i}) h3 a" "title" --session "$S")" '.response.data.value')
  PRICE=$(rb_jq "$(rb_capture text "css=section > div > ol.row > li:nth-of-type(${i}) p.price_color" --session "$S")" '.response.data.value')
  echo "Fiction #$i: $TITLE - $PRICE"
done

R_SUM_FICTION=$(rb_capture summary 20 --intent "horror_31 category link" --session "$S")
HSEL=$(rb_pick_selector "$R_SUM_FICTION" "horror_31")
R_CLICK_H=$(rb_capture click "$HSEL" --session "$S")

for i in 1 2 3 4; do
  TITLE=$(rb_jq "$(rb_capture attribute "css=section > div > ol.row > li:nth-of-type(${i}) h3 a" "title" --session "$S")" '.response.data.value')
  PRICE=$(rb_jq "$(rb_capture text "css=section > div > ol.row > li:nth-of-type(${i}) p.price_color" --session "$S")" '.response.data.value')
  echo "Horror #$i: $TITLE - $PRICE"
done

R_LUA=$(rb_capture lua --session "$S")
RAW_SCRIPT=$(rb_jq "$R_LUA" '.response.message')
GUIDANCE=$(rb_jq "$R_LUA" '.response.data.generationGuidance // []')

rb_capture session-kill "$S" --session "$S" >/dev/null
```

## Notes

- This example intentionally avoids direct `jq | head` pipes on live command output.
- Full response capture keeps debugging context intact when selector picks are wrong.
- `RAW_SCRIPT` is trace output; apply `generationGuidance` if a human-usable Lua script is needed.
