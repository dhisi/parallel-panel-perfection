# Roadmap

- [x] Import the public GitHub project (spark-tale-flair) into this workspace
- [x] Store the Z.ai and Agnes keys as encrypted server-only secrets (never in code, never in the browser)
- [x] Keep Z.ai on glm-4.5-flash (free) and Agnes on agnes-image-2.5-flash (free)
- [x] Replace the rate-limit handling: one adaptive, cross-tab image budget in the page
      (src/lib/image-rate.ts) plus a fast-failing server safety gate, so the run never
      freezes for 15-20 minutes after a limit
- [x] Remove renderBatch from the live panel path, distribute isolated requests by slot,
      and pause every drawing lane together when Agnes returns a rate limit
- [x] Honor Agnes 1015 Retry-After exactly and resume through one lane, preventing the
      repeated 20-second probes that kept extending a 10-minute provider block
