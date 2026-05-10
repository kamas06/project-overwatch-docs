# User Guide - Emails, Templates, and Campaigns

## Quick Overview

The communications feature uses two key terms:

| Term | Meaning |
|---|---|
| Template | A reusable subject, body, and optional routing definition stored in the Template Library |
| Campaign | The record created when a bulk email is launched from the members list |

The Communications area supports three operator workflows:
- sending an email to one member from that member's record
- launching a bulk email from the current members-list filters
- managing reusable templates and reviewing campaign history in the Communications workspace

> **Limitation:** Attachments are not currently supported. All communication is text-based (subject and body only).

> **Note:** Bulk campaigns are created when you launch them. The UI does not provide activate, deactivate, or delete actions for campaigns. Those actions apply to email templates.

**How emails are delivered:**

| Type | Sent via | What to expect |
|---|---|---|
| Custom email (one-to-one) | Gmail, as the logged-in operator | Appears in your Gmail Sent box |
| Template workflow email | Gmail, as the logged-in operator | Appears in your Gmail Sent box |
| Bulk campaign | Brevo (email marketing service) | Does not appear in Gmail; tracked in the Campaign dashboard |

---

## 1. Send a Custom Email to One Member

**When to use:** You need a one-off message for a single member.

**Steps:**
1. Open the member's detail page.
2. Select **Email Member**.
3. Leave **Message source** set to `Custom email`.
4. Review **To**, add optional **CC** and **Reply-to email**, then enter the **Subject** and **Body**.
5. Select **Send email**.

**What happens next:**
- The composer stays locked to that member.
- The result card shows the send status plus the rendered subject and body that were actually dispatched.
- The email is sent via your Gmail account — it will appear in your Gmail Sent box.

---

## 2. Send a Template Workflow Email to One Member

**When to use:** You want to start from a saved workflow template instead of writing the message from scratch.

**Example:** A template can be set up to notify the police about a new member joining the club. Instead of composing the same message each time, you select the template and send it with the new member's details pre-filled.

**Steps:**
1. Open the member's detail page and select **Email Member**.
2. Change **Message source** to `Template workflow`.
3. Choose the required **Template**.
4. Review the rendered **To**, **CC**, **Subject**, and **Body**.
5. Make any send-time edits you need.
6. Select the send action shown by the composer.

**Switching between custom and template mode:**
- Changing to `Template workflow` renders the selected template into **Subject** and **Body**.
- Changing back to `Custom email` clears the rendered template draft so you can start again with a blank message.
- Choosing a different template re-renders the draft from that template.

**Template overrides:**
- Template mode can preload fixed **To** and **CC** recipients from the template.
- You can still override **To**, **CC**, **Subject**, and **Body** before sending.
- The send result still records which template code was used.

> If the composer was opened from a template-specific shortcut or link, the template may already be selected when the page opens.

> The email is sent via your Gmail account — it will appear in your Gmail Sent box.

---

## 3. Launch a Bulk Email Campaign

**When to use:** You need to send the same message to every member matching the current directory filters.

**Steps:**
1. Open **Members**.
2. Apply the search, filters, and sort you want.
3. Select **Email N matching members**.
4. In the bulk composer, confirm the **Matching recipients** count.
5. Add optional **Reply-to email**, then enter the **Subject** and **Body**.
6. Select **Launch bulk campaign**.

**How audience capture works:**
- The composer captures the current members-list query.
- Pagination is removed before the send request is created.
- The campaign targets all members matching the filters, not only the rows visible on the current page.
- Individual recipient addresses are intentionally hidden in the bulk composer.

> **Brevo sending limits:** Bulk campaigns are delivered via Brevo. The club account is currently on the free tier with a limit of **300 emails per month**. Check the remaining allowance before launching a large campaign. Sent campaigns do not appear in Gmail.

---

## 4. Review Campaign History

**When to use:** You need to check the outcome of a bulk campaign after it has been launched.

**Steps:**
1. Open **Communications**.
2. Use **Campaign dashboard** to review the newest campaigns first.
3. Select a campaign to inspect recipient delivery details.
4. Review status counts, provider information, message IDs, and the recipient drill-down table.

**Important:**
- The dashboard is for tracking and review.
- New bulk campaigns are launched from **Members**, not created directly in the dashboard.

### Understanding Recipient Statuses

Each recipient in a campaign has an individual delivery status. Statuses are set initially when the campaign is launched, then updated automatically as Brevo reports back what happened to each email.

| Status | Meaning |
|---|---|
| `PENDING` | Recipient row created; Brevo has not yet acknowledged the message |
| `SENT` | Brevo has accepted the message and passed it to the receiving mail server |
| `DEFERRED` | Delivery temporarily delayed; Brevo will keep retrying |
| `DELIVERED` | The receiving mail server confirmed successful delivery |
| `OPENED` | The recipient opened the email (requires tracking pixel to load) |
| `CLICKED` | The recipient clicked a link inside the email |
| `BOUNCED` | Delivery failed — either a temporary soft bounce or a permanent hard bounce |
| `BLOCKED` | Brevo blocked the send (e.g. recipient is on a suppression list) |
| `INVALID` | The email address was rejected as invalid at send time |
| `SPAM` | The recipient marked the email as spam |
| `UNSUBSCRIBED` | The recipient used the Brevo unsubscribe link |
| `FAILED` | An unexpected error was reported by the provider |
| `DRY_RUN` | The campaign ran in dry-run mode; no email was actually sent |

**How statuses change:**

1. When the campaign launches, every recipient starts at `PENDING`.
2. Brevo sends the email and immediately fires a `sent` event — the status advances to `SENT`.
3. From `SENT`, the status advances further as Brevo delivers subsequent events: `DELIVERED`, `OPENED`, `CLICKED`, and so on.
4. Terminal statuses — `BOUNCED`, `BLOCKED`, `INVALID`, `SPAM`, `UNSUBSCRIBED`, `FAILED` — are not overwritten by later events.
5. If `email-enabled` is off, statuses stay at `DRY_RUN` and no emails leave the system.

> Statuses are updated by Brevo webhooks. There can be a short delay between an event occurring and it appearing in the dashboard.

---

## 5. Manage Reusable Email Templates

The **Template Library** lives in the **Communications** workspace below the campaign dashboard.

### Create a Template

1. Open **Communications**.
2. In **Template Library**, select **New template**.
3. Enter the immutable **Template code** and the user-facing **Display name**.
4. Add optional **To recipient** and **CC recipients** if the workflow needs fixed routing.
5. Enter the **Subject template** and **Body template**.
6. Review the preview, then select **Create template**.

### Edit a Template

1. Select **Edit** on the template you want to change.
2. Update the display name, routing, subject, or body.
3. Select **Save template**.

> The template code is locked after creation and cannot be edited later.

### Activate an Inactive Template

1. Find the inactive template in the library.
2. Select **Activate**.
3. The template becomes available again in template workflow sends.

### Deactivate an Active Template

1. Select **Deactivate** on the active template.
2. Confirm **Archive template**.
3. The template becomes inactive and is removed from new-send selection.

### Delete an Inactive Template

1. Make sure the template is already inactive.
2. Select **Delete**.
3. Confirm **Delete template**.

Use **Deactivate** when you want to stop future use but keep the template available for historical reference. Use **Delete** only for inactive templates you no longer need.

### Understanding Merge Variables

**Merge variables** let you insert dynamic information into your templates. When an email is sent, variables like `{{member.first_name}}` are replaced with the actual member's information.

**Available merge variables:**
- `{{member.first_name}}` — member's first name
- `{{member.last_name}}` — member's last name
- `{{member.full_name}}` — member's full name
- `{{member.email}}` — member's email address
- `{{member.brpc_number}}` — member's BRPC number
- `{{operator.display_name}}` — the logged-in operator's name

**How to use them:**
1. In the **Subject template** or **Body template** field, click the variable you want to insert.
2. The variable is appended to the currently focused field.
3. When the template is sent, the variable is replaced with the actual value for that member.

**Example:** If you type "Welcome, " in the subject template and then click `{{member.first_name}}`, the rendered subject becomes "Welcome, " + the member's first name. When sent to Jane, the subject shows "Welcome, Jane".