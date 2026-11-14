# Supermarket HR & Scheduling Management System

A comprehensive Java console application for managing a supermarket chain's human resources, dynamic shift scheduling, and delivery logistics across multiple branches. The system handles complex constraints such as role-based scheduling, weekly availability submissions, shift validation, and delivery integration, with all data persisted to a local SQLite database.

**Developed by Shira Eshcoli** — Information Systems Engineering Student at Ben-Gurion University of the Negev.
*(Built as a system design and implementation project for the Advanced Software Systems Development course).*

## My Core Contributions & System Engineering

While developed as part of a broader supermarket management system, my primary focus was engineering the **HR, Scheduling, and Deliveries modules** from the ground up:

*   **System Design & Modeling:** Architected the system using strict object-oriented design methodologies. Created comprehensive UML class diagrams, sequence diagrams, and operation contracts prior to implementation.
*   **Core Business Logic:** Engineered the `EmployeeService` and `ShiftService` to handle complex business rules, including role constraint validations, dynamic availability windows, shift overlap prevention, and delivery-to-driver matching logic.
*   **Software Architecture:** Implemented a robust multi-layered architecture (Presentation, Service, Domain, Repository) ensuring a clean separation of concerns.
*   **Data Persistence (DAO & DTO):** Designed the relational database schema and implemented the Data Access Object (DAO) pattern using SQLite and JDBC. Utilized Data Transfer Objects (DTOs) for secure and efficient data passage between layers.
*   **Build & Deployment:** Managed project dependencies and packaging via Maven, delivering a standalone, runnable JAR release.

## Tech Stack

| Technology | Purpose |
|------------|---------|
| **Java 19** | Core backend language |
| **SQLite (JDBC)** | Local relational database |
| **Maven** | Dependency management & packaging |
| **JUnit 4.13** | Unit & integration testing |
| **draw.io** | UML modeling (Class, Sequence, and Use-Case diagrams) |

## System Architecture

```text
employees.domain       — Domain objects (Employee, Shift, Branch, Delivery, Role...)
employees.service      — Business logic & validation (EmployeeService, ShiftService)
employees.repository   — Database access layer implementing the DAO pattern
employees.dto          — Data Transfer Objects for cross-layer communication
employees.presentation — CLI interface (ManagerUI, EmployeeUI, DataUI)
Core Business Rules & Constraints HandledShift Management: Shifts are divided into Morning (06:00–14:00) and Evening (14:00–22:00). Every shift must have at least one certified Shift Manager before it can be validated.Role Limitations: An employee can hold at most two roles in one shift, strictly conditional on one of them being a Shift Manager (e.g., Cashier + Shift Manager). Two regular roles are prohibited.Dynamic Availabilities: Employees submit their weekly availabilities within a strictly enforced, manager-defined submission window (defaulting to Wednesday 23:00).Override Protocols: Assigning an employee outside their declared availability triggers a pending "override request" that requires the employee's explicit system approval.Logistics Integration: A shift containing a scheduled delivery automatically updates its required roles to include a Storekeeper and a Driver. The system validates that the assigned Driver's license type matches the specific delivery truck type (TYPE_B, TYPE_C, TYPE_D).Database SchemaTableDescriptionbranchesSupermarket branches (e.g., Tel Aviv, Jerusalem)employeesCore employee records and statusesroles_poolSystem-wide authorized rolesemployee_rolesMany-to-many mapping of employees to their certified rolesavailabilitiesWeekly shift availability submissionsshifts & shift_required_rolesShift records and their dynamically calculated requirementsshift_assignmentsVerified employee-to-shift assignmentspending_shiftsOverride requests pending employee approvaldeliveriesMock delivery integration dataTesting & Demo DataThe system includes 33 isolated tests (Unit and Integration) running against an in-memory SQLite database.Upon the first run, the system automatically seeds comprehensive demo data, including branches, a default role pool, employees with varied certifications, a full week of shifts, and mock deliveries to demonstrate full system capabilities.Running the ApplicationEnsure you have Java 19+ installed. Run the packaged JAR from your terminal:Bashjava -jar adss2025_v02.jar
Note: The database file employees.db is generated automatically in the execution directory on the first run.
