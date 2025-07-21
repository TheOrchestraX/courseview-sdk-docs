# CourseView - MicroFrontend SDK Documentation

![Candlefox Logo](images/candlefox-logo.png)

Welcome to the official integration guide for the **MicroFrontend SDK** — a lightweight JavaScript SDK designed to embed dynamic course-related content from an external API directly into your website.

This SDK enables partners to display course information inline on any webpage using just a few lines of code.

---

## 🚀 Quick Start

### 1. Include the SDK in your HTML

```html
<!-- Container for SDK content -->
<div id="mf-container"></div>

<!-- Include the MicroFrontend SDK -->
<script src="https://sea-turtle-toolkit-yupbm.ondigitalocean.app/assets/sdk/micro-frontend-sdk.js"></script>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    MicroFrontend.init({
      target: "#mf-container", // CSS selector for the container
      occupationIds: "12,24", // Comma-separated occupation ID values
      affiliateId: "1", // String containing affiliate ID value
    });
  });
</script>
```

---

## ⚙️ Configuration Options

| Option          | Type     | Required | Description                                                   |
| --------------- | -------- | -------- | ------------------------------------------------------------- |
| `target`        | `string` | Yes      | CSS selector where the SDK will render content inline.        |
| `occupationIds` | `string` | No       | Comma-separated list of occupation IDs to filter the results. |
| `affiliateId`   | `string` | Yes      | String containing affiliate ID.                               |

---

## 📚 Full Documentation

For complete setup instructions, styling tips, and advanced usage:

👉 [View Full Documentation](./sdk_implementation_guide.md)

---

## 🎨 Styling

You can customize the appearance of the SDK container:

```css
#mf-container {
  max-width: 900px;
  margin: 1rem auto;
  padding: 0.5rem;
}
```

---

## 🛠 Version & Updates

- Current Version: `1.0`
- Changelog: Coming soon

![OrchestraX Logo](images/logo.png)
