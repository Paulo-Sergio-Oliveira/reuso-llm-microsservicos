# Registro de Execução — Coleta de Dados (Etapa 2)

- **Sistema:** Bookstore
- **Execução:** 3
- **Fornecedor / Interface:** Gemini
- **Modelo exibido na interface:** Gemini 4 Neon
- **Data de execução:** 2026-11-24
- **Prompt utilizado:** 02_prompts/few-shot-bookstore.md
- **Responsável pela coleta:** Enzo Hariel Ferreira Gaspar

---

## Resposta Completa do Modelo

### Section 1: Microservices Architecture (CSV)

```csv
Microservice,Responsibilities,Communicates With
Auth Service,Authenticate customer accounts; Issue and validate access tokens,Customer Service
Customer Service,Register customer accounts; Update customer profiles; Retrieve customer profile information,None
Product Service,Add product entries; Update product entries; Delete product entries; List available products for purchase,None
Cart Service,Add products to shopping cart; Remove products from shopping cart; View current shopping cart items,Product Service; Customer Service
Order Service,Create orders from shopping cart items; View order history and order details,Cart Service; Product Service; Customer Service
Payment Service,Process payments for completed orders,Order Service; Customer Service
Delivery Service,Schedule delivery of orders; Track delivery status of orders,Order Service; Customer Service
```

### Section 2: Reuse and Architectural Justifications

- **Customer Service (Shared Service):** Centralizes customer account registration and profile management so that `Auth Service`, `Cart Service`, `Order Service`, `Payment Service`, and `Delivery Service` can retrieve consistent customer data without duplicating customer storage or lookup logic. This is directly supported by Requirement 6 and indirectly enables customer-scoped operations across Requirements 2, 3, 4, and 5.
- **Auth Service (Centralized Authentication):** Isolates customer authentication and token issuance/validation from profile management so other microservices do not reimplement credential verification or session security logic. This separation is justified by Requirement 6 and supports secure access across Requirements 1–6.
- **Product Service (Shared Catalog Service):** Acts as the single source of truth for product entries and catalog listings, reused by `Cart Service` to validate items added to carts and by `Order Service` to verify product details and pricing during order creation. This avoids duplicating product catalog logic and is justified by Requirements 1, 2, and 3.
- **Cart Service (Reusable Shopping Cart Capability):** Encapsulates cart state and item manipulation independently from order processing, allowing `Order Service` to consume current cart items when creating orders rather than duplicating cart logic. This is supported by Requirements 2 and 3.
- **Order Service (Shared Order Domain Service):** Centralizes order creation, order history, and order detail retrieval so downstream fulfillment services (`Payment Service` and `Delivery Service`) reuse a single authoritative order representation instead of duplicating order tracking and validation logic. This is justified by Requirements 3, 4, and 5.
- **Payment Service and Delivery Service (Decoupled Fulfillment Services):** Separate payment processing and delivery scheduling/tracking into dedicated, independently scalable services that reuse `Order Service` and `Customer Service`. This keeps core order management focused while isolating external payment and logistics concerns, supported by Requirements 4 and 5.
