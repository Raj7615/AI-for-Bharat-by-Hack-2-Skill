# FFHR Requirements

## About FFHR (Farmer's Farm Health Report)

FFHR is an AI-driven farm intelligence system designed to analyze soil and crop health using structured and unstructured data. It follows a modular AI orchestration architecture, where each data source is processed independently and efficiently.

### Core Capabilities

- Multiple inputs such as farmer-provided data, soil parameters, crop images, and external datasets are ingested through dedicated data pipelines
- ETL-based data processing ensures data cleaning, transformation, and consistency before analysis
- AI models are orchestrated across workflows to generate accurate, personalized farm health insights
- Scalable and automated report generation enables reliable predictions and fast delivery of results
- The system is optimized for accessibility, making advanced agricultural intelligence available to small and marginal farmers

## What Makes FFHR Unique?

- Works in low network areas and quite lightweight for the device
- Combines soil, plant, and farm image analysis in a single mobile-based solution
- Works with just a smartphone camera - no expensive tools or lab tests needed
- Includes a DIY NPK testing kit (in development) for instant, on-field soil analysis
- Predicts the amount of NPK levels present in the soil by fusing different API data and offline data provided by the govt. Organization
- Predicts the amount of fertilizers and pesticides needed, preventing overuse and reducing costs

## The Solution (FFHR)

FFHR is a smart, AI-driven farm advisory system that enables small and marginal farmers to monitor soil health, optimize crop growth, and make informed decisions.

### Key Features

#### Crop Health Monitoring
- Detects crop stress, disease symptoms, and nutrient deficiencies
- Uses field data to assess overall crop vitality
- Early warning system to prevent yield loss

#### Weather Impact Assessment
- Integrates local weather data (rainfall, temperature, humidity)
- Predicts weather-related risks like drought, heat stress, or frost
- Helps farmers plan irrigation and crop protection

#### Pest & Disease Risk Prediction
- Predicts likelihood of pest attacks and crop diseases
- Uses historical patterns and environmental conditions
- Enables preventive measures instead of reactive treatment

#### Personalized Farm Health Report
- Generates an easy-to-understand Farm Health Score
- Provides crop-wise and field-wise insights
- Visual summaries for farmers with low digital literacy

### Data Processing

- The system analyzes soil and crop conditions using AI models and structured data pipelines to assess crop suitability and potential risks
- Multiple data sources such as farmer inputs, soil parameters, crop images, satellite-derived insights, weather patterns, and historical crop data are combined to generate accurate recommendations
- Personalized, farmer-friendly farm health reports are generated in regional languages, providing clear guidance on fertilizer usage, pesticide application, crop rotation, and preventive measures against pests and diseases
- A modular data fusion approach allows FFHR to progressively integrate soil sensors, satellite imagery (ISRO Bhuvan, Sentinel2), and weather datasets (NASA POWER) as the platform scales

## Functional Requirements

### FR1: User Management & Authentication
- **FR1.1**: System shall support farmer registration with mobile number and basic profile information
- **FR1.2**: System shall authenticate users using OTP-based login for security
- **FR1.3**: System shall maintain farmer profiles including name, location, farm details, and language preference
- **FR1.4**: System shall support multiple farms per farmer account
- **FR1.5**: System shall allow farmers to update their profile and farm information

### FR2: Data Collection & Input
- **FR2.1**: System shall accept farmer inputs including crop type, sowing date, irrigation method, and farming practices
- **FR2.2**: System shall capture and upload crop images from smartphone camera
- **FR2.3**: System shall capture and upload soil images from smartphone camera
- **FR2.4**: System shall accept manual soil parameter inputs (pH, moisture, texture)
- **FR2.5**: System shall validate all input data for completeness and correctness
- **FR2.6**: System shall support batch upload of multiple images
- **FR2.7**: System shall extract GPS coordinates and timestamp from uploaded images

### FR3: Image Processing & Analysis
- **FR3.1**: System shall preprocess images to enhance quality (brightness, contrast, noise reduction)
- **FR3.2**: System shall detect crop diseases from leaf images using CNN models
- **FR3.3**: System shall identify pest damage from crop images
- **FR3.4**: System shall assess crop health status (healthy, stressed, diseased)
- **FR3.5**: System shall analyze soil color and texture from soil images
- **FR3.6**: System shall detect nutrient deficiency symptoms from crop images
- **FR3.7**: System shall provide confidence scores for all image-based predictions

### FR4: NPK Prediction & Soil Analysis
- **FR4.1**: System shall predict nitrogen (N) levels in soil using data fusion
- **FR4.2**: System shall predict phosphorus (P) levels in soil using data fusion
- **FR4.3**: System shall predict potassium (K) levels in soil using data fusion
- **FR4.4**: System shall integrate government soil survey data for the farmer's region
- **FR4.5**: System shall integrate weather data to adjust NPK predictions
- **FR4.6**: System shall accept DIY NPK test kit results and calibrate predictions
- **FR4.7**: System shall provide NPK level categories (deficient, adequate, excess)
- **FR4.8**: System shall track historical NPK trends for each farm

### FR5: Weather Integration
- **FR5.1**: System shall fetch current weather data for farmer's location
- **FR5.2**: System shall fetch 7-day weather forecast
- **FR5.3**: System shall analyze rainfall patterns and predict irrigation needs
- **FR5.4**: System shall predict weather-related crop risks (drought, frost, heat stress)
- **FR5.5**: System shall provide weather-based farming advisories

### FR6: Fertilizer & Pesticide Recommendations
- **FR6.1**: System shall calculate optimal fertilizer quantity based on NPK predictions
- **FR6.2**: System shall recommend specific fertilizer types (urea, DAP, MOP, etc.)
- **FR6.3**: System shall provide fertilizer application schedule
- **FR6.4**: System shall recommend pesticides based on detected diseases and pests
- **FR6.5**: System shall calculate pesticide dosage and application method
- **FR6.6**: System shall prioritize organic and eco-friendly alternatives
- **FR6.7**: System shall estimate cost of recommended inputs

### FR7: Farm Health Scoring
- **FR7.1**: System shall calculate overall Farm Health Score (0-100)
- **FR7.2**: System shall calculate Soil Health Score based on NPK levels and soil parameters
- **FR7.3**: System shall calculate Crop Health Score based on image analysis
- **FR7.4**: System shall calculate Risk Score based on pest, disease, and weather factors
- **FR7.5**: System shall provide score interpretation and actionable insights

### FR8: Report Generation
- **FR8.1**: System shall generate comprehensive PDF farm health reports
- **FR8.2**: System shall include visual charts and graphs in reports
- **FR8.3**: System shall generate reports in regional languages (Hindi, Telugu, Tamil, Kannada, Marathi, Bengali, Punjabi)
- **FR8.4**: System shall include crop-wise analysis in reports
- **FR8.5**: System shall include field-wise analysis for multi-field farms
- **FR8.6**: System shall provide actionable recommendations in simple language
- **FR8.7**: System shall include cost-benefit analysis of recommendations
- **FR8.8**: System shall optimize report size for mobile viewing and sharing

### FR9: Offline Functionality
- **FR9.1**: System shall detect network connectivity status
- **FR9.2**: System shall cache government data for offline access
- **FR9.3**: System shall store user data locally when offline
- **FR9.4**: System shall perform basic analysis using cached models when offline
- **FR9.5**: System shall sync data automatically when network is restored
- **FR9.6**: System shall queue report generation requests for offline processing

### FR10: Notifications & Alerts
- **FR10.1**: System shall send notifications when reports are ready
- **FR10.2**: System shall send weather alerts for extreme conditions
- **FR10.3**: System shall send pest outbreak alerts based on regional data
- **FR10.4**: System shall send reminders for fertilizer application
- **FR10.5**: System shall send seasonal farming tips and advisories

## Non-Functional Requirements

### NFR1: Performance
- **NFR1.1**: Image upload shall complete within 30 seconds on 2G network
- **NFR1.2**: Image analysis shall complete within 2 minutes
- **NFR1.3**: Report generation shall complete within 5 minutes
- **NFR1.4**: API response time shall be < 3 seconds for 95% of requests
- **NFR1.5**: System shall support 10,000 concurrent users
- **NFR1.6**: Mobile app shall launch within 3 seconds

### NFR2: Scalability
- **NFR2.1**: System shall scale horizontally to handle increased load
- **NFR2.2**: Database shall handle 1 million farmer records
- **NFR2.3**: Storage shall accommodate 10 million images
- **NFR2.4**: System shall process 100,000 analysis requests per day

### NFR3: Availability & Reliability
- **NFR3.1**: System shall maintain 99.5% uptime
- **NFR3.2**: System shall have automated backup every 24 hours
- **NFR3.3**: System shall recover from failures within 15 minutes
- **NFR3.4**: Data shall be replicated across multiple availability zones

### NFR4: Security
- **NFR4.1**: All API communications shall use HTTPS/TLS encryption
- **NFR4.2**: User passwords shall be hashed using bcrypt
- **NFR4.3**: Farmer data shall be encrypted at rest
- **NFR4.4**: System shall implement rate limiting to prevent abuse
- **NFR4.5**: System shall log all security events
- **NFR4.6**: System shall comply with data privacy regulations

### NFR5: Usability
- **NFR5.1**: Mobile app shall support farmers with low digital literacy
- **NFR5.2**: UI shall use visual icons and minimal text
- **NFR5.3**: App shall support voice input for text fields
- **NFR5.4**: App shall provide tutorial/onboarding for first-time users
- **NFR5.5**: Reports shall use simple language avoiding technical jargon

### NFR6: Compatibility
- **NFR6.1**: Mobile app shall support Android 8.0 and above
- **NFR6.2**: Mobile app shall support iOS 12.0 and above
- **NFR6.3**: App shall work on devices with minimum 2GB RAM
- **NFR6.4**: App shall work on screen sizes from 4.5" to 7"
- **NFR6.5**: Backend shall support multiple API versions for backward compatibility

### NFR7: Maintainability
- **NFR7.1**: Code shall follow PEP 8 style guide for Python
- **NFR7.2**: All functions shall have docstrings
- **NFR7.3**: System shall have comprehensive logging
- **NFR7.4**: System shall have monitoring dashboards
- **NFR7.5**: AI models shall be versioned and deployable independently

### NFR8: Localization
- **NFR8.1**: System shall support 7+ Indian regional languages
- **NFR8.2**: Language switching shall not require app restart
- **NFR8.3**: All user-facing text shall be externalized for translation
- **NFR8.4**: Date, time, and number formats shall follow regional conventions

### NFR9: Accuracy
- **NFR9.1**: Crop disease detection shall have >85% accuracy
- **NFR9.2**: NPK prediction shall have <15% error margin
- **NFR9.3**: Pest detection shall have >80% accuracy
- **NFR9.4**: Weather predictions shall use reliable data sources

### NFR10: Cost Optimization
- **NFR10.1**: System shall use cost-effective AWS services
- **NFR10.2**: Images shall be compressed before storage
- **NFR10.3**: System shall use caching to reduce API calls
- **NFR10.4**: System shall use spot instances for batch processing

## User Stories

### Farmer Personas

**Persona 1: Raju - Small Farmer**
- 35 years old, 2 acres of land
- Grows rice and vegetables
- Basic smartphone user
- Speaks only Telugu
- Limited internet connectivity

**Persona 2: Lakshmi - Progressive Farmer**
- 42 years old, 5 acres of land
- Grows multiple crops
- Comfortable with technology
- Speaks Hindi and English
- Good internet access

### User Stories

**US1**: As Raju, I want to take a photo of my crop and get disease diagnosis, so that I can treat it before it spreads.

**US2**: As Lakshmi, I want to know the exact NPK levels in my soil, so that I can apply the right amount of fertilizer and save money.

**US3**: As Raju, I want to receive reports in Telugu with pictures, so that I can understand the recommendations easily.

**US4**: As Lakshmi, I want to track my farm health over multiple seasons, so that I can see improvements and plan better.

**US5**: As Raju, I want the app to work without internet, so that I can use it in my remote village.

**US6**: As Lakshmi, I want to get weather-based alerts, so that I can protect my crops from unexpected weather changes.

**US7**: As Raju, I want to know how much fertilizer to buy, so that I don't waste money on excess fertilizer.

**US8**: As Lakshmi, I want to compare different fields on my farm, so that I can manage them differently based on their needs.

**US9**: As Raju, I want simple yes/no recommendations, so that I can take action without confusion.

**US10**: As Lakshmi, I want to share my farm report with agricultural officers, so that I can get additional support.

## Constraints & Assumptions

### Constraints
- Must work on low-end Android devices (2GB RAM)
- Must function with intermittent 2G/3G connectivity
- Must keep mobile app size under 50MB
- Must use only smartphone camera (no external sensors initially)
- Must comply with Indian data protection laws
- Budget constraints for AWS infrastructure

### Assumptions
- Farmers have access to smartphones with cameras
- Farmers can take reasonably clear photos of crops and soil
- Government soil survey data is available and accessible
- Weather API data is reliable and available
- Farmers are willing to input basic farm information
- Regional language translations are accurate
- DIY NPK test kits will be available in future phases

## Technology Stack & Libraries

### Backend Libraries (Python)

#### Web Framework & API
- **fastapi** (>=0.95.2) - Modern, fast web framework for building APIs
- **uvicorn[standard]** (>=0.22.0) - ASGI server for running FastAPI applications
- **pydantic** (>=2.0.0) - Data validation using Python type annotations
- **pydantic-settings** (>=2.0.0) - Settings management using Pydantic
- **python-multipart** (>=0.0.6) - Multipart form data parsing for file uploads

#### Database & ORM
- **SQLAlchemy** (>=2.0.0) - SQL toolkit and ORM for PostgreSQL
- **asyncpg** (>=0.27.0) - Async PostgreSQL driver for Python
- **motor** (>=3.1.0) - Async MongoDB driver for Python
- **databases** (>=0.6.1) - Async database support for SQLAlchemy
- **alembic** (>=1.10.0) - Database migration tool for SQLAlchemy
- **psycopg2-binary** (>=2.9.0) - PostgreSQL adapter for Python

#### AI/ML & Computer Vision
- **tensorflow** (>=2.12.0) - Deep learning framework for CNN models
- **torch** (>=2.0.0) - PyTorch deep learning framework
- **torchvision** (>=0.15.0) - Computer vision models and utilities for PyTorch
- **opencv-python** (>=4.7.0) - Computer vision library for image processing
- **opencv-contrib-python** (>=4.7.0) - Additional OpenCV modules
- **Pillow** (>=9.5.0) - Python Imaging Library for image manipulation
- **scikit-learn** (>=1.2.0) - Machine learning library for NPK prediction
- **xgboost** (>=1.7.0) - Gradient boosting library for predictions
- **lightgbm** (>=3.3.0) - Light Gradient Boosting Machine for efficient ML
- **numpy** (>=1.24.0) - Numerical computing library
- **scipy** (>=1.10.0) - Scientific computing library
- **pandas** (>=2.0.0) - Data manipulation and analysis library
- **imageio** (>=2.28.0) - Library for reading and writing image data

#### Report Generation
- **reportlab** (>=4.1.1) - PDF generation library
- **matplotlib** (>=3.7.0) - Plotting library for charts and graphs
- **plotly** (>=5.14.0) - Interactive graphing library
- **jinja2** (>=3.1.0) - Template engine for report generation

#### External API Integration
- **httpx** (>=0.24.0) - Async HTTP client
- **aiohttp** (>=3.8.0) - Async HTTP client/server framework
- **requests** (>=2.28.0) - HTTP library for Python
- **boto3** (>=1.26.0) - AWS SDK for Python (S3, DynamoDB, etc.)
- **botocore** (>=1.29.0) - Low-level AWS service access

#### Caching & Performance
- **redis** (>=4.5.0) - Redis client for Python
- **aioredis** (>=2.0.0) - Async Redis client
- **celery** (>=5.2.0) - Distributed task queue for async processing
- **kombu** (>=5.2.0) - Messaging library for Celery

#### Security & Authentication
- **passlib[bcrypt]** (>=1.7.4) - Password hashing library
- **python-jose[cryptography]** (>=3.3.0) - JWT token implementation
- **cryptography** (>=40.0.0) - Cryptographic recipes and primitives
- **bcrypt** (>=4.0.0) - Password hashing function

#### Configuration & Environment
- **python-dotenv** (>=1.0.1) - Environment variable management
- **pyyaml** (>=6.0) - YAML parser for configuration files

#### Monitoring, Logging & Observability
- **structlog** (>=23.1.0) - Structured logging
- **prometheus-client** (>=0.16.0) - Prometheus metrics client
- **sentry-sdk** (>=1.25.0) - Error tracking and monitoring
- **python-json-logger** (>=2.0.0) - JSON log formatter

#### Testing Framework
- **pytest** (>=7.3.0) - Testing framework
- **pytest-asyncio** (>=0.21.0) - Async test support for pytest
- **pytest-cov** (>=4.0.0) - Coverage plugin for pytest
- **pytest-mock** (>=3.10.0) - Mock/stub support for pytest
- **hypothesis** (>=6.75.0) - Property-based testing library
- **factory-boy** (>=3.2.0) - Test fixtures replacement
- **faker** (>=18.9.0) - Fake data generation for testing
- **locust** (>=2.15.0) - Load testing framework

#### Internationalization
- **babel** (>=2.12.0) - Internationalization library
- **python-i18n** (>=0.3.9) - Translation library

#### Geospatial Processing
- **geopy** (>=2.3.0) - Geocoding library
- **shapely** (>=2.0.0) - Geometric objects manipulation
- **pyproj** (>=3.5.0) - Cartographic projections and coordinate transformations

#### Data Validation & Serialization
- **marshmallow** (>=3.19.0) - Object serialization/deserialization
- **jsonschema** (>=4.17.0) - JSON schema validation

#### Utilities
- **python-dateutil** (>=2.8.0) - Date/time utilities
- **pytz** (>=2023.3) - Timezone definitions
- **click** (>=8.1.0) - Command-line interface creation
- **tqdm** (>=4.65.0) - Progress bar library

#### Development Tools
- **black** (>=23.3.0) - Code formatter
- **isort** (>=5.12.0) - Import statement organizer
- **flake8** (>=6.0.0) - Code linter
- **mypy** (>=1.3.0) - Static type checker
- **pylint** (>=2.17.0) - Code analysis tool
- **pre-commit** (>=3.3.0) - Git hook scripts
- **ipython** (>=8.12.0) - Enhanced interactive Python shell
- **jupyter** (>=1.0.0) - Notebook for data exploration

### Frontend Libraries (React Native)

#### Core Framework
- **react** (>=18.2.0) - JavaScript library for building user interfaces
- **react-native** (>=0.72.0) - Framework for building native mobile apps

#### Navigation
- **@react-navigation/native** (>=6.1.0) - Routing and navigation
- **@react-navigation/stack** (>=6.3.0) - Stack navigator
- **@react-navigation/bottom-tabs** (>=6.5.0) - Bottom tab navigator
- **react-native-screens** (>=3.20.0) - Native navigation primitives
- **react-native-safe-area-context** (>=4.5.0) - Safe area handling

#### State Management
- **redux** (>=4.2.0) - Predictable state container
- **react-redux** (>=8.0.0) - React bindings for Redux
- **@reduxjs/toolkit** (>=1.9.0) - Redux utilities
- **redux-persist** (>=6.0.0) - Persist Redux state
- **redux-thunk** (>=2.4.0) - Async action support

#### API & Networking
- **axios** (>=1.4.0) - HTTP client
- **react-query** (>=3.39.0) - Data fetching and caching

#### Camera & Media
- **react-native-camera** (>=4.2.0) - Camera component
- **react-native-image-picker** (>=5.4.0) - Image selection
- **react-native-image-crop-picker** (>=0.38.0) - Image cropping
- **react-native-fast-image** (>=8.6.0) - Performant image component

#### Storage
- **@react-native-async-storage/async-storage** (>=1.18.0) - Async storage
- **react-native-sqlite-storage** (>=6.0.0) - SQLite database
- **realm** (>=11.10.0) - Mobile database (alternative)

#### UI Components
- **react-native-paper** (>=5.8.0) - Material Design components
- **react-native-vector-icons** (>=9.2.0) - Icon library
- **react-native-elements** (>=3.4.0) - Cross-platform UI toolkit
- **react-native-gesture-handler** (>=2.12.0) - Gesture handling

#### Forms & Validation
- **formik** (>=2.4.0) - Form management
- **yup** (>=1.2.0) - Schema validation

#### Internationalization
- **react-i18next** (>=12.3.0) - Internationalization framework
- **i18next** (>=22.5.0) - i18n framework

#### Utilities
- **lodash** (>=4.17.0) - Utility library
- **moment** (>=2.29.0) - Date/time manipulation
- **date-fns** (>=2.30.0) - Modern date utility library

#### File Handling
- **react-native-fs** (>=2.20.0) - File system access
- **react-native-pdf** (>=6.7.0) - PDF viewer
- **react-native-share** (>=9.0.0) - Share functionality

#### Notifications
- **@react-native-firebase/messaging** (>=18.0.0) - Push notifications
- **react-native-push-notification** (>=8.1.0) - Local notifications

#### Location & Maps
- **react-native-maps** (>=1.7.0) - Map component
- **@react-native-community/geolocation** (>=3.0.0) - Geolocation API

#### Performance & Monitoring
- **@sentry/react-native** (>=5.5.0) - Error tracking
- **react-native-performance** (>=5.1.0) - Performance monitoring

#### Development Tools
- **@react-native-community/eslint-config** (>=3.2.0) - ESLint config
- **prettier** (>=2.8.0) - Code formatter
- **typescript** (>=5.0.0) - TypeScript support
- **@types/react** (>=18.2.0) - TypeScript types for React
- **@types/react-native** (>=0.72.0) - TypeScript types for React Native

### Cloud & Infrastructure

#### AWS Services
- **AWS EC2** - Compute instances for backend hosting
- **AWS Lambda** - Serverless compute for batch processing
- **AWS S3** - Object storage for images and reports
- **AWS RDS** - Managed PostgreSQL database
- **AWS DynamoDB** - NoSQL database for real-time data
- **AWS ElastiCache** - Managed Redis for caching
- **AWS CloudFront** - CDN for content delivery
- **AWS Route 53** - DNS management
- **AWS CloudWatch** - Monitoring and logging
- **AWS SNS** - Push notification service
- **AWS SES** - Email service
- **AWS ECR** - Docker container registry
- **AWS ECS/EKS** - Container orchestration (optional)

#### External APIs
- **NASA POWER API** - Weather data
- **ISRO Bhuvan API** - Satellite imagery
- **Sentinel Hub API** - Sentinel-2 satellite data
- **Google Earth Engine API** - Geospatial analysis
- **Twilio API** - SMS for OTP (optional)
- **Firebase Cloud Messaging** - Push notifications

### DevOps & CI/CD

#### Containerization
- **Docker** (>=20.10.0) - Container platform
- **docker-compose** (>=2.17.0) - Multi-container orchestration

#### CI/CD
- **GitHub Actions** - Continuous integration and deployment
- **Jenkins** (optional) - Automation server

#### Monitoring & Logging
- **Prometheus** - Metrics collection
- **Grafana** - Metrics visualization
- **ELK Stack** (Elasticsearch, Logstash, Kibana) - Log management (optional)
- **Sentry** - Error tracking

#### Testing & Quality
- **SonarQube** - Code quality analysis (optional)
- **JMeter** - Performance testing (optional)

### Version Control & Collaboration
- **Git** - Version control system
- **GitHub** - Code repository and collaboration
- **GitHub Projects** - Project management

## Library Selection Rationale

### Why FastAPI?
- High performance (comparable to NodeJS and Go)
- Automatic API documentation (Swagger/OpenAPI)
- Built-in data validation with Pydantic
- Async support for better concurrency
- Easy to learn and use

### Why React Native?
- Cross-platform (Android & iOS) with single codebase
- Large community and ecosystem
- Native performance
- Hot reloading for faster development
- Reusable components

### Why TensorFlow & PyTorch?
- Industry-standard deep learning frameworks
- Pre-trained models available (ResNet, YOLO)
- TensorFlow Lite for mobile deployment
- Strong community support
- Comprehensive documentation

### Why PostgreSQL?
- ACID compliance for data integrity
- Advanced features (JSON support, full-text search)
- Excellent performance for complex queries
- Strong geospatial support (PostGIS)
- Open source and free

### Why DynamoDB?
- Serverless and fully managed
- Low latency for real-time data
- Automatic scaling
- Pay-per-use pricing
- Seamless AWS integration

### Why Redis?
- Extremely fast in-memory operations
- Support for various data structures
- Built-in pub/sub for real-time features
- Persistence options available
- Widely used and battle-tested

### Why ReportLab?
- Powerful PDF generation capabilities
- Programmatic control over layout
- Support for charts and images
- Multi-language support
- Open source and free
