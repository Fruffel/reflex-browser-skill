# Example: Fiction then Horror (Single Shell)

## User Prompt

`$reflex-browser Go to https://books.toscrape.com/index.html - fiction - give a summary of the first 4 books and then go to horror and give a summary of those first 4 books`

## Execution Rules Applied

1. One long-running `reflex-browser` shell only.
2. No extra shells for retries or parsing convenience.
3. Sequential JSON commands.
4. Reused server-assigned session id for the whole flow.

## Commands Sent

```json
{"id":"1","action":"open","arg1":"https://books.toscrape.com/index.html"}
{"id":"2","action":"click","arg1":"a[href*='fiction_10']"}
{"id":"3","action":"wait","arg1":"css=section > div > ol.row > li:nth-of-type(1) article.product_pod","arg2":"8000"}
{"id":"4","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(1) h3 a"}
{"id":"5","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(2) h3 a"}
{"id":"6","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(3) h3 a"}
{"id":"7","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(4) h3 a"}
{"id":"8","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(1) .price_color"}
{"id":"9","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(2) .price_color"}
{"id":"10","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(3) .price_color"}
{"id":"11","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(4) .price_color"}
{"id":"12","action":"open","arg1":"https://books.toscrape.com/catalogue/category/books/horror_31/index.html"}
{"id":"13","action":"wait","arg1":"css=section > div > ol.row > li:nth-of-type(1) article.product_pod","arg2":"8000"}
{"id":"14","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(1) h3 a"}
{"id":"15","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(2) h3 a"}
{"id":"16","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(3) h3 a"}
{"id":"17","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(4) h3 a"}
{"id":"18","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(1) .price_color"}
{"id":"19","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(2) .price_color"}
{"id":"20","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(3) .price_color"}
{"id":"21","action":"text","arg1":"css=section > div > ol.row > li:nth-of-type(4) .price_color"}
{"id":"22","action":"session"}
{"id":"23","action":"exit"}
```

## Observed Session

- `shell_ready.session`: `session-b7f5170b`
- `action:"session"` response: `Current session: session-b7f5170b`

## Result Summary

### Fiction (first 4)

1. `Soumission` - `£50.10`
2. `Private Paris (Private #10)` - `£47.61`
3. `We Love You, Charlie ...` - `£50.27`
4. `Thirst` - `£17.27`

### Horror (first 4)

1. `Security` - `£39.25`
2. `Follow You Home` - `£21.36`
3. `The Loney` - `£23.40`
4. `Pet Sematary` - `£10.56`

## Notes

- This example intentionally keeps one shell session for the full task.
- `click` was used for category navigation from home to Fiction.
- Explicit `wait` was used only after category transitions.
