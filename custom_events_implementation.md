![Candlefox Logo](images/candlefox-logo.png)

# Listening to `mf:track` and Using Event Data

This document explains how to _listen_ for the `mf:track` custom event. The _event data_ will automatically forward to analytics providers (GA4 / Facebook Pixel / dataLayer).

---

## 1) Event shape (what you will receive)

Listeners receive a DOM CustomEvent named `mf:track`. Its `event.detail` has the form:

```js
{
  event_type: 'TILE_CLICK',    // required string identifier for the event
  params: {                    // optional object with event metadata
    course_id: 123,
    affiliate_id: 45,
    session_id: 'session_...',
    event_id: 'mf-xxx-...',
    timestamp: new Date().toISOString(),
    source: 'course-logging-api'
  },
  options: {                   // optional routing/mapping hints (usually unused by listeners)
    providers: { ga: true, fb: true },
    mapper: null,
    event_id: 'explicit-id'
  }
}
```

**Important fields** your listeners will typically use:

- `event_type` — short string that indicates the event (e.g. `TILE_CLICK`, `TILE_IMPRESSION`, `ENQUIRY_FORM_OPEN`, `ENQUIRY_FORM_SUBMIT`).
- `params` — object carrying data (IDs, numeric values, strings).
- `params.event_id` — unique id for correlation (client/server dedupe).

---

## 2) Two recommended ways to listen

### A — Native DOM listener (raw)

Use `window.addEventListener('mf:track', handler)`. You get the raw `CustomEvent` and can inspect `event.detail` directly.

```js
function rawHandler(ev) {
  const d = ev && ev.detail ? ev.detail : null;
  if (!d) return; // guard
  const type = d.event_type;
  const params = d.params || {};
  console.log("mf:track raw", type, params);
  // forward or transform below
}

window.addEventListener("mf:track", rawHandler);

// to remove later:
window.removeEventListener('mf:track', rawHandler);
```

### B — Convenience API (`window.mfTracking.on`) — preferred

The `event-tracking.js` helper exposes `window.mfTracking.on(eventName, handler)` which filters events by `event_type` for you, and returns an `unsubscribe` function.

```js
// Listen for only TILE_CLICK events
const unsubscribe = window.mfTracking.on("TILE_CLICK", (params, options) => {
  console.log("tile click params", params);
  // forward to analytics (see examples below)
});

// Unsubscribe when you no longer need the listener
unsubscribe();
```

---

## 3) Debugging the events

- Use a quick console listener to inspect raw payloads:

  ```js
  window.addEventListener("mf:track", (e) => console.log("mf:track", e.detail));
  ```

- Verify provider calls appear in the network tab or provider debug consoles (GA DebugView, Facebook Pixel Helper).
- If events are missing, ensure the provider's JS has loaded and the page's runtime did not block execution.

---

_Last updated: Oct 24, 2025_

![OrchestraX Logo](images/logo.png)
