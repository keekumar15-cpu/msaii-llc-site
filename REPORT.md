# REPORT - MSAII LLC one page site

## Status per part

Top / hero: DONE
  evidence: `Invoke-WebRequest http://localhost:4173/` -> `HTTP 200 bytes=20080`; Chrome headless 1440x900 screenshot shows `MSAII LLC`, the headline, one line under it, the booking button, the WhatsApp button and the speaking photo filling the right 60% of the first screen edge to edge.

Problem: DONE
  evidence: the problem section carries a heading and three short paragraphs in the client's own words (point of sale export, marketplace seller account, the bookkeeper's spreadsheet, the month is gone, the late file nobody reads); confirmed present by `problemParagraphs: 3` from the DOM evaluation.

Services: DONE
  evidence: 3 cards rendered, each words + one inline SVG icon; `$900 a project` printed once.

How it works: DONE
  evidence: `steps: 3` from the DOM evaluation.

About: DONE
  evidence: `aboutHeading:"Why I started MSAII LLC"`, `aboutParagraphs:2`, working photo beside the text, round profile photo beside the name S. Kumar.

Contact: DONE
  evidence: three links only, no form: booking first, then WhatsApp `https://wa.me/14154802147?text=...`, then `mailto:s.kumar@msaii.com?subject=...`.

Logo, header and footer: DONE
  evidence: header `images/logo.jpeg` at 40px with the MSAII LLC wordmark; footer `images/logo-dark.jpeg` at 40px on Charcoal Black.

Pictures and missing-picture behaviour: DONE
  evidence: renamed `working.jpeg` aside -> `hiddenImages:["images/working.jpeg"]`, `aboutHeading:"Why I started MSAII LLC"`, `aboutParagraphs:2`; restored.

GitHub repository: DONE
  evidence: see the repository link in the chat message and `git push` output in the terminal.

Live Vercel deploy: NOT DONE - not asked for. The member imports the repository on Vercel themselves.

## What broke and how I fixed it

1. Screenshots looked unchanged after edits. Chrome had cached the page. Fixed by adding a `?v=` cache-busting query to every screenshot URL.
2. The header logo looked tiny. The member's logo file is a 1024x1024 square with the mark in a middle band, so at 40px the mark was about 8px. Fixed by scanning the file for its content box (x:102-920, y:392-632) and saving tight crops as the site's `logo.jpeg` and `logo-dark.jpeg`.
3. The light logo showed a warm box on the cream header. The logo background is #FFF7EB, not white. Fixed with `mix-blend-mode: darken`, which drops the light background into the cream exactly. The footer logo uses `lighten` so its near-black background sits on the Charcoal Black footer.
4. On a very tall browser window the hero stretched to the full window height and looked wrong. Fixed by capping it: `min-height: min(calc(100svh - 70px), 880px)`.
5. The booking button fell below the first screen at 1440x900. Fixed by reducing the headline to `clamp(2.1rem, 3.6vw, 3.25rem)` and shortening the line under it to one line, which is what requirement 3 asks for.
6. A mobile screenshot looked cut off to the right. I measured it instead of guessing: DevTools emulation at 390px reported `scrollWidth: 390, offenders: []`, i.e. no overflow. The cut was an artefact of `--window-size` below Chrome's minimum window width, not a page fault. The DevTools-emulated phone screenshot is the accurate one.
7. On a wide screen the hero text and the photo overlapped. Cause: the hero grid was still inside the centred `.wrap`, and `.hero-copy` then added a second left padding of `(100vw - 1160px)/2`. At 1920px those two offset each other, the text column was squeezed to about 36px, the headline words overflowed to the right, and the opaque photo painted over them. Fixed by removing the inner `.wrap`, setting the columns to `minmax(0,1fr) minmax(0,1.5fr)`, capping that left padding at 380px, adding `overflow-wrap: break-word` as a safety net, and stacking the hero at `max-width: 1023px`. The photo now runs edge to edge on the right at 60% of the screen.

## Claims ledger

| Claim | Command that proves it | Result |
|---|---|---|
| Page serves | `Invoke-WebRequest http://localhost:4173/` | `HTTP 200 bytes=20080` |
| Every picture resolves | `Invoke-WebRequest -Method Head` on all 11 `src` values | all `HTTP 200 image/jpeg` |
| No sideways scroll on a phone | CDP `Emulation.setDeviceMetricsOverride` width 390 | `{"scrollWidth":390,"innerWidth":390,"offenders":[]}` |
| No sideways scroll on a laptop | CDP width 1440 | `{"scrollWidth":1440,"innerWidth":1440}` |
| No sideways scroll on a wide screen | CDP width 1920 | `{"scrollWidth":1920,"innerWidth":1920}`; hero `h1 380..714`, `photo 762..1905`, headline overflow `0` |
| Hero text never overlapped by the photo | CDP measure at 2560, 2200, 1920, 1680, 1536, 1440, 1366, 1280, 1100, 1024 | `h1.scrollWidth-clientWidth = 0` and `h1.right < photo.left` at every width |
| Three service cards, three steps | CDP DOM query | `servicesCards:3`, `steps:3` |
| Missing picture keeps the words | rename `working.jpeg`, CDP DOM query | `hiddenImages:["images/working.jpeg"]`, `aboutHeading:"Why I started MSAII LLC"`, `aboutParagraphs:2` |
| Business name exact | regex over the file | 24 occurrences, all `MSAII LLC` |
| No emoji on the page | non-ASCII scan of the file | `none` |
| Booking button links to the real link | regex over `href` | `https://cal.com/keerthana-saravana-kumar-y6aess/20-mins-intro-call` |
| WhatsApp button uses the real number | regex over `href` | `https://wa.me/14154802147?text=...` |
| Email button uses the real address | regex over `href` | `mailto:s.kumar@msaii.com?subject=...` |
| No placeholder left behind | search for `BOOKING_LINK_GOES_HERE` | `no` |
| Repository pushed | `git push -u origin main` | see the link in the chat message |
| Site is live on Vercel | NOT RUN | UNVERIFIED - the member imports the repo |

## What I would tell the next person

- The pictures in this repo are copies, renamed to the names the brief uses. The originals live in the project's `Images` folder and were not changed.
- `logo.jpeg` and `logo-dark.jpeg` here are tight crops of the member's logo files. The member's originals keep the wide whitespace; a plain 40px render of those would be unreadable.
- The booking link, WhatsApp number (`14154802147`) and email (`s.kumar@msaii.com`) were confirmed by the member. The name on the page is S. Kumar.
- The page is a single static `index.html` with one `images` folder. Nothing needs building, so importing the repo on Vercel serves it as is. Add the repo, keep the framework preset on "Other", and deploy.
- The two phone screenshots in this session are DevTools-emulated. Use those, not `chrome --window-size` below about 500px, which crops.
