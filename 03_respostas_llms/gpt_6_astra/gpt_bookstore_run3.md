# Registro de Execução — Coleta de Dados (Etapa 2)

- **Sistema:** Bookstore
- **Execução:** 3
- **Fornecedor / Interface:** OpenAI — Codex CLI 0.172.0 (`codex exec`, conta ChatGPT), disparado via Orca CLI; sessão nova e efêmera por execução, sem ferramentas/MCP, em pasta vazia
- **Modelo exibido na interface:** gpt-6-astra (GPT-6-Astra), reasoning effort: medium
- **Data de execução:** 2026-11-24
- **Prompt utilizado:** 02_prompts/few-shot-bookstore.md
- **Responsável pela coleta:** Natan de Sousa Lucena

---

## Resposta Completa do Modelo

### Section 1: Microservices Architecture (CSV)

```csv
Microservice,Responsibilities,Communicates With
Product Service,Add product entries; Update product entries; Delete product entries; List available products for purchase,None
Cart Service,Add products to shopping carts; Remove products from shopping carts; View cart items; Clear purchased items after order creation,Product Service
Order Service,Create orders from cart items; Preserve purchased item and price snapshots; View order history and order details; Track order payment and delivery status,Cart Service; Product Service; Customer Service; Payment Service; Delivery Service
Payment Service,Process payments for submitted orders; Track payment outcomes; Prevent duplicate payment processing,None
Delivery Service,Schedule delivery of paid orders; Track delivery progress,None
Customer Service,Register customer accounts; Authenticate customers; Issue access tokens; Update customer profiles; Retrieve customer profiles,None
```

### Section 2: Reuse and Architectural Justifications

* **Product Service — shared catalog (Requirement 1):** Owns product information for both Cart and Order services. Cart uses it to validate selections; Order uses it to confirm current product details and prices at checkout. This avoids separate catalog implementations.

* **Cart and Order services — separate ownership (Requirements 2–3):** Cart owns mutable shopping selections; Order owns persistent purchase records. Order reuses Cart data at checkout and stores item and price snapshots so subsequent product changes or deletion do not alter order history. Cart clears purchased items only after successful order creation.

* **Customer Service — centralized accounts (Requirement 6):** Combines authentication and profile management within one customer boundary. A separate authentication microservice would add complexity without a stated requirement for independent identity management. Other services identify customers through signed tokens and enforce access to customer-owned resources using a reusable authorization component.

* **Order Service — checkout coordination (Requirements 3–5):** Coordinates cart retrieval, product validation, customer information, payment, and delivery. Payment and Delivery receive the required order snapshots rather than independently retrieving the same data. Their outcomes return through responses or asynchronous events; the CSV lists initiated service dependencies.

* **Payment Service — reusable payment capability (Requirement 4):** Centralizes payment processing and retry protection instead of embedding them in checkout. Interpreting “completed orders” as orders submitted at checkout, successful payment makes an order eligible for delivery. An order identifier serves as an idempotency key to prevent duplicate charges.

* **Delivery Service — centralized fulfillment (Requirement 5):** Owns scheduling and tracking. It receives an order and delivery-address snapshot, while Order retains references and status summaries for order details. This avoids duplicating delivery workflow logic or allowing later profile updates to change an existing shipment.

* **Shared technical components and service-owned data (Requirements 1–6):** Reuse libraries for token verification, observability, and consistent error handling. Each service owns its data and exposes it through contracts; shared libraries do not contain duplicated business workflows or require shared database tables.
