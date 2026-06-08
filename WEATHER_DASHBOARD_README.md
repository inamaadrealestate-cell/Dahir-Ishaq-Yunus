# Weather Dashboard

A modern, real-time weather dashboard built with NestJS backend and React/Next.js frontend, fetching data from OpenWeatherMap API.

## 🌦️ Features

- **Current Weather**: Real-time weather information for any city
- **Location Search**: Search weather by city name or coordinates (latitude/longitude)
- **5-Day Forecast**: Detailed weather forecast for the next 5 days
- **Multiple Cities**: Get weather for multiple cities at once
- **Caching**: Redis caching for improved performance
- **Beautiful UI**: Modern glassmorphism design with Tailwind CSS
- **Responsive**: Works on desktop, tablet, and mobile devices
- **API Documentation**: Swagger/OpenAPI docs included

## 🏗️ Architecture

```
Weather Dashboard
├── Backend (NestJS)
│   ├── Weather Module
│   ├── Weather Service (OpenWeatherMap API integration)
│   ├── Weather Controller (REST API endpoints)
│   └── DTOs (Request/Response models)
├── Frontend (React/Next.js)
│   ├── Weather Dashboard Page
│   ├── Search Component
│   └── Weather Display Components
└── Infrastructure
    ├── Caching (Redis)
    └── Docker Compose
```

## 📋 Requirements

- Node.js 18+
- npm 9+
- Docker & Docker Compose
- OpenWeatherMap API Key (free at https://openweathermap.org/api)

## 🚀 Quick Start

### 1. Get OpenWeatherMap API Key

```bash
# Visit https://openweathermap.org/api
# Sign up for a free account
# Get your API key from the dashboard
```

### 2. Configure Environment Variables

```bash
# Copy .env.example to .env
cp .env.example .env

# Add your OpenWeatherMap API key
OPENWEATHER_API_KEY=your_api_key_here
```

### 3. Start Infrastructure

```bash
# From project root
npm run docker:up
```

### 4. Start Backend

```bash
cd backend
npm install
npm run dev

# Should output:
# 🚀 INAMAAD API running on http://localhost:3000
# 📚 Documentation available at http://localhost:3000/api/docs
```

### 5. Start Frontend

```bash
# In a new terminal
cd frontend
npm install
npm run dev

# Should output:
# - ready started server on 0.0.0.0:3000
# - ready - compiled client and server successfully
```

### 6. Access the Dashboard

```
Frontend: http://localhost:3000/weather
API Docs: http://localhost:3000/api/docs
```

## 📡 API Endpoints

### Current Weather

#### Get weather by city name
```bash
GET /api/weather/current/city/:city

# Example
curl http://localhost:3000/api/weather/current/city/London
```

**Response:**
```json
{
  "city": "London",
  "country": "GB",
  "coordinates": {
    "lat": 51.5085,
    "lon": -0.1257
  },
  "temperature": {
    "current": 15.5,
    "feels_like": 14.8,
    "min": 13.2,
    "max": 17.9
  },
  "humidity": 72,
  "pressure": 1013,
  "visibility": 10000,
  "windSpeed": 4.5,
  "windDegree": 230,
  "cloudiness": 75,
  "description": "Cloudy",
  "details": "overcast clouds",
  "icon": "04d",
  "sunrise": "2026-06-08T04:45:00Z",
  "sunset": "2026-06-08T21:15:00Z",
  "timezone": 3600,
  "timestamp": "2026-06-08T12:30:00Z"
}
```

#### Get weather by coordinates
```bash
GET /api/weather/current/coordinates?lat=51.5074&lon=-0.1278

# Example
curl "http://localhost:3000/api/weather/current/coordinates?lat=51.5074&lon=-0.1278"
```

### Forecast

#### Get 5-day forecast
```bash
GET /api/weather/forecast/:city

# Example
curl http://localhost:3000/api/weather/forecast/London
```

**Response:**
```json
{
  "city": "London",
  "country": "GB",
  "forecast": {
    "6/8/2026": [
      {
        "time": "2026-06-08T15:00:00Z",
        "temperature": 16.5,
        "feels_like": 15.8,
        "humidity": 70,
        "pressure": 1012,
        "description": "Cloudy",
        "details": "overcast clouds",
        "icon": "04d",
        "windSpeed": 4.2,
        "precipitation": 0
      }
    ]
  }
}
```

#### Get weather for multiple cities
```bash
GET /api/weather/multiple?cities=London,Paris,Tokyo

# Example
curl "http://localhost:3000/api/weather/multiple?cities=London,Paris,Tokyo"
```

## 🎨 Frontend Components

### Weather Dashboard Page
- Main component at `frontend/app/weather/page.tsx`
- Real-time weather display
- Search functionality
- Forecast view
- Responsive grid layout

### Features
- Glassmorphism design
- Gradient backgrounds
- Weather icons from OpenWeatherMap
- Sunrise/sunset times
- Wind speed, humidity, pressure display
- Temperature conversion (Celsius)

## 🔄 Caching Strategy

- **Current Weather**: Cached for 5 minutes
- **Forecast**: Cached for 10 minutes
- **Multiple Cities**: Cached for 5 minutes
- Redis backend for distributed caching

## 📊 Data Flow

```
Frontend (React)
    ↓
HTTP Request to Backend API
    ↓
NestJS Controller
    ↓
Weather Service
    ↓
Check Redis Cache
    ├─ Cache Hit → Return cached data
    └─ Cache Miss
        ↓
        OpenWeatherMap API
        ↓
        Format Response
        ↓
        Store in Redis Cache
        ↓
        Return to Frontend
    ↓
Display on Dashboard
```

## 🧪 Testing

### Unit Tests
```bash
cd backend
npm run test
```

### Test Coverage
```bash
npm run test:cov
```

### Manual API Testing
```bash
# Using curl
curl http://localhost:3000/api/weather/current/city/London

# Using HTTPie
http localhost:3000/api/weather/current/city/London

# Using Postman
- Import OpenAPI docs from http://localhost:3000/api/docs
- Test all endpoints
```

## 📚 API Documentation

Swagger OpenAPI documentation available at:
```
http://localhost:3000/api/docs
```

## 🛠️ Troubleshooting

### Issue: API Key Invalid
```bash
# Check your API key is correct
# Verify it in .env file
OPENWEATHER_API_KEY=your_key_here

# Restart backend
npm run dev
```

### Issue: CORS Error
```bash
# CORS is enabled in app.module.ts
# Make sure frontend URL matches in environment
FRONTEND_URL=http://localhost:3000
```

### Issue: Cache Not Working
```bash
# Check Redis is running
docker-compose -f infra/docker-compose.dev.yml logs redis

# Clear cache if needed
docker-compose -f infra/docker-compose.dev.yml exec redis redis-cli FLUSHALL
```

### Issue: Port Already in Use
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
API_PORT=3001 npm run dev
```

## 📦 Dependencies

### Backend
- `@nestjs/core` - NestJS framework
- `@nestjs/axios` - HTTP client
- `@nestjs/cache-manager` - Caching support
- `@nestjs/config` - Environment configuration
- `@nestjs/swagger` - API documentation
- `axios` - HTTP requests
- `class-validator` - DTO validation

### Frontend
- `react` - React framework
- `next` - Next.js framework
- `axios` - HTTP client
- `tailwindcss` - CSS framework

## 🚀 Deployment

### Docker Deployment
```bash
# Build image
docker build -f infra/Dockerfile -t weather-dashboard:latest .

# Run container
docker run -p 3000:3000 -e OPENWEATHER_API_KEY=your_key weather-dashboard:latest
```

### Environment Variables for Production
```bash
OPENWEATHER_API_KEY=prod_key_here
DATABASE_URL=postgresql://...
REDIS_URL=redis://...
NODE_ENV=production
FRONTEND_URL=https://yourdomain.com
```

## 📄 License

MIT License - See [LICENSE](../../LICENSE)

## 🤝 Contributing

1. Create feature branch
2. Make changes
3. Add tests
4. Submit PR

## 📞 Support

For issues or questions:
1. Check existing issues on GitHub
2. Create new issue with details
3. Contact development team

---

**Weather Dashboard** - Real-time weather information at your fingertips! 🌦️
