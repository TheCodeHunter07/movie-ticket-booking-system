# Movie Ticket Booking System — Architecture

## 1. Project Overview

The Movie Ticket Booking System is a full-stack web application inspired by BookMyShow. It allows customers to browse movies, explore theatres and showtimes, select seats, and book movie tickets online.

The application also provides management functionality for Theatre Admins and Super Admins.

## 2. Technology Stack

* **Backend:** Python, Django, Django REST Framework
* **Database:** PostgreSQL
* **Frontend:** HTML, CSS, JavaScript, Bootstrap 5
* **Authentication:** Django authentication and role-based access control
* **Configuration:** Environment variables managed using `python-dotenv`
* **Payment:** Simulated payment flow; no real payment gateway or monetary transactions

## 3. User Roles

### Customer

* Register, log in, and manage their profile.
* Browse and search movies.
* Select a city, theatre, showtime, and seats.
* Complete a simulated payment and receive a booking confirmation.
* View booking history and use supported cancellation functionality.
* Add movie reviews, ratings, and favorites.

### Theatre Admin

* Manage permitted theatre-related information.
* Manage screens, seats, and show schedules according to assigned permissions.
* Access relevant theatre information and reports.

### Super Admin

* Manage application-wide information and users.
* Oversee movies, cities, theatres, and other administrative operations.
* Access relevant reports and management features.

## 4. Proposed Django Application Structure

### accounts

Responsible for user registration, authentication, profiles, and role-based permissions.

### locations

Responsible for city information and city management.

### movies

Responsible for movie details, genres, movie search, and filtering.

### theatres

Responsible for theatres, screens, and physical seat definitions.

### shows

Responsible for show scheduling and show-specific seat availability.

### bookings

Responsible for temporary seat locks, booking creation, booking-seat relationships, simulated payment processing, ticket generation, and cancellation workflows.

### reviews

Responsible for movie reviews and ratings.

### notifications

Responsible for booking confirmation emails and other required notifications.

## 5. Important Data Relationships

* A city can contain multiple theatres.
* A theatre can contain multiple screens.
* A screen can contain multiple physical seats.
* A movie can have multiple shows.
* A screen can host multiple shows at different times.
* A show has bookings and show-specific seat availability.
* A booking can contain multiple seats.
* A customer can create multiple bookings.
* A customer can submit reviews and maintain favorites.

The final Django models and their relationships will be implemented according to the assignment's database requirements.

## 6. Seat Booking and Concurrency

A physical seat belongs to a screen, but its availability depends on a particular show.

For example, seat A1 may be booked for a 6:00 PM show while remaining available for a 9:00 PM show.

The booking workflow must:

1. Check seat availability for the selected show.
2. Temporarily lock selected seats.
3. Prevent conflicting bookings during the lock period.
4. Validate the lock before confirming a booking.
5. Use database transactions and appropriate integrity constraints to prevent double booking.
6. Release expired or abandoned seat locks according to the implemented locking rules.

The final implementation must handle concurrent booking attempts safely.

## 7. Booking Workflow

1. The customer selects a movie.
2. The customer chooses a theatre, date, and showtime.
3. The application displays seats available for that show.
4. The customer selects seats.
5. The backend validates availability and creates temporary seat locks.
6. The application displays the booking summary.
7. The customer completes the simulated payment flow.
8. The backend confirms the booking safely.
9. The system generates a ticket with a QR code.
10. The system sends a confirmation email and records the booking in the customer's history.

## 8. Security and Access Control

* Protect authenticated endpoints.
* Enforce permissions based on user roles.
* Validate incoming request data.
* Keep database credentials and secrets outside version control.
* Use Django's security mechanisms and database transactions appropriately.
* Prevent unauthorized access to bookings and administrative operations.

## 9. Development Principles

* Keep each Django app focused on its responsibility.
* Use Git feature branches and pull requests targeting the `development` branch.
* Test changes before committing.
* Maintain `requirements.txt` as project dependencies change.
* Update this document when significant architectural decisions change.
* Follow the functional requirements and acceptance criteria in the project assignment.
