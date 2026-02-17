# Trackbit Master

Unified repository for Trackbit application with backend and frontend as Git submodules.

## Project Structure

```
trackbit_master/
├── docker-compose.yml    # Main orchestration file
├── .env.example          # Environment template
├── .env                  # Your local environment (create from .env.example)
├── README.md
├── backend/              # Submodule: school_ops_sv
└── frontend/             # Submodule: school_ops_ui
```

## Quick Start

### 1. Clone the Repository

```bash
# Clone with submodules in one command
git clone --recurse-submodules https://github.com/Rakeshkoyya/trackbit_master.git
cd trackbit_master

# Or if already cloned without submodules:
git submodule update --init --recursive
```

### 2. Configure Environment

```bash
# Copy the example environment file
cp .env.example .env

# Edit .env and configure:
# - DATABASE_URL: Your AWS RDS PostgreSQL connection string
# - JWT_SECRET_KEY: Authentication secret (min 32 characters)
```

### 3. Build and Run

```bash
# Build and start all services
docker-compose up --build

# Or run in detached mode
docker-compose up --build -d
```

### 4. Access the Application

| Service  | URL                          | Description        |
|----------|------------------------------|--------------------|
| Frontend | http://localhost:3000        | Main application   |
| Backend  | http://localhost:8000        | API server         |
| API Docs | http://localhost:8000/docs   | Swagger UI         |

## Services

### Database (AWS RDS)
- External PostgreSQL hosted on AWS
- Configure via `DATABASE_URL` in `.env`
- Format: `postgresql://user:password@your-rds-endpoint:5432/database`

### Backend (FastAPI)
- Port: `8000`
- Built from `./backend/Dockerfile`
- Health check: `/health`

### Frontend (Next.js)
- Port: `3000`
- Built from `./frontend/Dockerfile`
- API calls to `/api/v1/*` are proxied to backend

## Common Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f backend

# Rebuild a specific service
docker-compose up --build backend

# Remove all containers (fresh start)
docker-compose down
```

## Updating Submodules

When backend or frontend repos have new commits:

```bash
# Update both submodules to latest
cd backend && git pull origin main && cd ..
cd frontend && git pull origin main && cd ..

# Commit the submodule updates
git add backend frontend
git commit -m "Update submodules to latest"
git push
```

Or update a specific submodule:

```bash
git submodule update --remote backend
git submodule update --remote frontend
```

## Development

For local development without Docker, refer to the individual README files:
- [Backend README](backend/README.md)
- [Frontend README](frontend/README.md)

## Troubleshooting

### Submodules are empty after clone
```bash
git submodule update --init --recursive
```

### Database connection issues
- Verify your `DATABASE_URL` in `.env` is correct
- Ensure AWS RDS security group allows connections from your IP/container
- Check that the database exists and credentials are valid

### Port conflicts
Ensure ports 3000 and 8000 are not in use by other applications.

### Rebuild from scratch
```bash
docker-compose down
docker-compose up --build
```

## License

See individual submodule repositories for license information.
