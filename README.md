# Docker Compose Setup for Keycloak and PostgreSQL

Este repositório Docker Compose configura o Keycloak e o PostgreSQL, fornecendo um sistema de autenticação com armazenamento de dados. Ele especifica as configurações de serviço, incluindo variáveis de ambiente para acesso e uma rede personalizada para comunicação entre serviços. Ideal para desenvolvimento, essa configuração estabelece as bases para o gerenciamento de autenticação de aplicativos.

## Services

### PostgreSQL Service (`postgres`):
- **Image:** Uses the Docker image `postgres:16-bullseye` to run a PostgreSQL server.
- **Volumes:** Maps a volume named `postgres_data` to `/var/lib/postgresql/data` inside the container for storage of the database data.
- **Environment Variables:** Configures the database with the name (`POSTGRES_DB`), user (`POSTGRES_USER`), and password (`POSTGRES_PASSWORD`) specified by the environment variables.
- **Networks:** Connects to a custom network `keycloak_network` for communication with the Keycloak service.

### Keycloak Service (`keycloak`):
- **Image:** Utilizes `quay.io/keycloak/keycloak:23.0.6` for running a Keycloak server.
- **Command:** Specifies `start` as the command to run Keycloak.
- **Environment Variables:** Sets up various settings including hostname (`KC_HOSTNAME`), port (`KC_HOSTNAME_PORT`), HTTP configuration (`KC_HTTP_ENABLED`, `KC_HOSTNAME_STRICT_HTTPS`), health check (`KC_HEALTH_ENABLED`), admin credentials (`KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD`), and database connection details (`KC_DB`, `KC_DB_URL`, `KC_DB_USERNAME`, `KC_DB_PASSWORD`).
- **Ports:** Exposes port `8080` on the host, mapping it to port `8080` on the Keycloak container for web access.
- **Restart Policy:** Configured to always restart unless manually stopped.
- **Dependency:** Declares a dependency on the `postgres` service, ensuring it starts first.
- **Networks:** Also connected to the `keycloak_network`.

## Volumes

- **postgres_data:** Um volume nomeado para armazenar os dados do PostgreSQL durante as reinicializações do contentor, utilizando o controlador de armazenamento local predefinido.

## Networks

- **keycloak_network:** Uma rede personalizada usando o driver bridge, facilitando a comunicação entre os contentores Keycloak e PostgreSQL.

## Environment Variables

Specifies the values for database configuration (`POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`) and Keycloak admin credentials (`KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD`), which are essential for the services to communicate and operate securely.
