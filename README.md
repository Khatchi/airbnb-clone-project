# PROJECT OVERVIEW

The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb. It involves a deep dive into full-stack development, focusing on backend systems, database design, API development, and application security. This project enables learners to understand complex architectures, workflows, and collaborative team dynamics while building a scalable web application.

# Project Goals

- **User Management:** Implement a secure system for user registration, authentication, and profile management.

- **Property Management:** Develop features for property listing creation, updates, and retrieval.

- **Booking System:** Create a booking mechanism for users to reserve properties and manage booking details.

- **Payment Processing:** Integrate a payment system to handle transactions and record payment details.

- **Review System:** Allow users to leave reviews and ratings for properties.

- **Data Optimization:** Ensure efficient data retrieval and storage through database optimizations.

# Technology Stack

- **Django:** A high-level Python web framework used for building the RESTful API.

- **Django REST Framework:** Provides tools for creating and managing RESTful APIs.

- **PostgreSQL:** A powerful relational database used for data storage.

- **GraphQL:** Allows for flexible and efficient querying of data.

- **Celery:** For handling asynchronous tasks such as sending notifications or processing payments.

- **Redis:** Used for caching and session management.

- **Docker:** Containerization tool for consistent development and deployment environments.

- **CI/CD Pipelines:** Automated pipelines for testing and deploying code changes.

# Team Roles

## Business Analyst (BA)
- **Key Responsibilities:**
  - Understands customer's business processes
  - Translates business needs into technical requirements
  - Bridges gap between stakeholders and development team
- **Value Added:**
  - Ensures alignment between business goals and technical implementation
  - Shapes software to maximize business value

## Product Owner (PO)
- **Key Responsibilities:**
  - Owns product vision and evolution
  - Maintains product backlog
  - Ensures final product meets requirements
- **Difference from BA:**
  - More strategic (customer-focused)
  - BA is more tactical (technical translation)
- **Common in:** Agile environments with changing requirements

## Project Manager (PM)
- **Key Responsibilities:**
  - Ensures on-time, on-budget delivery
  - Manages team workflow and motivation
- **Methodology Specifics:**
  - Waterfall: Task distribution and scheduling
  - Agile: Process improvement and cross-team coordination

## UI/UX Designer
- **Key Responsibilities:**
  - Creates user journeys and interfaces
  - Conducts user research and testing
- **Two Aspects:**
  - UI: Visual design and interactivity
  - UX: Overall user experience and flow

## Software Architect
- **Key Responsibilities:**
  - Designs high-level system architecture
  - Sets coding standards and integration protocols
  - Performs code reviews
- **When Critical:** Complex systems or legacy modernization

## Software Developer
- **Front-end:**
  - Builds user interfaces
  - Ensures cross-platform compatibility
- **Back-end:**
  - Implements business logic
  - Handles databases and integrations
- **Full-stack:** Combines both front-end and back-end

## Quality Assurance (QA) Engineer
- **Key Responsibilities:**
  - Verifies functional requirements
  - Tests non-functional aspects (performance, security)
  - Documents defects and test results
- **Output:** Production-ready, stable software

## Test Automation Engineer
- **Key Responsibilities:**
  - Develops automated test scripts
  - Maintains test automation frameworks
- **Advantage:** Enables continuous testing in CI/CD pipelines

## DevOps Engineer
- **Key Responsibilities:**
  - Implements CI/CD pipelines
  - Bridges development and operations
  - Automates deployment processes
- **Result:** Faster, more reliable software releases


# Technology Stack

| **Category**       | **Technology**         | **Purpose**                                                                 |
|--------------------|------------------------|-----------------------------------------------------------------------------|
| **Backend**        | Django                 | High-level Python web framework for rapid development                      |
| **API**            | Django REST Framework  | Builds RESTful APIs with authentication, serialization, and documentation  |
| **Database**       | PostgreSQL             | Relational database for structured data storage                            |
| **Query Language** | GraphQL (w/ Graphene)  | Flexible data querying for frontend clients                                |
| **Async Tasks**    | Celery + Redis         | Handles background jobs (emails, payments, etc.)                           |
| **Caching**        | Redis                  | Session management and performance optimization                            |
| **Containerization**| Docker                | Consistent environments from development to production                     |
| **CI/CD**          | GitHub Actions         | Automated testing and deployment pipelines                                 |



## Database Design

### Key Entities and Relationships

#### 1. **Users**
- **Fields**:
  - `id` (Primary Key)
  - `username` (Unique)
  - `email` (Unique)
  - `password_hash` (Encrypted)
  - `role` (Host/Guest)
- **Relationships**:
  - One-to-Many with `Properties` (A user can list multiple properties)
  - One-to-Many with `Bookings` (A user can make multiple bookings)
  - One-to-Many with `Reviews` (A user can write multiple reviews)

#### 2. **Properties**
- **Fields**:
  - `id` (Primary Key)
  - `title` (Property name)
  - `price_per_night` (Decimal)
  - `location` (Address/Coordinates)
  - `host_id` (Foreign Key → Users)
- **Relationships**:
  - Many-to-One with `Users` (Each property belongs to one host)
  - One-to-Many with `Bookings` (A property can have multiple bookings)
  - One-to-Many with `Reviews` (A property can receive multiple reviews)

#### 3. **Bookings**
- **Fields**:
  - `id` (Primary Key)
  - `start_date` (DateTime)
  - `end_date` (DateTime)
  - `total_price` (Calculated)
  - `guest_id` (Foreign Key → Users)
  - `property_id` (Foreign Key → Properties)
- **Relationships**:
  - Many-to-One with `Users` (A booking belongs to one guest)
  - Many-to-One with `Properties` (A booking is for one property)
  - One-to-One with `Payments` (Each booking has one payment)

#### 4. **Reviews**
- **Fields**:
  - `id` (Primary Key)
  - `rating` (Integer, 1-5)
  - `comment` (Text)
  - `guest_id` (Foreign Key → Users)
  - `property_id` (Foreign Key → Properties)
- **Relationships**:
  - Many-to-One with `Users` (A review is written by one user)
  - Many-to-One with `Properties` (A review is for one property)

#### 5. **Payments**
- **Fields**:
  - `id` (Primary Key)
  - `amount` (Decimal)
  - `status` (Pending/Completed/Failed)
  - `booking_id` (Foreign Key → Bookings)
  - `payment_method` (Stripe/PayPal/etc.)
- **Relationships**:
  - One-to-One with `Bookings` (Each payment is linked to one booking)

### Entity-Relationship Diagram (Conceptual)

Users ──(1:N)─── Properties
│ │
│(1:N) │(1:N)
↓ ↓
Bookings ───(1:1)─── Payments
│
│(1:N)
↓
Reviews