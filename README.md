# All-Purpose-Viewer
Helps you view/check Videos on all Popular Platforms including YouTube, Facebook, and others

# 🎬 Video Playback Monitor

**Reliable verification of HTML5 video playback availability — across regions, platforms, and time.**

Video Playback Monitor is a lightweight monitoring Actor that checks whether a video can actually start playing on a publicly accessible webpage. It opens the page in a real browser environment, attempts playback using safe user interactions, and reports clear, actionable results.

Designed for **monitoring, QA, and availability verification** — not for traffic generation or engagement.

---

## ✅ Why use this Actor?

Because “the page loads” doesn’t mean **the video plays**.

This Actor helps you detect:

* videos that silently fail to start
* region-specific playback restrictions
* intermittent player issues
* login or consent walls blocking playback
* regressions introduced by site changes

All with **predictable cost and transparent results**.

---

## 🔍 What this Actor does

For each run, the Actor:

1. Opens the target page using a lightweight browser session
2. Attempts to start video playback using safe browser interactions:

   * muted autoplay
   * a single click on the video element
   * a single click on common play buttons
3. Confirms playback by detecting real time progression
4. Stops playback cleanly and exits
5. Emits a structured result explaining **what happened and why**

Each run produces **one clear playback availability check**.

---

## 🧪 Typical use cases

* Video playback availability monitoring
* QA testing of video embeds before release
* Detecting regional playback issues
* Monitoring CDN or player reliability
* Debugging “video not playing” user reports
* Long-term monitoring when paired with a scheduler

If video availability matters, this Actor fits.

---

## 🌍 Platform compatibility

Playback verification depends on whether a platform allows HTML5 video playback without authentication and with basic user interaction.

| Platform  | Compatibility   | Notes                                              |
| --------- | --------------- | -------------------------------------------------- |
| X         | ✅ Works         | Public videos usually autoplay or play after click |
| Reddit    | ✅ Works         | Native HTML5 video player                          |
| LinkedIn  | ✅ Works         | Public posts only                                  |
| Vimeo     | ✅ Works         | Designed for embeds                                |
| Facebook  | ⚠️ Partial      | Many videos require login                          |
| Instagram | ⚠️ Partial      | Login wall on most videos                          |
| TikTok    | ⚠️ Partial      | Headless playback often blocked                    |
| YouTube   | ❌ Limited       | User gesture and DRM restrictions                  |
| Snapchat  | ❌ Not supported | App-first platform                                 |

Platforms marked ⚠️ or ❌ may correctly report playback as unavailable due to platform restrictions.

---

## 🔎 How this Actor behaves on restricted platforms (e.g. YouTube)

Some platforms intentionally restrict automated video playback unless a logged-in, interactive user is present. **YouTube is a common example**.

On these platforms, the Actor still provides **useful monitoring signals**, even when full playback cannot be confirmed.

### What typically happens

When checking a public YouTube video page, the Actor may report:

* `LOGIN_REQUIRED`
  → Playback requires authentication or additional user interaction

* `PLAYBACK_BLOCKED`
  → The video player is present, but playback does not advance

This is **expected behavior**, not an error.

---

### Why this is still useful

Even when playback cannot be confirmed, the result answers important questions:

* Is the page reachable from a given region?
* Is the video element present at all?
* Has the page structure changed?
* Is playback gated behind login, consent, or policy checks?
* Does behavior differ by country or time?

For monitoring and QA workflows, these signals are often **exactly what you want to detect**.

---

### Recommended usage on restricted platforms

Users often get the best results by:

* Running checks from **multiple regions**
* Comparing results over time
* Watching for **changes in failure reason**

  * e.g. `PLAYBACK_BLOCKED → NO_VIDEO`
* Using the Actor to validate **availability**, not engagement

---

### Important expectations

This Actor:

* does **not** attempt to bypass platform safeguards
* does **not** simulate logged-in users
* does **not** defeat DRM or autoplay restrictions

If a platform requires authenticated, interactive viewing, the Actor will **report that fact transparently**.

---

## 📊 Clear results you can trust

Each run produces a single dataset record with:

* whether playback was confirmed
* **why** playback failed when it did
* region and session context
* timestamp for auditing and monitoring

### Failure reasons explained

| Reason              | Meaning                                     |
| ------------------- | ------------------------------------------- |
| `NO_VIDEO`          | No HTML5 video element found                |
| `LOGIN_REQUIRED`    | Playback requires authentication or consent |
| `PLAYBACK_BLOCKED`  | Video exists but did not advance            |
| `NAVIGATION_FAILED` | Page could not be loaded                    |
| `ERROR`             | Unexpected runtime error                    |

These classifications help distinguish real playback issues from access restrictions.

---

## 🧾 Example output snippets

Each run produces exactly **one dataset record**.
Below are typical examples to help you understand what to expect.

---

### ✅ Example: successful playback verification

```json
{
  "url": "https://example.com/video-page",
  "sessionId": "session-123",
  "countryCode": "US",
  "watchSeconds": 60,
  "playbackConfirmed": true,
  "failureReason": null,
  "timestamp": "2026-02-03T14:21:10.382Z"
}
```

**What this means**

* The page loaded successfully
* A video element was found
* Playback started and advanced
* The video played for the configured duration
* Playback availability is confirmed from this region

---

### ⚠️ Example: restricted platform (e.g. YouTube)

```json
{
  "url": "https://www.youtube.com/watch?v=abc123",
  "sessionId": "session-456",
  "countryCode": "DE",
  "watchSeconds": 60,
  "playbackConfirmed": false,
  "failureReason": "PLAYBACK_BLOCKED",
  "timestamp": "2026-02-03T14:27:55.904Z"
}
```

**What this means**

* The page was reachable
* A video player was detected
* Playback did not advance
* The platform requires additional user interaction, authentication, or policy acceptance

This is **expected behavior** on restricted platforms and represents a valid monitoring signal.

---

### 🔐 Example: login or consent required

```json
{
  "url": "https://social.example.com/video/789",
  "sessionId": "session-789",
  "countryCode": "IN",
  "watchSeconds": 60,
  "playbackConfirmed": false,
  "failureReason": "LOGIN_REQUIRED",
  "timestamp": "2026-02-03T14:33:18.117Z"
}
```

**What this means**

* Playback is gated behind login or consent
* The restriction was detected transparently
* No attempt was made to bypass safeguards

---

### Key takeaway

> A result where `playbackConfirmed = false` is not a failure of the Actor — it is a **meaningful monitoring outcome**.

---

## 💸 Cost & performance

* One lightweight browser session per run
* Aggressive resource blocking
* Explicit media cleanup
* Memory usage capped and predictable

Each run is billed as **one playback availability check**, regardless of outcome.

This makes the Actor suitable for:

* ad-hoc checks
* scheduled monitoring
* long-term pipelines

---

## ⏱ Optional: scheduled rechecks

For ongoing monitoring, this Actor can be paired with an optional replay scheduler that:

* re-runs checks after configurable delays
* rotates regions automatically
* enforces daily execution limits
* reduces unnecessary retries

The scheduler is optional and provided separately.

---

## 🚫 What this Actor does NOT do

To set clear expectations:

* It does not bypass login, consent, or DRM restrictions
* It does not simulate real viewers or engagement
* It does not generate traffic, impressions, or views
* It does not attempt to evade platform safeguards

Failures on gated platforms are **expected and meaningful results**.

---

## ⚖️ Responsible usage

You are responsible for ensuring that your usage complies with the terms and policies of the websites you monitor.

This Actor is designed for legitimate monitoring and QA workflows.

---

## ⭐ Summary

**Video Playback Monitor** gives you a reliable, cost-controlled way to answer a simple but critical question:

> *“Can this video actually start playing right now — and if not, why?”*

Even on restricted platforms, the Actor provides **diagnostic signals** that help you monitor availability, detect changes, and respond confidently.


