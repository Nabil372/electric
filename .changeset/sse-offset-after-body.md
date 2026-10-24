---
'@electric-sql/client': patch
---

Stop skipping rows when an SSE response ends before its up-to-date. The client
took an SSE response's `electric-offset` header as the stream's offset as soon
as the response opened, but applies the body's messages only when an up-to-date
arrives, so a body that broke, was aborted, or was a catch-up chunk ending
without an up-to-date dropped its rows while the next request asked for the
offset after them. In SSE mode the offset now moves only with an up-to-date,
and after a body that ends with messages no up-to-date has flushed, the client
long polls from the same offset until the next up-to-date before returning to
SSE.
