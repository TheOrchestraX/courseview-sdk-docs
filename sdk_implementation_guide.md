![Candlefox Logo](images/candlefox-logo.png)

# Overview

This document provides detailed, step-by-step instructions for integrating the CourseView (MicroFrontend) SDK into your web application. The SDK enables you to load and display data from an external API directly within your site, rendering content inline based on occupation filters.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Initialization](#initialization)
4. [Configuration Options](#configuration-options)
5. [Integration Example](#integration-example)
6. [Dynamic Parameters](#dynamic-parameters)
7. [Error Handling & Troubleshooting](#error-handling--troubleshooting)
8. [Styling & Theming](#styling--theming)

---

## Prerequisites

- A modern web browser with JavaScript enabled.
- Access to your web application's source code to include scripts and markup.
- Network access to `https://sea-turtle-toolkit-yupbm.ondigitalocean.app`.

---

## Installation

Add the SDK script tag to your HTML, ideally before your closing `</body>` tag:

```html
<!-- Include the MicroFrontend SDK -->
<script src="https://sea-turtle-toolkit-yupbm.ondigitalocean.app/assets/sdk/micro-frontend-sdk.js"></script>
```

This downloads and registers `MicroFrontend` globally for use in your code.

---

## Initialization

Ensure your container element exists in the DOM:

```html
<div id="mf-container"></div>
```

Then initialize the SDK after the page loads:

```html
<script>
  document.addEventListener("DOMContentLoaded", function () {
    MicroFrontend.init({
      target:        "#mf-container", // CSS selector for the inline container
      occupationIds: "12,24",         // Comma-separated occupation IDs
      affiliateId: "1",               // String containing affiliate ID value
    });
  });
</script>
```

The SDK will query the external API, retrieve up to 20 course records based on the provided occupation IDs, and render them inside the specified `#mf-container` element.

---

## Configuration Options

| Option          | Type     | Required | Description                                                   |
| --------------- | -------- | -------- | ------------------------------------------------------------- |
| `target`        | `string` | Yes      | CSS selector where the SDK will render content inline.        |
| `occupationIds` | `string` | No       | Comma-separated list of occupation IDs to filter the results. |
| `affiliateId`   | `string` | Yes      | String containing affiliate ID.                               |

---

## Integration Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>SDK Inline Integration</title>
</head>
<body>

  <!-- Container for the SDK content -->
  <div id="mf-container"></div>

  <!-- SDK Script -->
  <script src="https://sea-turtle-toolkit-yupbm.ondigitalocean.app/assets/sdk/micro-frontend-sdk.js"></script>
  <script>
    document.addEventListener("DOMContentLoaded", () => {
      MicroFrontend.init({
        target:        "#mf-container",
        occupationIds: "12,24,31",
        affiliateId: "1",
      });
    });
  </script>

</body>
</html>
```

---

## Dynamic Parameters

If your application determines `occupationIds` dynamically (e.g. based on user actions or metadata), generate the comma-separated string and pass it into `init`:

```js
const ids = fetchSelectedOccupationIds(); // returns [12,24,31]
MicroFrontend.init({
  target:        '#mf-container',
  occupationIds: ids.join(','),
  affiliateId: "1",
});
```

---

## Error Handling & Troubleshooting

| Symptom                  | Resolution                                         |
| ------------------------ | -------------------------------------------------- |
| No content appears       | Verify `#mf-container` exists.                     |
| SDK script fails to load | Confirm the script URL is correct and not blocked. |
| Invalid `occupationIds`  | Ensure it’s a comma-separated string of integers.  |
| Invalid `affiliateId`    | Ensure it’s a string of a single integer.          |

---

## Styling & Theming

Target `#mf-container` in your CSS to adjust layout:

```css
#mf-container {
  max-width: 900px;
  margin: 1rem auto;
  padding: 0.5rem;
}
```

---

*Last updated: July 21, 2025*

![OrchestraX Logo](images/logo.png)
