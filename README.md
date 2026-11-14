# Supermarket HR Management System

A Java console application for managing a supermarket chain's human resources and deliveries across multiple branches. The system handles employee records, role-based scheduling, weekly availability submissions, shift validation, and delivery integration, with all data persisted to a local SQLite database.

Built as part of the *Advanced Software Systems Development (ADSS)* course, using a layered architecture (Domain / Service / Repository / DTO / Presentation) and the DAO pattern for data access.

## Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 19 | Core language |
| SQLite (JDBC) | 3.53.1.0 | Local persistence |
| JUnit | 4.13.2 | Unit & integration testing |
| draw.io | — | UML modeling (use cases, class & sequence diagrams) |

## My Contribution

This project was developed as a group assignment. **My individual contribution was the Employees and Deliveries modules**, including:

- **Employee management** — the domain model, services and repositories for hiring, role assignment, deactivation, and employee data.
- **Availability & scheduling** — weekly availability submissions, submission-deadline logic, and assigning employees to shifts based on their availability and roles.
- **Deliveries** — the delivery domain and its integration with shifts, including the storekeeper/driver requirements and driver-license-to-truck matching logic.

Other parts of the broader supermarket system (Suppliers and Inventory) were developed separately and are not part of my contribution.

## Running the System

```
java -jar adss2025_v02.jar
```

> The JAR must be run from the directory where it is located. The database file `employees.db` is created automatically in that directory on the first run; on later runs, data is loaded from the existing database.

## Architecture

```
employees.domain       — Domain objects (Employee, Shift, Branch, Delivery, Role...)
employees.service      — Business logic (EmployeeService, ShiftService, DeliveryService)
employees.repository   — Database access layer (DAO pattern)
employees.dto          — Data Transfer Objects (BranchDTO, EmployeeDTO, ShiftDTO...)
employees.presentation — UI (ManagerUI, EmployeeUI, DataUI, MockDeliveryData)
```

## System Overview

The system supports two types of users — **HR Manager** and **Employee** — and manages:

- Employee management (hiring, roles, availability)
- Shift scheduling and assignment
- Branch management
- Delivery integration (mock)

### Login

**Manager:** select option `1` from the main menu and enter the manager password `ILoveNituz`, then select a branch.

**Employee:** select option `2` from the main menu and enter your ID number.

## Business Rules

- **Shift types:** Morning (06:00–14:00) and Evening (14:00–22:00).
- **Shift Manager:** every shift must have at least one Shift Manager before it can be validated. Shift Manager is a certification — an employee who holds it can also fill another role in the same shift (e.g. Cashier).
- **Two roles per shift:** an employee can hold at most two roles in one shift, and only if one of them is Shift Manager. Two regular roles (e.g. Cashier + Storekeeper) are not allowed.
- **Availabilities:** employees submit the days and shift types they are available for. An employee who has not submitted availabilities is considered available for all shifts.
- **Submission deadline:** weekly availabilities can be submitted only within the submission window (default: Wednesday 23:00). The manager can change the deadline (menu option 20).
- **Override request:** if a manager assigns an employee to a shift they did not declare available for, the employee receives a pending request and must approve it before the assignment is confirmed.
- **Deactivate employee:** removes them from all future shifts but keeps their record in the system.
- **Fixed day off:** each employee has a fixed weekly day off and cannot be assigned to any shift on that day.
- **Delivery requirements:** a shift with a scheduled delivery must have at least one Storekeeper and one Driver assigned before it can be validated.
- **Driver license:** the driver's license type must match the truck type of the delivery (TYPE_B, TYPE_C, TYPE_D).

## Manager Menu

| Option | Description |
|--------|-------------|
| 1 | Add new employee |
| 2 | Update employee details |
| 3 | Deactivate employee (removes from future shifts) |
| 4 | View all employees |
| 5 | View employee details |
| 6 | Assign role to employee |
| 7 | Remove role from employee |
| 8 | Create new shift |
| 9 | View shifts by date |
| 10 | Assign employee to shift |
| 11 | Remove employee from shift |
| 12 | Send override request (assign outside availability) |
| 13 | View available and qualified employees for shift |
| 14 | Validate shift (check Shift Manager assigned) |
| 15 | View all shifts for an employee |
| 16 | View current week shifts |
| 17 | Add new role to company pool |
| 18 | View company role pool |
| 19 | Remove role from company pool |
| 20 | Set availability submission deadline |
| 21 | Switch branch |
| 0 | Back to main menu |

## Employee Menu

| Option | Description |
|--------|-------------|
| 1 | View my assigned shifts |
| 2 | View pending shift requests |
| 3 | Respond to pending shift request (approve/reject) |
| 4 | Add availability (day + shift type) |
| 5 | Remove availability |
| 6 | View my availabilities |
| 7 | Clear all availabilities |
| 8 | View my personal details |
| 0 | Back to main menu |

## Database Schema

| Table | Description |
|-------|-------------|
| `branches` | All company branches |
| `employees` | All employees |
| `roles_pool` | Company role pool |
| `employee_roles` | Roles assigned to each employee |
| `availabilities` | Employee availability submissions |
| `shifts` | All shifts |
| `shift_required_roles` | Required roles per shift |
| `shift_assignments` | Employee assignments to shifts |
| `pending_shifts` | Pending override requests |
| `deliveries` | Mock delivery data |

## Testing

| Test Class | Tests | Type |
|------------|-------|------|
| EmployeeTest | 8 | Unit |
| ShiftTest | 6 | Unit |
| ServiceTest | 7 | Unit |
| IntegrationTest | 12 | Integration |
| **Total** | **33** | |

All tests run against an in-memory SQLite database and are fully isolated from one another.

## Pre-loaded Demo Data

On first run the system seeds demo data for testing: sample employees across two branches (Tel Aviv and Jerusalem), a full week of Morning/Evening shifts per branch, a default role pool (Cashier, Storekeeper, Shift Manager, General Worker, Driver), and four mock deliveries covering different truck types for delivery-integration testing.
