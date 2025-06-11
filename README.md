# Example Voting App

A simple distributed application running across multiple Docker containers.

## Architecture

![Architecture diagram](architecture.png)

The application consists of five components:

- **Voting App**: A Python web application that lets users vote between two options (Cats vs Dogs by default)
- **Result App**: A Node.js web application that displays the voting results in real-time
- **Worker**: A Java service that consumes votes from Redis and stores them in Postgres
- **Redis**: A Redis queue that collects new votes
- **Postgres**: A PostgreSQL database that stores voting data

## Getting Started

### Prerequisites

- Docker and Docker Compose
- Kubernetes (optional, for Kubernetes deployment)

### Run the app in Docker Compose

The easiest way to get started is to use Docker Compose:

```
docker-compose up
```

The application will be running at:
- Vote: http://localhost:5000
- Result: http://localhost:5001

### Run the app in Kubernetes

The application can also be deployed on Kubernetes using the provided specification files:

```
kubectl apply -f k8s-specifications/
```

The vote and result services are exposed as LoadBalancers.

## Alternative Deployments

### Java Worker

To run the application with a Java worker instead of the default .NET worker:

```
docker-compose -f docker-compose-javaworker.yml up
```

### Windows Containers

To run the application using Windows containers:

```
docker-compose -f docker-compose-windows.yml up
```

For Windows Server 1809 or Windows 10 1809:

```
docker-compose -f docker-compose-windows-1809.yml up
```

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if the same browser votes again.

## Development

The application is designed to be extensible. Each component can be developed and deployed independently.

### Vote

The voting app is a Python Flask application that builds with a simple Dockerfile. It uses Redis to queue votes.

### Result

The result app is a Node.js application that uses Socket.io to update the vote count in real-time. It reads voting data from a Postgres database.

### Worker

The worker is available in multiple languages:
- Java worker (Dockerfile.j)
- .NET worker (dotnet directory)

The worker processes votes from Redis and stores them in Postgres.

## License

See the [LICENSE](LICENSE) file for details.