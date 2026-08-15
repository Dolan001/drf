---
name: implement-drf-realtime
description: Implement requirement-backed Django Channels WebSocket chat, notifications, presence, or live events with Redis and PostgreSQL durability. Use for DRF slices containing realtime behavior.
---

# Implement DRF realtime

Complete the realtime capability group and the ordinary DRF vertical-slice contract. Keep consumers
as protocol adapters; services own commands/transactions and selectors own resync/history. Require
independent realtime evidence before handoff.

Read `references/django-channels.md` for every realtime slice. Also read the vertical-slice
production and database references when persistence, attachments, notifications, or tasks change.
