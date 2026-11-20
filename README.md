![Candlefox Logo](images/candlefox-logo.png)

# CourseView - MicroFrontend SDK Documentation

Welcome to the official integration guide for the **MicroFrontend SDK** — a lightweight JavaScript SDK designed to embed one or more Course Views (dynamic course listings) from an external API directly into your website.

Partners add a script and an element; no other code is needed.

---

## 🚀 Quick Start (SDK v6)

Add the script once, then one element per Course View:

```html
<!-- Include the CourseView SDK once per page -->
<script src="https://courseview.candlefox.com/assets/sdk/v6/course-view.js" defer></script>

<!-- One element per Course View; several on one page are fine -->
<div data-course-view-id="UUID"></div>

<!-- Dynamic Course View: set the subject (or data-occupation-ids) -->
<div data-course-view-id="UUID" data-subject="art,accounting"></div>
```

👉 [SDK v6 guide](./sdk_v6_guide.md): attributes, dynamic Course Views, the JavaScript API, events and moving from the previous SDK.

---

## ⚙️ Attributes

| Attribute             | Required | Description |
| --------------------- | -------- | ----------- |
| `data-course-view-id` | Yes      | The Course View ID (UUID). |
| `data-subject`        | No       | Comma-separated subjects to filter the results. |
| `data-occupation-ids` | No       | Comma-separated occupation IDs to filter the results. Use this or `data-subject`, not both. |
| `data-qualifications` | No       | With `data-subject`: comma-separated qualification levels, e.g. `"diploma,advanced_diploma"`. |
| `data-level`          | No       | Starting value of the Level filter. Visitors can change it. |
| `data-delivery`       | No       | Starting value of the Delivery filter, e.g. `"online"`. Visitors can change it. |

---

## 🕘 Previous SDK

Pages using `micro-frontend-sdk.js` and `MicroFrontend.init()` keep working. Its guide: [Previous SDK](./sdk_implementation_guide.md). To move a page to v6, see [Moving from the previous SDK](./sdk_v6_guide.md#moving-from-the-previous-sdk).

---

## 📚 Full Documentation

👉 [SDK v6 guide](./sdk_v6_guide.md)

---

## 🎨 Styling

Each Course View uses the style set in Marketing Hub. To adjust the space around it, give its element a class of your own (e.g. `<div class="my-courses" data-course-view-id="UUID">`) and style that; the course cards inside also carry `data-course-view-id`:

```css
.my-courses {
  max-width: 900px;
  margin: 1rem auto;
  padding: 0.5rem;
}
```

---

## 📚 Other Documentations

👉 [Custom Events Implementation](./custom_events_implementation.md)

---

## 🛠 Version & Updates

- Current Version: `6.0` (`assets/sdk/v6/course-view.js`)
- Changelog: Coming soon

_Last updated: Nov 20, 2026_

![OrchestraX Logo](images/logo.png)
