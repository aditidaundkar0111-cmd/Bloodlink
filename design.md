# BloodLink – System Design Document

## Architecture Overview
BloodLink follows a three-tier architecture:
- **Presentation Layer**: HTML/CSS/JS frontend
- **Application Layer**: Flask backend with business logic
- **Data Layer**: SQL database

## System Architecture Diagram
```
┌─────────────────────────────────────────────────────────┐
│                    Client Browser                        │
│              (HTML + CSS + JavaScript)                   │
└────────────────────┬────────────────────────────────────┘
                     │ HTTP/HTTPS
┌────────────────────▼────────────────────────────────────┐
│                  Flask Application                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Routes & Controllers                             │  │
│  │  - Auth, Donor, Search, Admin                    │  │
│  └──────────────────┬───────────────────────────────┘  │
│  ┌──────────────────▼───────────────────────────────┐  │
│  │  Business Logic & AI Services                     │  │
│  │  - Donor Matching, Spam Detection, Alerts        │  │
│  └──────────────────┬───────────────────────────────┘  │
└────────────────────┬┴───────────────────────────────────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
┌───────▼──────┐ ┌──▼─────────┐ ┌▼──────────────┐
│   SQL DB     │ │  Maps API  │ │  Email/SMS    │
│   (SQLite/   │ │  (Google)  │ │  Service      │
│  PostgreSQL) │ └────────────┘ └───────────────┘
└──────────────┘
```

## Database Schema

### Users Table
```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    phone VARCHAR(15) NOT NULL,
    role VARCHAR(20) DEFAULT 'donor',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_verified BOOLEAN DEFAULT FALSE
);
```

### Donors Table
```sql
CREATE TABLE donors (
    donor_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    blood_group VARCHAR(5) NOT NULL,
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    address TEXT,
    city VARCHAR(50),
    is_available BOOLEAN DEFAULT TRUE,
    last_donation_date DATE,
    donation_count INTEGER DEFAULT 0,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

### Requests Table
```sql
CREATE TABLE blood_requests (
    request_id INTEGER PRIMARY KEY AUTOINCREMENT,
    requester_id INTEGER NOT NULL,
    blood_group VARCHAR(5) NOT NULL,
    urgency_level VARCHAR(20),
    location TEXT,
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (requester_id) REFERENCES users(user_id)
);
```

### Reports Table
```sql
CREATE TABLE reports (
    report_id INTEGER PRIMARY KEY AUTOINCREMENT,
    reporter_id INTEGER NOT NULL,
    reported_user_id INTEGER NOT NULL,
    reason TEXT,
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (reporter_id) REFERENCES users(user_id),
    FOREIGN KEY (reported_user_id) REFERENCES users(user_id)
);
```

## API Endpoints

### Authentication
- `POST /api/register` - Register new user
- `POST /api/login` - User login
- `POST /api/logout` - User logout
- `POST /api/reset-password` - Password reset

### Donor Management
- `GET /api/donors` - Get all donors (with filters)
- `GET /api/donors/<id>` - Get specific donor
- `PUT /api/donors/<id>` - Update donor profile
- `PATCH /api/donors/<id>/availability` - Toggle availability

### Search
- `POST /api/search` - Search donors by blood group and location
- `GET /api/donors/nearby` - Get nearby donors

### Requests
- `POST /api/requests` - Create blood request
- `GET /api/requests/<id>` - Get request details
- `PUT /api/requests/<id>` - Update request status

### Admin
- `GET /api/admin/users` - List all users
- `POST /api/admin/verify/<id>` - Verify donor
- `GET /api/admin/reports` - View reports
- `PUT /api/admin/reports/<id>` - Handle report

## AI/ML Components

### 1. Smart Donor Matching Algorithm
```python
def find_nearest_donors(patient_location, blood_group, radius_km=10):
    # Calculate distance using Haversine formula
    # Filter by blood group compatibility
    # Sort by distance and availability
    # Apply ML scoring for reliability
    return sorted_donors
```

### 2. Spam Detection Model
- Features: registration patterns, profile completeness, activity frequency
- Algorithm: Random Forest Classifier
- Training data: verified vs flagged users

### 3. Urgency Prioritization
- Factors: time since request, urgency level, donor response rate
- Weighted scoring system
- Auto-escalation for critical cases

### 4. Donation Reminder System
- Cron job checks last_donation_date
- Sends reminders after 90 days
- Personalized messaging based on donation history

## UI/UX Design

### Key Pages

1. **Landing Page**
   - Hero section with search bar
   - Quick blood group buttons
   - Call-to-action for donor registration

2. **Donor Registration**
   - Multi-step form
   - Location picker with map
   - Blood group selection

3. **Search Results**
   - Map view with donor markers
   - List view with distance
   - Filter options (availability, distance)

4. **Donor Profile**
   - Contact information
   - Donation history
   - Availability status
   - Verification badge

5. **Dashboard**
   - For donors: requests received, donation history
   - For seekers: active requests, contacted donors

### Design Principles
- Clean, minimal interface
- High contrast for readability
- Large touch targets for mobile
- Emergency red color scheme for urgency
- Green for availability indicators

## Security Measures

1. **Authentication**
   - Password hashing (bcrypt)
   - Session management
   - CSRF protection

2. **Data Protection**
   - SQL parameterized queries
   - Input validation and sanitization
   - Rate limiting on API endpoints

3. **Privacy**
   - Contact info only visible after verification
   - Option to hide profile temporarily
   - Data encryption in transit (HTTPS)

## Deployment Architecture

### Development
- Local Flask server
- SQLite database
- Mock Maps API

### Production
- Cloud hosting (AWS/Heroku/DigitalOcean)
- PostgreSQL database
- Redis for caching
- Nginx reverse proxy
- SSL certificate

## Performance Optimization

1. **Database**
   - Indexing on blood_group, location, availability
   - Query optimization
   - Connection pooling

2. **Caching**
   - Cache donor search results (5 min TTL)
   - Static asset caching
   - API response caching

3. **Frontend**
   - Lazy loading for maps
   - Minified CSS/JS
   - Image optimization

## Future Enhancements

1. Mobile app (React Native/Flutter)
2. Real-time chat between donor and seeker
3. Blood bank integration
4. Donation camps calendar
5. Gamification (badges, leaderboard)
6. Multi-language support
7. SMS notifications
8. Social media integration
