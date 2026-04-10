# Smart Elevator Control System – ER Diagram

excalidraw - https://inapp.app/hiteshdhayal/excalidraw
eraser - https://inapp.app/hiteshdhayal/eraser

This project presents the **database design for a Smart Elevator Control Platform** used in large buildings such as corporate towers, malls, hospitals, and airports.

Modern high-rise buildings operate **multiple elevators simultaneously**, serving hundreds or thousands of passengers daily. The system must efficiently manage:

* multiple buildings
* elevator infrastructure
* floor requests
* elevator dispatching
* ride tracking
* maintenance operations
* telemetry monitoring

This ER diagram models the **core backend database structure** required to support such an intelligent elevator management platform.

---

# System Architecture

The system is organized into five logical layers:

1. **Infrastructure Layer**
2. **Elevator Hardware Layer**
3. **Configuration Layer**
4. **Real-Time Operations**
5. **Analytics & History**

This separation ensures **clear boundaries between static infrastructure data and dynamic operational data**, which is essential for scalable infrastructure systems.

---

# ER Diagram

![ER Diagram](diagram.png)

*(Place your exported ER diagram image in the repository and name it `diagram.png`.)*

---

# Entities Description

## 1. Buildings

Stores information about buildings connected to the platform.

**Key Attributes**

* `building_id` – Primary identifier
* `building_name`
* `city`, `state`, `country`
* `address`
* `number_of_floors`
* `year_built`
* `created_at`, `updated_at`

**Relationships**

* One building contains many floors
* One building contains many elevator shafts
* One building contains many elevators
* One building may have multiple zones

---

## 2. Building Zones

Large buildings often divide elevators into **zones** to improve efficiency.

Example:

* Zone A → Floors 1–20
* Zone B → Floors 21–40

**Attributes**

* `zone_id`
* `building_id`
* `zone_name`
* `min_floor`
* `max_floor`

---

## 3. Floors

Represents individual floors within a building.

**Attributes**

* `floor_id`
* `building_id`
* `zone_id`
* `floor_number`
* `floor_name`
* `is_accessible`
* `is_service_floor`

Each floor belongs to exactly **one building**.

---

## 4. Elevator Shaft

A shaft is the **vertical structure** where an elevator operates.

**Attributes**

* `shaft_id`
* `building_id`
* `zone_id`
* `shaft_number`
* `installation_date`
* `status`

Each shaft contains **exactly one elevator**.

---

## 5. Elevators

Represents the actual elevator machines.

**Attributes**

* `elevator_id`
* `building_id`
* `shaft_id`
* `elevator_code`
* `manufacturer`
* `model`
* `capacity`
* `max_speed`
* `installation_date`
* `last_inspection_date`
* `is_active`

Each elevator belongs to:

* one building
* one shaft

---

## 6. Elevator Floor Service

A **junction table** representing which floors an elevator can serve.

Why this is needed:

* one elevator serves multiple floors
* one floor may be served by multiple elevators

**Attributes**

* `elevator_floor_id`
* `elevator_id`
* `floor_id`
* `is_primary_stop`

---

## 7. Elevator Status

Stores **real-time operational state** of elevators.

This data changes frequently and therefore is separated from the elevator configuration.

**Attributes**

* `status_id`
* `elevator_id`
* `current_floor_id`
* `state` (idle, moving, stopped, maintenance)
* `direction` (up, down)
* `door_state`
* `load_percentage`
* `updated_at`

---

## 8. Floor Requests

When a passenger presses an elevator call button, a **floor request** is generated.

**Attributes**

* `request_id`
* `floor_id`
* `request_direction`
* `priority`
* `request_source`
* `request_time`
* `status`

---

## 9. Ride Assignment

The elevator dispatch system assigns a request to a specific elevator.

**Attributes**

* `assignment_id`
* `request_id`
* `elevator_id`
* `dispatch_algorithm`
* `assigned_at`
* `eta_seconds`
* `status`

---

## 10. Ride Log

Stores completed trips for analytics and monitoring.

**Attributes**

* `ride_id`
* `request_id`
* `elevator_id`
* `start_floor_id`
* `destination_floor_id`
* `passenger_count`
* `ride_distance`
* `start_time`
* `pickup_time`
* `end_time`
* `ride_duration_seconds`

This table enables queries like:

* Which elevator handled the most rides?
* Average trip duration
* Peak traffic times

---

## 11. Maintenance Records

Tracks maintenance activities performed on elevators.

**Attributes**

* `maintenance_record_id`
* `elevator_id`
* `maintenance_type`
* `issue_description`
* `severity`
* `technician_name`
* `status`
* `started_at`
* `resolved_at`
* `cost`

---

## 12. Sensor Telemetry

Modern elevators transmit operational telemetry for monitoring.

**Attributes**

* `telemetry_id`
* `elevator_id`
* `motor_temperature`
* `door_cycles`
* `energy_usage`
* `vibration_level`
* `recorded_at`

This enables predictive maintenance.

---

# Key Relationships

| Relationship           | Description                               |
| ---------------------- | ----------------------------------------- |
| Building → Floors      | One building contains many floors         |
| Building → Elevators   | One building has many elevators           |
| Elevator → Floors      | Many-to-many via `ELEVATOR_FLOOR_SERVICE` |
| Floor → Requests       | Floor generates elevator calls            |
| Request → Assignment   | Request assigned to an elevator           |
| Elevator → Ride Logs   | Elevators complete multiple rides         |
| Elevator → Maintenance | Maintenance history per elevator          |
| Elevator → Telemetry   | Continuous sensor monitoring              |

---

# Example System Questions Supported

This database design supports queries such as:

* How many elevators are installed in a building?
* Which elevators serve a specific floor?
* Which elevator handled the most requests today?
* What is the current status of each elevator?
* Which requests are still pending?
* Which elevators require maintenance?
* What is the average ride duration per building?

---

# Design Principles

This ER design follows several key system design principles:

**Separation of concerns**

* Infrastructure
* Configuration
* Operational data
* Historical analytics

**Scalability**

Supports multiple buildings and thousands of ride requests per day.

**Real-world practicality**

The design reflects how real elevator dispatch platforms operate.

---

# Tools Used

This ER diagram was designed using:

* **Eraser**
* Database modeling principles from large-scale infrastructure systems

---

# Future Improvements

Possible extensions to the system include:

* passenger traffic prediction
* AI-based elevator dispatch
* emergency evacuation logic
* smart destination control panels
* building IoT integration

---

# Author

Project created as part of a **database design and system architecture exercise** for modeling a large-scale smart infrastructure platform.
