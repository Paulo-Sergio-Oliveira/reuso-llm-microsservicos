# Registro de Execução — Coleta de Dados (Etapa 2)

- **Sistema:** Bookstore
- **Execução:** 1
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
Auth Service,Authenticate customers; Issue and validate access tokens,Customer Service
Customer Service,Register customer accounts; Update customer profiles; Retrieve customer profile information,None
Product Service,Add product entries; Update product entries; Delete product entries; List available products for purchase,None
Cart Service,Add products to shopping cart; Remove products from shopping cart; View items in shopping cart,Product Service; Customer Service
Order Service,Create orders from shopping cart items; View order history; Retrieve order details,Cart Service; Product Service; Customer Service
Payment Service,Process payments for completed orders,Order Service; Customer Service
Delivery Service,Schedule order deliveries; Track delivery status of orders,Order Service; Customer Service
```

### Section 2: Reuse and Architectural Justifications

- **Customer Service (Shared Service):** Centralizes customer account registration and profile management as a single source of truth. It is reused by Auth Service (for credential verification lookups), Cart Service (to associate shopping carts with customers), Order Service (to link orders and order history to customers), Payment Service (to access customer billing details), and Delivery Service (to retrieve customer shipping details). This avoids duplicating customer data storage and profile logic across services and is justified by Requirement 6 while supporting Requirements 2, 3, 4, and 5.
- **Auth Service (Centralized Authentication Capability):** Separates authentication and access token issuance/validation from customer profile management. Centralizing this cross-cutting capability prevents Customer Service, Product Service, Cart Service, Order Service, Payment Service, and Delivery Service from reimplementing credential verification or token validation logic. This decision is justified by Requirement 6 and secures operations across Requirements 1 through 5.
- **Product Service (Shared Catalog Service):** Centralizes product catalog management (creating, updating, deleting, and listing available products). Both Cart Service (when adding or displaying items in a cart) and Order Service (when validating product details and pricing during order creation) consume Product Service instead of duplicating product data or catalog lookup logic. This is justified by Requirement 1 and reused to support Requirements 2 and 3.
- **Cart Service (Reusable Pre-Order State Service):** Isolates transient shopping cart operations from permanent order management. Order Service consumes Cart Service to retrieve current cart items when creating an order, preventing duplication of cart item management within Order Service. This is justified by Requirement 2 and reused by Requirement 3.
- **Order Service (Shared Order Management Service):** Serves as the authoritative source for order creation, order details, and order history. Both Payment Service (to process payments against completed orders) and Delivery Service (to schedule and track deliveries for orders) reuse Order Service rather than maintaining duplicate order records or order-retrieval logic. This is justified by Requirement 3 and reused to support Requirements 4 and 5.
- **Payment Service and Delivery Service (Decoupled Downstream Services):** Encapsulating payment processing and delivery scheduling/tracking into separate services keeps Order Service free of payment-processing and shipping-logistics responsibilities while keeping inter-service dependencies unidirectional and acyclic. Both services reuse Order Service and Customer Service for shared domain data, justified by Requirements 4 and 5 (building on Requirements 3 and 6).
