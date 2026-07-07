# TaxWise — AI-Based Tax Assistant

TaxWise is a modern web application that helps Indian taxpayers simplify filing, organize tax documents, and generate personalized AI-backed recommendations.

It combines:
- secure user authentication with Google sign-in,
- document upload and OCR/AI extraction,
- tax profile management,
- AI recommendations for regime selection and savings,
- conversational tax support via a chat endpoint.

## Key Features

### 1. Secure Authentication
- Google OAuth login via `/api/auth/google`
- JWT-based session tokens
- Auth-protected APIs for profile, documents, and recommendations

### 2. Document Management
- Upload tax documents via `/api/documents/parse`
- Uses Multer for file uploads and Cloudinary for storage
- Supports document types like `form16`, `payslip`, `insurance_receipt`, `rent_receipt`, `bank_statement`, `investment_proof`, `nps_statement`, `elss_statement`
- Extracts structured data with Gemini AI and updates the user tax profile automatically

### 3. Tax Profile and Recommendations
- Tax profile stored per user and financial year
- Supports income, deductions, taxes, investments, and expenses
- Generates AI recommendations with Gemini based on the current profile
- Recommendation data is stored in MongoDB and returned from `/api/recommendations`

### 4. AI Chat Support
- Conversational assistant available via `/api/chat`
- Uses a server-side chat service to forward user messages to Gemini AI

### 5. Dashboard Experience
- React + Vite frontend
- Tailwind CSS styling
- Tabs for profile, secure vault, command center, and recommendations
- Dynamic recommendation loading and scan animations

## Project Structure

```text
TaxWise/
├── client/                  # Frontend React + Vite app
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── utils/
│   └── package.json
├── server/                  # Backend Express API
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── services/
│   └── package.json
└── README.md
```

## Tech Stack

### Frontend
- React 19
- Vite
- Tailwind CSS
- React Router DOM
- Axios
- Google OAuth client

### Backend
- Node.js
- Express 5
- MongoDB + Mongoose
- Cloudinary
- Multer file upload
- Google Gemini AI
- Google OAuth token verification
- JWT auth

## Important Backend Endpoints

### Authentication
- `POST /api/auth/google` — Google OAuth login

### User Profile
- `GET /api/users/profile` — fetch current profile
- `PUT /api/users/profile` — update user profile fields

### Tax Profile
- `GET /api/tax-profile` — retrieve tax profile for current user
- `PUT /api/tax-profile` — update tax profile values

### Documents
- `POST /api/documents/parse` — upload and parse a document
- `GET /api/documents` — get user documents and tax profile

### Recommendations
- `GET /api/recommendations` — fetch AI tax recommendations

### Chat
- `POST /api/chat` — send a message and receive AI response

### Verification
- `POST /api/verify` — verify MSG91 access token

## Environment Variables

Create a `.env` file inside `server/` with:

```env
PORT=3001
MONGO_URI=<your-mongodb-uri>
JWT_SECRET=<your-jwt-secret>
GEMINI_API_KEY=<your-google-gemini-api-key>
GOOGLE_CLIENT_ID=<your-google-client-id>
CLOUDINARY_CLOUD_NAME=<cloudinary-cloud-name>
CLOUDINARY_API_KEY=<cloudinary-api-key>
CLOUDINARY_API_SECRET=<cloudinary-api-secret>
MSG91_AUTH_KEY=<your-msg91-auth-key>
```

### Frontend env
Create `.env` in `client/` with:

```env
VITE_GOOGLE_CLIENT_ID=<your-google-client-id>
```

## Setup & Run

### 1. Install dependencies

```bash
cd client
npm install
cd ../server
npm install
```

### 2. Start the backend

```bash
cd server
npm run dev
```

### 3. Start the frontend

```bash
cd ../client
npm run dev
```

## Data Model Summary

### User
- `name`, `email`, `password`, `googleId`, `provider`
- nested `profile` fields for PAN/Aadhaar, residential status, employment type, and income sources

### TaxProfile
- `income`, `deductions`, `taxes`, `investments`, `expenses`
- `taxSummary` for old/new regime, recommended regime, and tax saved
- `aiSuggestions`, `documents`, `profileStatus`, and `profileVersion`

### Document
- `userId`, `documentType`, `originalFileName`, `fileHash`
- `cloudinaryUrl`, `extractedData`

### Recommendation
- `recommendedRegime`, `estimatedSaving`
- `recommendations`, `governmentSchemes`, `missingDocuments`, `warnings`, `summary`

## Notes

- The document parsing pipeline uses Gemini AI to extract raw JSON from tax documents.
- Uploaded files are stored temporarily on disk and then uploaded to Cloudinary.
- The backend deletes local uploads after processing.
- The recommendation engine is triggered when the tax profile updates.
- The app is designed for guidance and should not replace certified tax advice.

## Disclaimer

TaxWise is an assistive application. It provides guidance and AI-generated recommendations, but it is not a substitute for professional tax or legal advice.

---