---
'@electric-sql/client': patch
---

Stop counting an SSE response that delivered messages as a short connection.
The server closes a live SSE request that is behind the log end as soon as it
has sent the catch-up, so a burst of writes ended several SSE responses in a
row within milliseconds and the client fell back to long polling for the rest
of the stream's life, logging the proxy-buffering warning. A short response now
counts toward the fallback only when its body carried no messages.
