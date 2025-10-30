# Aztec Local Development Environment

A Docker Compose setup for running a complete Aztec development environment with block explorers and supporting services.

## Components

### Currently Implemented:
- **Anvil**: Local Ethereum network (L1) running on port 8545
- **Aztec Local Network**: Aztec L2 network running on port 8080, connected to Anvil
- **Otterscan**: Lightweight block explorer for viewing the local Ethereum (L1) network

### Planned (Placeholders included):
- Bridge between L1 and L2
- Bridge application

## Prerequisites

- Docker and Docker Compose installed
- At least 4GB of available RAM
- Ports 8545, 8080, 8081, and 5100 available

## Quick Start

1. Clone this repository and navigate to the project directory:
```bash
cd local-aztec
```

2. Start the services:
```bash
docker-compose up -d
```

This will:
- Pull the official Aztec Docker image
- Start Anvil (local Ethereum L1 network)
- Start the Aztec L2 network (connects to Anvil and deploys contracts)
- Start Otterscan block explorer for viewing L1 activity

## Accessing Services

Once running, you can access:

- **Anvil RPC**: http://localhost:8545
- **Aztec Sandbox**: http://localhost:8080
- **Otterscan Block Explorer**: http://localhost:5100

## Service Details

### Anvil
A local Ethereum development network that runs as a separate service:
- Provides the L1 blockchain for Aztec to deploy its contracts
- Runs on port 8545
- Configured with chain ID 31337

### Aztec Local Network
The Aztec L2 network that connects to Anvil:
- Runs in sandbox mode with test accounts pre-configured
- Deploys L1 contracts to Anvil on startup
- Provides the privacy-focused Layer 2 functionality
- Accessible on port 8080

### Otterscan
A fast and lightweight Ethereum block explorer:
- Connects to your local Anvil instance to view L1 activity
- Access the web UI at http://localhost:5100
- View blocks, transactions, and contract deployments
- Monitor the L1 contracts deployed by Aztec

## Development Usage

### Connecting to the Networks

#### From Host Machine:
- Ethereum RPC: `http://localhost:8545`
- Aztec RPC: `http://localhost:8080`

#### From Other Docker Containers:
- Ethereum RPC: `http://anvil:8545`
- Aztec RPC: `http://aztec:8080`

### Viewing Logs

```bash
# All services
docker-compose logs -f

# Specific services
docker-compose logs -f anvil
docker-compose logs -f aztec
docker-compose logs -f otterscan
```

### Stopping Services

```bash
docker-compose down

# To also remove volumes (reset state)
docker-compose down -v
```

## Extending the Setup

### Adding Bridge Services

1. Create `bridge/` and `bridge-app/` directories with their respective Dockerfiles
2. Uncomment the bridge services in `docker-compose.yml`
3. Update the environment variables in the service definitions as needed

## Troubleshooting

### Port Conflicts
If you get port binding errors, ensure no other services are using ports 8545, 8080, 8081, or 5100.

### Aztec Installation Fails
The Aztec installation requires internet access. Ensure your Docker daemon can reach external networks.

### Otterscan Not Displaying Data
If Otterscan loads but doesn't show blockchain data, ensure Anvil is running:
```bash
docker-compose exec anvil cast block-number --rpc-url http://localhost:8545
```
Note: Otterscan connects to Anvil from your browser, so it uses `localhost:8545`.

### Low Performance
Aztec requires significant resources. Ensure Docker has at least 4GB RAM allocated.

## Network Architecture

All services are connected via the `aztec-network` Docker bridge network, allowing them to communicate using service names as hostnames.

## Data Persistence

Aztec data is persisted in the `aztec-data` volume. To reset the environment completely:
```bash
docker-compose down -v
```

## Contributing

To add new services:
1. Add the service definition to `docker-compose.yml`
2. Ensure it's connected to the `aztec-network`
3. Update this README with the new service details

## License

This setup is for development purposes only.