# Testing Guide - INAMAAD Real Estate Platform

## 🧪 Local Testing Setup

### Prerequisites
- Node.js 18+
- npm 9+
- Docker & Docker Compose
- Git

### Step 1: Clone & Setup

```bash
# Clone repository
git clone https://github.com/inamaadrealestate-cell/Dahir-Ishaq-Yunus.git
cd Dahir-Ishaq-Yunus

# Checkout feature branch
git checkout feat/complete-project-setup

# Copy environment variables
cp .env.example .env

# Install root dependencies
npm install
```

### Step 2: Start Infrastructure

```bash
# Start Docker services (PostgreSQL, Redis, Elasticsearch)
npm run docker:up

# Verify services are running
npm run docker:logs

# Expected output:
# - postgres: ready to accept connections
# - redis: Ready to accept connections
# - elasticsearch: listening on port 9200
```

### Step 3: Backend Setup & Testing

```bash
# Navigate to backend
cd backend

# Install dependencies
npm install

# Verify project structure
ls -la src/
# Should show: main.ts, app.module.ts, database/, modules/

# Build TypeScript
npm run build

# Start development server
npm run dev

# Expected output:
# 🚀 INAMAAD API running on http://localhost:3000
# 📚 Documentation available at http://localhost:3000/api/docs
```

### Step 4: API Testing

**Swagger API Documentation:**
```
http://localhost:3000/api/docs
```

**Health Check:**
```bash
curl http://localhost:3000/health
```

**Database Connection Test:**
```bash
# The app should connect to PostgreSQL without errors
# Check terminal output for:
# - TypeORM connection established
# - Migrations running (if any)
```

### Step 5: Run Tests

```bash
# Unit tests
npm run test

# Watch mode
npm run test:watch

# Coverage report
npm run test:cov
```

---

## ✅ Testing Checklist

### Infrastructure
- [ ] PostgreSQL container running on 5432
- [ ] Redis container running on 6379
- [ ] Elasticsearch container running on 9200
- [ ] All health checks passing

### Backend
- [ ] Backend dependencies installed
- [ ] TypeScript compiles successfully
- [ ] App starts on port 3000
- [ ] Swagger docs accessible at /api/docs
- [ ] Database connection established
- [ ] All 9 modules loaded

### Code Structure
- [ ] 8 database entities created
- [ ] 9 feature modules initialized
- [ ] app.module.ts imports all modules
- [ ] main.ts configures Swagger
- [ ] Environment configuration working

---

## 🔍 Detailed Testing Scenarios

### Test 1: Database Connection
```bash
# Inside Docker container
docker-compose -f infra/docker-compose.dev.yml exec postgres psql -U inamaad -d inamaad_db

# Should open PostgreSQL prompt
inamaad_db=#
```

### Test 2: Redis Connection
```bash
docker-compose -f infra/docker-compose.dev.yml exec redis redis-cli

# Should show Redis prompt
127.0.0.1:6379>
```

### Test 3: Elasticsearch Health
```bash
curl http://localhost:9200/_health

# Expected response:
# {"status":"green",...}
```

### Test 4: NestJS Module Loading
```bash
# Check terminal logs for:
# AuthModule
# UsersModule
# PropertiesModule
# ListingsModule
# JvOpportunitiesModule
# BookingsModule
# PaymentsModule
# MessagesModule
# ReviewsModule
```

---

## 📋 Project Structure Verification

```bash
cd backend

# Verify entities exist
ls src/database/entities/
# Expected: user.entity.ts, property.entity.ts, listing.entity.ts, etc.

# Verify modules exist
ls src/modules/
# Expected: auth/, users/, properties/, listings/, jv-opportunities/, etc.

# Verify main files
ls -la src/ | grep -E "(main|app.module)"
# Expected: app.module.ts, main.ts
```

---

## 🐛 Troubleshooting

### Issue: Port Already in Use
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
API_PORT=3001 npm run dev
```

### Issue: Database Connection Error
```bash
# Check if PostgreSQL container is running
docker-compose -f infra/docker-compose.dev.yml ps

# View container logs
docker-compose -f infra/docker-compose.dev.yml logs postgres

# Verify database exists
docker-compose -f infra/docker-compose.dev.yml exec postgres psql -U inamaad -l
```

### Issue: Module Import Error
```bash
# Clear build cache
rm -rf dist/ node_modules/

# Reinstall
npm install

# Rebuild
npm run build

# Start fresh
npm run dev
```

### Issue: Missing Environment Variables
```bash
# Verify .env file exists
ls -la .env

# Copy from template if missing
cp .env.example .env

# Restart app
npm run dev
```

---

## 📊 Expected File Structure After Setup

```
INAMAAD-Real-Estate/
├── backend/
│   ├── src/
│   │   ├── database/
│   │   │   └── entities/
│   │   │       ├── user.entity.ts ✅
│   │   │       ├── property.entity.ts ✅
│   │   │       ├── listing.entity.ts ✅
│   │   │       ├── jv-opportunity.entity.ts ✅
│   │   │       ├── booking.entity.ts ✅
│   │   │       ├── payment.entity.ts ✅
│   │   │       ├── message.entity.ts ✅
│   │   │       └── review.entity.ts ✅
│   │   ├── modules/
│   │   │   ├── auth/ ✅
│   │   │   ├── users/ ✅
│   │   │   ├── properties/ ✅
│   │   │   ├── listings/ ✅
│   │   │   ├── jv-opportunities/ ✅
│   │   │   ├── bookings/ ✅
│   │   │   ├── payments/ ✅
│   │   │   ├── messages/ ✅
│   │   │   └── reviews/ ✅
│   │   ├── app.module.ts ✅
│   │   └── main.ts ✅
│   ├── package.json ✅
│   ├── tsconfig.json ✅
│   └── dist/ (generated after build)
├── infra/
│   ├── docker-compose.dev.yml ✅
│   └── Dockerfile
├── .env ✅
├── package.json ✅
└── README.md ✅
```

---

## 🚀 Next Steps After Successful Test

1. **Verify all files are present** ✅
2. **Backend runs without errors** ✅
3. **Swagger documentation loads** ✅
4. **Database connection successful** ✅
5. **All modules initialized** ✅

**Then proceed to:**
- Create API controllers
- Implement services & business logic
- Write unit tests
- Setup frontend
- Configure CI/CD

---

## 📝 Testing Results Template

Copy & paste when testing:

```
✅ Infrastructure
- [ ] PostgreSQL running
- [ ] Redis running
- [ ] Elasticsearch running

✅ Backend
- [ ] Dependencies installed
- [ ] TypeScript builds
- [ ] App starts on :3000
- [ ] Swagger accessible
- [ ] Database connected
- [ ] Modules loaded

✅ Code Quality
- [ ] No linting errors
- [ ] All imports resolved
- [ ] Entities defined
- [ ] Modules initialized

✅ Final Status: READY FOR DEVELOPMENT
```

---

**Testing Guide Created**: 2026-06-08  
**Status**: All files staged for commit ✅
