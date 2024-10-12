# Docker Compose Setup for Keycloak and PostgreSQL - SYNTHI AI

This Docker Compose file is designed for SYNTHI AI, setting up Keycloak and PostgreSQL to provide a robust authentication system with secure data storage. It includes specific service configurations, environment variables for access, and a custom network for seamless inter-service communication. This setup is well-suited for development and paves the way for effective application authentication management.

## Services

### PostgreSQL Service (`postgres`):
- **Image:** Utilizes the Docker image `postgres:16.2` to run a PostgreSQL server.
- **Volumes:** Maps a volume named `postgres_data` to `/var/lib/postgresql/data` inside the container for persistent storage of database data.
- **Environment Variables:** Configures the database with the following:
  - `POSTGRES_DB`: The name of the database.
  - `POSTGRES_USER`: The username for database access.
  - `POSTGRES_PASSWORD`: The password for the specified user.
- **Networks:** Connects to a custom network `synthi_network` for communication with the Keycloak service.

### Keycloak Service (`keycloak`):
- **Image:** Uses `quay.io/keycloak/keycloak:26.0.0` to run a Keycloak server.
- **Command:** Specifies `start` as the command to execute Keycloak.
- **Environment Variables:** Sets up essential settings, including:
  - `KC_HOSTNAME`: The hostname for Keycloak access.
  - `KC_HOSTNAME_PORT`: The port for hostname access.
  - `KC_HTTP_ENABLED`: Enables HTTP configuration.
  - `KC_HOSTNAME_STRICT_HTTPS`: Enforces HTTPS for hostname access.
  - `KC_HEALTH_ENABLED`: Enables health checks.
  - `KEYCLOAK_ADMIN`: Admin username.
  - `KEYCLOAK_ADMIN_PASSWORD`: Admin password.
  - Database connection details (`KC_DB`, `KC_DB_URL`, `KC_DB_USERNAME`, `KC_DB_PASSWORD`).
- **Ports:** Exposes port `8080` on the host, mapping it to port `8080` on the Keycloak container for web access.
- **Restart Policy:** Configured to always restart unless manually stopped.
- **Dependency:** Declares a dependency on the `postgres` service, ensuring it starts first.
- **Networks:** Also connected to the `synthi_network`.

## Volumes

- **postgres_data:** A named volume for storing PostgreSQL data across container restarts, utilizing the default local storage driver.

## Networks

- **synthi_network:** A custom network using the bridge driver, facilitating efficient communication between the Keycloak and PostgreSQL containers.

## Environment Variables

The Docker Compose file specifies critical values for database configuration (`POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`) and Keycloak admin credentials (`KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD`). These are essential for secure communication and operation of the services.