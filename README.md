# GoCab
Production-grade backend system inspired by Uber, built using Spring Boot microservices with real-time driver assignment, Redis geospatial queries, WebSockets, and JWT-based security.

🧠 System Overview
The platform handles:
Authentication & authorization
Ride booking lifecycle
Location-based driver discovery
Real-time driver communication
Reviews for drivers & passengers

Communication model:
REST → synchronous business flows
WebSockets → real-time driver updates
Async messaging → booking state updates
Redis → low-latency geo queries

🧩 Microservices & Responsibilities
🔐 uberAuthService - https://github.com/Mukund964/UberAuthService
JWT-based authentication
Spring Security filters
Role-based authorization (USER / DRIVER)
Tech: Spring Boot, Spring Security, JWT

📦 uberBookingService (Core Service) - https://github.com/Mukund964/UberBookingService
Handles booking creation & lifecycle
Coordinates with:
Location Service (nearby drivers)
Socket Server (driver notifications)
Uses synchronized blocks to:
Prevent race conditions
Ensure only one driver per booking
Processes async booking updates (Kafka design)
Tech: Spring Boot, REST, Concurrency

📍 uberLocationService - https://github.com/Mukund964/UberLocationService
Stores driver locations in Redis
Uses Redis GeoSpatial operations
Fetches nearby drivers in real time
Tech: Spring Boot, Redis

💬 uberSocketServer - https://github.com/Mukund964/UberSocketService
WebSocket-based real-time server
Notifies drivers of new bookings
Receives accept/reject events
Sends async updates to Booking Service
Tech: WebSockets, Async Messaging

⭐ uberReviewService - https://github.com/Mukund964/Spring_ReviewService
CRUD operations for driver & passenger reviews
Reviews linked to completed rides
Tech: Spring Boot, REST, JPA

🗂️ uberEntityService - https://github.com/Mukund964/UberEntityService
Centralized domain models
Database schema & migrations via Flyway
Shared entities across services
Tech: JPA, Flyway

🔍 uberServiceDiscovery - https://github.com/Mukund964/UberServiceDiscovery
Eureka Server
Dynamic service registration & discovery
Tech: Spring Cloud Netflix Eureka


🔄 Booking & Driver Assignment Flow
User requests booking → Booking Service
Booking Service fetches nearby drivers → Location Service (Redis Geo)
Drivers notified in real time → Socket Server (WebSocket)
Driver responds (accept/reject)
Async update sent to Booking Service
synchronized block ensures single driver assignment
Booking confirmed & status updated


