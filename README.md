# 🏥 microservices-app-medical

**A microservices-based medical management system built with Spring Boot and Spring Cloud, laying the groundwork for service discovery, centralized configuration, API gateway routing, and dedicated services for patients and doctors ("médecins").**



---

## 📖 Overview

This project is structured as **five independent Spring Boot modules**:

| Module | Role |
|---|---|
| **Discovery Service** | Eureka-based service registry (scaffolded, ready for `@EnableEurekaServer` and business logic) |
| **Config Service** | Spring Cloud Config server for centralized configuration management (scaffolded, ready for `@EnableConfigServer`) |
| **Gateway Service** | Spring Cloud Gateway entry point, registered as a Eureka client, intended to route requests to the downstream services |
| **Patient Service** | Manages patient data, built with Spring Data JPA and MySQL, registered as a Eureka and Config client |
| **Médecin Service** | Manages doctor data, built with Spring Data JPA and MySQL, registered as a Eureka and Config client |

> ⚠️ **Early-stage scaffold.** Each module currently contains only its Spring Boot bootstrap class. Controllers, entities and business logic (as well as the Eureka / Config server enabling annotations and the datasource configuration) are still to be implemented. See the [Roadmap](#-roadmap).

---

## 🏗️ Target Architecture

```text
                    ┌──────────────────────┐
     Client ──────► │   Gateway Service    │
                    └───────┬──────┬───────┘
                            │      │ routes
                  ┌─────────▼┐   ┌─▼──────────┐
                  │ Patient  │   │  Médecin   │
                  │ Service  │   │  Service   │
                  └────┬─────┘   └─────┬──────┘
                       │               │
                     MySQL           MySQL

  Every service ──► registers with Discovery Service (Eureka)
  Patient and Médecin services ──► load their configuration from Config Service
```

---

## 🛠️ Technologies Used

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot, Spring Cloud (Config, Gateway, Netflix Eureka), Spring Cloud 2025.0.0 |
| Persistence | Spring Data JPA, MySQL (`mysql-connector-j`) |
| Monitoring | Spring Boot Actuator |
| Build tool | Maven |
| Other | Lombok |

---

## 📋 Prerequisites

- Java 17+ (JDK)
- Maven 3.6+ (or the included Maven Wrapper `./mvnw`)
- MySQL Server (for `patient-service` and `medecin-service`)

---

## 📁 Project Structure

```text
microservices-app-medical/
├── discovery-service/   # Eureka service discovery server
├── config-service/      # Spring Cloud Config server
├── gateway-service/     # Spring Cloud Gateway (routing)
├── patient-service/     # Patient management microservice (JPA + MySQL)
└── medecin-service/     # Doctor management microservice (JPA + MySQL)
```

---

## 🚀 Run Locally

### 1. Clone the project

```bash
git clone https://github.com/RajaAifa/microservices-app-medical.git
cd microservices-app-medical
```

### 2. Build and run a service

Each module is built and run independently. From each service's directory:

```bash
cd <service-name>
./mvnw clean install
./mvnw spring-boot:run
```

On Windows, use `mvnw.cmd` instead of `./mvnw`.

### 3. Recommended startup order

Once the services are fully configured:

```bash
# 1. Discovery Service
cd discovery-service && ./mvnw spring-boot:run

# 2. Config Service
cd config-service && ./mvnw spring-boot:run

# 3. Gateway Service
cd gateway-service && ./mvnw spring-boot:run

# 4. Business services
cd patient-service && ./mvnw spring-boot:run
cd medecin-service && ./mvnw spring-boot:run
```

Run each command in its own terminal, from the project root.

---

## ⚙️ Configuration

`patient-service` and `medecin-service` depend on `spring-cloud-starter-config`, so they expect to pull their configuration (including the MySQL datasource settings) from the Config Service at startup. Before running them, make sure to:

- Set `spring.config.import` to point to the Config Service URL
- Provide the MySQL connection settings (`spring.datasource.url`, `username`, `password`), either through the Config Service or directly in each service's `application.properties`
- Set `eureka.client.serviceUrl.defaultZone` to point to the Discovery Service

> 🔒 Never commit real passwords. Keep placeholder values in the repository, or read the credentials from environment variables, for example `spring.datasource.password=${DB_PASSWORD:your_mysql_password}`.

---

## 🗺️ Roadmap

- [ ] Enable the Eureka server on `discovery-service` (`@EnableEurekaServer`)
- [ ] Enable the Config server on `config-service` (`@EnableConfigServer`) and add a configuration source
- [ ] Define gateway routes in `gateway-service`
- [ ] Add entities, repositories, services and controllers for `patient-service`
- [ ] Add entities, repositories, services and controllers for `medecin-service`
- [ ] Add the datasource configuration for MySQL

---

## 👩‍💻 Author

**Raja Aifa**

🎓 Professional Master's Degree in Web Services and Multimedia, Tunisia

---

## 📜 License

This project is intended for educational purposes.
