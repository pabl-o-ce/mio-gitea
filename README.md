# Gitea Docker Compose Setup

This repository contains a Docker Compose configuration for setting up Gitea, a self-hosted Git service, along with a PostgreSQL database and a Gitea runner.

## Prerequisites

- Docker
- Docker Compose
- Git

## Setup Instructions

1. Clone this repository:
   ```
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Create a `.env` file in the root directory of the project with the following variables (adjust as needed):
   ```
   POSTGRES_USER=gitea
   POSTGRES_PASSWORD=your_secure_password
   POSTGRES_DB=gitea

   GITEA__database__DB_TYPE=postgres
   GITEA__database__HOST=gitea-postgres:5432
   GITEA__database__NAME=gitea
   GITEA__database__USER=gitea
   GITEA__database__PASSWD=your_secure_password

   GITEA_RUNNER_REGISTRATION_TOKEN=your_runner_registration_token
   ```

3. Create the necessary directories for Gitea data and config:
   ```
   sudo mkdir -p /mnt/gitea/data /mnt/gitea/config
   ```

4. Update the `extra_hosts` section in the `gitea-runner` service of the `docker-compose.yml` file:
   Replace `<git-domain>` and `<registry-domain>` with your actual domain names, and `<IP>` with the corresponding IP addresses.

5. Start the services:
   ```
   docker-compose up -d
   ```

6. Access Gitea through your web browser at `http://localhost:3000` (or the appropriate host if deployed remotely).

7. Complete the initial Gitea setup through the web interface.

## Services

- **Gitea**: The main Gitea service.
- **PostgreSQL**: The database used by Gitea.
- **Gitea Runner**: A runner for Gitea Actions.

## Ports

- Gitea SSH: 2222
- Gitea Web: 3000 (default, not explicitly mapped in this configuration)

## Volumes

- `gitea-data`: Stores Gitea data.
- `gitea-config`: Stores Gitea configuration.
- `gitea-postgres`: Stores PostgreSQL data.

## Networks

- `gitea`: Internal network for Gitea services.
- `middle-earth`: External network (must be created separately if not already existing).

## Customization

You can customize the Gitea instance by modifying the `.env` file or the `docker-compose.yml` file. Refer to the [Gitea documentation](https://docs.gitea.io/en-us/) for more configuration options.

## Maintenance

- To update the services, pull the latest images and restart:
  ```
  docker-compose pull
  docker-compose up -d
  ```

- To view logs:
  ```
  docker-compose logs -f
  ```

- To stop the services:
  ```
  docker-compose down
  ```

## Backup

It's recommended to regularly backup the `/mnt/gitea` directory and the PostgreSQL database.

## Security Notes

- Make sure to use strong passwords in the `.env` file.
- Consider using HTTPS for production deployments.
- Regularly update your Docker images and host system.

For more information on securing your Gitea instance, refer to the [Gitea security documentation](https://docs.gitea.io/en-us/security/).
