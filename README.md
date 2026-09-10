# NALB — Capstone Defense Presentation

**Development of a Real-Time Inventory Management and Customization Platform for Local Furniture Traders**

Capstone project defense presentation · Fall 2025–2026
Libyan International University · Faculty of Information Technology · Department of Software Engineering

**Student:** Abdulrahman Ali Elnihwi (4101)
**Supervisor:** Assist. Dr. Abdulmajid Hissen

---

## ▶ View the presentation

**https://abdurahman-cj18.github.io/NALB/**

Use `←` `→` to navigate · `F` for full screen · `P` to auto-play · click any diagram to enlarge it.

---

## About the project

Most furniture warehouses in Libya close at around 2 PM, while the retail showrooms
they supply stay open until nine in the evening. For the whole second half of the
trading day a trader has no reliable way to confirm whether an item is in stock —
so a sale is either guessed at, deferred to the next morning, or lost.

A survey of twelve retail traders measured the cost of that gap:

- **83.4%** receive customer requests after the warehouses close, frequently or almost daily
- **75%** name not knowing live quantities as their greatest difficulty
- **66.7%** still depend on a telephone call to establish whether an item exists

**NALB** is a closed business-to-business platform that gives a warehouse's authorized
traders continuous, authenticated access to live stock, a structured customization
module for locally manufactured furniture, and a collision-free order code for every
order placed. There is no public registration route anywhere in the system — every
account is issued by the warehouse, which is what keeps wholesale pricing confidential.

## Built with

| Layer | Technology |
| --- | --- |
| Trader mobile app | Flutter · Riverpod · Dio |
| Admin dashboard | Laravel Blade · Tailwind CSS |
| Backend API | Laravel 13 · REST · Sanctum |
| Database | SQLite |
| Notifications | Firebase Cloud Messaging |
| Testing | PHPUnit |

## Engineering notes

The demanding part of the work was not the number of screens but the correctness of a
few shared operations. Stock is a single quantity that many traders may consume at the
same moment, and an order is only meaningful if it can never exist without the stock it
claims.

- The entire order sequence — reserving stock, creating the order and its line items,
  and allocating the identifier — runs inside **one database transaction**.
- Products are locked in **ascending id order** so that concurrent placements can never
  form a circular wait.
- Uniqueness of the order code is delegated to a **database constraint** rather than an
  application-level check-then-insert, which would itself be a race.
- Prices are stored as **integers in the smallest currency unit**, so totals carry no
  rounding error.
- Domain events are dispatched **after the transaction commits**, so a notification can
  never describe an order that was rolled back.

Testing confirmed the result directly: ten simultaneous orders against five available
units produced exactly five accepted orders, five rejections, and no negative stock.

All **23 documented test cases** pass, and every functional requirement from FR-1 through
FR-10 is covered by the requirement-to-test traceability matrix.

## This repository

Contains the defense presentation only — a single self-contained HTML file with every
diagram and screenshot embedded. The platform source code is maintained separately.
