# API Documentation
## MedPharm Clinical Trial Backend API

**Version:** 1.0
**Base URL:** `https://api.medpharm-trials.com/v1` (fictional)
**Authentication:** Bearer Token (JWT)
**Protocol:** HTTPS only (TLS 1.3)

---

## Important: Mock API for Students

**This API does not exist yet.** This documentation describes what the backend API **will look like** when implemented.

**For Lab 3 Students:**

Since the backend doesn't exist, you have two options:

### Option A: Offline-Only Mode (Recommended for Lab 3)
- Skip API calls entirely
- Store everything locally in SQLite
- Mock the sync status (always show "synced")
- Focus on app architecture and UI

### Option B: Mock API Server
- Use `json_server` or similar to create a mock REST API
- See "Setting Up Mock API" section below
- Good practice for full-stack understanding

**Most students should use Option A** and focus on the Flutter app. The offline-first architecture means the app works perfectly without a backend!

---

## Table of Contents

1. [Authentication](#1-authentication)
2. [Enrollment](#2-enrollment)
3. [Assessments](#3-assessments)
4. [Sync](#4-sync)
5. [Alerts](#5-alerts)
6. [Audit Logs](#6-audit-logs)
7. [Error Handling](#7-error-handling)
8. [Rate Limiting](#8-rate-limiting)
9. [Setting Up Mock API](#9-setting-up-mock-api)

---

## 1. Authentication

### 1.1 Token-Based Authentication

All API requests (except enrollment validation) require a Bearer token in the header:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Token Expiry:** 30 days
**Refresh:** Automatic on any API call within 7 days of expiry

### 1.2 Token Storage

**Client Responsibilities:**
- Store token securely (iOS Keychain / Android Keystore)
- Include in all API requests
- Handle token expiry gracefully
- Clear token on logout

---

## 2. Enrollment

### 2.1 Validate Enrollment Code

**Endpoint:** `POST /enrollment/validate`

**Purpose:** Verify enrollment code and retrieve study details

**Request Headers:**
```http
Content-Type: application/json
```

**Request Body:**
```json
{
  "enrollmentCode": "MED-2025-A1B2C3",
  "deviceInfo": {
    "platform": "android",
    "osVersion": "13.0",
    "deviceModel": "Pixel 7",
    "appVersion": "1.0.0"
  }
}
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "data": {
    "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
    "enrollmentCode": "MED-2025-A1B2C3",
    "trialSite": {
      "id": "SITE-001",
      "name": "AGH University Hospital",
      "location": "Kraków, Poland"
    },
    "investigator": {
      "name": "Dr. Maria Kowalska",
      "contact": "m.kowalska@agh.edu.pl"
    },
    "assessmentWindow": {
      "startTime": "08:00",
      "endTime": "22:00",
      "timezone": "Europe/Warsaw"
    },
    "trialDuration": {
      "startDate": "2025-11-01",
      "endDate": "2026-01-31",
      "durationDays": 91
    },
    "authToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdHVkeUlkIjoiOGY3ZTZkNWMtNGIzYS0yOTE4LTdjNmQtNWU0ZjNhMmIxYzBkIiwiaWF0IjoxNjk5ODk2MDAwLCJleHAiOjE3MDI0ODgwMDB9.5J8K9L0M1N2O3P4Q5R6S7T8U9V0W1X2Y3Z4",
    "tokenExpiry": "2025-12-06T12:00:00Z"
  }
}
```

**Error Responses:**

**Invalid Code (404 Not Found):**
```json
{
  "status": "error",
  "error": {
    "code": "INVALID_ENROLLMENT_CODE",
    "message": "The enrollment code is invalid or has expired.",
    "details": "Please check your code and try again. Contact your trial coordinator if the problem persists."
  }
}
```

**Code Already Used (409 Conflict):**
```json
{
  "status": "error",
  "error": {
    "code": "CODE_ALREADY_USED",
    "message": "This enrollment code has already been used.",
    "details": "Each code can only be used once. Contact your trial coordinator for assistance."
  }
}
```

**Code Expired (410 Gone):**
```json
{
  "status": "error",
  "error": {
    "code": "CODE_EXPIRED",
    "message": "This enrollment code has expired.",
    "details": "Enrollment codes are valid for 30 days. Please request a new code from your coordinator."
  }
}
```

**Rate Limit (429 Too Many Requests):**
```json
{
  "status": "error",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many attempts. Please try again later.",
    "details": "Maximum 3 attempts per 15 minutes allowed.",
    "retryAfter": 900
  }
}
```

### 2.2 Confirm Consent

**Endpoint:** `POST /enrollment/consent`

**Purpose:** Record that user has accepted informed consent

**Request Headers:**
```http
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
  "consentVersion": "v2.1",
  "consentAccepted": true,
  "consentTimestamp": "2025-11-06T14:30:00Z",
  "digitalSignature": {
    "accepted": true,
    "ipAddress": "192.168.1.100",
    "userAgent": "MedPharm/1.0.0 (Android 13)"
  }
}
```

**Success Response (201 Created):**
```json
{
  "status": "success",
  "data": {
    "consentId": "consent_8f7e6d5c4b3a",
    "recordedAt": "2025-11-06T14:30:01Z",
    "message": "Consent successfully recorded."
  }
}
```

**Error Response (400 Bad Request):**
```json
{
  "status": "error",
  "error": {
    "code": "CONSENT_ALREADY_RECORDED",
    "message": "Consent has already been recorded for this study ID."
  }
}
```

---

## 3. Assessments

### 3.1 Get Questionnaire Configuration

**Endpoint:** `GET /questionnaires/config`

**Purpose:** Fetch the latest questionnaire configuration (questions, options, logic)

**Request Headers:**
```http
Authorization: Bearer {token}
```

**Query Parameters:**
```
?version=latest
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "data": {
    "version": "1.2.0",
    "effectiveDate": "2025-11-01",
    "questionnaires": {
      "nrs": {
        "enabled": true,
        "question": "On a scale of 0-10, how much pain do you feel right now?",
        "scaleMin": 0,
        "scaleMax": 10,
        "labels": {
          "0": "No pain",
          "10": "Worst pain imaginable"
        }
      },
      "vas": {
        "enabled": true,
        "question": "Please slide to indicate your pain level.",
        "scaleMin": 0,
        "scaleMax": 100
      },
      "mcgill": {
        "enabled": true,
        "sections": [
          {
            "id": "sensory",
            "title": "What does your pain feel like?",
            "instructions": "Select the words that best describe your pain.",
            "words": [
              "Throbbing", "Shooting", "Stabbing", "Sharp", "Cramping",
              "Gnawing", "Hot-burning", "Aching", "Heavy", "Tender"
            ]
          }
        ]
      },
      "custom": {
        "enabled": true,
        "questions": [
          {
            "id": "q1",
            "type": "yes_no",
            "question": "Did you take your medication as prescribed today?",
            "required": true
          },
          {
            "id": "q2",
            "type": "multiple_choice",
            "question": "How would you rate your sleep last night?",
            "options": ["Excellent", "Good", "Fair", "Poor", "Very Poor"],
            "required": true
          },
          {
            "id": "q3",
            "type": "likert_5",
            "question": "How much did pain interfere with your daily activities?",
            "labels": {
              "1": "Not at all",
              "5": "Completely"
            },
            "required": true
          }
        ]
      }
    }
  }
}
```

### 3.2 Submit Assessment (Sync)

**Endpoint:** `POST /assessments/sync`

**Purpose:** Upload completed assessment data

**Request Headers:**
```http
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "assessmentId": "8a7b6c5d-4e3f-2a1b-0c9d-8e7f6a5b4c3d",
  "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
  "timestamp": "2025-11-06T09:15:00Z",
  "timeWindowStart": "2025-11-06T08:00:00Z",
  "timeWindowEnd": "2025-11-06T22:00:00Z",
  "completedAt": "2025-11-06T09:17:32Z",
  "deviceInfo": {
    "platform": "android",
    "osVersion": "13.0",
    "appVersion": "1.0.0"
  },
  "data": {
    "nrs": {
      "score": 6,
      "completedAt": "2025-11-06T09:15:30Z"
    },
    "vas": {
      "score": 58,
      "completedAt": "2025-11-06T09:16:05Z"
    },
    "mcgill": {
      "selectedWords": ["Throbbing", "Aching", "Heavy"],
      "ppi": 3,
      "completedAt": "2025-11-06T09:16:45Z"
    },
    "custom": {
      "q1": "yes",
      "q2": "Good",
      "q3": 2,
      "completedAt": "2025-11-06T09:17:32Z"
    }
  }
}
```

**Success Response (201 Created):**
```json
{
  "status": "success",
  "data": {
    "assessmentId": "8a7b6c5d-4e3f-2a1b-0c9d-8e7f6a5b4c3d",
    "syncedAt": "2025-11-06T09:17:35Z",
    "serverTimestamp": "2025-11-06T09:17:35.123Z",
    "message": "Assessment successfully recorded."
  }
}
```

**Error Response - Duplicate (409 Conflict):**
```json
{
  "status": "error",
  "error": {
    "code": "DUPLICATE_ASSESSMENT",
    "message": "An assessment with this ID already exists.",
    "details": "If you believe this is an error, contact support."
  }
}
```

**Error Response - Outside Window (422 Unprocessable Entity):**
```json
{
  "status": "error",
  "error": {
    "code": "OUTSIDE_TIME_WINDOW",
    "message": "Assessment timestamp is outside the allowed time window.",
    "details": "Assessments must be completed between 08:00-22:00 local time."
  }
}
```

### 3.3 Batch Sync (Multiple Assessments)

**Endpoint:** `POST /assessments/sync/batch`

**Purpose:** Upload multiple assessments at once (for offline queue)

**Request Headers:**
```http
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
  "assessments": [
    {
      "assessmentId": "assessment_001",
      "timestamp": "2025-11-05T09:15:00Z",
      "data": { /* ... assessment data ... */ }
    },
    {
      "assessmentId": "assessment_002",
      "timestamp": "2025-11-06T09:15:00Z",
      "data": { /* ... assessment data ... */ }
    }
  ]
}
```

**Success Response (207 Multi-Status):**
```json
{
  "status": "partial_success",
  "data": {
    "totalReceived": 2,
    "successful": 1,
    "failed": 1,
    "results": [
      {
        "assessmentId": "assessment_001",
        "status": "success",
        "syncedAt": "2025-11-06T10:30:15Z"
      },
      {
        "assessmentId": "assessment_002",
        "status": "error",
        "error": {
          "code": "DUPLICATE_ASSESSMENT",
          "message": "Assessment already exists"
        }
      }
    ]
  }
}
```

---

## 4. Sync

### 4.1 Check Sync Status

**Endpoint:** `GET /sync/status`

**Purpose:** Check if there are pending server-side changes

**Request Headers:**
```http
Authorization: Bearer {token}
```

**Query Parameters:**
```
?studyId=8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d
&lastSync=2025-11-06T09:00:00Z
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "data": {
    "serverTime": "2025-11-06T14:30:00Z",
    "lastSync": "2025-11-06T09:00:00Z",
    "hasPendingChanges": false,
    "pendingFromServer": 0,
    "pendingFromClient": 3,
    "message": "You have 3 assessments pending upload."
  }
}
```

---

## 5. Alerts

### 5.1 Send Coordinator Alert

**Endpoint:** `POST /alerts`

**Purpose:** Trigger alerts to trial coordinators (system-generated)

**Request Headers:**
```http
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
  "alertType": "MISSED_ASSESSMENT",
  "severity": "medium",
  "details": {
    "missedDate": "2025-11-06",
    "consecutiveMissed": 1,
    "lastCompletedDate": "2025-11-05"
  },
  "timestamp": "2025-11-06T22:05:00Z"
}
```

**Success Response (202 Accepted):**
```json
{
  "status": "success",
  "data": {
    "alertId": "alert_8f7e6d5c",
    "queued": true,
    "message": "Alert has been queued for delivery to coordinator."
  }
}
```

**Alert Types:**
- `MISSED_ASSESSMENT` - Patient missed scheduled assessment
- `SYNC_FAILURE` - Data not synced within 48 hours
- `HIGH_PAIN_SCORE` - Pain score ≥9/10 for multiple days
- `SUDDEN_PAIN_INCREASE` - Pain increased ≥3 points
- `CONSENT_WITHDRAWN` - Patient withdrew consent

---

## 6. Audit Logs

### 6.1 Submit Audit Log

**Endpoint:** `POST /audit/log`

**Purpose:** Upload audit trail events for regulatory compliance

**Request Headers:**
```http
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "logs": [
    {
      "logId": "log_001",
      "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
      "timestamp": "2025-11-06T09:15:00Z",
      "eventType": "ASSESSMENT_STARTED",
      "eventDetails": {
        "assessmentId": "8a7b6c5d-4e3f-2a1b-0c9d-8e7f6a5b4c3d",
        "action": "User tapped 'Begin Assessment' button"
      },
      "deviceInfo": {
        "platform": "android",
        "osVersion": "13.0",
        "appVersion": "1.0.0"
      }
    },
    {
      "logId": "log_002",
      "studyId": "8f7e6d5c-4b3a-2918-7c6d-5e4f3a2b1c0d",
      "timestamp": "2025-11-06T09:17:32Z",
      "eventType": "ASSESSMENT_COMPLETED",
      "eventDetails": {
        "assessmentId": "8a7b6c5d-4e3f-2a1b-0c9d-8e7f6a5b4c3d",
        "duration": 152,
        "action": "Assessment submitted successfully"
      },
      "deviceInfo": {
        "platform": "android",
        "osVersion": "13.0",
        "appVersion": "1.0.0"
      }
    }
  ]
}
```

**Success Response (201 Created):**
```json
{
  "status": "success",
  "data": {
    "logsReceived": 2,
    "message": "Audit logs successfully recorded."
  }
}
```

**Standard Event Types:**
- `APP_INSTALLED` / `APP_UNINSTALLED`
- `USER_ENROLLED` / `CONSENT_ACCEPTED` / `CONSENT_WITHDRAWN`
- `ASSESSMENT_STARTED` / `ASSESSMENT_COMPLETED` / `ASSESSMENT_ABANDONED`
- `NOTIFICATION_RECEIVED` / `NOTIFICATION_OPENED`
- `SETTINGS_CHANGED`
- `SYNC_INITIATED` / `SYNC_SUCCEEDED` / `SYNC_FAILED`
- `DATA_EXPORTED`

---

## 7. Error Handling

### 7.1 Standard Error Format

All errors follow this format:

```json
{
  "status": "error",
  "error": {
    "code": "ERROR_CODE_HERE",
    "message": "Human-readable error message",
    "details": "Additional context or instructions",
    "field": "fieldName",
    "timestamp": "2025-11-06T14:30:00Z",
    "requestId": "req_8f7e6d5c"
  }
}
```

### 7.2 Common HTTP Status Codes

| Code | Meaning | When Used |
|------|---------|-----------|
| 200 | OK | Successful GET request |
| 201 | Created | Successful POST (resource created) |
| 202 | Accepted | Request queued for processing |
| 400 | Bad Request | Invalid request format or missing fields |
| 401 | Unauthorized | Missing or invalid auth token |
| 403 | Forbidden | Valid token but insufficient permissions |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource (e.g., code already used) |
| 410 | Gone | Resource expired or deleted |
| 422 | Unprocessable Entity | Valid format but business logic error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server-side error |
| 503 | Service Unavailable | Maintenance or temporary outage |

### 7.3 Error Codes

**Authentication Errors:**
- `INVALID_TOKEN` - Token is malformed or expired
- `TOKEN_EXPIRED` - Token needs refresh
- `UNAUTHORIZED` - No token provided

**Enrollment Errors:**
- `INVALID_ENROLLMENT_CODE` - Code doesn't exist
- `CODE_ALREADY_USED` - Code has been redeemed
- `CODE_EXPIRED` - Code past validity date

**Assessment Errors:**
- `DUPLICATE_ASSESSMENT` - Assessment ID already exists
- `OUTSIDE_TIME_WINDOW` - Assessment outside allowed hours
- `INCOMPLETE_DATA` - Missing required fields
- `INVALID_SCORE` - Score out of valid range

**Sync Errors:**
- `SYNC_CONFLICT` - Server data conflicts with local data
- `NETWORK_ERROR` - Connection failed
- `TIMEOUT` - Request took too long

**Rate Limiting:**
- `RATE_LIMIT_EXCEEDED` - Too many requests

---

## 8. Rate Limiting

### 8.1 Rate Limits by Endpoint

| Endpoint | Limit | Window |
|----------|-------|--------|
| `/enrollment/validate` | 3 requests | 15 minutes |
| `/assessments/sync` | 100 requests | 1 hour |
| `/assessments/sync/batch` | 10 requests | 1 hour |
| All other endpoints | 100 requests | 1 minute |

### 8.2 Rate Limit Headers

Every response includes rate limit information:

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1699896600
```

### 8.3 Handling Rate Limits

When rate limited (429 response):

```json
{
  "status": "error",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later.",
    "retryAfter": 900
  }
}
```

**Client should:**
1. Read `retryAfter` seconds
2. Wait that duration
3. Retry request
4. Implement exponential backoff for repeated failures

---

## 9. Setting Up Mock API

### Option 1: json-server (Simplest)

**Step 1: Install json-server**
```bash
npm install -g json-server
```

**Step 2: Create `mock-api/db.json`**
```json
{
  "enrollments": [
    {
      "code": "MED-2025-TEST01",
      "studyId": "test-study-001",
      "used": false
    }
  ],
  "assessments": [],
  "auditLogs": []
}
```

**Step 3: Run mock server**
```bash
json-server --watch mock-api/db.json --port 3000
```

**Step 4: Update Flutter app**
```dart
// lib/core/config/api_config.dart
class ApiConfig {
  static const String baseUrl = 'http://localhost:3000'; // Mock server
  // static const String baseUrl = 'https://api.medpharm-trials.com/v1'; // Real server (future)
}
```

### Option 2: Mockoon (More Features)

1. Download [Mockoon](https://mockoon.com/)
2. Import this API spec
3. Start mock server
4. Point Flutter app to `http://localhost:3001`

### Option 3: Flutter Mock (No Server Needed)

Create a mock service in Flutter:

```dart
// lib/core/services/mock_api_service.dart

class MockApiService {
  Future<Map<String, dynamic>> validateEnrollmentCode(String code) async {
    // Simulate network delay
    await Future.delayed(Duration(seconds: 1));

    // Mock valid codes
    if (code == 'MED-2025-TEST01') {
      return {
        'status': 'success',
        'data': {
          'studyId': 'test-study-001',
          'authToken': 'mock_token_12345',
          // ... rest of response
        }
      };
    }

    // Mock invalid code
    throw Exception('INVALID_ENROLLMENT_CODE');
  }

  Future<void> syncAssessment(Map<String, dynamic> assessment) async {
    await Future.delayed(Duration(seconds: 2));
    print('Mock: Assessment synced: ${assessment['assessmentId']}');
    // Always succeed
  }
}
```

### Recommended Approach for Lab 3

**Skip API calls entirely:**

```dart
// In your sync service
Future<void> syncAssessments() async {
  // For Lab 3: Just mark as synced immediately
  final unsynced = await _getUnsyncedAssessments();

  for (var assessment in unsynced) {
    // TODO in future: Actually call API
    // await _apiService.syncAssessment(assessment);

    // For now: Just mark as synced
    await _markAsSynced(assessment.id);
    print('✅ Mock sync: ${assessment.id}');
  }
}
```

This way students can:
- Focus on Flutter app architecture
- Build offline-first features
- Not worry about backend complexity
- Still see how API integration would work

Later, when the real backend exists, just uncomment the API calls!

---

## 10. Security Considerations

### 10.1 Client-Side Security

**DO:**
- ✅ Store tokens in secure storage (Keychain/Keystore)
- ✅ Use HTTPS only (no HTTP)
- ✅ Validate SSL certificates
- ✅ Implement certificate pinning
- ✅ Clear tokens on logout
- ✅ Handle token expiry gracefully

**DON'T:**
- ❌ Store tokens in SharedPreferences/UserDefaults
- ❌ Log sensitive data (tokens, PII)
- ❌ Send data over HTTP
- ❌ Ignore SSL certificate errors

### 10.2 API Security (Server-Side)

**Required:**
- TLS 1.3 encryption
- JWT token authentication
- Rate limiting
- Input validation
- SQL injection prevention
- CORS configuration
- Audit logging

---

## 11. Testing the API

### 11.1 Manual Testing with cURL

**Test enrollment validation:**
```bash
curl -X POST https://api.medpharm-trials.com/v1/enrollment/validate \
  -H "Content-Type: application/json" \
  -d '{
    "enrollmentCode": "MED-2025-TEST01",
    "deviceInfo": {
      "platform": "android",
      "osVersion": "13.0",
      "deviceModel": "Pixel 7",
      "appVersion": "1.0.0"
    }
  }'
```

**Test assessment sync:**
```bash
curl -X POST https://api.medpharm-trials.com/v1/assessments/sync \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d @assessment_sample.json
```

### 11.2 Postman Collection

A Postman collection with all endpoints is available at:
`docs/postman/MedPharm_API.postman_collection.json` (TODO: create this)

Import into Postman for easy testing.

### 11.3 Automated Tests

Backend API should have:
- Unit tests for business logic
- Integration tests for endpoints
- Load tests for performance
- Security tests for vulnerabilities

---

## 12. API Versioning

### Current Version: v1

**URL Pattern:** `https://api.medpharm-trials.com/v1/*`

**When to bump version:**
- Breaking changes to request/response format
- Removed fields
- Changed authentication method

**Backwards compatibility:**
- v1 will be supported for minimum 12 months after v2 release
- Clients should gracefully handle unknown fields
- Clients should include `App-Version` header

---

## 13. Support & Troubleshooting

### 13.1 Common Issues

**"401 Unauthorized" on every request:**
- Check token is being sent in `Authorization` header
- Verify token hasn't expired
- Ensure format is `Bearer {token}` not just `{token}`

**"Network error" in app:**
- Check internet connection
- Verify API base URL is correct
- Check firewall/proxy settings
- Try from different network

**"422 Unprocessable Entity" on assessment:**
- Validate all required fields are present
- Check score values are in valid range (NRS: 0-10, VAS: 0-100)
- Ensure timestamp is within assessment window

### 13.2 Support Contacts

**For Lab 3 Students:**
- Your instructor (for mock API questions)
- Course forum/discussion board

**For Production Deployment:**
- API Support: api-support@medpharm-trials.com (fictional)
- Emergency Hotline: +48 XXX XXX XXX
- Status Page: status.medpharm-trials.com

---

## 14. Changelog

### Version 1.0.0 (2025-11-06)
- Initial API specification
- Enrollment endpoints
- Assessment sync endpoints
- Audit log endpoints
- Alert endpoints

### Future Versions

**v1.1.0 (Planned)**
- Questionnaire update notifications
- Profile management endpoints
- Data export endpoint

**v2.0.0 (Future)**
- GraphQL support
- WebSocket for real-time sync
- Batch operations optimization

---

## Appendix: Sample Data

### Sample Valid Enrollment Codes (for testing)

```
MED-2025-TEST01  (unused)
MED-2025-TEST02  (unused)
MED-2025-USED01  (already used - should fail)
MED-2025-EXP01   (expired - should fail)
INVALID-CODE     (doesn't exist - should fail)
```

### Sample Assessment JSON

See `docs/samples/assessment_example.json` (TODO: create this)

---

**Questions about this API?**
- Check the PRD.md for business requirements
- Refer to ARCHITECTURE_GUIDE.md for client implementation
- Ask your instructor for clarification

**Remember:** This API doesn't exist yet. For Lab 3, focus on the offline-first Flutter app. API integration can come later!
