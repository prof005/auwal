# AgroForecast: Mobile App for Weather Forecasting, Planting, Harvesting, and Crop Guidance

## Project Overview

AgroForecast is a comprehensive mobile application designed to assist farmers with real-time weather forecasting, crop-specific planting recommendations, harvesting predictions, and agricultural advisory services. The application leverages modern technologies to help farmers make informed decisions and improve crop productivity.

## Key Features

- 🌤️ **Real-time Weather Forecasting** - Current conditions, hourly, and 7-day forecasts
- 🌾 **Crop Recommendations** - Location-based planting guidance for 8+ crop types
- 📅 **Planting Calendar** - Seasonal schedules and automated reminders
- 🎯 **Harvest Prediction** - Expected harvest dates and readiness indicators
- 🚨 **Agricultural Alerts** - Push notifications for weather events and farming milestones
- 📍 **Location-Based Services** - GPS-powered localized recommendations
- 🌙 **Dark/Light Mode** - User preference support
- 📱 **Offline Support** - Core functionality available offline

## Technology Stack

### Frontend
- **Framework:** Flutter (Latest Version)
- **State Management:** Riverpod
- **Design:** Material Design 3
- **Local Storage:** Hive
- **HTTP Client:** Dio

### Backend
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB
- **Authentication:** JWT
- **Password Security:** bcrypt
- **API Documentation:** Swagger/OpenAPI

### External APIs
- **Weather Data:** OpenWeatherMap API
- **Location Services:** Google Maps API

## Project Structure

```
AgroForecast/
├── frontend/               # Flutter mobile application
│   ├── lib/
│   │   ├── main.dart
│   │   ├── screens/
│   │   ├── widgets/
│   │   ├── providers/
│   │   ├── services/
│   │   ├── models/
│   │   └── config/
│   ├── pubspec.yaml
│   └── test/
├── backend/                # Node.js backend API
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── middleware/
│   │   ├── services/
│   │   └── config/
│   ├── package.json
│   └── tests/
├── docs/                   # Documentation
│   ├── SRS.md
│   ├── API_DOCUMENTATION.md
│   ├── INSTALLATION_GUIDE.md
│   ├── USER_MANUAL.md
│   ├── SYSTEM_ARCHITECTURE.md
│   └── diagrams/
└── deployment/             # Deployment configurations
    ├── docker/
    ├── kubernetes/
    └── ci-cd/
```

## Supported Crops

- Maize
- Rice
- Tomato
- Cassava
- Yam
- Groundnut
- Millet
- Sorghum

## Development Phases

1. **Phase 1:** Requirements Analysis
2. **Phase 2:** System Design
3. **Phase 3:** Database Design
4. **Phase 4:** Backend Development
5. **Phase 5:** Frontend Development
6. **Phase 6:** API Integration
7. **Phase 7:** Testing
8. **Phase 8:** Deployment

## Getting Started

### Prerequisites
- Flutter SDK 3.0+
- Node.js 16+
- MongoDB 5.0+
- Git

### Installation

See [INSTALLATION_GUIDE.md](docs/INSTALLATION_GUIDE.md) for detailed setup instructions.

### Development

```bash
# Clone repository
git clone https://github.com/prof005/auwal.git
cd auwal

# Setup backend
cd backend
npm install
npm run dev

# Setup frontend (in new terminal)
cd frontend
flutter pub get
flutter run
```

## API Documentation

Full API documentation is available in [API_DOCUMENTATION.md](docs/API_DOCUMENTATION.md)

## Security

- JWT-based authentication
- bcrypt password hashing (10 rounds)
- Input validation and sanitization
- Role-based access control
- HTTPS-only API communication
- Secure token storage on mobile

## Testing

Comprehensive test suites for both frontend and backend:

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
flutter test
```

## Deployment

### Android
- Build APK: `flutter build apk`
- Build AAB: `flutter build appbundle`
- Deploy to Google Play Store

### Backend
- Docker containerization
- Cloud deployment ready (AWS/GCP/Azure)
- See [deployment/](deployment/) for configurations

## Documentation

- [Software Requirements Specification](docs/SRS.md)
- [System Architecture](docs/SYSTEM_ARCHITECTURE.md)
- [API Documentation](docs/API_DOCUMENTATION.md)
- [Installation Guide](docs/INSTALLATION_GUIDE.md)
- [User Manual](docs/USER_MANUAL.md)

## Contributing

This project follows standard Git workflow with:
- `main` branch for production-ready code
- `develop` branch for development
- Feature branches for new features
- Pull request reviews before merging

## License

This project is submitted as a Final Year Project.

## Author

Developed as a Final Year Project in Agricultural Technology and Software Engineering

## Support

For issues, questions, or suggestions, please create an issue in the repository.

---

**Last Updated:** June 2026
**Status:** Development Phase
