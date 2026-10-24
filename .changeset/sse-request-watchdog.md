---
'@electric-sql/client': patch
---

Apply the `liveRequestTimeoutMs` watchdog to live SSE requests. A half-open
SSE connection (for example after a phone changes networks) could hold the
stream with no updates until TCP gave up, because the watchdog only covered
live long polls and refresh catch-ups. An SSE request is now restarted when it
receives nothing for `liveRequestTimeoutMs`: the timer starts with the request
and re-arms when the headers arrive and on every body chunk, including the
server's keep-alive comments, so a healthy idle stream stays open.
