---
'@electric-sql/client': patch
---

Stop counting an SSE response that moved the stream's offset as a short
connection. The server closes a live SSE request that is behind the log end as
soon as it has sent the catch-up, so a burst of writes ended several SSE
responses in a row within milliseconds and the client fell back to long polling
for the rest of the stream's life, logging the proxy-buffering warning. A short
response that leaves the offset where the request started, such as a replay of
the same operations, still counts toward the fallback.
