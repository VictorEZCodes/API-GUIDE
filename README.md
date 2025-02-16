# AirBuckets Cloud Storage API Documentation

## Authentication

### Register a New Account
```bash
curl -X POST https://api.airbuckets.cloud/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your_username",
    "password": "your_password",
    "email": "your_email@example.com"
  }'
```

### Login
```bash
curl -X POST https://api.airbuckets.cloud/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your_username",
    "password": "your_password"
  }'
```
Response includes JWT token to use for authenticated requests.

## File Operations

### Upload Single File
```bash
curl -X POST https://api.airbuckets.cloud/api/files/upload \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -F "file=@/path/to/your/file.ext"
```

### Upload Multiple FIles
```bash
curl -X POST https://api.airbuckets.cloud/api/files/upload/bulk \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -F "files=@/path/to/file1.ext" \
  -F "files=@/path/to/file2.ext"
```

### Download File
```bash
curl -X GET https://api.airbuckets.cloud/api/files/{fileId} \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  --output downloaded_file.ext
```

### List Files
```bash
curl -X GET https://api.airbuckets.cloud/api/files \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Delete File
```bash
curl -X DELETE https://api.airbuckets.cloud/api/files/delete/{messageId} \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## Rate Limits
- 100 requests per 15 minutes per IP address
- File size limit: 2GB per file
- Bulk upload limit: 50 files per request

## JavaScript Example
```javascript
// Upload file example
async function uploadFile(file, token) {
  const formData = new FormData();
  formData.append('file', file);

  const response = await fetch('https://api.airbuckets.cloud/api/files/upload', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`
    },
    body: formData
  });

  return await response.json();
}

// Download file example
async function downloadFile(fileId, token) {
  const response = await fetch(`https://api.airbuckets.cloud/api/files/${fileId}`, {
    headers: {
      'Authorization': `Bearer ${token}`
    }
  });

  if (!response.ok) throw new Error('Download failed');
  return await response.blob();
}
```

## Error Handling
The API returns standard HTTP status codes:

- 200: Success
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error
Each error response includes a JSON object with error details:

```json
{
  "error": "Error message",
  "message": "Additional details (if any)"
}
```

## Support
For API support or to report issues, please visit our GitHub repository or contact support.
