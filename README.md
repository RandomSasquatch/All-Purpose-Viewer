# All-Purpose-Viewer
Helps you view/check Videos on all Popular Platforms including YouTube, Facebook, and others

This is Store-ready, conservative, and aligned with Apify reviewer expectations.

This Apify Actor Verifies HTML5 video playback availability using a lightweight browser session and residential networks.
This Actor opens a publicly accessible webpage, attempts to start video playback using safe browser interactions, confirms that playback advances for a short duration, and reports the result in a structured dataset.

The Actor is designed for monitoring, QA, and availability verification, not for audience analytics or engagement measurement.

What this Actor does :::

Opens a webpage containing an HTML5 video

Attempts playback using:

muted autoplay
a single click on the video element
a single click on common play buttons
Confirms playback by detecting time progression
Stops playback explicitly and exits cleanly
Emits a structured result with clear success or failure classification

What this Actor does NOT do :::

It does not bypass login or authentication walls
It does not defeat DRM or platform safeguards
It does not simulate human behavior
It does not measure views, traffic, or engagement

Typical use cases :::

Video playback availability monitoring
Regional playback verification
QA testing of video embeds
Detecting intermittent playback failures
Long-term monitoring when paired with a scheduler Actor

Platform compatibility :::

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

Failure reasons :::

If playback cannot be verified, the Actor reports a failure reason:

| Reason              | Meaning                                     |
| ------------------- | ------------------------------------------- |
| `NO_VIDEO`          | No HTML5 video element found                |
| `LOGIN_REQUIRED`    | Playback requires authentication or consent |
| `PLAYBACK_BLOCKED`  | Video exists but did not advance            |
| `NAVIGATION_FAILED` | Page could not be loaded                    |
| `ERROR`             | Unexpected runtime error                    |

These classifications help distinguish true playback failures from platform or access restrictions.

Cost and performance :::

Uses a single lightweight browser instance
Aggressively blocks non-essential resources
Explicitly shuts down video playback
Memory usage is capped and predictable

This ensures reliable execution and controlled costs even at scale.

Optional: time-delayed replays
For long-term monitoring scenarios, this Actor can be paired with an optional replay scheduler Actor that re-runs playback checks after configurable delays and across regions.

The scheduler is optional and not required for standard usage.

Responsibility notice :::

Users are responsible for ensuring they have the right to verify playback on target websites and that their usage complies with applicable terms and policies.

Summary :::

Video Playback Monitor provides a reliable, cost-controlled way to verify whether HTML5 video playback is available on publicly accessible pages, with clear and honest reporting when playback is restricted.
