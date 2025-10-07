# DeepAtlas

DeepAtlas is an AI-powered geospatial analysis platform that combines interactive mapping with intelligent chat capabilities to analyze and visualize infrastructure risks, particularly focusing on flood risk assessment for public facilities.

## Overview

DeepAtlas provides an intuitive interface where users can interact with a map and an AI assistant to query and analyze geospatial data. The platform demonstrates risk assessment capabilities by identifying vulnerable public infrastructure (schools, hospitals, civic buildings) in flood-prone areas, complete with financial impact estimates.

### Key Features

- **Interactive Map Interface**: Built with Leaflet and MapTiler for rich geospatial visualization
- **AI Chat Assistant**: Natural language interface for querying infrastructure risks
- **Risk Assessment**: Identifies public facilities at risk with damage estimates
- **Real-time Updates**: Dynamic map updates based on chat queries
- **Drawing Tools**: Geoman integration for custom polygon creation
- **Responsive Design**: Modern UI with Ant Design components

## Architecture

The project consists of two main components:

### Backend (Node.js + Fastify)
- RESTful API built with Fastify framework
- DynamoDB integration for chat persistence
- JWT authentication system
- Rate limiting and security middleware (Helmet, CORS)
- Swagger documentation support
- Error handling with custom error management

### Frontend (React + TypeScript)
- React 18 with TypeScript
- Leaflet for interactive mapping
- MapTiler for base map layers
- Ant Design for UI components
- React Router for navigation
- Context API for state management

## Prerequisites

- **Node.js** (v18 or higher)
- **Docker** and **Docker Compose** (for containerized deployment)
- **AWS Account** (for DynamoDB, or LocalStack for local development)
- **MapTiler API Key** (free tier available at https://maptiler.com)

## Environment Setup

### Backend Environment Variables

Create a `.env` file in `backend/app/env/`:

```env
# Database Configuration
POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DB=your_database_name

# Network Configuration
API_PORT=3000
POSTGRES_PORT=5432
API_IP=172.20.0.2
POSTGRES_IP=172.20.0.3
PRISMA_IP=172.20.0.4
SUBNET=172.20.0.0/16

# Prisma Configuration
DOCKER_DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${POSTGRES_IP}:5432/${POSTGRES_DB}

# JWT Configuration
JWT_SECRET=your_secure_jwt_secret

# AWS Configuration (for DynamoDB)
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_DEFAULT_REGION=us-east-1

# DynamoDB Table
CHATS_TABLE=Chats

# Stage (dev/prod)
STAGE=dev

# API Configuration
API_BASE_URL=http://localhost:3000
```

### Frontend Configuration

Update `frontend/src/utils/ApiURL.json`:

```json
{
  "API_URL": "http://localhost:3000",
  "WS_URL": "wss://localhost:3000"
}
```

Update the MapTiler API key in `frontend/src/components/MapComponent.tsx`:

```typescript
const mtLayer = new MaptilerLayer({
  apiKey: "YOUR_MAPTILER_API_KEY",
  style: "YOUR_STYLE_ID",
});
```

## Installation & Running

### Option 1: Docker Compose (Recommended)

1. **Clone the repository**
   ```bash
   git clone https://github.com/EpsilonFO/DeepAtlas.git
   cd DeepAtlas
   ```

2. **Set up environment variables**
   ```bash
   cp backend/app/env/.env_example backend/app/env/.env
   # Edit .env with your configuration
   ```

3. **Build and run with Docker Compose**
   ```bash
   docker-compose up --build
   ```

4. **Access the application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3000
   - API Documentation: http://localhost:3000/docs

### Option 2: Local Development

#### Backend Setup

```bash
cd backend/app
npm install
npm run dev
```

The backend will start on port 3000.

#### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will start on port 5173.

## Project Structure

```
deepatlas/
├── backend/
│   └── app/
│       ├── src/
│       │   ├── config/          # Server configuration (CORS, Helmet, Rate Limiting)
│       │   ├── entities/        # TypeScript interfaces and schemas
│       │   ├── enums/           # Enumerations (HTTP status, errors, MIME types)
│       │   ├── libs/            # Utility libraries (error handling, pagination)
│       │   ├── middlewares/     # Express-like middlewares (auth, error catching)
│       │   ├── repositories/    # Data access layer
│       │   ├── routes/          # API endpoints
│       │   │   ├── chat/        # Chat-related endpoints
│       │   │   └── health/      # Health check endpoint
│       │   └── services/        # Business logic (DynamoDB, JWT, Chat)
│       ├── docker/              # Docker configuration
│       └── env/                 # Environment variables
│
├── frontend/
│   └── src/
│       ├── components/          # Reusable React components
│       │   ├── dashboardLayout/ # Main layout component
│       │   ├── sidebar/         # Chat sidebar
│       │   ├── tchat/           # Chat components
│       │   └── ErrorBoundary/   # Error handling
│       ├── views/               # Page-level components
│       │   ├── chat/            # Chat container view
│       │   ├── homeContent/     # Home page
│       │   └── notFound/        # 404 page
│       ├── contexts/            # React Context providers
│       ├── config/              # App configuration
│       ├── utils/               # Utility functions and plugins
│       └── services/            # API communication layer
│
└── docker-compose.yml           # Docker orchestration
```

## API Endpoints

### Chat Endpoints

- `POST /chat` - Create a new chat
  ```json
  {
    "chatId": "uuid",
    "prompt": "Your question here"
  }
  ```

- `GET /chat/:chatId` - Retrieve chat messages
- `PUT /chat/:chatId` - Append message to existing chat
  ```json
  {
    "prompt": "Your follow-up question"
  }
  ```

### Health Check

- `GET /health` - Service health status

## Features in Detail

### Map Functionality

- **Interactive Visualization**: Pan, zoom, and explore geospatial data
- **Custom Polygons**: Draw risk zones using Geoman tools
- **Marker Clustering**: Efficient display of multiple infrastructure points
- **Heatmaps**: Visualize risk intensity across regions
- **Search**: Find locations and infrastructure by name
- **Scale Control**: Distance measurement tool

### Chat Intelligence

The AI assistant can:
- Answer queries about infrastructure risks
- Identify vulnerable public facilities
- Provide damage estimates
- Update the map with relevant markers and polygons
- Simulate response times for realistic interactions

### Current Demo Focus

The application demonstrates flood risk assessment for Nice, France, including:
- **51+ public facilities** at risk (schools, hospitals, civic buildings)
- **Risk zones** highlighted with custom polygons
- **Financial impact estimates** (up to €15 million for major flooding)
- **Interactive exploration** of vulnerable infrastructure

## Technologies Used

### Backend
- **Fastify** - High-performance web framework
- **TypeScript** - Type-safe JavaScript
- **DynamoDB** - NoSQL database for chat storage
- **AWS SDK** - Cloud services integration
- **JSON Web Tokens** - Authentication
- **Bcrypt** - Password encryption
- **AJV** - JSON schema validation

### Frontend
- **React 18** - UI framework
- **TypeScript** - Type safety
- **Leaflet** - Interactive maps
- **MapTiler** - Map tile provider
- **Ant Design** - UI component library
- **Geoman** - Drawing and editing tools
- **React Router** - Client-side routing
- **UUID** - Unique identifier generation

### DevOps
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **LocalStack** - Local AWS services emulation

## Development Notes

### Demo Mode

For demonstration purposes, the chat functionality includes a simulated response feature that:
- Generates responses after a random 5-10 second delay
- Displays predefined risk assessment data for Nice
- Automatically updates the map with relevant markers
- Shows a modal indicating the feature is in demo mode

To enable real AI responses, integrate with your preferred AI service in `backend/app/src/services/chat-service.ts`.

### Map Customization

Custom polygons and markers can be added in:
- `frontend/src/utils/customPolygon.ts` - Predefined risk zones
- `frontend/src/views/chat/ChatContainer.tsx` - Dynamic map updates

## Troubleshooting

### Backend Issues

- **Port already in use**: Change `API_PORT` in `.env`
- **DynamoDB connection fails**: Verify AWS credentials or ensure LocalStack is running
- **CORS errors**: Check CORS configuration in `backend/app/src/config/cors.ts`

### Frontend Issues

- **Map doesn't load**: Verify MapTiler API key
- **API connection fails**: Check `API_URL` in `ApiURL.json`
- **Build errors**: Clear `node_modules` and reinstall dependencies

## Future Enhancements

- [ ] Real AI integration (OpenAI, Claude, or custom model)
- [ ] Multi-region support
- [ ] Historical data analysis
- [ ] Real-time sensor data integration
- [ ] Export functionality for reports
- [ ] User authentication and authorization
- [ ] Multi-language support

## License

This project is proprietary software. All rights reserved.

## Contributors

Developed as part of a geospatial risk assessment initiative.

## Support

For issues or questions, please contact the development team or create an issue in the repository.
