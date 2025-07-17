# Daily Email Digest Preferences

Happy to refine this feature request according to INVEST criteria!

- **I**ndependent: Isolated from other features; existing notification-settings module is already isolated with no dependencies.
- **N**egotiable: Time picker component can be dropped if time runs short, shipping only the on/off toggle.
- **V**aluable: Reduces unwanted emails, projected to lower churn by 2% within one quarter.
- **E**stimable: Development requires 2 days, QA requires 1 day.
- **S**mall: Work fits easily within a single sprint (3 days total).
- **T**estable: Clear success criteria including UI toggle functionality, time selection, database updates, and error handling.

## Implementation Details

**Frontend:**
- Add "Email Digests" section under Settings → Notifications
- Include on/off toggle (default on)
- Add time picker (00:00-23:45, 15-min increments, default 08:00)
- Internationalize all new strings

**Backend:**
- Endpoint: PATCH /v1/user/preferences/digest (requires authentication)
- Payload: { enabled: boolean, send_time: "HH:MM" }
- Store in user_preferences.email_digest table
- Return 400 for invalid time format or unauthenticated requests

**Testing Requirements:**
- Jest unit tests for all paths (success, unauthorized, validation errors)
- Cypress E2E test verifying full user flow and database updates

Let me know if anything else would help!

---

## Previous Request:

## Feature Request – Daily Email Digest Preferences

### Context
Customers like the daily digest but want more control. 65 % of active users have asked to pause or change delivery time. Reducing unwanted email should lower churn by at least 2 %.

---

### Requirements
* **UI** – Add a new "Email Digests" section under **Settings → Notifications**.  
  * Toggle switch: **On / Off** (default **On**).  
  * Time picker: 00:00 – 23:45 in 15-minute increments, visible only when toggle = On, default 08:00.
* **Back-end API** – `PATCH /v1/user/preferences/digest` (session-token auth).  
  * Payload: `{ enabled: boolean, send_time: "HH:MM" }`
  * Persist to `user_preferences.email_digest` (`enabled`, `send_time`).
* **Scheduler** – Daily-digest job fetches `send_time` per user; falls back to 08:00 if null.
* **Tech stack** – Vue 3 front end, Go API, Postgres DB.
* **i18n** – All new strings added to translation files.

---

### Acceptance Criteria
1. Authenticated user can disable digests; DB row updates (`enabled = false`).
2. When enabled, digest is delivered at the chosen time the **next day**.
3. API returns **400** on invalid `send_time` or unauthenticated request.
4. Jest unit tests cover happy path, unauthorized, and validation error.
5. Cypress e2e: user sets time to 18:30 → DB row shows 18:30 → success toast appears.

---

### Effort & Size
* **Dev**: 2 days  
* **QA**: 1 day  
Fits easily inside a single sprint.

---

### Dependencies
None. Existing notification-settings module is already isolated.

---

### Negotiable Scope
If time runs short, drop the **time picker** and ship only the On/Off toggle.

---

### Value
Fewer "too many emails" complaints → projected 2 % reduction in churn within one quarter.

