# Scanner App

A mobile-optimized web application for scanning items and uploading their information.

## Features

- User authentication with email/password
- Image upload with EXIF data extraction
- Barcode scanning using device camera
- Geolocation tracking
- Form for additional item details (shop location, SKU price, SKU name)
- Confirmation dialog before submission
- Mobile-optimized UI using Material-UI

## Prerequisites

- Node.js 14.0 or later
- npm 6.0 or later

## Installation

1. Clone the repository:
```bash
git clone [repository-url]
cd scanerapp
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory and add your API endpoint:
```
VITE_API_ENDPOINT=your_api_endpoint_here
```

## Development

To start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:3000`

## Development with HTTPS (Required for iOS Camera Access)

For iOS devices (both Safari and Chrome), camera access requires HTTPS. To run the development server with HTTPS:

1. Generate a self-signed certificate (one-time setup):
```bash
npm run generate-cert
```

2. Start the development server with HTTPS:
```bash
npm run dev:https
```

3. When accessing the app:
   - Open https://localhost:3000
   - Accept the self-signed certificate warning
   - For iOS devices, use your computer's local IP address (e.g., https://192.168.1.100:3000)

Note: You may need to manually trust the certificate on iOS devices:
1. Go to Settings > General > About > Certificate Trust Settings
2. Enable full trust for the development certificate

## Building for Production

To create a production build:

```bash
npm run build
```

The built files will be in the `dist` directory.

## Usage

1. Open the application in a mobile browser
2. Log in with your credentials
3. Upload an image or scan a barcode
4. Fill in additional item details
5. Review the information in the confirmation dialog
6. Submit the data

## API Integration

The application expects the following API endpoints:

- POST `/login` - For user authentication
- POST `/submit` - For submitting item data

The submit endpoint should accept multipart/form-data with the following fields:
- image (file)
- barcode (string)
- shopLocation (string)
- skuPrice (string)
- skuName (string)
- latitude (number)
- longitude (number)

## Security

- Authentication token is stored in localStorage
- Geolocation access is requested with user permission
- Camera access is requested with user permission

## License

MIT 