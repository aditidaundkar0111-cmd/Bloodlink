# BloodLink – Emergency Blood Donor Finder

## Project Overview
BloodLink is an emergency blood donor finder platform that connects patients in need with registered blood donors in their locality, enabling quick and efficient blood donation during emergencies.

## Problem Statement
During medical emergencies, families struggle to find blood donors quickly due to:
- No centralized local donor database
- Time-consuming manual search through contacts
- Difficulty in locating donors by blood group and proximity
- Lack of real-time donor availability information

## Solution
A web-based platform where:
1. Users register as blood donors with their details
2. Patients/families search for donors by blood group
3. System shows nearest available donors instantly
4. Direct contact mechanism for urgent requests

## Functional Requirements

### 1. User Management
- **Donor Registration**
  - Name, contact number, email
  - Blood group selection
  - Location (address/coordinates)
  - Availability status
  - Last donation date
  
- **User Authentication**
  - Login/Signup system
  - Password recovery
  - Profile management

### 2. Search & Discovery
- **Blood Group Search**
  - Filter donors by blood group
  - Location-based search
  - Distance calculation from patient location
  
- **Donor Listing**
  - Display nearest donors first
  - Show donor availability status
  - Display contact information

### 3. Contact & Communication
- **Direct Contact**
  - Phone number display
  - Email contact option
  - In-app messaging (optional)

### 4. AI-Powered Features
- **Smart Sorting**
  - Nearest-donor algorithm using location data
  - Availability-based prioritization
  
- **Urgency Alerts**
  - Priority notifications for urgent requests
  - Emergency broadcast to nearby donors
  
- **Spam Detection**
  - Identify and flag fake donor profiles
  - Verify donor authenticity
  - Report mechanism for suspicious activity
  
- **Donation Reminders**
  - Track last donation date
  - Remind donors when eligible again (90 days)
  - Encourage regular donations

### 5. Admin Panel
- User management
- Donor verification
- Report handling
- Analytics dashboard

## Non-Functional Requirements

### 1. Performance
- Fast search results (< 2 seconds)
- Handle concurrent users
- Optimized map loading

### 2. Security
- Secure password storage (hashing)
- Data privacy compliance
- Protected contact information
- SQL injection prevention

### 3. Usability
- Simple, intuitive interface
- Mobile-responsive design
- Accessible to non-technical users

### 4. Reliability
- 99% uptime
- Data backup mechanisms
- Error handling and logging

## Technology Stack
- **Backend**: Flask (Python)
- **Frontend**: HTML, CSS, JavaScript
- **Database**: SQL (SQLite/PostgreSQL)
- **Maps**: Google Maps API / OpenStreetMap
- **AI/ML**: Python libraries (scikit-learn, pandas)

## User Roles
1. **Donor**: Registers, updates availability, receives requests
2. **Patient/Seeker**: Searches for donors, contacts them
3. **Admin**: Manages platform, verifies users, handles reports

## Success Metrics
- Number of registered donors
- Response time for donor search
- Successful blood donation connections
- User satisfaction ratings
- Platform uptime percentage
