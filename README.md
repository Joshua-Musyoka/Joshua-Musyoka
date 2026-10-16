# Clinic Booking System

A REST API for managing appointments at a small clinic.

The system allows patients to view available 30-minute appointment
slots, book appointments, cancel bookings, and reschedule appointments.

## 1. System Design

### Architecture

The application uses a modular monolith built with FastAPI and
PostgreSQL.

The API layer handles HTTP requests, validation, and responses.
The service layer implements booking rules.
The data-access layer manages database operations.

The application is containerized with Docker and deployed to Render.
GitHub Actions runs automated checks and manages deployment.

### Technology Stack

- Python
- FastAPI
- PostgreSQL
- SQLAlchemy
- Alembic
- Pytest
- Docker
- GitHub Actions
- Render

### Data Models

Doctor:

- id
- full_name
- specialty
- is_active
- created_at

Patient:

- id
- full_name
- email
- phone
- created_at

DoctorWorkingHours:

- id
- doctor_id
- weekday
- start_time
- end_time
- is_active

Appointment:

- id
- patient_id
- doctor_id
- start_time
- end_time
- status
- cancellation_reason
- created_at
- updated_at

### Booking Rules

1. Appointments last exactly 30 minutes.
2. Appointments must fall within the doctor's working hours.
3. Appointments must not be in the past.
4. Bookings within one hour of the current time are rejected.
5. A doctor cannot have overlapping active appointments.
6. Cancelled appointments release their slots.
7. Cancellation requires a reason.
8. Already-cancelled appointments cannot be cancelled again.
9. Cancelled appointments cannot be rescheduled.
10. Rescheduling validates the new slot using the same rules
    as a new booking.
11. Upcoming patient appointments are returned in chronological order.

### Key Design Decisions

- PostgreSQL provides transactions and database-level protection
  against conflicting appointments.
- FastAPI provides request validation and generated API documentation.
- A modular monolith keeps the initial system simple to develop
  and deploy.
- Working hours are stored separately from doctors to support
  different schedules.
- Timestamps are stored in UTC, with clinic-local dates and times
  interpreted using an explicit timezone.
- Appointment cancellation changes the appointment status rather
  than deleting the record, preserving an audit trail.
- Database migrations are version-controlled.

### Trade-offs

A modular monolith is simpler to deploy than microservices and is
appropriate for the initial scale.

PostgreSQL-specific constraints reduce portability but provide
stronger protection against concurrent booking requests.

The initial working-hours model supports recurring weekly schedules.
Exceptions such as holidays, doctor leave, and temporary closures
can be introduced as the system grows.

### Assumptions

- The clinic operates in the Africa/Nairobi timezone.
- Appointment durations are fixed at 30 minutes.
- Each doctor has a configurable weekly working schedule.
- Patients are identified by a patient ID supplied to the API.
- The initial version supports one clinic location.
- Cancelled appointments remain in the database for traceability.

## 2. Local Development

Instructions will be added after the application implementation
and database configuration are complete.

## 3. API Documentation

FastAPI provides interactive documentation at `/docs`.

## 4. Deployment and CI/CD

Deployment instructions, the public application URL, and the
GitHub Actions workflow will be documented after deployment.

## 5. AI Reflection

The assessment's AI reflection will be completed after development,
based on the actual work performed and decisions made.
