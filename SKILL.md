# iGreet Claude Skill

Use iGreet tools to create greeting cards. Prefer draft-first; schedule or send only when asked.

## Rules

1. **Pick design first:** `list_card_categories` → `list_cards` → pass `card` (catalog id). Do not confuse with `list_my_cards`.
2. **Find existing cards:** `list_my_cards` returns your created instances (`cardId`, status, `editUrl`). B2C shows **paid only** (unpaid checkout drafts are hidden). Panel shows workspace creations. Filter with `status` (`all|draft|scheduled|sent|archived`).
3. **Ask card product type (B2C):** before `create_igreet_card`, confirm **digital** (solo) vs **digital_group** (collaborative). Pass `delivery_type`. Do not guess if unclear.
4. **Who to greet:** `get_upcoming_occasions`, `list_reminders` / `get_reminder`; panel also `search_contacts` / `search_team_members`. Recurring reminders expand into every occurrence inside the requested `days` window.
5. **Reminders:** `create_reminder` / `update_reminder` (B2C + panel). Pass `name`, `occasion` (or `occasion_type`), `date`. Optional `remind_date` (defaults to date−7d), `repeat_interval` (`none|weekly|monthly|yearly`). Panel may link `contact_id`.
6. **Contacts (panel only):** `create_contact` / `update_contact` with `name` + `email`. Duplicate email → `contact_email_exists` (use update). Over plan contact limit → `plan_limit_reached`.
7. **B2C bundles:** a matching card bundle is required for that `delivery_type`. Create fails with `bundle_required` (no draft created). Call `get_bundle_purchase`, show **purchaseUrl**, wait for purchase, retry. Check remaining bundles with `get_available_bundles`.
8. **Panel limits (not bundles):** `create_panel_card` uses subscription plan limits (e.g. starter/trial ~50 creations per user per month; higher plans unlimited). Over limit → `422 plan_limit_reached` (no draft). Bundles APIs return `bundles_not_applicable` for panel tokens.
9. **Consumer (B2C):** `create_igreet_card` / `update_card_recipient` with `recipient` + `recipient_email` + required `delivery_type`.
10. **Panel (B2B):** `create_panel_card` / `update_panel_recipients` with `recipients: [{ name, email, contact_id? }]`.
11. **Messages (cards can have several texts):**
    - `list_card_messages` → `element_id` + content
    - `update_card_message` with `element_id` (omit = primary text)
    - `add_card_message` / `delete_card_message`
12. **Delete card:** `delete_card` — B2C soft-deletes (hidden from My Cards); panel permanently removes the creation.
13. Always show **editUrl**. Do not claim sent unless send succeeded.
14. **Sent-card locks (match web):** recipients/sender locked after send (`recipient_locked` / `sender_locked`). Panel scheduled: cannot add/remove emails (`schedule_recipients_locked`; rename in place OK). Message edits lock after send for **physical** only; **digital** stay editable. Check `recipients_locked` / `messages_locked` on `get_card_status` / `list_my_cards`.
15. **Schedule / send:** both B2C and panel need the **`send`** token scope (`schedule_card` / `send_card`). `send_at` ≥ 1 hour ahead. Optional: `suggest_greeting` for message ideas; `list_card_fonts` for allowlisted fonts.

## Naming

- `card` — catalog design id from `list_cards` (pass on create)
- `cardId` — your card instance from `create_*` or `list_my_cards` (update / schedule / send / delete / status)
- `delivery_type` — `digital` or `digital_group` (required on B2C create)
- `purchaseUrl` — B2C browser link to buy a card bundle (`/bundles/buy`)
