![Candlefox Logo](images/candlefox-logo.png)

# CourseView - MicroFrontend SDK Documentation

Welcome to the official integration guide for the **MicroFrontend SDK** — a lightweight JavaScript SDK designed to embed dynamic course-related content from an external API directly into your website.

This SDK enables partners to display course information inline on any webpage using just a few lines of code.

---

## 🚀 Quick Start

### 1. Include the SDK in your HTML

```html
<!-- Container for SDK content -->
<div class="mf-container"></div>

<!-- Include the MicroFrontend SDK -->
<script src="https://courseview.orchestrax.com/assets/sdk/micro-frontend-sdk.js"></script>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    MicroFrontend.init({
      target: ".mf-container", // CSS selector for the container
      courseViewId: "UUID", // String containing Course View UUID value
      // Use one of the params below, not both
      occupationIds: "12,24", // Optional: Comma-separated occupation IDs
      subject: "art,accounting", // Optional: Comma-separated subject strings
    });
  });
</script>
```

---

## ⚙️ Configuration Options

| Option          | Type     | Required | Description                                                   |
| --------------- | -------- | -------- | ------------------------------------------------------------- |
| `target`        | `string` | Yes      | CSS selector where the SDK will render content inline.        |
| `courseViewId`  | `UIUD`   | Yes      | String containing Course View ID.                             |
| `occupationIds` | `string` | No       | Comma-separated list of occupation IDs to filter the results. |
| `subject`       | `string` | No       | Comma-separated list of subjects to filter the results.       |
| `qualifications` | `string` | No      | With `subject`: comma-separated qualification levels to limit the results to, e.g. `"diploma,advanced_diploma"`. See [Limiting by qualification level](https://theorchestrax.github.io/courseview-sdk-docs/sdk_implementation_guide.html#limiting-by-qualification-level). |
| `level`         | `string` | No       | Starting value of the course view's Level filter. Visitors can change it. |
| `delivery`      | `string` | No       | Starting value of the course view's Delivery filter, e.g. `"online"`. Visitors can change it. |

---

## 📚 Full Documentation

For complete setup instructions, styling tips, and advanced usage:

👉 [View Full Documentation](./sdk_implementation_guide.md)

---

## 🎨 Styling

You can customize the appearance of the SDK container:

```css
.mf-container {
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

- Current Version: `1.0`
- Changelog: Coming soon

_Last updated: Feb 18, 2026_

![OrchestraX Logo](images/logo.png)
