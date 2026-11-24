# Registro de Execução — Coleta de Dados (Etapa 2)

- **Sistema:** Bookstore
- **Execução:** 2
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
Product Service,Add product entries; Update product entries; Delete product entries; List available products; Retrieve product details and prices,None
Cart Service,Add products to carts; Remove products from carts; View cart items; Provide cart contents for checkout; Clear purchased cart items,Product Service
Order Service,Create orders from cart items; Preserve purchased item and price snapshots; View order history; Retrieve order details; Track order payment and delivery status,Cart Service; Product Service; Customer Service; Payment Service; Delivery Service
Payment Service,Process payments for created orders; Record payment outcomes; Prevent duplicate payment processing,None
Delivery Service,Schedule order deliveries; Track delivery progress,None
Customer Service,Register customers; Authenticate customers; Issue access tokens; Update customer profiles; Retrieve customer profiles,None
```

### Section 2: Reuse and Architectural Justifications

- **Product Service — shared catalog:** Owns product data and pricing for Requirement 1. Cart Service reuses it to validate product selections under Requirement 2; Order Service reuses it to validate current products and prices at checkout under Requirement 3. Orders retain purchase snapshots so later product updates or deletions do not alter order history.

- **Cart and Order Services — distinct lifecycles:** Cart Service owns editable purchase intentions under Requirement 2. Order Service owns durable purchase records under Requirement 3 and consumes cart contents instead of duplicating cart management. Only successfully purchased cart items are cleared.

- **Order Service — checkout coordination:** Coordinates order creation, payment requests, and delivery requests across Requirements 3–5. It supplies Payment Service with the order identifier and amount, and Delivery Service with the order items and delivery address. This avoids having both services independently reconstruct orders or retrieve customer profiles.

- **Payment Service — centralized payment processing:** Owns payment processing and payment records under Requirement 4. Idempotent payment requests prevent retries from charging the same order twice. Payment outcomes return to Order Service through responses or events.

- **Delivery Service — centralized fulfillment:** Owns scheduling and tracking under Requirement 5. Delivery progress returns to Order Service through responses or events; Order Service stores only the status needed for order views.

- **Customer Service — combined identity and profiles:** Centralizes registration, authentication, and profile management under Requirement 6. A separate authentication microservice would add an unnecessary boundary at this scope. Services reuse a token-validation component to validate signed tokens locally and enforce customer ownership of carts and orders.

- **Data ownership and dependency conventions:** Each service owns its data; consumers use APIs or events rather than shared database tables. The CSV lists initiated dependencies between the listed services, excluding responses, event infrastructure, and external payment or shipping providers. The payment requirement is interpreted as payment after order creation, with delivery scheduled after successful payment.
