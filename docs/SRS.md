# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## AgroForecast: Mobile App for Weather Forecasting, Planting, Harvesting, and Crop Guidance

**Document Version:** 1.0  
**Date:** June 2026  
**Status:** APPROVED

---

## 1. INTRODUCTION

### 1.1 Purpose
This document specifies the complete software requirements for the AgroForecast mobile application. It defines functional requirements, non-functional requirements, constraints, and design considerations.

### 1.2 Scope
The AgroForecast application is a comprehensive farming assistance platform that provides:
- Real-time weather forecasting
- Crop-specific recommendations
- Planting and harvesting schedules
- Agricultural guidance and alerts
- User profile management

### 1.3 Target Users
- Small-scale farmers
- Agricultural extension officers
- Farm managers
- Agricultural advisors

---

## 2. SYSTEM OVERVIEW

### 2.1 System Architecture
```
┌─────────────────────────────────────────┐
│     Flutter Mobile App (Frontend)        │
│  - Responsive UI                         │
│  - Offline Support                       │
│  - Dark/Light Mode                       │
└────────────────┬────────────────────────┘
                 │ HTTPS API
                 ↓
┌─────────────────────────────────────────┐
│   Node.js/Express Backend                │
│  - REST API                              │
│  - JWT Authentication                    │
│  - Business Logic                        │
└────────────────┬────────────────────────┘
                 │ Database
                 ↓
┌─────────────────────────────────────────┐
│     MongoDB Database                     │
│  - User Data                             │
│  - Crop Data                             │
│  - Weather Data                          │
│  - Schedules & Notifications             │
└─────────────────────────────────────────┘

External Services:
- OpenWeatherMap API (Weather Data)
- Google Maps API (Location Services)
- Firebase FCM (Push Notifications)
```

### 2.2 Key Technologies
- **Frontend:** Flutter, Riverpod, Material Design 3
- **Backend:** Node.js, Express.js, MongoDB
- **Authentication:** JWT, bcrypt
- **APIs:** OpenWeatherMap, Google Maps, Firebase FCM

---

## 3. FUNCTIONAL REQUIREMENTS

### FR-1: USER MANAGEMENT

#### FR-1.1 User Registration
```
Actor: New User
Precondition: User has device with app installed
Main Flow:
1. User opens app
2. User selects "Sign Up"
3. User enters: Email, Phone, Password, Location
4. System validates inputs
5. System creates user account
6. System sends verification email
7. User verifies email
8. System activates account
Postcondition: User account created and verified
```

**Requirements:**
- Email validation (RFC 5322 standard)
- Password strength check (min 8 chars, alphanumeric)
- Phone number validation
- Location selection via GPS or map
- Email verification link

#### FR-1.2 User Authentication
```
Actor: Registered User
Main Flow:
1. User enters email and password
2. System validates credentials
3. System generates JWT token (valid 24 hours)
4. System returns token to client
5. Client stores token securely
Postcondition: User authenticated and logged in
```

**Requirements:**
- bcrypt password verification (10 rounds)
- JWT token generation (HS256 algorithm)
- Token expiration (24 hours)
- Refresh token mechanism
- Secure token storage on mobile

#### FR-1.3 Profile Management
```
Actor: Logged-in User
Precondition: User authenticated
Main Flow:
1. User accesses profile screen
2. System displays current profile data
3. User can update: Name, Email, Phone, Location
4. User can change password
5. System validates changes
6. System updates database
7. System confirms success
Postcondition: Profile updated
```

#### FR-1.4 User Dashboard
```
Actor: Logged-in User
Main Flow:
1. User opens app
2. System displays dashboard
3. Dashboard shows:
   - Current weather for user location
   - Upcoming planting dates
   - Harvest readiness status
   - Recent notifications
   - Quick action buttons
Postcondition: Dashboard displayed
```

### FR-2: WEATHER FORECAST MODULE

#### FR-2.1 Current Weather
```
Actor: User
Main Flow:
1. User opens Weather screen
2. System gets user location (GPS or saved)
3. System calls OpenWeatherMap API
4. System displays:
   - Current temperature
   - Humidity percentage
   - Wind speed and direction
   - Atmospheric pressure
   - UV index
   - Weather condition (icon + description)
5. System caches data (TTL: 30 minutes)
Postcondition: Current weather displayed
```

**Data Points:**
- Temperature (°C)
- Feels-like temperature
- Humidity (%)
- Wind speed (km/h)
- Wind direction (degrees)
- Pressure (hPa)
- Visibility (m)
- UV Index (0-11+)

#### FR-2.2 Hourly Forecast
```
Actor: User
Main Flow:
1. User opens Weather screen
2. System fetches 24-hour forecast
3. System displays hourly cards:
   - Time
   - Temperature
   - Precipitation probability
   - Weather icon
4. User can scroll horizontally
Postcondition: Hourly forecast displayed
```

#### FR-2.3 7-Day Forecast
```
Actor: User
Main Flow:
1. User navigates to 7-day forecast
2. System displays daily cards:
   - Date and day
   - High/Low temperature
   - Weather condition
   - Precipitation probability
   - Rainfall amount (mm)
3. User can tap for details
Postcondition: 7-day forecast displayed
```

#### FR-2.4 Location-Based Weather
```
Actor: User
Main Flow:
1. App requests location permission
2. User grants permission
3. System uses GPS to detect location
4. System retrieves weather for that location
5. System displays location name (reverse geocoding)
6. User can add multiple locations
7. User can switch between locations
Postcondition: Location-based weather enabled
```

#### FR-2.5 Rainfall Prediction
```
Actor: User
Main Flow:
1. User views weather screen
2. System displays:
   - Hourly rainfall prediction (mm)
   - Total daily rainfall
   - Rainfall probability by hour
3. System shows rainfall timeline
Postcondition: Rainfall data displayed
```

### FR-3: CROP RECOMMENDATION MODULE

#### FR-3.1 Crop Selection
```
Actor: User
Main Flow:
1. User navigates to Crop Recommendations
2. System displays 8 supported crops:
   - Maize, Rice, Tomato, Cassava
   - Yam, Groundnut, Millet, Sorghum
3. Each crop shows:
   - Icon
   - Name
   - Brief description
4. User selects a crop
Postcondition: Crop selected
```

#### FR-3.2 Planting Recommendations
```
Actor: User
Precondition: Crop selected, Location set
Main Flow:
1. System analyzes:
   - Current weather patterns
   - Historical weather data
   - Soil conditions
   - Crop requirements
2. System calculates:
   - Best planting period (date range)
   - Recommended planting date
   - Days until optimal planting
3. System displays:
   - Recommended date
   - Reason for recommendation
   - Required soil conditions
   - Water requirements
   - Expected yield information
Postcondition: Recommendations displayed
```

#### FR-3.3 Crop Information
```
Actor: User
Precondition: Crop selected
Main Flow:
1. System displays crop details:
   - Growth cycle duration (days)
   - Expected maturity duration
   - Optimal temperature range (°C)
   - Water requirements (mm)
   - Soil type preferences
   - Pest common to crop
   - Diseases and prevention
   - Harvesting tips
Postcondition: Crop details displayed
```

#### FR-3.4 Location-Based Recommendations
```
Actor: User
Precondition: Location set
Main Flow:
1. System filters crops by location
2. System shows:
   - Suitable crops for region
   - Success rates (% of farmers)
   - Yield expectations
   - Seasonal availability
3. System ranks crops by suitability
Postcondition: Location-based crops displayed
```

### FR-4: PLANTING CALENDAR MODULE

#### FR-4.1 Calendar Generation
```
Actor: User
Precondition: Crops selected
Main Flow:
1. User selects crops to track
2. System generates monthly calendar
3. Calendar shows:
   - Planting dates (highlighted)
   - Optimal planting windows
   - Harvest dates (estimated)
   - Important events
4. User can view:
   - Monthly view
   - Weekly view
   - Detailed view for each date
Postcondition: Calendar generated and displayed
```

#### FR-4.2 Seasonal Planning
```
Actor: User
Main Flow:
1. System displays seasonal information:
   - Major planting seasons
   - Best crops per season
   - Crop rotation recommendations
   - Inter-cropping opportunities
2. System suggests:
   - Companion planting
   - Fallowing periods
Postcondition: Seasonal guidance displayed
```

#### FR-4.3 Reminders
```
Actor: User
Main Flow:
1. System sets reminders for:
   - Planting dates (7 days before)
   - Harvesting dates (3 days before)
   - Pest monitoring
   - Disease prevention
2. User can customize reminder times
3. System sends push notifications
4. User can confirm task completion
Postcondition: Reminders configured
```

### FR-5: HARVEST PREDICTION MODULE

#### FR-5.1 Harvest Date Calculation
```
Actor: System
Input: Crop type, Planting date, Weather data
Process:
1. Retrieve crop maturity duration
2. Calculate base harvest date = Planting date + Duration
3. Adjust for weather conditions:
   - Temperature deviations
   - Rainfall variations
   - Cold/heat stress
4. Generate date range (±3 days)
Output: Expected harvest date and range
```

**Algorithm:**
```
Base Harvest = Planting Date + Crop Maturity Days
Adjustment Factor = f(Temperature, Rainfall, Humidity)
Final Harvest Date = Base Harvest + Adjustment Factor
Date Range = Final Harvest Date ± 3 days
```

#### FR-5.2 Harvest Readiness
```
Actor: User
Main Flow:
1. System monitors crop maturity:
   - Days since planting
   - Weather condition changes
   - Visual growth indicators
2. System calculates readiness percentage
3. System displays:
   - Maturity percentage
   - Days to harvest
   - Current readiness status (0-25%, 25-50%, 50-75%, 75-100%)
   - Visual progress bar
Postcondition: Readiness status displayed
```

#### FR-5.3 Harvest Reminders
```
Actor: System
Main Flow:
1. System sends notifications when:
   - Harvest date approaches (7 days before)
   - Weather conditions optimal (3 days before)
   - Crop reaches 80% maturity
2. Notifications include:
   - Harvest timing advice
   - Required equipment
   - Market price information (if available)
   - Storage recommendations
Postcondition: Notifications sent
```

### FR-6: AGRICULTURAL GUIDANCE MODULE

#### FR-6.1 Weather-Based Alerts
```
Actor: System
Main Flow:
1. System monitors weather conditions
2. System triggers alerts for:
   - Heavy rainfall (> 50mm/day)
   - Drought conditions (no rain > 14 days)
   - Storm warnings (wind > 60 km/h)
   - Frost warnings (temp < 0°C)
   - Heat warnings (temp > 40°C)
3. System sends push notifications
4. Alerts include guidance:
   - Protective measures
   - Crop protection steps
   - Harvest timing adjustments
Postcondition: Alerts sent and displayed
```

#### FR-6.2 Farming Reminders
```
Actor: System
Main Flow:
1. System generates reminders for:
   - Planting activities
   - Harvesting activities
   - Pest monitoring
   - Disease prevention sprays
   - Fertilizer application
   - Irrigation scheduling
2. Reminders sent via:
   - Push notifications
   - In-app messages
   - Notification center
Postcondition: Reminders configured
```

#### FR-6.3 Seasonal Guidance
```
Actor: System
Main Flow:
1. System provides seasonal tips:
   - Best practices for current season
   - Crop-specific care instructions
   - Recommended fertilizers
   - Pest management strategies
   - Water management
2. Content personalized by:
   - Crop type
   - Location
   - Weather patterns
Postcondition: Guidance displayed
```

---

## 4. NON-FUNCTIONAL REQUIREMENTS

### NFR-1: PERFORMANCE
- API response time: < 1 second (95th percentile)
- Page load time: < 2 seconds
- Forecast update: < 5 seconds
- Support 10,000+ concurrent users
- Database query time: < 500ms
- Image loading: < 1 second

### NFR-2: RELIABILITY
- System uptime: 99.5%
- Automated backups: Every 24 hours
- Mean Time To Recovery (MTTR): < 1 hour
- Data loss rate: < 0.001%

### NFR-3: SECURITY
- JWT authentication with HS256
- bcrypt password hashing (10 rounds)
- HTTPS/TLS 1.2+ for all communications
- Input validation and sanitization
- SQL injection prevention
- XSS protection
- CSRF tokens
- Rate limiting (100 req/min per IP)

### NFR-4: SCALABILITY
- Horizontal scaling support
- Load balancing (round-robin)
- Database replication
- CDN for static assets
- Cache layer (Redis)

### NFR-5: USABILITY
- Intuitive UI following Material Design 3
- Response to user actions: < 100ms
- Accessibility features (WCAG 2.1 AA)
- Dark and Light mode support
- Localization ready

### NFR-6: MAINTAINABILITY
- Code coverage: > 80%
- Unit tests for all functions
- Integration tests for APIs
- Clear code documentation
- Version control with Git
- CI/CD pipeline

### NFR-7: AVAILABILITY
- Offline-first architecture
- Local data caching (Hive)
- Auto-sync when online
- Works without internet (basic features)
- Data sync priority queue

---

## 5. DATA REQUIREMENTS

### Collections/Tables

#### Users Collection
```
{
  _id: ObjectId,
  email: String (unique),
  phone: String,
  passwordHash: String (bcrypt),
  firstName: String,
  lastName: String,
  location: {
    latitude: Number,
    longitude: Number,
    address: String
  },
  role: String (farmer|officer|admin),
  preferences: {
    theme: String (light|dark|auto),
    language: String,
    notificationsEnabled: Boolean
  },
  createdAt: Date,
  updatedAt: Date,
  isVerified: Boolean
}
```

#### Crops Collection
```
{
  _id: ObjectId,
  name: String,
  icon: String (URL),
  description: String,
  maturityDays: Number,
  temperatureRange: { min: Number, max: Number },
  waterRequirements: Number (mm),
  soilType: [String],
  pests: [String],
  diseases: [String],
  harvestingTips: String,
  yield: String,
  createdAt: Date
}
```

#### PlantingSchedules Collection
```
{
  _id: ObjectId,
  userId: ObjectId,
  cropId: ObjectId,
  location: {
    latitude: Number,
    longitude: Number,
    address: String
  },
  plantingDate: Date,
  expectedHarvestDate: Date,
  status: String (planned|in_progress|completed),
  fieldArea: Number (hectares),
  quantityPlanted: String,
  notes: String,
  createdAt: Date,
  updatedAt: Date
}
```

#### WeatherData Collection
```
{
  _id: ObjectId,
  location: {
    latitude: Number,
    longitude: Number
  },
  temperature: Number,
  humidity: Number,
  windSpeed: Number,
  windDirection: Number,
  pressure: Number,
  visibility: Number,
  uvIndex: Number,
  rainfall: Number,
  description: String,
  icon: String,
  timestamp: Date,
  expiresAt: Date (TTL index)
}
```

#### HarvestSchedules Collection
```
{
  _id: ObjectId,
  plantingScheduleId: ObjectId,
  expectedHarvestDate: Date,
  actualHarvestDate: Date,
  readinessPercentage: Number,
  readinessStatus: String,
  yield: Number,
  qualityAssessment: String,
  notes: String,
  createdAt: Date,
  updatedAt: Date
}
```

#### Notifications Collection
```
{
  _id: ObjectId,
  userId: ObjectId,
  type: String (weather|planting|harvest|reminder),
  title: String,
  message: String,
  priority: String (low|medium|high),
  read: Boolean,
  data: Object,
  createdAt: Date,
  expiresAt: Date (TTL index)
}
```

---

## 6. API REQUIREMENTS

### Authentication Endpoints
```
POST /api/auth/register
POST /api/auth/login
POST /api/auth/verify-email
POST /api/auth/refresh-token
POST /api/auth/logout
```

### User Endpoints
```
GET /api/users/profile
PUT /api/users/profile
POST /api/users/change-password
DELETE /api/users/account
```

### Weather Endpoints
```
GET /api/weather/current
GET /api/weather/forecast/hourly
GET /api/weather/forecast/7day
GET /api/weather/alerts
```

### Crop Endpoints
```
GET /api/crops
GET /api/crops/:id
GET /api/crops/recommendations
GET /api/crops/by-location/:location
```

### Planting Schedule Endpoints
```
GET /api/schedules
POST /api/schedules
GET /api/schedules/:id
PUT /api/schedules/:id
DELETE /api/schedules/:id
```

### Harvest Endpoints
```
GET /api/harvest/:scheduleId
PUT /api/harvest/:scheduleId
GET /api/harvest/readiness/:scheduleId
```

### Notification Endpoints
```
GET /api/notifications
GET /api/notifications/:id
PUT /api/notifications/:id/read
DELETE /api/notifications/:id
```

---

## 7. EXTERNAL INTEGRATIONS

### OpenWeatherMap API
- Current weather data
- Hourly forecasts
- 7-day forecasts
- Weather alerts
- Polling interval: 30 minutes

### Google Maps API
- GPS location detection
- Reverse geocoding
- Place searches
- Distance calculations

### Firebase Cloud Messaging (FCM)
- Push notifications
- Topic subscriptions
- Device token management

---

## 8. CONSTRAINTS

1. Mobile app limited to Android and iOS
2. Requires minimum Android 5.0 (API 21)
3. Requires minimum iOS 11.0
4. Weather data subject to OpenWeatherMap availability
5. Location accuracy limited by GPS precision (±5-10m)
6. Internet required for real-time data sync
7. Battery consumption for background services
8. Storage space: minimum 50MB on device

---

## 9. ACCEPTANCE CRITERIA

- ✅ All FR requirements implemented and tested
- ✅ All NFR requirements met
- ✅ 95% API endpoint coverage
- ✅ 80% UI test coverage
- ✅ Security audit passed
- ✅ Performance benchmarks met
- ✅ Documentation complete (50+ pages)
- ✅ User acceptance testing passed
- ✅ Deployment successful

---

**Document Version:** 1.0  
**Last Updated:** June 2026  
**Status:** APPROVED  
**Next Review:** December 2026
