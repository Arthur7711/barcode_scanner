# Scanner App API Contract

## Base URL
```
https://api.example.com/v1
```

## Authentication

### Login
Authenticates a user and returns a JWT token.

```
POST /login
```

#### Request Body
```json
{
  "email": "string",
  "password": "string"
}
```

#### Response
```json
{
  "token": "string",     // JWT token
  "expiresIn": "number"  // Token expiration time in seconds
}
```

#### Error Response
```json
{
  "error": "string",
  "message": "string"
}
```

#### Status Codes
- 200: Success
- 401: Invalid credentials
- 422: Validation error
- 500: Server error

## Data Submission

### Submit Item Data
Submits item data including image, barcode, and metadata.

```
POST /submit
```

#### Headers
```
Authorization: Bearer <token>
Content-Type: multipart/form-data
```

#### Request Body (multipart/form-data)
```yaml
image: File (optional)
  - Accepted formats: jpg, jpeg, png
  - Max size: 10MB
  - EXIF data will be processed if available

barcode: string (required)
  - Format: EAN, UPC, Code 128, Code 39, or Codabar
  - Length: 8-14 characters

shopLocation: string (optional)
  - Max length: 200 characters

skuPrice: string (optional)
  - Format: Decimal number
  - Example: "99.99"

skuName: string (optional)
  - Max length: 200 characters

latitude: number (optional)
  - Format: Decimal degrees
  - Range: -90 to 90
  - Example: 51.5074

longitude: number (optional)
  - Format: Decimal degrees
  - Range: -180 to 180
  - Example: -0.1278
```

#### Success Response
```json
{
  "id": "string",        // Unique identifier for the submission
  "timestamp": "string", // ISO 8601 format
  "status": "success",
  "message": "Item data submitted successfully"
}
```

#### Error Response
```json
{
  "error": "string",
  "message": "string",
  "details": {           // Optional field-specific errors
    "field": "error message"
  }
}
```

#### Status Codes
- 201: Created successfully
- 400: Bad request / Invalid data
- 401: Unauthorized
- 413: File too large
- 415: Unsupported media type
- 422: Validation error
- 500: Server error

## Error Codes and Messages

### Authentication Errors
- `AUTH001`: Invalid credentials
- `AUTH002`: Token expired
- `AUTH003`: Invalid token format
- `AUTH004`: Token required

### Validation Errors
- `VAL001`: Invalid barcode format
- `VAL002`: Invalid price format
- `VAL003`: Text field exceeds maximum length
- `VAL004`: Invalid coordinates
- `VAL005`: Required field missing

### File Errors
- `FILE001`: File too large
- `FILE002`: Unsupported file type
- `FILE003`: File upload failed
- `FILE004`: File corrupted

### Server Errors
- `SRV001`: Database error
- `SRV002`: File storage error
- `SRV003`: External service error

## Rate Limiting
- Rate limit: 100 requests per minute per IP
- Rate limit headers included in response:
  ```
  X-RateLimit-Limit: 100
  X-RateLimit-Remaining: 99
  X-RateLimit-Reset: 1640995200
  ```

## Data Retention
- Images are stored for 90 days
- Metadata is stored indefinitely
- EXIF data is stripped from images after processing

## Security Requirements
- TLS 1.2 or higher required
- JWT tokens expire after 24 hours
- File uploads scanned for malware
- CORS enabled for specified domains only
- Content-Security-Policy headers implemented 