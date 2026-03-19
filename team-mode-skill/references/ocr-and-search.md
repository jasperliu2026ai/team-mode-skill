# OCR and Search Playbook

## OCR Default

When the task is OCR, screenshot text extraction, scanned document reading, or image-to-text:
- Use `paddleocr-text-recognition`
- Prefer its provided script flow over manual vision reading
- If the API/config is broken, show the exact error and stop
- Do not silently switch to a different OCR path

Why this rule exists:
- The team already standardized on this OCR route
- Repeatedly choosing the wrong OCR path causes drift and inconsistent output
- A single default route keeps execution predictable

## Search Middle Platform Default

When the task needs search beyond a single quick lookup, especially for:
- technical troubleshooting
- multi-source aggregation
- image aggregation
- answer-oriented search

Prefer `search-middle-platform`.
Repository: `https://github.com/jasperliu2026ai/search-middle-platform`

Current internal shape:
- `search_hub.js` — unified entry
- `search_router_mvp.js` — route selection
- `tech_search_*` — technical search stack
- `multi_source_search_mvp.js` — general multi-source search
- `search_multi_source_images.js` — image aggregation
- `search_arbiter_mvp.js` — result arbitration
- `search_answer_mvp.js` / `search_answer_nl.js` — final answer synthesis

## Recommended Search Workflow

1. Classify the query into `tech`, `general`, or `image`
2. Route to the right search pipeline
3. Aggregate raw results
4. De-duplicate and arbitrate
5. Produce a concise final answer plus raw result location

## Delivery Warning

Do not confuse retrieval with delivery.
Examples of false completion:
- Raw files exist but the user cannot access them
- An image/file was created but not actually sent
- Search output is noisy and not narrowed to the requested scope

Use user-visible delivery as the finish line.

## Extension Guidance

If extending this team's search capability:
- Keep the hub-router-arbiter-answer shape
- Add sources behind the routing layer rather than hardcoding one-off flows
- Preserve a simple external interface (`query`, optional `mode`)
- Keep beta claims honest; do not oversell source quality or arbitration quality
