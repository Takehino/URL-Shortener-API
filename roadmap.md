# URL Shortener API with Analytics

## Mengapa Project Ini?

-   Menunjukkan pemahaman REST API
-   Implementasi caching
-   Handle high traffic scenarios
-   Analytics & tracking
-   Rate limiting
-   Clean architecture
-   Dokumentasi API profesional

## Fitur Utama

1. URL shortening dengan custom aliases
2. Analytics per URL:
    - Click counts
    - Geographic data
    - Referrer tracking
    - Device stats
3. User management
4. Rate limiting
5. API documentation
6. Dashboard API stats

## Tech Stack

```
Core:
- Node.js + Express/NestJS
- TypeScript
- PostgreSQL
- Redis (caching)

Tools:
- Docker
- Swagger/OpenAPI
- Jest
- GitHub Actions
```

## Project Roadmap

### Fase 1: Foundation (1-2 minggu)

#### Setup & Architecture

-   Project structure setup
-   Database design
-   Docker configuration
-   Basic API setup
-   Authentication system

#### Initial Features

-   URL shortening logic
-   Basic CRUD operations
-   Error handling
-   Logging system

### Fase 2: Core Features (2-3 minggu)

#### URL Management

-   Custom alias support
-   URL validation
-   Expiration dates
-   QR code generation

#### Analytics System

-   Click tracking
-   Geographic tracking
-   Device detection
-   Referrer tracking
-   Analytics aggregation

### Fase 3: Performance & Security (1-2 minggu)

#### Optimization

-   Redis caching
-   Rate limiting
-   Database indexing
-   Query optimization

#### Security

-   Input sanitization
-   XSS protection
-   CORS setup
-   API key management

### Fase 4: Testing & Documentation (1-2 minggu)

#### Testing

-   Unit tests
-   Integration tests
-   Load testing
-   Security testing

#### Documentation

-   API documentation
-   Swagger setup
-   Deployment guide
-   Architecture documentation

## Database Schema

```javascript
// User Schema
{
  _id: ObjectId,
  email: String,
  password: String,
  createdAt: Date,
  lastLogin: Date,
  isActive: Boolean
}

// URL Schema
{
  _id: ObjectId,
  userId: ObjectId,
  originalUrl: String,
  shortCode: String,
  createdAt: Date,
  expiresAt: Date,
  isCustom: Boolean,
  analytics: {
    totalClicks: Number,
    uniqueClicks: Number,
    lastAccessed: Date
  }
}

// Click Analytics Schema
{
  _id: ObjectId,
  urlId: ObjectId,
  timestamp: Date,
  ipAddress: String,
  userAgent: String,
  referer: String,
  location: {
    country: String,
    city: String,
    coordinates: [Number, Number]
  },
  device: {
    type: String,
    browser: String,
    os: String
  }
}
```

## API Endpoints

### Public API

```
GET /:shortCode - Redirect to original URL
POST /api/urls - Create short URL
GET /api/urls/:shortCode/stats - Get URL stats (public)
```

### Protected API

```
GET /api/urls - List user's URLs
PUT /api/urls/:shortCode - Update URL
DELETE /api/urls/:shortCode - Delete URL
GET /api/analytics - Get detailed analytics
```

## Key Implementations

### 1. URL Shortening Algorithm

```typescript
function generateShortCode(length: number = 6): string {
    const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    const code = Array.from({ length }, () => charset[Math.floor(Math.random() * charset.length)]).join("");
    return code;
}
```

### 2. Caching Strategy

```typescript
// Redis caching middleware
async function urlCache(req: Request, res: Response, next: NextFunction) {
    const shortCode = req.params.shortCode;
    const cached = await redis.get(`url:${shortCode}`);

    if (cached) {
        return res.redirect(cached);
    }

    next();
}
```

### 3. Rate Limiting

```typescript
const rateLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: "Too many requests, please try again later",
});
```

## Testing Strategy

1. Unit Tests:

    - URL shortening logic
    - Validation functions
    - Helper utilities

2. Integration Tests:

    - API endpoints
    - Database operations
    - Caching system

3. Load Tests:
    - Concurrent requests
    - Cache performance
    - Database performance

## Deployment Considerations

1. Container Setup:

    - Separate containers for:
        - API
        - Database
        - Redis
        - Nginx

2. CI/CD Pipeline:

    - Automated testing
    - Linting
    - Docker build
    - Deploy to staging

3. Monitoring:
    - Error tracking
    - Performance metrics
    - Usage statistics

## Portfolio Highlights

1. Technical Skills:

    - API Design
    - Database Optimization
    - Caching Strategies
    - Security Implementation

2. Business Value:

    - Scalable Architecture
    - Performance Metrics
    - Analytics Features

3. Code Quality:
    - Clean Architecture
    - Test Coverage
    - Documentation
    - Error Handling
