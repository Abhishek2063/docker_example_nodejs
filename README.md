# Redis Express Counter App

A simple Node.js web application built with Express.js and Redis that tracks the number of times the homepage has been visited. This application demonstrates the integration of Express.js with Redis for persistent data storage and includes Docker containerization for easy deployment.

## 🚀 Features

- **Visit Counter**: Tracks and displays the number of homepage visits
- **Redis Integration**: Uses Redis for persistent counter storage
- **Health Checks**: Built-in health endpoint for monitoring
- **Docker Support**: Fully containerized with Docker Compose
- **Environment Configuration**: Configurable through environment variables
- **Graceful Shutdown**: Proper cleanup of Redis connections
- **Additional Routes**: Counter management and status endpoints

## 📋 Prerequisites

- **Node.js** (v18 or higher)
- **Docker** and **Docker Compose**
- **Redis** (if running locally without Docker)

## 🛠️ Installation & Setup

### Option 1: Using Docker Compose (Recommended)

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd redis-express-app
   ```

2. **Build and start the services**
   ```bash
   docker-compose up --build
   ```

3. **Access the application**
   - Main app: http://localhost:8000
   - Health check: http://localhost:8000/health

### Option 2: Local Development

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Start Redis server**
   ```bash
   # Using Docker
   docker run -d -p 6379:6379 redis:7-alpine
   
   # Or install Redis locally and run
   redis-server
   ```

3. **Configure environment variables**
   Create a `.env` file:
   ```bash
   REDIS_HOST=localhost
   REDIS_PORT=6379
   PORT=8000
   ```

4. **Start the application**
   ```bash
   npm start
   ```

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Homepage with visit counter (increments on each visit) |
| GET | `/health` | Health check endpoint with Redis status |
| GET | `/count` | Get current counter value without incrementing |
| GET | `/reset` | Reset the counter to zero |

### Example Responses

**GET /** 
```
🎉 This webpage has been viewed 5 time(s)
```

**GET /health**
```json
{
  "status": "OK",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "redis": "connected"
}
```

**GET /count**
```json
{
  "count": 5,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

## 🐳 Docker Configuration

### Dockerfile
- Uses `node:18-alpine` for a lightweight base image
- Creates a non-root user for security
- Exposes port 8000
- Optimized for production use

### docker-compose.yml
- **Redis Service**: Uses `redis:7-alpine` with persistent storage
- **App Service**: Builds from local Dockerfile
- **Networking**: Services communicate via custom network
- **Health Checks**: Both services include health monitoring
- **Volumes**: Redis data persists between container restarts

## ⚙️ Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `REDIS_HOST` | `localhost` | Redis server hostname |
| `REDIS_PORT` | `6379` | Redis server port |
| `PORT` | `8000` | Application server port |
| `NODE_ENV` | - | Node.js environment (production/development) |

## 🧪 Testing the Counter Functionality

1. **Start the application**
   ```bash
   docker-compose up --build
   ```

2. **Test the counter increment**
   ```bash
   # Visit the homepage multiple times
   curl http://localhost:8000
   curl http://localhost:8000
   curl http://localhost:8000
   ```
   
   Each request should show an incremented counter:
   ```
   🎉 This webpage has been viewed 1 time(s)
   🎉 This webpage has been viewed 2 time(s)
   🎉 This webpage has been viewed 3 time(s)
   ```

3. **Verify counter persistence**
   ```bash
   # Restart the containers
   docker-compose down
   docker-compose up
   
   # Visit the homepage - counter should continue from last value
   curl http://localhost:8000
   ```

4. **Check health status**
   ```bash
   curl http://localhost:8000/health
   ```

5. **Reset counter (optional)**
   ```bash
   curl http://localhost:8000/reset
   ```

## 📊 Monitoring

### Docker Health Checks
Both services include health checks:
- **App**: Checks `/health` endpoint every 30 seconds
- **Redis**: Uses `redis-cli ping` every 30 seconds

### Logs
View application logs:
```bash
# All services
docker-compose logs

# App only
docker-compose logs app

# Redis only
docker-compose logs redis

# Follow logs in real-time
docker-compose logs -f
```

## 🛡️ Security Features

- **Non-root user**: Application runs as non-privileged user in container
- **Network isolation**: Services communicate via private Docker network
- **Environment variables**: Sensitive configuration externalized
- **Graceful shutdown**: Proper cleanup of connections and resources

## 🚨 Troubleshooting

### Common Issues

1. **Redis connection failed**
   ```bash
   # Check if Redis container is running
   docker-compose ps
   
   # Check Redis logs
   docker-compose logs redis
   ```

2. **Port already in use**
   ```bash
   # Change port in docker-compose.yml or stop conflicting service
   sudo lsof -i :8000
   ```

3. **Counter not persisting**
   ```bash
   # Check if Redis volume is properly mounted
   docker volume ls
   docker volume inspect redis-express-app_redis_data
   ```

### Debug Mode
Run with debug output:
```bash
# Set NODE_ENV to development
NODE_ENV=development docker-compose up
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and add tests
4. Commit your changes: `git commit -am 'Add feature'`
5. Push to the branch: `git push origin feature-name`
6. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Resources

- [Express.js Documentation](https://expressjs.com/)
- [Redis Documentation](https://redis.io/documentation)
- [Docker Documentation](https://docs.docker.com/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)