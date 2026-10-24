---
'@electric-sql/client': patch
---

Stop subset snapshots from tripping fast-loop detection. `requestSnapshot`
pauses the stream, which aborts its in-flight request, and the stream restarts
at the same offset when the snapshot is done. Fast-loop detection counted every
request start, so a stream that was not yet up to date and served a few dozen
snapshots in quick succession was torn down with "Client is stuck in a fast
retry loop". It now counts only requests that got a response; aborted requests
are ignored, so a real loop of repeated responses is still detected.
