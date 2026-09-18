# Gym Booking System — Big-Picture Story Breakdown

Source: `docs/gym-booking-brief.md` (approved). This is the first UX/story artifact in the pipeline — one level down from the brief, one level up from detailed stories. It names the major admin-facing capabilities and workflows the system needs to support. It intentionally stops short of UI layout, field-level detail, or acceptance criteria; that level comes in the detailed-stories pass.

Single user type for this pass: the **gym admin**. There are no teacher or member logins — every story below is something the admin does, on behalf of users and teachers who don't interact with the system directly (brief: "System users — Admins only, for now").

---

## 1. Users & Membership Plans

### 1.1 Register and maintain users
As an admin, I want to register new users and maintain their records, so that I have a system of record for everyone who uses the gym.
*Grounded in: "Admins register users" (Overview); "Users — Registered and managed by admins."*

### 1.2 Define and maintain membership plans
As an admin, I want to define membership plans that vary by access type (gym-only vs. gym + lessons) and duration (monthly, single-day access, single-day class enrollment), so that I can offer the plan structures the gym sells.
*Grounded in: "Membership plans" section — access type and duration dimensions.*

### 1.3 Assign and change a user's membership plan
As an admin, I want to assign a membership plan to a user and change it when needed, so that each user's access and lesson credit reflect the plan they're actually on.
*Grounded in: "Each user holds a membership plan"; plan governs the lesson-credit rules used in scheduling (see 3.x).*

---

## 2. Teacher Availability & Capacity

### 2.1 Enter and edit teacher office hours
As an admin, I want to enter and edit each teacher's office hours on an admin-only screen, so that the schedule reflects availability the teachers report to me verbally or by chat, week to week.
*Grounded in: "Teachers... report their own office hours, which can change week to week... Teachers report their availability out-of-system... the admin enters and edits it... nothing teacher-facing." 3 teachers total.*

### 2.2 Enter and edit teacher class capacity
As an admin, I want to enter and edit each teacher's class capacity, so that enrollment stays within the limits each teacher sets for their own classes.
*Grounded in: "Sets their own class capacity"; same admin-only entry screen as 2.1.*

*Note: parallel classes run by different teachers at the same time are expected and not a conflict. This isn't a separate workflow of its own, but it constrains how scheduling (section 3) is built — see the grounding notes on 3.1 and 3.3, where it's restated in context.*

---

## 3. Class Scheduling & Enrollment

### 3.1 Enroll a user into a class slot
As an admin, I want to enroll a user into a teacher's class slot, so that the user has a scheduled class consistent with their membership plan.
*Grounded in: "Admins enroll users into a teacher's class slot." Lesson-inclusive plans carry a weekly session credit (2/week base) that bounds how much can be enrolled — the detailed mechanics of enforcing that credit belong to the next pass. Because "parallel classes across teachers are expected and not treated as a conflict" (Teachers section; see also 2.2), the system doesn't need to treat two different teachers each running a class at the same time slot as a facility/resource-level conflict when enrolling into either one. (This is about the facility hosting simultaneous classes, not about whether any one user can be enrolled into two overlapping classes at once — the brief doesn't address that either way, so nothing here asserts it.)*

### 3.2 Reschedule a single session
As an admin, I want to move a single scheduled session to another slot with the same teacher, so that I can accommodate a one-off change without disturbing the rest of the user's schedule or moving them to a different teacher.
*Grounded in: "Admins can reschedule a single session to another slot with the same teacher." A reschedule from the prior week can unlock a 3rd weekly session credit (hard-capped at 3/week, non-stacking) for lesson-inclusive plans — this rule shapes the workflow but its enforcement details are for the detailed pass. The membership's expiration date is a hard cap on this: no reschedule may push a session past the membership's expiration date. Also grounded in the brief: unused or missed lessons are simply lost when a membership expires or a session is otherwise missed — there's no makeup session or credit rollover beyond the 2/3-per-week carryover rule already covered above.*

### 3.3 Overhaul a user's whole weekly schedule
As an admin, I want to replace a user's entire weekly class schedule in one pass, so that I can handle bigger changes (e.g. a new recurring schedule) without rescheduling each session individually.
*Grounded in: "Admins can overhaul a user's whole weekly schedule." The same facility-level rule noted in 3.1 — different teachers running classes at the same time isn't treated as a conflict — applies here too; see 3.1 for what that does and doesn't cover. The membership's expiration date is a hard cap here too: the brief now states this explicitly as applying to "all reschedules, single-session or full weekly overhaul alike" — no reschedule of any kind, including an overhaul, may push a session past the membership's expiration date. The no-compensation rule in 3.2 (unused/missed lessons are simply lost, no makeup or extra rollover) applies here as well, since an overhaul is still subject to the same lesson-credit rules.*

### 3.4 Reassign students when a teacher drops a slot
As an admin, I want to manually reassign the students enrolled in a slot that a teacher has dropped, so that no one is left without a class when a teacher's availability changes.
*Grounded in: "If a teacher drops a slot that has enrolled students, admins manually reassign those students to a new slot. No automatic reassignment."*

---

## 4. Payments & Renewals

### 4.1 Record a payment and renew a membership
As an admin, I want to record a payment against a user's membership and have that renew the membership, so that membership records stay current with what's actually been paid.
*Grounded in: "Admin records a payment, which renews the membership." Record-keeping only — no access lockout tied to payment status.*

### 4.2 See renewal reminders on the dashboard
As an admin, I want the dashboard to surface which memberships are coming up for renewal, so that I know who to follow up with before their membership lapses.
*Grounded in: "The system surfaces a renewal reminder inside the admin dashboard."*

---

## Out of scope (carried from the brief, not restated as stories)

- Teacher and member logins
- Automatic reassignment when a teacher's availability changes
- Payment enforcement or access lockout for non-payment
- Anything not enumerated in the approved brief

## Open questions for the next pass

None of the above required inventing scope beyond the brief. Details the brief doesn't specify — e.g., the exact UI for capacity/availability entry, how the 2-vs-3 lesson credit is displayed or enforced at the point of scheduling, what "single-day access" vs. "single-day class enrollment" plans restrict a user to enrolling in, and the exact reassignment flow in 3.4 — are left for the detailed-stories pass rather than guessed here.

- **No-compensation rule vs. 3.4 (reassignment on a dropped slot):** the brief's no-compensation rule ("if a user fails to use/attend all their scheduled lessons... or otherwise misses sessions, there is no compensation... unused or missed lessons are simply lost") isn't scoped to reschedule/overhaul the way the expiration-cap rule is — it's stated more generally. It's unclear how this interacts with 3.4: if a teacher drops a slot and the admin can't reassign the affected student before that session's original time passes, does that session count as a "missed session" the student simply loses (per the no-compensation rule), or is the admin expected to always reassign before the slot occurs so this case doesn't arise? The brief doesn't say. Flagging this for the detailed-stories pass rather than assuming either answer.
