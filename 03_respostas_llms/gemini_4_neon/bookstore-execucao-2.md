# Registro de Execução — Coleta de Dados (Etapa 2)

- **Sistema:** Bookstore
- **Execução:** 2
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
Customer Service,Register customer accounts; Update customer profile information; Retrieve customer profile information,None
Product Service,Add product entries; Update product entries; Delete product entries; List available products for purchase,None
Shopping Cart Service,Add products to shopping cart; Remove products from shopping cart; View items currently in the cart,Product Service; Customer Service
Order Service,Create orders from shopping cart items; View order history; Retrieve order details,Shopping Cart Service; Product Service; Customer Service
Payment Service,Process payments for completed orders; Record payment status,Order Service; Customer Service
Delivery Service,Schedule order deliveries; Track delivery status of orders,Order Service; Customer Service
```

### Section 2: Reuse and Architectural Justifications

- **Customer Service (Shared Service):** Centralizes customer registration and profile management as the single source of truth for customer data. It is reused by Auth Service, Shopping Cart Service, Order Service, Payment Service, and Delivery Service (for identity verification, cart ownership, order history, billing details, and delivery addresses), preventing duplication of customer management logic across services. Justified by Requirement 6 and consumed to support Requirements 2, 3, 4, and 5.
- **Auth Service (Centralized Authentication):** Isolates customer authentication and token issuance/validation from customer profile data management so other domain services do not reimplement credential verification or security logic. Justified by Requirement 6.
- **Product Service (Shared Catalog Service):** Centralizes product catalog management and product listing as a foundational service with no outgoing dependencies. It is reused by both Shopping Cart Service (to validate and display products added to a cart) and Order Service (to reference authoritative product information when creating orders), avoiding duplicated product catalog logic. Justified by Requirement 1 and supports Requirements 2 and 3.
- **Shopping Cart Service (Reusable Pre-Checkout Service):** Encapsulates cart item management separately from persistent orders so Order Service can consume cart contents directly when creating orders without mixing transient shopping state into order management. Justified by Requirements 2 and 3.
- **Order Service (Shared Order Context):** Centralizes order creation, order details, and order history so downstream services—Payment Service and Delivery Service—can reuse authoritative order information rather than duplicating order data or checkout logic. Justified by Requirement 3 and supports Requirements 4 and 5.
- **Payment Service and Delivery Service (Decoupled Post-Order Services):** Isolate payment processing and delivery scheduling/tracking into specialized services that consume Order Service and Customer Service. Keeping them separate from Order Service avoids overloading order management with billing and logistics responsibilities while preserving an acyclic dependency structure. Justified by Requirements 4 and 5 (supported by Requirements 3 and 6).
