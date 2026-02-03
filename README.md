# Senior Developer Exercise: Real-time Order Tracking System

## Objective
Implement a **real-time order tracking dashboard** for a food delivery service that demonstrates:
- Advanced technical skills across the full stack
- Performance optimization and scalability
- Clean code and robust error handling
- Modern development practices

## Expected Duration: 1 week (5 working days)

## Tech Stack
- **Backend**: .NET 8 with SignalR/WebSockets
- **Frontend**: Angular 17 with RxJS
- **Database**: SQL Server (Entity Framework Core)
- **Cache/Message Queue**: Redis + RabbitMQ
- **Infrastructure**: Kubernetes, Docker
- **Monitoring**: Application Insights/Serilog

## Business Scenario
A food delivery company needs a real-time dashboard to track:
- Order status updates (Pending → Preparing → Out for Delivery → Delivered)
- Delivery driver locations (simulated)
- Estimated delivery times
- System performance metrics

## Exercise Scope

### Day 1: Backend Foundation
**Requirements:**
1. Create a .NET 8 Web API with:
   - Clean Architecture (Onion/Hexagonal)
   - Entity Framework Core with code-first migrations
   - Repository pattern with Unit of Work

2. Implement:
   - Order entity with status transitions
   - Driver entity with location tracking
   - Real-time hub using SignalR
   - Background service for simulating driver movements

**Database Schema:**
```sql
Orders (Id, CustomerId, RestaurantId, Status, CreatedAt, EstimatedDelivery, ActualDelivery)
OrderItems (Id, OrderId, MenuItemId, Quantity, Price)
Drivers (Id, Name, VehicleType, CurrentLat, CurrentLong, Status)
DriverAssignments (Id, OrderId, DriverId, AssignedAt, CompletedAt)
```

### Day 2: Advanced Backend Features
**Implement:**
1. **CQRS Pattern**:
   - Separate commands (CreateOrder, UpdateOrderStatus)
   - Separate queries (GetActiveOrders, GetDriverPerformance)

2. **Event-Driven Architecture**:
   - Publish order events to RabbitMQ
   - Consume events for notifications and analytics

3. **Performance Optimization**:
   - Redis caching for frequently accessed data
   - Database query optimization with indexes
   - Response compression

4. **Testing**:
   - Unit tests with xUnit/NUnit
   - Integration tests with TestContainers
   - Load testing with k6

### Day 3: Angular Frontend
**Build a reactive dashboard with:**
1. **Real-time Components**:
   - Live order list with status indicators
   - Interactive map showing driver locations (use Leaflet/Mapbox)
   - Order statistics and KPIs

2. **State Management**:
   - NgRx or Services with BehaviorSubject
   - SignalR service with reconnection logic
   - Optimistic updates for better UX

3. **Advanced Angular Features**:
   - Lazy-loaded modules
   - Route guards and resolvers
   - Custom directives/pipes
   - Reactive forms with validation

### Day 4: Integration & Optimization
**Implement:**
1. **Real-time Features**:
   - SignalR integration for live updates
   - WebSocket fallback strategies
   - Connection health monitoring

2. **Performance**:
   - Virtual scrolling for order lists
   - Image optimization and lazy loading
   - Bundle size optimization
   - Change detection optimization

3. **User Experience**:
   - Toast notifications for order updates
   - Loading skeletons
   - Error boundaries and retry logic
   - Progressive Web App (PWA) capabilities

### Day 5: DevOps & Production Readiness
**Containerize and Deploy:**
1. **Docker Setup**:
   - Multi-stage builds for .NET and Angular
   - Docker Compose for local development
   - Health checks and readiness probes

2. **Kubernetes Configuration**:
   - Deployments and Services for all components
   - ConfigMaps and Secrets management
   - Horizontal Pod Autoscaling configuration
   - Ingress with SSL termination

3. **Monitoring & Logging**:
   - Structured logging with Serilog
   - Application Insights/OpenTelemetry integration
   - Custom metrics dashboard
   - Log aggregation configuration

## Technical Challenges to Solve

### 1. **Race Condition Prevention**
Implement optimistic concurrency control for order status updates:
```csharp
public async Task<bool> UpdateOrderStatus(int orderId, OrderStatus newStatus)
{
    // Handle concurrent updates
}
```

### 2. **Geospatial Queries**
Optimize driver location queries:
```sql
-- Find nearest available drivers within 5km
SELECT * FROM Drivers 
WHERE Status = 'Available'
AND geography::Point(@Lat, @Long, 4326).STDistance(
    geography::Point(CurrentLat, CurrentLong, 4326)
) < 5000
```

### 3. **Real-time Data Sync**
Handle disconnected clients and data reconciliation:
```typescript
// Angular service to handle SignalR reconnection
reconnectAndSync() {
    // Implement data sync logic
}
```

### 4. **Performance Under Load**
Simulate 1000 concurrent users with:
- Connection pooling
- Database connection management
- Memory usage optimization

## Evaluation Criteria

### Code Quality (30%)
- SOLID principles adherence
- Clean, readable, and maintainable code
- Comprehensive test coverage (>85%)
- Proper error handling and validation

### Technical Excellence (40%)
- Efficient algorithms and data structures
- Optimal database design and queries
- Performance and scalability considerations
- Security best practices implemented

### Architecture (20%)
- Appropriate design patterns
- Loose coupling and high cohesion
- Event-driven design implementation
- Microservices readiness

### DevOps (10%)
- Containerization quality
- Environment configuration
- Monitoring setup
- Deployment automation

## Stretch Goals
1. **Implement WebRTC** for real-time driver-customer communication
2. **Add predictive analytics** for delivery time estimation using ML.NET
3. **Create a mobile app** with React Native sharing the same backend
4. **Implement circuit breaker** pattern for external service calls
5. **Add end-to-end tests** with Cypress/Playwright

## Submission Requirements
Provide a GitHub repository with:

```
📁 order-tracking-system/
├── 📁 src/
│   ├── 📁 OrderTracking.API/ (.NET 8 Web API)
│   ├── 📁 OrderTracking.Domain/
│   ├── 📁 OrderTracking.Application/
│   ├── 📁 OrderTracking.Infrastructure/
│   ├── 📁 OrderTracking.Tests/
│   └── 📁 order-tracking-ui/ (Angular app)
├── 📁 kubernetes/
│   ├── 📁 manifests/ (K8s YAML files)
│   └── helm-chart/ (Helm chart)
├── 📁 scripts/
│   ├── database/ (migration scripts)
│   └── deployment/ (CI/CD scripts)
├── 📁 docs/
│   ├── API.md (Swagger/Postman)
│   └── ARCHITECTURE.md
├── docker-compose.yml
├── Makefile (or build.ps1)
└── README.md (with setup instructions)
```

## Success Metrics
- API handles 1000 concurrent connections
- Real-time updates < 500ms latency
- Dashboard loads in < 3 seconds
- Zero memory leaks in 24-hour stress test
- 99.9% successful WebSocket connections
- Comprehensive logging and monitoring

## Bonus Points For:
- Implementing feature flags for gradual rollout
- Adding dark/light theme toggle
- Creating a comprehensive API documentation
- Implementing rate limiting
- Adding automated performance testing

This exercise tests senior-level skills in system design, performance optimization, and full-stack development while maintaining production-quality standards.
