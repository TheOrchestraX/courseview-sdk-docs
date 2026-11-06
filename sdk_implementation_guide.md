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
- Network access to `https://courseview.orchestrax.com`.

---

## Installation

Add the SDK script tag to your HTML, ideally before your closing `</body>` tag:

```html
<!-- Include the MicroFrontend SDK -->
<script src="https://courseview.orchestrax.com/assets/sdk/micro-frontend-sdk.js"></script>
```

This downloads and registers `MicroFrontend` globally for use in your code.

---

## Initialization

Ensure your container element exists in the DOM:

```html
<div class="mf-container"></div>
```

Then initialize the SDK after the page loads:

```html
<script>
  document.addEventListener("DOMContentLoaded", function () {
    MicroFrontend.init({
      target: ".mf-container", // CSS selector for the inline container
      courseViewId: "UUID", // String containing Course View UUID value
      // Use one of the params below, not both
      occupationIds: "12,24", // Optional: Comma-separated occupation IDs
      subject: "art,accounting", // Optional: Comma-separated subject strings
    });
  });
</script>
```

The SDK will query the external API, retrieve up to 20 course records based on the provided occupation IDs, and render them inside the specified `.mf-container` element.

---

## Configuration Options

| Option          | Type     | Required | Description                                                   |
| --------------- | -------- | -------- | ------------------------------------------------------------- |
| `target`        | `string` | Yes      | CSS selector where the SDK will render content inline.        |
| `courseViewId`  | `UUID`   | Yes      | String containing Course View ID.                             |
| `occupationIds` | `string` | No       | Comma-separated list of occupation IDs to filter the results. |
| `subject`       | `string` | No       | Comma-separated list of subjects to filter the results.       |
| `qualifications` | `string` | No      | With `subject`: comma-separated qualification levels to limit the results to, e.g. `"diploma,advanced_diploma"`. See [Limiting by qualification level](#limiting-by-qualification-level). |
| `level`         | `string` | No       | Starting value of the course view's Level filter. Visitors can change it. |
| `delivery`      | `string` | No       | Starting value of the course view's Delivery filter, e.g. `"online"`. Visitors can change it. |

---

## Integration Example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>SDK Inline Integration</title>
  </head>
  <body>
    <!-- Container for the SDK content -->
    <div class="mf-container"></div>

    <!-- SDK Script -->
    <script src="https://courseview.orchestrax.com/assets/sdk/micro-frontend-sdk.js"></script>
    <script>
      document.addEventListener("DOMContentLoaded", () => {
        MicroFrontend.init({
          target: ".mf-container",
          courseViewId: "UUID",
          // Use one of the params below, not both
          occupationIds: "12,24,31",
          subject: "art,accounting,fitness",
        });
      });
    </script>
  </body>
</html>
```

---

## Dynamic Parameters

If your application determines `occupationIds` or `subjects` dynamically (e.g. based on user actions or metadata), generate the comma-separated string and pass it into `init`:

```js
const ids = fetchSelectedOccupationIds(); // returns [12,24,31]
MicroFrontend.init({
  target: ".mf-container",
  courseViewId: "UUID",
  occupationIds: ids.join(","),
});

// Or

const subjects = fetchSelectedSubjects(); // returns ["art","accounting","fitness"]
MicroFrontend.init({
  target: ".mf-container",
  courseViewId: "UUID",
  subject: subjects.join(","),
});
```

### Limiting by qualification level

A subject course view can also be limited to some qualification levels, for example a page about business diplomas. Tick **Let the page also set qualification levels** in the course view's Filtering step, then pass `qualifications` with `subject`:

```js
MicroFrontend.init({
  target: ".mf-container",
  courseViewId: "UUID",
  subject: "business",
  qualifications: "diploma,advanced_diploma",
});
```

- Only the subject's courses at those levels are shown. Visitors can narrow the list with the course view's own filters, but "Clear all" and paging keep the levels.
- The levels you pass replace the Qualifications saved on the course view.
- When `qualifications` is set, no popular courses are added if fewer than 4 courses match.
- `qualifications` works with `subject` only, not with `occupationIds`.
- An array works too: `qualifications: ["diploma", "advanced_diploma"]`.

Values are course level codes, lower case with underscores. Common Australian ones: `short`, `skillset`, `certificate_i`, `certificate_ii`, `certificate_iii`, `certificate_iv`, `diploma`, `advanced_diploma`, `graduate_certificate`, `graduate_diploma`, `associate_degree`, `bachelor`, `masters`, `postgraduate`.

### Changing filters after load

`MicroFrontend.update()` reloads the course view with new values, for example after a visitor answers a question on your page:

```js
MicroFrontend.update({ level: "diploma", delivery: "online" });
```

---

## Error Handling & Troubleshooting

| Symptom                  | Resolution                                         |
| ------------------------ | -------------------------------------------------- |
| No content appears       | Verify `.mf-container` exists.                     |
| SDK script fails to load | Confirm the script URL is correct and not blocked. |
| Invalid `courseViewId`   | Ensure it’s a valid Course View ID of UUID type.   |
| Invalid `occupationIds`  | Ensure it’s a comma-separated string of integers.  |
| Invalid `subject`        | Ensure it’s a comma-separated string of strings.   |
| Invalid `qualifications` | Use level codes such as `diploma` or `certificate_iv` (letters, digits and underscores). Any other value makes the course view fail to load. |

---

## Styling & Theming

Target `.mf-container` in your CSS to adjust layout:

```css
.mf-container {
  max-width: 900px;
  margin: 1rem auto;
  padding: 0.5rem;
}
```

---

_Last updated: Feb 18, 2026_

![OrchestraX Logo](images/logo.png)
