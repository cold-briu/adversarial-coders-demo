# Gym Booking System — Project Brief

Scope summary produced by the project-manager agent from a conversation with the user. This is a shared-understanding brief, not a spec — no acceptance criteria or technical detail. It's the handoff point for whichever agent writes the actual spec, once that agent exists.

## Overview

A scheduling and membership-management system for a gym, used by gym admins. Admins register users, manage memberships, enroll users into teacher-led classes, adjust schedules, and record payments.

## Users

Registered and managed by admins. Each user holds a membership plan.

## Membership plans

Plans vary along two dimensions:

- **Access type**: gym-only, or gym + lessons
- **Duration**: monthly, single-day access, single-day class enrollment

Lesson-inclusive plans carry a weekly session credit: 2 lessons per week by default, with a 3rd available in a given week if the user rescheduled a session from the prior week. This carryover is hard-capped at one extra session per week (3/week maximum) — it does not stack even across multiple consecutive reschedules.

## Teachers

3 teachers total. Each:

- Reports their own office hours, which can change week to week
- Sets their own class capacity

Teachers report their availability out-of-system (verbally, chat, etc.); the admin enters and edits it. The system only needs an admin-facing screen to enter/edit teacher availability and capacity — nothing teacher-facing.

Parallel classes across teachers are expected and not treated as a conflict.

## Scheduling

- Admins enroll users into a teacher's class slot.
- Admins can reschedule a single session to another slot with the *same* teacher.
- Admins can overhaul a user's whole weekly schedule.
- The membership's expiration date is a hard cap for **all** reschedules, single-session or full weekly overhaul alike: no reschedule of any kind may push a session past the membership's expiration date.
- If a user fails to use/attend all their scheduled lessons before the membership expires, or otherwise misses sessions, there is no compensation: unused or missed lessons are simply lost, with no makeup session or credit rollover beyond the existing 2-per-week/3-per-week carryover rule.

## Availability changes

If a teacher drops a slot that has enrolled students, admins manually reassign those students to a new slot. No automatic reassignment.

## Payments

- Admin records a payment, which renews the membership.
- The system surfaces a renewal reminder inside the admin dashboard.
- Record-keeping only — no access lockout for non-payment.

## System users

Admins only, for now.

## Out of scope (for now)

- Teacher and member logins
- Automatic reassignment when a teacher's availability changes
- Payment enforcement / access lockout
- Additional features to be defined later
