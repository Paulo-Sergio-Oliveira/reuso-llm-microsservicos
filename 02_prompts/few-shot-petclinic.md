You are a software architect specializing in microservices architectures.

Given the textual requirements of a software system, design a microservices architecture. Pay attention to component and service reuse opportunities, avoiding unnecessary duplication of responsibilities.

Provide your response in two clearly labeled sections:

### Section 1: Microservices Architecture (CSV)
Generate a CSV table with the exact columns below:
- Microservice
- Responsibilities (separate multiple responsibilities with semicolons)
- Communicates With (separate multiple services with semicolons; use "None" if there are no outgoing dependencies)

### Section 2: Reuse and Architectural Justifications
Explain the architectural decisions made regarding reuse (such as shared services, centralized capabilities, or reusable components), indicating which requirements justify each decision.

---

### Example

**System Requirements:**
1. Manage users (register, authenticate, update profile, delete account).
2. Allow users to create, edit, and delete blog posts.
3. Enable users to comment on posts.

**Expected Output:**

#### Section 1: Microservices Architecture (CSV)
```csv
Microservice,Responsibilities,Communicates With (avoid commas inside column values)
Auth Service,Authenticate users; Issue and validate access tokens,User Service
User Service,Register users; Manage user profiles; Delete user accounts,None
Post Service,Create blog posts; Edit blog posts; Delete blog posts,User Service
Comment Service,Create comments on blog posts; Manage comments,Post Service; User Service

```

#### Section 2: Reuse and Architectural Justifications

* **User Service (Shared Service):** Centralizes all user data management and is consumed by Auth Service, Post Service, and Comment Service. This avoids duplicating user lookup and profile management logic across services and is directly supported by Requirement 1.
* **Auth Service (Centralized Authentication):** Isolates authentication and token validation so other services do not reimplement credential verification, supported by Requirement 1.

---

### Target System

Now apply the exact same format to the following system requirements:

Spring PetClinic Microservices - Requirements

1. Manage Clients:
   - Add, update, and delete client information.
   - View detailed information about existing clients.

2. Manage Pets:
   - Add, update, and delete pet information.
   - Link pets to their respective owners.

3. Manage Visits:
   - Register new visits for pets.
   - View visit history for a specific pet.

4. Manage Veterinarians:
   - Add, update, and delete vet information.
   - View specialties for each veterinarian.

5. Service Discovery:
   - Register and locate services using Eureka.

6. Centralized Configuration:
   - Manage service configurations through a Config Server.

7. API Gateway:
   - Route client requests to appropriate backend services.

8. Monitoring and Administration:
   - Monitor services and applications using Admin Server.
