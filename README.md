# Fitness Influencer Coaching Platform — Database Design

This is my submission for the **Databases** module of Web Dev Cohort 2026.

The task was to design an ER diagram for an online fitness coaching platform — the kind a fitness influencer would use to move from managing clients over Instagram DMs to running a proper structured platform with plans, sessions, payments, and progress tracking.

---

## What the platform is about?

A fitness influencer starts small — a few clients, some DMs, Google Meet calls. But as the brand grows, things get messy. You need a real system that can handle:

- Onboarding clients and trainers
- Selling one-time or subscription-based coaching plans
- Scheduling consultations and live sessions
- Tracking a client's weight, measurements, and check-ins over time
- Storing trainer notes and feedback separately
- Managing payments and billing cycles

This database design tries to cover all of that without overcomplicating things.

---

## ER Diagram

![Fitness Coaching Platform ER Diagram](https://res.cloudinary.com/db7qmdfr2/image/upload/v1775580791/diagram-export-4-7-2026-10_07_03-PM_a4zpi5.png)

> **Note:** Replace the Cloudinary link above with your actual uploaded image URL after uploading to [cloudinary.com](https://cloudinary.com).

---

## Entities at a glance

| Entity | What it stores |
|---|---|
| `users` | Everyone on the platform — trainers and clients both |
| `trainer` | Trainer-specific info like bio, specializations, hourly rate |
| `client` | Client-specific info like health conditions, fitness goals, body stats |
| `coaching_plans` | Plans a trainer creates — one-time, subscription, or consultation only |
| `enrollments` | Which client enrolled in which plan under which trainer |
| `sessions` | Consultation calls, coaching calls, or live group sessions |
| `subscriptions` | Billing info tied to an enrollment |
| `payments` | Individual payment records — UPI, card, cash, etc. |
| `progress_logs` | Weekly check-ins with weight, body measurements, body fat % |
| `progress_photos` | Photos attached to a progress log |
| `trainer_notes` | Private notes a trainer writes about a client |

---

## A few design decisions I made

**Why separate `users`, `trainer`, and `client` tables?**  
Because not everyone is the same type of user. A trainer doesn't need a `dob` or `fitness_goals`, and a client doesn't need `hourly_rate` or `specializations`. Splitting them out keeps things clean and lets the `users` table stay minimal.

**Why does `enrollments` exist as its own table?**  
Because a client can buy multiple plans over time, and one plan can be bought by many clients. The enrollment is the relationship between them — it also stores start/end dates and status, which neither the client nor the plan should own.

**Sessions**  
A session is a scheduled call — it has a meeting link, a duration, a status.

**Why store `trainer_notes` separately?**  
Because trainer notes are private observations — not something the client necessarily sees. Keeping them in their own table makes it easy to control access.

**Payments vs Subscriptions**  
A subscription tracks the billing and its current status (active, expired, cancelled). A payment is an individual transaction. One subscription can have many payments over time, so they're separate.

---

## Relationships overview

```
users --< trainer
users --< client

trainer --< coaching_plans
trainer --< enrollments
trainer --< sessions
trainer --< trainer_notes

client --< enrollments
client --< sessions
client --< trainer_notes
client --< payments
client --< progress_logs

coaching_plans --< enrollments

enrollments --| subscriptions
enrollments --< payments
enrollments --< sessions

progress_logs --< progress_photos
```

---

## Tools used

- **Eraser.io** — for designing the ER diagram
- Dark theme because it just looks better

---

## Submission info

- **Cohort:** Web Dev Cohort 2026  
- **Module:** Databases  
- **Deadline:** Apr 8, 2026, 12:30 AM  
- **Diagram file:** See the image above or open `er-diagram.png` in this repo
