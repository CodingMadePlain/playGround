# Feedback Dashboard — Implementation Document

## 1. How the System Works

This application has two parts that talk to each other over the internet:

1. **Hosted Server (PHP)** — A public-facing web server that:
   - Shows a feedback form to users.
   - Receives the submitted form data via a POST request.
   - Saves each piece of feedback into a JSON file on the server.
   - Serves that JSON file to any authorised request (so the dashboard can read it).

2. **Local Server (HTML/CSS/JS)** — A local machine that:
   - Displays a dashboard webpage.
   - Uses JavaScript `fetch()` to pull the JSON file from the hosted server.
   - Renders the feedback entries in a clean, readable layout.

**Data flow summary:**

```
User fills form ──POST──▶ Hosted Server (PHP) ──writes──▶ feedback.json
                                                              │
Local Dashboard (JS) ◀──fetch() GET──────────────────────────┘
```

---

## 2. File Structure

### Hosted Server Files

All hosted files go inside a single folder on the web server (e.g. `feedback/`).

```
feedback/
├── index.php          # The feedback form (HTML + PHP form handler)
├── api.php            # API endpoint — returns JSON data & handles CORS
└── data/
    └── feedback.json  # Stored feedback entries (created automatically)
```

### Local Server Files

All local files go inside `workarea/feedback-dashboard/`.

```
workarea/feedback-dashboard/
├── index.html         # Dashboard page structure
├── style.css          # Dashboard styling
└── script.js          # Fetches and displays feedback data
```

---

## 3. Hosted Server — Detailed Plan

### 3.1 `index.php` — Feedback Form & Handler

**Purpose:** Display an HTML form and process submissions.

**Behaviour:**

1. When the page loads (GET request), display a form with three fields:
   - **Name** (text input, required)
   - **Email** (email input, required)
   - **Message** (textarea, required)
2. When the form is submitted (POST request):
   - Read and validate the three fields (trim whitespace, check not empty).
   - Sanitise all input with `htmlspecialchars()` to prevent XSS.
   - Read the existing `data/feedback.json` file (or start with an empty array if it doesn't exist).
   - Append a new entry with the fields: `name`, `email`, `message`, `submitted_at` (ISO 8601 timestamp).
   - Write the updated array back to `data/feedback.json`.
   - Show a success message and a link back to the form.
3. If validation fails, redisplay the form with an error message.

**Key implementation notes:**

- Use `declare(strict_types=1);` at the top.
- Use `json_decode()` / `json_encode()` with `JSON_PRETTY_PRINT` for readability.
- Use `file_get_contents()` and `file_put_contents()` with `LOCK_EX` to prevent write conflicts.
- The `data/` directory must be writable by the web server.

### 3.2 `api.php` — JSON API Endpoint

**Purpose:** Serve the feedback data as JSON so the local dashboard can fetch it.

**Behaviour:**

1. Set the response header to `Content-Type: application/json`.
2. Set CORS headers to allow the local dashboard to make cross-origin requests:
   - `Access-Control-Allow-Origin` — set to the specific local origin (or `*` for development).
   - `Access-Control-Allow-Methods: GET, OPTIONS`
   - `Access-Control-Allow-Headers: Content-Type`
3. Handle preflight `OPTIONS` requests by returning `200` with the CORS headers.
4. For `GET` requests:
   - Read `data/feedback.json`.
   - If the file doesn't exist, return an empty JSON array `[]`.
   - Output the JSON content.
5. Reject all other HTTP methods with a `405 Method Not Allowed` response.

**Key implementation notes:**

- Use `declare(strict_types=1);`.
- Only read the file, never write.
- Consider adding a simple shared-secret token check via a query parameter or header (e.g. `?token=abc123`) to restrict access. This is optional for a learning exercise but good practice.

### 3.3 `data/feedback.json` — Data File

**Purpose:** Flat-file storage for all feedback entries.

**Example content:**

```json
[
  {
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "message": "Great course, learned a lot!",
    "submitted_at": "2026-03-05T10:30:00+00:00"
  },
  {
    "name": "Bob Smith",
    "email": "bob@example.com",
    "message": "The exercises were very helpful.",
    "submitted_at": "2026-03-05T11:15:00+00:00"
  },
  {
    "name": "Carol Lee",
    "email": "carol@example.com",
    "message": "Could use more examples on arrays.",
    "submitted_at": "2026-03-05T14:02:00+00:00"
  }
]
```

**Notes:**

- The file is created automatically on the first form submission.
- Each entry is a JSON object with four keys.
- The array is ordered chronologically (newest last).

---

## 4. Local Dashboard — Detailed Plan

### 4.1 `index.html` — Page Structure

**Purpose:** Semantic HTML skeleton for the dashboard.

**Structure:**

```
<header>  → Page title: "Feedback Dashboard"
<main>
  <section id="controls">  → Refresh button
  <section id="stats">     → Summary: total count of feedback entries
  <section id="feedback">  → Container where feedback cards are injected by JS
</main>
<footer>  → Simple footer text
```

**Key implementation notes:**

- Link to `style.css` in `<head>`.
- Load `script.js` with `defer` at the bottom of `<head>` (or before closing `</body>`).
- Use semantic elements (`<header>`, `<main>`, `<section>`, `<article>`, `<footer>`).
- Include a `<noscript>` fallback message.
- Ensure WCAG 2.1 AA: proper heading hierarchy, colour contrast, focus styles.

### 4.2 `style.css` — Dashboard Styling

**Purpose:** Clean, mobile-first responsive layout.

**Plan:**

- Use CSS custom properties (variables) for colours and spacing.
- Mobile-first: single-column layout by default.
- Use CSS Grid on wider screens (≥768px) to show feedback cards in a 2-column grid.
- Each feedback card is a bordered/rounded box with:
  - Name (bold heading)
  - Email (smaller, muted text)
  - Message (body text)
  - Timestamp (footer, formatted nicely)
- A simple colour scheme (light background, white cards, accent colour for the header).
- Style the refresh button clearly.
- Loading state: a subtle "Loading…" indicator.
- Error state: red-toned message if fetch fails.

### 4.3 `script.js` — Fetch & Render Logic

**Purpose:** Fetch feedback data from the hosted server and render it to the page.

**Behaviour:**

1. Define a constant for the API URL (the hosted server's `api.php` endpoint).
2. On `DOMContentLoaded`, call a `loadFeedback()` function.
3. `loadFeedback()`:
   - Show a loading indicator in the feedback container.
   - Use `fetch(API_URL)` to request the JSON data.
   - Parse the response with `.json()`.
   - Pass the array to a `renderFeedback(data)` function.
   - If `fetch()` fails (network error or non-OK status), show an error message in the container.
4. `renderFeedback(data)`:
   - Clear the feedback container.
   - Update the stats section with the total count.
   - If the array is empty, show "No feedback yet."
   - Otherwise, loop through each entry and create an `<article>` card with the name, email, message, and formatted date.
   - Append all cards to the container.
5. Attach a click handler to the Refresh button that calls `loadFeedback()` again.

**Key implementation notes:**

- Use modern ES6+: `const`, `let`, arrow functions, template literals, `async/await`.
- Format the date using `new Date(submitted_at).toLocaleString()`.
- Use `textContent` (not `innerHTML`) when inserting user-provided data to prevent XSS.
- No external libraries — vanilla JS only.

---

## 5. CORS Considerations

Because the dashboard runs on a **different origin** (local server) than the hosted server, the browser will block requests unless the hosted server sends proper CORS headers.

**What to set in `api.php`:**

| Header | Value | Purpose |
|---|---|---|
| `Access-Control-Allow-Origin` | `*` (dev) or specific origin (prod) | Allows the browser to accept the response |
| `Access-Control-Allow-Methods` | `GET, OPTIONS` | Permitted HTTP methods |
| `Access-Control-Allow-Headers` | `Content-Type` | Permitted request headers |

The browser may send a preflight `OPTIONS` request before the actual `GET`. The API must respond to `OPTIONS` with a `200` status and the CORS headers.

---

## 6. Security Checklist

| Concern | Mitigation |
|---|---|
| XSS via form input | Sanitise with `htmlspecialchars()` before storing; use `textContent` in JS |
| File injection | Never use user input in file paths |
| JSON corruption | Use `LOCK_EX` flag when writing; validate JSON after decode |
| Unauthorised writes | Only `index.php` writes; `api.php` is read-only |
| Directory traversal | The `data/` directory should have no directory listing (use `.htaccess`) |
| CORS abuse | Restrict `Access-Control-Allow-Origin` to the dashboard's origin in production |

---

## 7. Setup & Deployment Steps

### Hosted Server

1. Upload `index.php`, `api.php`, and the `data/` folder to the web server.
2. Create the `data/` directory if it doesn't exist.
3. Set `data/` permissions so PHP can read and write (typically `755` or `775`).
4. Optionally add a `data/.htaccess` file with `Deny from all` to prevent direct browser access to the JSON file (all access should go through `api.php`).
5. Test the form at `https://yourserver.com/feedback/index.php`.
6. Test the API at `https://yourserver.com/feedback/api.php` — it should return `[]` or existing entries.

### Local Dashboard

1. Open the `workarea/feedback-dashboard/` folder.
2. Update the `API_URL` constant in `script.js` to point to the hosted server's `api.php` URL.
3. Open `index.html` in a browser (via a local server like VS Code Live Server, or Python's `http.server` to avoid CORS issues with file:// protocol).
4. The dashboard should fetch and display any submitted feedback.

---

## 8. Testing Plan

| # | Test | Expected Result |
|---|---|---|
| 1 | Load the feedback form in a browser | Form displays with Name, Email, Message fields |
| 2 | Submit the form with valid data | Success message shown; entry added to `feedback.json` |
| 3 | Submit the form with empty fields | Error message shown; no data written |
| 4 | Visit `api.php` directly in browser | JSON array of feedback entries returned |
| 5 | Open local dashboard | Feedback entries display as cards |
| 6 | Click the Refresh button | Dashboard re-fetches and updates |
| 7 | Submit new feedback, then refresh dashboard | New entry appears on dashboard |
| 8 | Delete `feedback.json`, then visit `api.php` | Empty array `[]` returned (no error) |
| 9 | Delete `feedback.json`, then submit form | File recreated with one entry |

---

## 9. Summary of Files to Create

| # | File | Location | Language |
|---|---|---|---|
| 1 | `index.php` | Hosted server `feedback/` | PHP + HTML |
| 2 | `api.php` | Hosted server `feedback/` | PHP |
| 3 | `data/.htaccess` | Hosted server `feedback/data/` | Apache config |
| 4 | `index.html` | `workarea/feedback-dashboard/` | HTML |
| 5 | `style.css` | `workarea/feedback-dashboard/` | CSS |
| 6 | `script.js` | `workarea/feedback-dashboard/` | JavaScript |

Total: **6 files** to implement.
