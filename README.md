# Workshop 2 — Design Artifacts and System Modeling

**Project:** SmartSnack Office – Automated Healthy Snack Supply System  
**Group 1:** Django sin cadenas  
**Course:** Software Engineering II — 2026-I  
**Universidad Nacional de Colombia · Faculty of Engineering**

---

## Table of Contents

1. [CRC Cards](#1-crc-cards)
2. [Mockups](#2-mockups)
3. [Business Process Model (BPMN)](#3-business-process-model-bpmn)
4. [Architecture Diagram](#4-architecture-diagram)
5. [Class Diagram (UML)](#5-class-diagram-uml)
6. [Relational Database Model (ER Diagram)](#6-relational-database-model-er-diagram)

---

## 1. CRC Cards

**Location:** `Workshop-2/Deliverables/CRC_Cards_SmartSnackOffice.pdf`

CRC (Class–Responsibility–Collaborator) cards identify the main classes of the system, what each class is responsible for, and which other classes it collaborates with.

The main classes identified for SmartSnack Office are:

| Class | Responsibilities | Collaborators |
|---|---|---|
| `User` | Store authentication data, manage role, track login attempts, revoke tokens | `Buyer`, `Seller`, `Admin`, `JWTToken` |
| `Buyer` | Manage cart, wishlist, nutritional goals, dietary restrictions, place orders | `Cart`, `Order`, `WishList`, `NutritionalGoal`, `Review`, `Recommendation` |
| `Seller` | Publish and manage products, manage inventory, handle promotions, respond to queries, process claims | `Product`, `Promotion`, `Order`, `CustomerQuery`, `Claim` |
| `Admin` | Moderate users and products, manage categories/tags, audit transactions, configure AI weights, trigger backups | `User`, `Product`, `Category`, `Tag`, `TransactionLog`, `AIConfig` |
| `Product` | Store product data (name, ingredients, nutritional table, price, stock, images), be searchable and recommendable | `Category`, `Tag`, `Review`, `Promotion`, `Seller` |
| `Cart` | Hold product selections temporarily, calculate subtotal/tax/total in real time, persist across sessions | `Buyer`, `Product`, `CartItem` |
| `Order` | Record confirmed purchases, track state transitions (Pending → Paid → Shipped → Delivered → Refunded), trigger inventory deduction | `Buyer`, `Seller`, `Product`, `Payment`, `ShippingLabel` |
| `Payment` | Interface with external payment gateway, handle callbacks, manage refunds | `Order`, `PaymentGateway` |
| `Review` | Store star rating (1–5) and optional text (≤500 chars) linked to a delivered order | `Buyer`, `Product`, `Order` |
| `Promotion` | Define discount rules (%, fixed, 2x1, expiry-based) with start/end dates; auto-activate | `Seller`, `Product` |
| `AIAgent` | Profile users continuously, generate recommendations (Item-based CF), detect price/purchase anomalies, retrain models via Celery | `Buyer`, `Product`, `Recommendation`, `CeleryTask` |
| `Recommendation` | Deliver ranked product suggestions filtered by dietary restrictions and stock | `Buyer`, `Product`, `AIAgent` |
| `NutritionalGoal` | Store nutrient targets and periods; calculate and display progress based on purchases | `Buyer`, `Product`, `Order` |
| `WishList` | Track out-of-stock products a buyer wants; trigger restock notifications | `Buyer`, `Product`, `NotificationService` |
| `Category` / `Tag` | Classify products; reassign on deletion | `Product`, `Admin` |

---

## 2. Mockups

**Location:** `IS2_-_Diseño.docx` — Section: *Mockups* (Images 1–5)

Mockups were created for the main screens of the application. Each wireframe illustrates the layout and key UI components available to each actor.

| # | Screen | Actor | Description |
|---|---|---|---|
| 1 | **Catalog** | Buyer | Browse all products organized by category. AI-selected items are highlighted on the main page. Includes a search bar and category filter panel. |
| 2 | **Login** | All roles | Main entry point. Users authenticate with email and password and are redirected to the panel matching their role (Buyer, Seller, or Admin). |
| 3 | **User Profile** | Buyer | Manage personal data: name, email, delivery addresses, payment methods, allergies, and nutritional goal progress bar. Accessible via the user icon in the header. |
| 4 | **Wish List** | Buyer | View products saved as "Desired". Each item shows image, name, current price, and availability. Actions: add to cart or remove from list. |
| 5 | **Checkout / Payment** | Buyer | Confirm cart contents, validate delivery address, and be redirected to the external payment gateway to complete the transaction. |

---

## 3. Business Process Model (BPMN)

**Location:** `IS2_-_Diseño.docx` — Section: *Diagramas BPMN* (Images 6–7)

Two core business processes were documented using BPMN notation.

### Process 1 — Login (Image 6)

Models the authentication flow for any user role. The process covers entering credentials, backend JWT validation, handling of failed attempts (rate-limiting after 5 failures, 30-minute lockout), and redirection to the role-specific panel on success.

### Process 2 — Product Purchase (Image 7)

This is the primary business process of the platform. It covers the full buyer journey:

1. The buyer browses the AI-curated catalog and searches or filters products.
2. The buyer selects a product; the system checks dietary restrictions and blocks incompatible items.
3. The buyer adds items to the cart; stock is temporarily reserved (15 min) on checkout initiation.
4. The buyer is redirected to the external payment gateway.
5. The gateway sends a callback; the system confirms or cancels the order and deducts inventory.
6. The seller updates the order state (Paid → Shipped → Delivered).
7. The buyer receives the order and submits a rating/review.
8. Throughout the entire flow, the AI Agent collects behavioral events to improve future personalized recommendations.

---

## 4. Architecture Diagram

**Location:** `IS2_-_Diseño.docx` — Section: *Análisis del proyecto* (Analysis document) and workshop deliverable

The architecture follows a layered approach with the following tiers:

| Layer | Components | Technologies |
|---|---|---|
| **Clients** | Responsive web UI for Buyer, Seller, Admin | Next.js (React) |
| **API layer** | REST endpoints, RBAC middleware, OpenAPI 3.0 docs, JWT auth | Django REST Framework, SimpleJWT |
| **Services** | Auth, Catalog, Orders, AI Engine, Notifications, PDF Generator | Django apps, Celery workers |
| **Infrastructure** | Container orchestration, async task queue, CI/CD pipeline, logging | Docker Compose, Redis, GitHub Actions |
| **Data** | Relational store, ML model storage, database backups | PostgreSQL, Scikit-learn, pg_dump |
| **External** | Payment processing and email delivery | Payment Gateway (REST), SMTP |

**Key communication flows:**
- All client–server communication uses HTTPS/TLS 1.2+.
- JWT tokens (access: 60 min, refresh: 7 days) are validated on every authenticated request.
- Async tasks (model retraining, anomaly detection, notifications) run via Celery + Redis without blocking the API.
- The payment gateway integrates via REST callbacks to `/api/payments/callback`.

---

## 5. Class Diagram (UML)

**Location:** `IS2_-_Diseño.docx` — Section: *Diagrama UML* (Image 8)

The UML class diagram covers the main domain entities, their attributes, methods, and relationships (associations, generalizations, compositions).

**Main classes and key relationships:**

- `User` is the base class; `Buyer`, `Seller`, and `Admin` extend it via inheritance.
- `Buyer` has a composition relationship with `Cart`, `WishList`, and `NutritionalGoal` (1-to-1).
- `Buyer` has a 1-to-many association with `Order` and `Review`.
- `Seller` has a 1-to-many association with `Product` and `Promotion`.
- `Product` is associated with `Category` (many-to-one) and `Tag` (many-to-many).
- `Order` aggregates `OrderItem` entries, each referencing a `Product`.
- `Payment` is associated 1-to-1 with `Order` and communicates with the external `PaymentGateway`.
- `AIAgent` reads from `UserEvent` and `Product` to produce `Recommendation` instances.

---

## 6. Relational Database Model (ER Diagram)

**Location:** `IS2_-_Diseño.docx` — Section: *Diagrama Entidad-Relación* (Image 9)

The relational schema is implemented in PostgreSQL and managed through Django's native migration system (`makemigrations` / `migrate`).

**Core tables and relationships:**

| Table | Primary Key | Notable Foreign Keys | Notes |
|---|---|---|---|
| `users` | `id` (UUID) | — | Base table; `role` field distinguishes Buyer/Seller/Admin |
| `buyers` | `user_id` (FK) | `users.id` | Extends `users`; stores delivery address, dietary restrictions |
| `sellers` | `user_id` (FK) | `users.id` | Extends `users`; stores commercial and tax info |
| `products` | `id` (UUID) | `sellers.user_id`, `categories.id` | Includes nutritional table as JSONB; `status` field for suspension |
| `product_tags` | composite | `products.id`, `tags.id` | Many-to-many join table |
| `categories` | `id` | — | Managed by Admin; deletion triggers reassignment |
| `tags` | `id` | — | Managed by Admin |
| `carts` | `id` | `buyers.user_id` | Persists across sessions |
| `cart_items` | `id` | `carts.id`, `products.id` | Stores quantity; validated against available stock |
| `orders` | `id` (UUID) | `buyers.user_id` | `status` enum: Pending → Paid → Shipped → Delivered → Refunded |
| `order_items` | `id` | `orders.id`, `products.id` | Snapshot of price at time of purchase |
| `payments` | `id` | `orders.id` | Stores gateway transaction ID; no card data stored locally |
| `reviews` | `id` | `buyers.user_id`, `products.id`, `orders.id` | Rating 1–5; text ≤500 chars; only for Delivered orders |
| `promotions` | `id` | `sellers.user_id` | Type, value, date range; auto-activate/deactivate |
| `promotion_products` | composite | `promotions.id`, `products.id` | Many-to-many join |
| `wishlist_items` | `id` | `buyers.user_id`, `products.id` | Triggers notification on restock |
| `nutritional_goals` | `id` | `buyers.user_id` | Nutrient, target value, unit, period |
| `user_events` | `id` | `buyers.user_id`, `products.id` | Behavioral events feed the AI Agent |
| `recommendations` | `id` | `buyers.user_id`, `products.id` | Output of Item-based CF model |
| `claims` | `id` | `orders.id`, `buyers.user_id` | Refund/return requests; resolved by Seller |
| `audit_logs` | `id` | `users.id` | Records all admin-level actions |
| `customer_queries` | `id` | `buyers.user_id`, `products.id`, `sellers.user_id` | Support questions and seller responses |

**Integrity rules enforced at the database level:**
- Stock (`products.stock`) is constrained to `≥ 0`.
- Price (`products.price`) is constrained to `> 0`.
- Order state transitions are validated in application logic before persisting.
- Stock deduction on payment confirmation runs inside an atomic transaction (`SELECT … FOR UPDATE`) to prevent race conditions.

---

## References

- IEEE Std 830 — IEEE Recommended Practice for Software Requirements Specifications.
- BPMN Guide — Visual Paradigm: https://www.visual-paradigm.com/guide/bpmn/what-is-bpmn/
- Architecture Diagrams — Lucidchart: https://www.lucidchart.com/blog/how-to-draw-architectural-diagrams
- UML Class Diagrams — UML Diagrams: https://www.uml-diagrams.org/class-diagrams-overview.html
- Django REST Framework: https://www.django-rest-framework.org/
- SimpleJWT: https://django-rest-framework-simplejwt.readthedocs.io/
- Scikit-learn: https://scikit-learn.org/
- Celery: https://docs.celeryq.dev/
- Docker: https://docs.docker.com/
- PostgreSQL: https://www.postgresql.org/docs/

---
