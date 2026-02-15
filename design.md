# FFHR Design Document

## Architecture Overview

FFHR follows a modular AI orchestration architecture, where each data source is processed independently and efficiently through dedicated data pipelines.

### System Components

1. **Frontend Layer** - Mobile application for farmer interaction
2. **API Layer** - REST API for data ingestion and report delivery
3. **AI/ML Processing Layer** - Model orchestration and inference
4. **Data Pipeline Layer** - ETL processing and data fusion
5. **Storage Layer** - Structured and unstructured data management
6. **Report Generation Layer** - Automated PDF generation

## Technology Stack

### Frontend
- **React Native** – Cross-platform mobile application (Android & iOS)
- **JavaScript / TypeScript** – Application logic and state management
- **JSX** – Responsive and interactive user interface components

### Backend & Processing
- **Python** – Core server-side logic
- **FastAPI** – REST API framework
- **AWS EC2 / AWS Lambda** – Backend hosting and execution

### Database & Storage
- **Amazon S3** – 
  - Storage of crop & soil images
  - Storage of generated PDF farm health reports
- **Amazon RDS (PostgreSQL / MySQL)** – 
  - Structured data (farmer profiles, farm details, recommendations)
- **Amazon DynamoDB** – 
  - Real-time soil data
  - AI prediction results
  - Application metadata

### Report Generation
- **ReportLab (Python)** – Automated PDF Farm Health Report generation
- **Amazon S3** – Secure storage and access of generated reports

### AI/ML
- **CNN (Convolutional Neural Networks)** - Crop disease detection from images
- **TensorFlow / PyTorch** - Deep learning frameworks for training models
- **LightGBM / XGBoost** - Efficient models for crop/fertilizer predictions
- **OpenCV** - Image preprocessing and augmentation
- **Google Earth Engine API** - Optional satellite-based soil and crop analysis

## Data Processing Architecture

### ETL Pipeline
- Data cleaning and validation
- Transformation and normalization
- Consistency checks before analysis

### Data Fusion Approach
- Farmer-provided data
- Soil parameters
- Crop images
- Satellite-derived insights (ISRO Bhuvan, Sentinel2)
- Weather patterns (NASA POWER)
- Historical crop data
- Government offline data

### AI Model Orchestration
- Independent processing of each data source
- Workflow-based model execution
- Result aggregation and scoring

## Key Design Principles

- **Lightweight & Offline-First** - Works in low network areas
- **Accessibility** - Smartphone camera-based, no expensive equipment
- **Scalability** - Modular architecture for progressive feature integration
- **Localization** - Regional language support for farmer-friendly reports
- **Cost Optimization** - Prevents overuse of fertilizers and pesticides

## Detailed System Architecture

### Microservices Architecture

The FFHR system is built using a microservices architecture with the following services:

1. **API Gateway Service**
   - Request routing and load balancing
   - Authentication and authorization
   - Rate limiting and throttling
   - API versioning support

2. **User Management Service**
   - Farmer registration and profile management
   - OTP-based authentication
   - Session management
   - Multi-farm support per user

3. **Image Processing Service**
   - Image upload and storage
   - Image preprocessing (resize, enhance, normalize)
   - Metadata extraction (GPS, timestamp)
   - Image quality validation

4. **AI Inference Service**
   - Crop disease detection using CNN
   - Pest identification
   - Nutrient deficiency detection
   - Model versioning and A/B testing

5. **NPK Prediction Service**
   - Data fusion from multiple sources
   - NPK level prediction
   - DIY test kit integration
   - Historical trend analysis

6. **Weather Service**
   - Weather data fetching from NASA POWER API
   - Weather forecast processing
   - Risk prediction (drought, frost, heat)
   - Irrigation recommendations

7. **Advisory Engine Service**
   - Fertilizer recommendation calculation
   - Pesticide recommendation
   - Crop rotation suggestions
   - Cost-benefit analysis
   - Cost estimation for fertilizers and pesticides
   - Organic alternative suggestions
   - Application schedule generation

8. **Report Generation Service**
   - PDF report generation using ReportLab
   - Multi-language support
   - Chart and graph generation
   - Report optimization for mobile

9. **Notification Service**
   - Push notifications
   - SMS alerts
   - Email notifications
   - Alert scheduling
   - Weather alerts for extreme conditions
   - Pest outbreak alerts based on regional data
   - Fertilizer application reminders
   - Seasonal farming tips delivery

10. **Data Sync Service**
    - Offline data synchronization
    - Conflict resolution
    - Queue management
    - Retry logic

### Data Flow Architecture

```
Mobile App → API Gateway → Authentication
                ↓
        Request Router
                ↓
    ┌───────────┴───────────┐
    ↓                       ↓
Image Upload          Farmer Input
    ↓                       ↓
Image Processing      Data Validation
    ↓                       ↓
AI Inference          NPK Prediction
    ↓                       ↓
    └───────────┬───────────┘
                ↓
        Advisory Engine
                ↓
        Report Generation
                ↓
        S3 Storage
                ↓
        Notification
                ↓
        Mobile App
```

## Database Design

### PostgreSQL Schema (Structured Data)

**farmers table**
```sql
CREATE TABLE farmers (
    farmer_id UUID PRIMARY KEY,
    phone_number VARCHAR(15) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    language_preference VARCHAR(10) DEFAULT 'en',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

**farms table**
```sql
CREATE TABLE farms (
    farm_id UUID PRIMARY KEY,
    farmer_id UUID REFERENCES farmers(farmer_id),
    farm_name VARCHAR(100),
    location GEOGRAPHY(POINT),
    farm_size_acres DECIMAL(10,2),
    soil_type VARCHAR(50),
    irrigation_type VARCHAR(50),
    created_at TIMESTAMP DEFAULT NOW()
);
```

**crops table**
```sql
CREATE TABLE crops (
    crop_id UUID PRIMARY KEY,
    farm_id UUID REFERENCES farms(farm_id),
    crop_type VARCHAR(50) NOT NULL,
    sowing_date DATE,
    expected_harvest_date DATE,
    status VARCHAR(20) DEFAULT 'active'
);
```

**analysis_requests table**
```sql
CREATE TABLE analysis_requests (
    request_id UUID PRIMARY KEY,
    farm_id UUID REFERENCES farms(farm_id),
    request_type VARCHAR(50),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT NOW(),
    completed_at TIMESTAMP
);
```

**reports table**
```sql
CREATE TABLE reports (
    report_id UUID PRIMARY KEY,
    request_id UUID REFERENCES analysis_requests(request_id),
    report_url VARCHAR(500),
    language VARCHAR(10),
    farm_health_score DECIMAL(5,2),
    generated_at TIMESTAMP DEFAULT NOW()
);
```

### DynamoDB Schema (Real-time Data)

**ImageAnalysisResults**
```json
{
    "analysis_id": "string (partition key)",
    "timestamp": "number (sort key)",
    "farm_id": "string",
    "image_url": "string",
    "disease_detected": "string",
    "confidence_score": "number",
    "pest_detected": "string",
    "health_status": "string",
    "ttl": "number"
}
```

**NPKPredictions**
```json
{
    "prediction_id": "string (partition key)",
    "farm_id": "string (GSI partition key)",
    "timestamp": "number (GSI sort key)",
    "nitrogen_level": "number",
    "phosphorus_level": "number",
    "potassium_level": "number",
    "confidence": "number",
    "data_sources": ["array"],
    "ttl": "number"
}
```

**WeatherData**
```json
{
    "location_id": "string (partition key)",
    "timestamp": "number (sort key)",
    "temperature": "number",
    "humidity": "number",
    "rainfall": "number",
    "forecast": ["array"],
    "ttl": "number"
}
```

## API Design

### REST API Endpoints

**Authentication**
- `POST /api/v1/auth/register` - Register new farmer
- `POST /api/v1/auth/login` - Send OTP
- `POST /api/v1/auth/verify` - Verify OTP and login

**Farmer Management**
- `GET /api/v1/farmers/{farmer_id}` - Get farmer profile
- `PUT /api/v1/farmers/{farmer_id}` - Update farmer profile
- `GET /api/v1/farmers/{farmer_id}/farms` - List all farms

**Farm Management**
- `POST /api/v1/farms` - Create new farm
- `GET /api/v1/farms/{farm_id}` - Get farm details
- `PUT /api/v1/farms/{farm_id}` - Update farm details
- `DELETE /api/v1/farms/{farm_id}` - Delete farm

**Image Upload**
- `POST /api/v1/images/upload` - Upload crop/soil image
- `GET /api/v1/images/{image_id}` - Get image details
- `DELETE /api/v1/images/{image_id}` - Delete image

**Analysis**
- `POST /api/v1/analysis/request` - Request farm analysis
- `GET /api/v1/analysis/{request_id}` - Get analysis status
- `GET /api/v1/analysis/{request_id}/results` - Get analysis results

**Reports**
- `GET /api/v1/reports/{report_id}` - Download report
- `GET /api/v1/reports/farm/{farm_id}` - List all reports for farm
- `POST /api/v1/reports/{report_id}/share` - Share report

**NPK Prediction**
- `POST /api/v1/npk/predict` - Get NPK prediction
- `POST /api/v1/npk/diy-test` - Submit DIY test results
- `GET /api/v1/npk/history/{farm_id}` - Get NPK history

**Weather**
- `GET /api/v1/weather/current/{location}` - Get current weather
- `GET /api/v1/weather/forecast/{location}` - Get weather forecast

**Recommendations**
- `GET /api/v1/recommendations/{farm_id}` - Get farming recommendations
- `GET /api/v1/recommendations/fertilizer/{farm_id}` - Get fertilizer recommendations
- `GET /api/v1/recommendations/pesticide/{farm_id}` - Get pesticide recommendations
- `GET /api/v1/recommendations/cost-estimate/{farm_id}` - Get cost estimate for recommendations

**Notifications**
- `GET /api/v1/notifications/{farmer_id}` - Get all notifications
- `POST /api/v1/notifications/subscribe` - Subscribe to push notifications
- `PUT /api/v1/notifications/{notification_id}/read` - Mark notification as read
- `GET /api/v1/notifications/alerts/weather` - Get weather alerts
- `GET /api/v1/notifications/alerts/pest` - Get pest outbreak alerts

### API Request/Response Examples

**Request Farm Analysis**
```json
POST /api/v1/analysis/request
{
    "farm_id": "uuid",
    "analysis_type": "comprehensive",
    "images": [
        {
            "type": "crop",
            "image_id": "uuid"
        },
        {
            "type": "soil",
            "image_id": "uuid"
        }
    ],
    "farmer_inputs": {
        "crop_type": "rice",
        "sowing_date": "2024-06-15",
        "irrigation_method": "drip",
        "soil_ph": 6.5
    }
}
```

**Response**
```json
{
    "request_id": "uuid",
    "status": "processing",
    "estimated_completion": "2024-07-01T10:30:00Z",
    "message": "Analysis request submitted successfully"
}
```

## AI/ML Model Architecture

### Crop Disease Detection Model

**Architecture**: ResNet50-based CNN
- Input: 224x224 RGB images
- Output: Disease class + confidence score
- Classes: 20+ common crop diseases
- Training data: 50,000+ labeled images
- Accuracy target: >85%

**Model Pipeline**:
1. Image preprocessing (resize, normalize)
2. Feature extraction using ResNet50
3. Classification head (fully connected layers)
4. Softmax activation for probability distribution
5. Post-processing for confidence thresholding

### NPK Prediction Model

**Architecture**: Ensemble of XGBoost + LightGBM
- Input features: Soil parameters, weather data, historical data, location
- Output: N, P, K levels (continuous values)
- Training data: Government soil survey + field test data
- Error margin target: <15%

**Feature Engineering**:
- Soil texture encoding
- Weather aggregation (7-day, 30-day averages)
- Seasonal indicators
- Crop type encoding
- Historical NPK trends

### Pest Detection Model

**Architecture**: YOLO v5 for object detection
- Input: Variable size images
- Output: Bounding boxes + pest class + confidence
- Classes: 15+ common pests
- Real-time inference capability

### Cost Estimation Model

**Purpose**: Calculate accurate cost estimates for fertilizers and pesticides

**Components**:
1. **Market Price Database**: Real-time pricing data for agricultural inputs
2. **Quantity Calculator**: Calculates required quantities based on farm size and recommendations
3. **Cost Optimizer**: Suggests cost-effective alternatives
4. **ROI Estimator**: Predicts return on investment based on expected yield improvement

**Data Sources**:
- Government agricultural market prices
- Local retailer pricing data
- Historical cost trends
- Yield improvement statistics

**Output**:
- Total estimated cost
- Cost breakdown by input type
- Cost comparison with alternatives
- Expected ROI percentage
- Payback period estimation

## Security Architecture

### Authentication Flow
1. Farmer enters phone number
2. System generates 6-digit OTP
3. OTP sent via SMS (using AWS SNS or Twilio)
4. Farmer enters OTP
5. System validates OTP (5-minute expiry)
6. JWT token issued (24-hour expiry)
7. Refresh token issued (30-day expiry)

**OTP Service Design:**
- OTP generation using secure random number generator
- OTP storage in Redis with TTL (5 minutes)
- Rate limiting: Max 3 OTP requests per phone number per hour
- SMS delivery via AWS SNS or Twilio API
- OTP validation with attempt tracking (max 3 attempts)
- Automatic cleanup of expired OTPs

### Data Encryption
- TLS 1.3 for data in transit
- AES-256 encryption for data at rest
- Encrypted S3 buckets for images and reports
- Encrypted database connections

### Access Control
- Role-based access control (RBAC)
- Farmer can only access their own data
- Admin role for system management
- API key authentication for internal services

## Mobile App Architecture

### User Interface Design Principles
- **Low Digital Literacy Support**: Visual icons with minimal text
- **Voice Input**: Speech-to-text for form fields
- **Tutorial System**: Interactive onboarding for first-time users
- **Simple Navigation**: Maximum 3 taps to any feature
- **Visual Feedback**: Clear indicators for all actions

### Voice Input Architecture
- Integration with device native speech recognition
- Fallback to Google Speech-to-Text API
- Support for 7+ Indian regional languages
- Real-time transcription display
- Confidence scoring for accuracy
- Manual correction capability

### Onboarding & Tutorial System
- Step-by-step guided tour on first launch
- Interactive tooltips for key features
- Video tutorials for camera usage
- Skip option for experienced users
- Progress tracking (5-step onboarding)
- Language selection as first step

### Offline Architecture

### Offline Capabilities
1. **Local Data Storage**: SQLite database on mobile device
2. **Cached Models**: Lightweight TensorFlow Lite models
3. **Cached Data**: Government soil data, crop information
4. **Queue System**: Pending requests stored locally
5. **Sync Strategy**: Automatic sync when network available

### Offline Data Sync Flow
```
1. User performs action offline
2. Action stored in local queue
3. Network connectivity detected
4. Queue items uploaded to server
5. Server processes requests
6. Results downloaded to device
7. Local cache updated
8. Queue cleared
```

## Performance Optimization

### Caching Strategy
- **Redis Cache**: API responses, weather data, user sessions
- **CDN**: Static assets, report PDFs
- **Database Query Cache**: Frequently accessed data
- **Mobile App Cache**: Images, reports, user data

### Image Optimization
- Client-side compression before upload
- Progressive JPEG format
- Thumbnail generation for previews
- Lazy loading in mobile app

### API Optimization
- Response pagination
- Field filtering (return only requested fields)
- Batch API endpoints
- GraphQL for flexible queries (future)

## Monitoring & Logging

### Monitoring Metrics
- API response times
- Error rates
- Model inference times
- Database query performance
- Storage usage
- Active users
- Report generation times

### Logging Strategy
- Structured logging (JSON format)
- Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Centralized logging using CloudWatch
- Log retention: 30 days
- Alert triggers for critical errors

### Health Checks
- `/health` endpoint for service status
- Database connectivity check
- External API availability check
- Model loading status
- Storage availability check

## Deployment Architecture

### AWS Infrastructure
- **EC2**: FastAPI application servers (Auto Scaling Group)
- **Lambda**: Serverless functions for batch processing
- **S3**: Image and report storage
- **RDS**: PostgreSQL database (Multi-AZ)
- **DynamoDB**: Real-time data storage
- **ElastiCache**: Redis for caching
- **CloudFront**: CDN for content delivery
- **Route 53**: DNS management
- **CloudWatch**: Monitoring and logging
- **SNS**: Push notifications
- **SES**: Email notifications

### CI/CD Pipeline
1. Code commit to GitHub
2. GitHub Actions triggered
3. Run tests (unit, integration, property-based)
4. Build Docker images
5. Push to ECR (Elastic Container Registry)
6. Deploy to staging environment
7. Run smoke tests
8. Manual approval for production
9. Deploy to production (blue-green deployment)
10. Health check validation

### Environment Configuration
- **Development**: Local Docker containers
- **Staging**: AWS with reduced capacity
- **Production**: AWS with full capacity and redundancy

## Disaster Recovery

### Backup Strategy
- **Database**: Automated daily backups, 30-day retention
- **Images**: S3 versioning enabled
- **Configuration**: Version controlled in Git
- **Recovery Time Objective (RTO)**: 1 hour
- **Recovery Point Objective (RPO)**: 24 hours

### Failover Strategy
- Multi-AZ database deployment
- Auto Scaling for EC2 instances
- S3 cross-region replication (future)
- Health check-based traffic routing
