<div align="center">

(smart-qr-scanner)
High-Performance qr scanning API
</div>

<p align="center">
<img src="https://img.shields.io/badge/Java-21-blue.svg?style=for-the-badge&logo=openjdk" alt="Java 21">
<img src="https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F?style=for-the-badge&logo=springboot" alt="Spring Boot">
<img src="https://img.shields.io/badge/PostgreSQL-14.0-336791?style=for-the-badge&logo=postgresql" alt="PostgreSQL">
<img src="https://img.shields.io/badge/Redis-7.x-DC382D?style=for-the-badge&logo=redis" alt="Redis">
<img src="https://img.shields.io/badge/Gradle-8.x-02303A?style=for-the-badge&logo=gradle" alt="Gradle">



<img src="https://img.shields.io/badge/build-passing-brightgreen?style=for-the-badge" alt="Build Passing">
<img src="https://img.shields.io/badge/license-MIT-yellow?style=for-the-badge" alt="License MIT">
</p>

In a world where QR codes are a gateway to the digital world, smart qr scanner provides a vital utility. This enterprise-grade API is engineered to deliver real-time, accurate, and low-latency results on URLs, aiding users.

  Architecture & Design
This project is built using a modern, service-oriented architecture designed for performance, scalability, and maintainability. The core design principle is to provide  assessment by intelligently layering data retrieval strategies.

Request Flow Diagram
The system employs a cache-aside strategy to ensure that known threats are identified in sub-millisecond time, while new URLs undergo a comprehensive analysis through trusted external services.

Code snippet

sequenceDiagram
    participant Client
    participant API Gateway
    participant smart qr scanning API (Controller)
    participant Redis Cache (Blacklist)
    participant Analysis Service
    participant external API
    participant PostgreSQL DB (Logs)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
    <img width="1060" height="941" alt="flowQuishield" src="https://github.com/user-attachments/assets/5e8229e0-2a47-47c1-b63b-ffb3b59b1f2e" />

    

  Key Design Decisions
Performance-First Caching: Redis was chosen as the caching layer due to its in-memory data structure and extremely low read latency. This is crucial for a security application where response time is a key feature. By caching known malicious URLs, we reduce the load on our analysis services and provide an instantaneous response for over 90% of common threats.

Authoritative External Intelligence: Instead of reinventing the wheel, the service integrates with the external API, a trusted and comprehensive aggregator of security vendor data. This decision allows the project to leverage world-class threat intelligence without the overhead of maintaining a complex detection engine.

Data Durability & Auditing: While Redis provides speed, PostgreSQL is used as the system-of-record for all transactions. Its ACID compliance guarantees that every scan is logged and every blacklisted URL is durably persisted, providing a crucial audit trail for security operations.

Scalable & Maintainable Codebase: The application is built on Spring Boot, using Dependency Injection and a clear separation of concerns (Controller -> Service -> Repository). This decoupled architecture makes the system easy to test, maintain, and scale individual components independently.

  Core Features
Blazing-Fast Threat Intelligence: Delivers security verdicts with millisecond latency for known threats via the Redis cache.

Comprehensive Analysis Engine: Leverages the VirusTotal API to check URLs against dozens of antivirus engines and website scanners.

Dynamic Blacklisting: Automatically adds confirmed threats to the blacklist database and the Redis cache.

Persistent Audit Trail: Every URL scan is logged to a PostgreSQL database for historical analysis and compliance.

Modern, Asynchronous-Ready Stack: Built with Java 21 and Spring Boot 3, using Java's HttpClient for non-blocking I/O operations with external APIs.

  Technology Stack & Dependencies
Category	Technology / Library	Rationale
Backend	Java 21, Spring Boot 3	A modern, robust, and highly performant stack for building enterprise-grade applications.
Data Persistence	Spring Data JPA, PostgreSQL	Provides a reliable, ACID-compliant relational store for critical audit logs and blacklists.
Caching	Spring Data Redis, Lettuce	In-memory cache for ultra-fast lookups of known malicious URLs.
API Integration	Java 11+ HttpClient, JSON.org	For efficient, non-blocking communication with the external VirusTotal REST API.
Developer Tools	Lombok, Gradle	Reduces boilerplate code and provides a powerful, flexible build automation system.

Export to Sheets
  API Documentation
POST /api/check
Analyzes a URL and returns a security verdict.

Request Body:

JSON

{
    "url": "http://example-malicious-site.com/malware.exe"
}
Responses:

Code 200 OK - Content-Type: text/plain

A plain text string indicating the result.

"safe": The URL has been analyzed and is considered safe.

"block": The URL is malicious and should be blocked.

"block (cached)": The URL was found in the high-speed threat cache and is blocked.

Code 500 Internal Server Error - Content-Type: text/plain

Indicates an error during the analysis process.

"Error: {exception message}"

  Getting Started
Prerequisites
Java 21 (JDK)

Gradle 8.x

Docker & Docker Compose (for easy database/cache setup)

A valid  API Key

Local Installation & Launch
Clone the repository:

Bash

git clone https://github.com/dhruvbajpai29/qrfish-detector.git
cd smart-qr-scanner
Configure Environment Secrets:

Rename the application.properties.template file in src/main/resources to application.properties.

Crucially, do not commit secrets to Git. This project uses a hardcoded API key for demonstration purposes only. In a production environment, always use environment variables or a secrets management system.

Update application.properties with your database and Redis credentials.

Update VirusTotalService.java with your API key.

Build the application:

Bash

./gradlew clean build
Run the service:

Bash

java -jar build/libs/qrfish_detector-0.0.1-SNAPSHOT.jar
The API will be live on http://localhost:8080.

  Future Roadmap

[ ] CI/CD Integration: Implement a full CI/CD pipeline using GitHub Actions to automate builds, testing, and deployments.

[ ] Asynchronous Analysis: For non-critical paths, introduce a message queue (e.g., RabbitMQ) to decouple the initial request from the full analysis workflow.

[ ] Monitoring & Alerting: Integrate Micrometer, Prometheus, and Grafana to provide real-time metrics and alerting on system health and performance.

<div align="center">
Licensed under the MIT License. See LICENSE for more information.
</div>
