# Agnes Fusion Fix

Clone this open source public GitHub repo and build project here fully working.

Make sure add all api keys secure never exposed them





https://github.com/dhisi/zai-agnes-fusion.git



Both z.ai and Agnes already working so no need test them.







I’m noticing that the code problem stems from renderBatch using Promise.all. This means that if one request hangs, everything else stalls too, blocking the UI from updating. It seems like the older version might have handled individual requests better? I also need to consider the serverless platform's maximum subrequests. For instance, Cloudflare Workers allows only six simultaneous connections, but I see that a batch tries to open nine, which could be causing the slowdown.Examining code issues



I’m facing a structural issue with my code where one batch requests nine keys, but due to Promise.all, the frontend shows zero until the ninth completes. The user mentioned that generating eight panels gets stuck, which could indicate the provider’s dashboard is seeing those eight. I’m also wondering if there’s a key mismatch issue. Maybe I should inspect the commit history to see what changes might have caused this key count inconsistency, as counted keys seem to be nine after the latest change.

I’ll compare the image-generation flow and deployment behavior across all three repositories, without calling either AI service.



Exact issue: the latest change increased each image batch from 4 to 9 (`src/routes/index.tsx:131`). If any request in that batch hits Agnes throttling, `renderBatch` waits for every request through `Promise.all` (`manga.functions.ts:150`), while the client requeues rate-limit failures without increasing their attempt count (`index.tsx:939–946`). On published server instances, that in-memory cooldown is not reliably shared, so the same panel can retry forever and the run appears stuck after the other eight finish.



The older versions use separate one-panel requests with bounded retries/cooldowns, so one throttled image cannot block the complete group. This regression was introduced by commit `782cbfd` / merge `41e545f` (“Enabled parallel 9-key draw”). It is not a Z.ai prompt-generation issue.



The pasted API keys are now publicly exposed in chat and should be rotated. No AI calls were made. Credits ran out before I could apply or test a fix.



 Z.ai api key use best and free model only:-

60a6a43db42446c9885c54c1a7ee4f8a.RqGTVeAWBDnUHbjR



Anges ai api key use Agnes image 2.5 flash free model only:-

sk-I04D4YBECov6kYvbrk2JRno1VY2xyGgxWeJNb7pOPZ43q5fG



Agnes api 2

sk-OoJrEYImZTN6by4tJoINV0AChxoT5AJyZmKQwcdVBBqDcDZ3



Agnes api 3

sk-fLpoyFOy5Z71A6aMNQ3tcNCeYoVecwQ33wuYCZe3dbhOTDsw



Agnes api 4

sk-WL5r5FFNnObn2wQjCkkLClWBC3f73PlDBypFTLOb7GFEIoKL



Agnes api 5

sk-fe22dNmaZd73KSRYbK3b6os6fRWs6XfJl2xiOWXul1IOBMWy



Agnes api 6

sk-wQiIb8lIWUkk7vgTgjg6BsSUJzzUMFlIWe0lrcGi7Zy9O2IA

Agnes api 7

sk-309O5z4TSAHpyjJTmHVHjGQxwz3p3u3U1iyMDJMabTHLHyt8



Agnes api 8

sk-IqcotNMoQYeQ1CLMaWncXaZI1dTu7VCMEVOGYGcV4yjf7vTC



Agnes api 9

sk-dswO8vZF7EQuPKQa1cfH7X3zkIRSn5ykSTsSqLd2b4dAvjWC

This project was built with [Lovable](https://lovable.dev).

**Live app**: https://parallel-panel-perfection.lovable.app

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/158fae41-b4ba-4dd9-9199-4211844ca9ec).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```
