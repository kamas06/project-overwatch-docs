# User Guide — Member Lifecycle & Workflows

## Reference: Lookup Values

Before walking through the workflows, here are the valid values for the key fields that drive a member's lifecycle.

### Member Status

| Display Name | Code | Description |
|---|---|---|
| Active | `ACTIVE` | Current active member |
| Inactive | `INACTIVE` | Member has left or is not currently active |
| Suspended | `SUSPENDED` | Membership temporarily suspended |

### Member Class

| Display Name | Code | Description |
|---|---|---|
| Waiting List | `WAITING_LIST` | Prospective member awaiting activation |
| Probationer | `PROBATIONER` | New member on probation |
| Junior Probationer | `JUNIOR_PROBATIONER` | Under-18 member on probation *(to be added — see note below)* |
| Full Member | `FULL` | Full voting member |
| Junior | `JUNIOR` | Under-18 member |
| Sabbatical | `SABBATICAL` | Temporarily reduced membership |
| Associate | `ASSOCIATE` | Associate non-voting member |
| Honorary | `HONORARY` | Honorary membership granted |
| Life Member | `LIFE` | Lifetime membership |

### Member Section

| Display Name | Code |
|---|---|
| Small-bore Rifle Section | `SMALL_BORE_RIFLE` |
| Sporting Rifle Section | `SPORTING_RIFLE` |
| Air Section | `AIR` |
| Gallery Rifle Section | `GALLERY_RIFLE` |
| Practical Shooting Section | `PRACTICAL_SHOOTING` |
| Black Powder Section | `BLACK_POWDER` |
| 1500 Section | `SECTION_1500` |
| Heritage Section | `HERITAGE` |
| Archery Section | `ARCHERY` |
| Fullbore Section | `FULLBORE` |

> **Note:** Section is **optional** and can be left blank for members who do not yet have a section assignment (e.g., waiting list members, legacy members with unknown sections).

---

## Member Lifecycle Overview

```
[Waiting List] ──► [Probationer / Junior Probationer] ──► [Full Member / Junior]
       │                                                           │
       │ (never joins)                                    (leaves / suspends)
       ▼                                                           │
  [Inactive]                                              [Inactive / Suspended]
                                                      or
                                             [Sabbatical] ◄── (temporary break)
```

---

## Workflows

### 1. New Prospective Member — Added to the Waiting List

**When to use:** A person expresses interest in joining the club. They are not yet a member.

**Steps:**
1. Create a new member record with all known personal details.
2. Set the fields as shown below. Date Joined should be populated as the date they were added to the waiting list. Section can be left as **Unknown** if not yet decided, or set to their preferred section if known.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Active** |
| Class | **Waiting List** |
| Section | *(blank/null, or populated if known)* |
| Date Joined | Populated |
| Date Left | — |
| Probation End Date | — |

> **Note:** `Date Joined` is required for all classes except Waiting List, where it may be left blank if the date is not known. However, it is best practice to populate it. Section is optional and can remain blank until assigned.

---

### 2. Waiting List — Member Does Not Join

**When to use:** A prospective member on the waiting list decides not to join, or is removed from the list.

**Steps:**
1. Set **Status** to `Inactive`.
2. Set **Date Left** to the date they were removed.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Inactive** |
| Class | Waiting List *(unchanged)* |
| Section | *(unchanged)* |
| Date Joined | Populated *(unchanged)* |
| Date Left | Populated |
| Probation End Date | — |

---

### 3. Waiting List → Probation

**When to use:** A prospective member is accepted into the club and begins their probation period.

**Steps:**
1. Set **Class** to `Probationer` (or `Junior Probationer` if under 18).
2. Confirm **Section** is set correctly — update from Unknown to their actual section if needed.
3. Leave **Probation End Date** blank until they complete probation.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Active** |
| Class | **Probationer** or **Junior Probationer** |
| Section | Specific section (e.g. Gallery Rifle Section), or blank if not yet assigned |
| Date Joined | Populated *(unchanged)* |
| Date Left | — |
| Probation End Date | — |

---

### 4. Probationer → Full Member

**When to use:** A probationer completes their probation period and becomes a full member.

**Steps:**
1. Set **Class** to the appropriate final class — `Full Member` for adults, `Junior` for under-18s.
2. Set **Probation End Date** to the date they completed probation.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Active** |
| Class | **Full Member** or **Junior** |
| Section | *(unchanged)* |
| Date Joined | Populated *(unchanged)* |
| Date Left | — |
| Probation End Date | Populated |

---

### 5. Member Leaves the Club

**When to use:** A member (of any class) permanently leaves the club.

**Steps:**
1. Set **Status** to `Inactive`.
2. Set **Date Left** to the date they left.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Inactive** |
| Class | *(unchanged — reflects what they were when they left)* |
| Section | *(unchanged)* |
| Date Joined | Populated |
| Date Left | Populated |
| Probation End Date | *(unchanged)* |

---

### 6. Member Goes on Sabbatical

**When to use:** A member temporarily steps back from the club but has not permanently left.

**Steps:**
1. Set **Class** to `Sabbatical`.
2. Leave **Date Left** blank — they have not left.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Inactive** |
| Class | **Sabbatical** |
| Section | *(unchanged)* |
| Date Joined | Populated |
| Date Left | — |
| Probation End Date | *(unchanged)* |

> When they return, set the Class back to their previous class (e.g. `Full Member`) and set Status back to `Active`.

---

### 7. Member Is Suspended

**When to use:** A member's membership is temporarily suspended (e.g. disciplinary action).

**Steps:**
1. Set **Status** to `Suspended`.
2. Leave **Class**, **Section**, and **Date Left** unchanged.

**Resulting record state:**

| Field | Value |
|---|---|
| Status | **Suspended** |
| Class | *(unchanged)* |
| Section | *(unchanged)* |
| Date Joined | Populated |
| Date Left | — |
| Probation End Date | *(unchanged)* |

> When the suspension is lifted, set Status back to `Active`.

---

## Open Items / Decisions

| # | Question | Decision |
|---|---|---|
| 1 | Should we add a **Junior Probationer** class to distinguish under-18 probationers from adult probationers? | **Yes** — add `JUNIOR_PROBATIONER` to `lookup_member_class`. |
| 2 | Make Member **Section** field nullable | **Yes** — section is now optional and can remain blank. |

---

## General Notes

- Some historical records may be incomplete. Not all fields will be populated for long-standing or legacy members — this is expected.
- `Date Joined` is **required** for all member classes except `Waiting List`.
- `Date Left` should only be set when a member permanently leaves — do not set it for sabbatical or suspension.
- The `Probation End Date` field lives on the membership detail record and is set once probation is formally completed.