![Candlefox Logo](images/candlefox-logo.png)

# CourseView SDK v6

Version 6 of the SDK embeds one or more Course Views on a page. Each one is independent: its own courses, filters, pagination, course details and styling.

New integrations should use v6. Pages using the [previous SDK](./sdk_implementation_guide.md) keep working; see [Moving from the previous SDK](#moving-from-the-previous-sdk).

---

## Table of Contents

1. [Quick start (no code)](#quick-start-no-code)
2. [Several Course Views on one page](#several-course-views-on-one-page)
3. [Attributes](#attributes)
4. [Dynamic Course Views](#dynamic-course-views)
5. [JavaScript API](#javascript-api)
6. [Events](#events)
7. [Moving from the previous SDK](#moving-from-the-previous-sdk)
8. [Error Handling & Troubleshooting](#error-handling--troubleshooting)
9. [Styling](#styling)

---

## Quick start (no code)

1. Add the SDK script to the page once, ideally before the closing `</body>` tag:

   ```html
   <script src="https://courseview.candlefox.com/assets/sdk/v6/course-view.js" defer></script>
   ```

2. Add an element where the Course View should appear, with its Course View ID:

   ```html
   <div data-course-view-id="UUID"></div>
   ```

That's all. The SDK finds the element and loads the Course View into it. You can copy both parts, with your Course View ID filled in, from the last step of the Course View wizard in Marketing Hub ("No-code embed").

The order doesn't matter: the element can come before or after the script, and elements added to the page later (by a CMS block, a tag manager or a single-page app) are picked up when they appear.

---

## Several Course Views on one page

Add one element per Course View. The script is still added once.

```html
<script src="https://courseview.candlefox.com/assets/sdk/v6/course-view.js" defer></script>

<h2>Business courses</h2>
<div data-course-view-id="UUID-1"></div>

<h2>IT diplomas</h2>
<div data-course-view-id="UUID-2"></div>
```

Each Course View keeps its own layout and styling, and a visitor filtering or paging one doesn't affect the others. The same Course View can also appear more than once.

---

## Attributes

| Attribute              | Required | Description |
| ---------------------- | -------- | ----------- |
| `data-course-view-id`  | Yes      | The Course View ID (UUID). |
| `data-subject`         | No       | Dynamic Course Views: comma-separated subject values, e.g. `"art,accounting"`. |
| `data-occupation-ids`  | No       | Dynamic Course Views: comma-separated occupation IDs, e.g. `"12,24"`. Use this or `data-subject`, not both. |
| `data-qualifications`  | No       | With `data-subject`: comma-separated qualification levels to limit the results to, e.g. `"diploma,advanced_diploma"`. See [Limiting by qualification level](#limiting-by-qualification-level). |
| `data-level`           | No       | Starting value of the Course View's Level filter. Visitors can change it. |
| `data-delivery`        | No       | Starting value of the Course View's Delivery filter, e.g. `"online"`. Visitors can change it. |

Put any content you want shown until the Course View loads (or if it can't load) inside the element; it is replaced once the Course View is ready:

```html
<div data-course-view-id="UUID">
  <a href="/courses">Browse our courses</a>
</div>
```

---

## Dynamic Course Views

A dynamic Course View shows courses for the subject or occupation the page is about. Set the value on the element:

```html
<!-- Subject Course View -->
<div data-course-view-id="UUID" data-subject="business"></div>

<!-- Occupation Course View -->
<div data-course-view-id="UUID" data-occupation-ids="12,24"></div>
```

On a server-rendered page, print the page's own values into the attribute. If the values are only known in the browser, use the [JavaScript API](#javascript-api).

### Limiting by qualification level

A subject Course View can also be limited to some qualification levels, for example a page about business diplomas. Tick **Let the page also set qualification levels** in the Course View's Filtering step, then set `data-qualifications` with `data-subject`:

```html
<div
  data-course-view-id="UUID"
  data-subject="business"
  data-qualifications="diploma,advanced_diploma"
></div>
```

- Only the subject's courses at those levels are shown. Visitors can narrow the list with the Course View's own filters, but "Clear all" and paging keep the levels.
- The levels you pass replace the Qualifications saved on the Course View.
- When qualifications are set, no popular courses are added if fewer than 4 courses match.
- Qualifications work with a subject only, not with occupation IDs.

Values are course level codes, lower case with underscores. Common Australian ones: `short`, `skillset`, `certificate_i`, `certificate_ii`, `certificate_iii`, `certificate_iv`, `diploma`, `advanced_diploma`, `graduate_certificate`, `graduate_diploma`, `associate_degree`, `bachelor`, `masters`, `postgraduate`.

---

## JavaScript API

For single-page apps, and for pages that set or change values from JavaScript. Load the script with `data-auto-mount="false"` if you want to mount only from code (otherwise elements with `data-course-view-id` are still mounted automatically).

```html
<div id="courses"></div>

<script src="https://courseview.candlefox.com/assets/sdk/v6/course-view.js" data-auto-mount="false"></script>
<script>
  const courseView = CourseView.mount(document.getElementById("courses"), {
    courseViewId: "UUID",
    subject: "business",
  });

  // Later, for example after a visitor answers a question on your page:
  courseView.update({ level: "diploma", delivery: "online" });

  // When the element is no longer needed:
  courseView.unmount();
</script>
```

| Call | Description |
| ---- | ----------- |
| `CourseView.mount(element, options)` | Loads a Course View into `element` (an element or a CSS selector) and returns its handle, or `null` if the element isn't found or there is no `courseViewId`. Mounting an element that is already mounted updates it instead. |
| `handle.update(options)` | Reloads the Course View with the given options merged into the current ones. |
| `handle.unmount()` | Removes the Course View from the element. |
| `CourseView.get(element)` | The handle for an element, or `null`. |
| `CourseView.autoMount(root)` | Mounts every `[data-course-view-id]` element inside `root` (default: the whole page). |
| `CourseView.unmountAll()` | Removes every Course View on the page. |

Options match the [attributes](#attributes): `courseViewId`, `subject`, `occupationIds`, `qualifications`, `level`, `delivery`. `subject`, `occupationIds` and `qualifications` also accept arrays, e.g. `qualifications: ["diploma", "advanced_diploma"]`.

Options for single-page apps that change route without a page load:

| Option | Description |
| ------ | ----------- |
| `refreshOnNavigation` | `true` reloads the Course View when the page's URL changes (history navigation). |
| `occupationFromUrl` | With `refreshOnNavigation`: `true` takes the occupation from the page URL's `?occupation=` parameter when `occupationIds` isn't set. |
| `occupationExtractor` | With `refreshOnNavigation`: a function `(window) => "12,24"` returning the occupation IDs for the current route. |

Attributes are read when the element is mounted; changing them afterwards has no effect. To change what a mounted Course View shows, call `update()`.

Elements that are removed from the page are unmounted automatically.

---

## Events

Each event is dispatched on the Course View's element and bubbles, so you can listen on the element or on `document`.

| Event        | When | `event.detail` |
| ------------ | ---- | -------------- |
| `mf:mounted` | The Course View has loaded (also after `update()`). | `{ container, instance }` |
| `mf:error`   | The Course View could not load. The element keeps the content it had. | `{ container, instance, error }` |

```js
document.addEventListener("mf:error", (event) => {
  console.warn("Course View did not load", event.detail.error);
});
```

---

## Moving from the previous SDK

Pages using `micro-frontend-sdk.js` keep working; there is no deadline to move.

**Simplest change:** swap the script URL. v6 understands `MicroFrontend.init()`, so the rest of the page can stay as it is:

```html
<!-- Before -->
<script src="https://courseview.candlefox.com/assets/sdk/micro-frontend-sdk.js"></script>

<!-- After -->
<script src="https://courseview.candlefox.com/assets/sdk/v6/course-view.js"></script>
```

With v6, each element matched by `target` gets its own Course View, so a page with several `MicroFrontend.init()` calls for different Course Views now shows all of them (the previous SDK showed only the last one). `MicroFrontend.update()` updates every Course View created through `MicroFrontend.init()`.

`MicroFrontend.init()` in v6 keeps the previous SDK's defaults: the Course View reloads when a single-page app changes route, and an occupation Course View falls back to the URL's `?occupation=` parameter. The attribute embed and `CourseView.mount()` don't do this unless you ask for it (see the [single-page app options](#javascript-api)). If your page relies on it, keep `MicroFrontend.init()` or pass those options to `CourseView.mount()`.

**Recommended when you next edit the page:** replace the container and the `MicroFrontend.init()` call with an element carrying the attributes:

```html
<!-- Before -->
<div class="mf-container"></div>
<script>
  MicroFrontend.init({ target: ".mf-container", courseViewId: "UUID", subject: "business" });
</script>

<!-- After -->
<div data-course-view-id="UUID" data-subject="business"></div>
```

Use only one of the two SDK scripts on a page.

---

## Error Handling & Troubleshooting

| Symptom | Resolution |
| ------- | ---------- |
| Nothing appears | Check the script URL and that the element has `data-course-view-id`. Listen for `mf:error` to see why. |
| The fallback content stays | The Course View could not load: check the Course View ID, and that your website's domain is registered on the Course View in Marketing Hub. |
| Invalid `data-occupation-ids` | Use a comma-separated list of numbers. |
| Invalid `data-subject` | Use comma-separated subject values. |
| Invalid `data-qualifications` | Use level codes such as `diploma` or `certificate_iv` (letters, digits and underscores). Any other value makes the Course View fail to load. |
| Only the last of several Course Views shows | The page is still loading `micro-frontend-sdk.js`. Switch the script to v6. |

---

## Styling

Each Course View uses the style set in Marketing Hub. To adjust the space around it, give the element you added a class of your own and style that (the course cards inside also carry `data-course-view-id`, so don't style the attribute):

```html
<div class="my-courses" data-course-view-id="UUID"></div>
```

```css
.my-courses {
  max-width: 900px;
  margin: 1rem auto;
}
```

---

_Last updated: Nov 20, 2026_

![OrchestraX Logo](images/logo.png)
