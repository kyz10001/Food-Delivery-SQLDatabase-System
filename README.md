# Food-Delivery-SQLDatabase-System

A 3rd Normal Form (3NF) relational database system built in Oracle SQL Developer to model, manage, and analyze multi-suburb food delivery operations for the platform.

---

## How to use (Files)
ERD_Diagram.drawio - Contains the ERD Diagram in draw.io
Schema_and_Data.sql - Contains the SQL file
Food-Delivery-SQLDatabase (Design and Implementation - Includes SQL CODE) - Contains the full word report that documents the whole design and implementation process (Includes SQL Code and relevant screenshots)

---

## Business Context & Operational Rules
The platform manages transactional workflows across customers, restaurants, menu items, drivers, and delivery orders. To ensure operational consistency, the system enforces several enterprise constraints:
* **Suburb Alignment:** Customers can only order from restaurants delivering within their designated suburb, with deliveries assigned to drivers operating in that same area.
* **Single-Restaurant Orders:** Each order is restricted to items from a single restaurant to streamline driver dispatching.
* **Driver Dispatch Controls:** Drivers handle one delivery at a time; active assignments automatically set driver availability (`IsAvailable`) to `'N'`.
* **Preparation & SLA Tracking:** Dishes are categorized into delivery SLA brackets (`Fast <15m`, `Regular 15-30m`, `Worth the Wait >30m`) to monitor delivery performance.

---

## Database Architecture

### Data Model & Relational Schema (3NF)
The schema consists of 7 normalized tables designed to eliminate data redundancy and prevent operational anomalies:

* `CUSTOMER` `(CustomerID, FullName, Phone, Email, DeliveryAddress, Suburb)`
* `RESTAURANT` `(RestaurantID, Ethnicity, RestaurantStyle, RestaurantName, RestaurantDescription, Suburb)`
* `RESTAURANT_CERTIFICATION` `(RestaurantID, Certification)`
* `DISH` `(DishID, RestaurantID, DishName, DishDescription, PrepMethod, MainIngredient, CourseType, DishPrice, Kilojoules, IsVegetarian, IsGlutenFree, IsDairyFree, DeliveryTimeCategory)`
* `DRIVER` `(DriverID, DriverName, IsAvailable, Suburb)`
* `ORDER` `(OrderID, CustomerID, RestaurantID, DriverID, DeliveryAddress, DeliveryDateTimeRequested, DeliveryDateTimeActual, OrderStatus, TotalCost, Suburb)`
* `ORDER_DISH` `(OrderID, DishID, Quantity)`

---

## Analytical SQL Views & Business Intelligence

The system includes pre-configured Oracle SQL views to power operational dashboards and business reporting:

| View Name | Core Function / Business Intelligence Use Case |
| --- | --- |
| `ViewA` | **Order Dispatch Details:** Retrieves full order details for driver pick-up and customer delivery confirmation. 
| `ViewB` | **Dietary & SLA Filtering:** Filters vegetarian dishes deliverable within 30 minutes in specific suburbs. 
| `ViewC` | **Restaurant Daily Orders:** Tracks daily order volumes and delivery timestamps for specific merchant locations. 
| `ViewD` | **Dietary Catalog Search:** Generates menu item details and prices for vegetarian-friendly establishments. 
| `ViewE` | **Driver Activity Log:** Performs an outer join (`LEFT JOIN`) to log driver assignments and delivery timestamps for a given date. 
| `ViewF` | **Active Driver Pool:** Identifies real-time driver availability by suburb for new order dispatching. 
| `ViewG` | **Merchant Performance:** Aggregates total historical order counts across all onboarded restaurants. 
| `ViewH` | **Digital Menu Booklet:** Generates restaurant menu listings including descriptions, course categories, and pricing. 
| `ViewI` | **Regional Order Volume:** Ranks order counts by suburb in descending order to identify top-performing markets. 
| `ViewJ` | **Delivery SLA & Delay Analysis:** Calculates late order rates and average delay duration (in minutes) against preparation SLAs.

---

## Deployment & Setup

### Requirements
* **Database Platform:** Oracle Database 19c or higher / Oracle SQL Developer
* **SQL Dialect:** Oracle SQL (PL/SQL compatible)
