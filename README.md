# Project Chimera - Infinite Spatial Canvas (XR)

## Overview
Project Chimera is a web-based 3D collaboration platform that provides real-time multi-user interaction, 3D model viewing, and AI-powered analysis capabilities. The platform mirrors XR/VR headset interfaces in a browser environment, making advanced spatial collaboration accessible without specialized hardware.

## Purpose & Goals
- Enable geographically distributed teams to collaborate on 3D models in real-time
- Provide AI-powered analysis and suggestions for 3D model optimization
- Support multiple 3D file formats (GLTF, GLB, OBJ, FBX)
- Integrate external data sources via Model Context Protocol (MCP)

## Recent Changes

### 2025-11-14: MCP and AI Integration
- **Anthropic Claude Integration**: Added AI analysis service using Claude Sonnet 4 for automatic 3D model analysis and annotation suggestions
- **MCP Server Architecture**: Implemented Model Context Protocol server with tools for:
  - 3D model metadata retrieval
  - GitHub repository integration (placeholder for future implementation)
  - Jira issues integration (placeholder for future implementation)
  - Model complexity analysis
- **Backend APIs**: Created REST endpoints for model management, annotations, and AI analysis
- **WebSocket Support**: Real-time collaboration features for cursor tracking and annotation synchronization
- **AI Analysis Panel**: New UI component showing AI-generated suggestions with approve/reject workflow
- **PostgreSQL Database**: Database created for persistent storage (schema ready for migration)

## Project Architecture

### Frontend (`client/`)
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter for client-side routing
- **State Management**: React Query for server state, React hooks for local state
- **3D Rendering**: Three.js for WebGL-based 3D visualization
- **UI Components**: Shadcn UI + Tailwind CSS with XR-inspired design
- **Real-time**: Socket.io-client for WebSocket connections

### Backend (`server/`)
- **Runtime**: Node.js with Express
- **Storage**: In-memory storage (MemStorage) with interface ready for PostgreSQL migration
- **AI Service**: Anthropic Claude Sonnet 4 integration for model analysis
- **MCP Server**: Model Context Protocol server for external data integration
- **WebSocket**: Socket.io for real-time collaboration features
- **File Upload**: Multer with disk storage (`/uploads` directory)

### Key Components

#### 3D Collaboration
- `Scene3D.tsx`: Main 3D viewport using Three.js with orbit controls
- `TopBar.tsx`: Session management, user presence indicators
- `ModelLibraryPanel.tsx`: 3D model upload and management
- `UserPresencePanel.tsx`: Live user presence with camera positions
- `BottomToolbar.tsx`: View modes, manipulation tools, visualization controls

#### AI Features
- `AIAnalysisPanel.tsx`: Display AI-generated suggestions with approval workflow
- `server/ai-service.ts`: Claude integration for model analysis
- `server/mcp-server.ts`: MCP server for external data source integration

#### Data Schema (`shared/schema.ts`)
- **Users**: Basic user authentication structure
- **Models**: 3D model metadata (name, file URL, type, size, uploader)
- **Annotations**: Spatial annotations with 3D positions and descriptions

## User Preferences
- **Design Language**: XR/Spatial computing inspired (Apple Vision Pro + Material Design)
- **Color Scheme**: Dark mode primary, light mode support
- **Fonts**: Inter for UI, JetBrains Mono for technical data
- **AI Provider**: Anthropic Claude (Sonnet 4)

## External Integrations

### Configured
- **Anthropic AI**: Claude Sonnet 4 for model analysis (requires `ANTHROPIC_API_KEY`)
- **Socket.io**: Real-time collaboration
- **PostgreSQL**: Database for persistent storage

### Planned (MCP Connectors)
- **GitHub**: Code repository integration for project context
- **Jira**: Issue tracking integration for model-related tasks
- **3D Model Repositories**: External CAD/model database connectors

## Environment Variables
- `ANTHROPIC_API_KEY`: Anthropic AI API key for Claude integration
- `DATABASE_URL`: PostgreSQL connection string
- `SESSION_SECRET`: Express session secret
- `NODE_ENV`: Environment (development/production)

## Development Commands
- `npm run dev`: Start development server (frontend + backend)
- `npm run build`: Build for production
- `npm start`: Run production build
- `npm run db:push`: Push database schema changes

## API Endpoints

### Models
- `GET /api/models` - List all models
- `GET /api/models/:id` - Get model by ID
- `POST /api/models` - Upload new model (multipart/form-data)
- `DELETE /api/models/:id` - Delete model

### Annotations
- `GET /api/annotations/:roomId` - Get annotations for a room
- `POST /api/annotations` - Create annotation
- `DELETE /api/annotations/:id` - Delete annotation

### AI Analysis
- `POST /api/ai/analyze-model` - Analyze model with Claude (requires modelId, optional screenshot)
- `POST /api/ai/analyze-model/stream` - Stream analysis results (Server-Sent Events)

### WebSocket Events
- `join-room` / `leave-room`: Room management
- `cursor-update` / `cursor-moved`: Real-time cursor tracking
- `annotation-created` / `annotation-deleted`: Annotation synchronization

## Known Limitations & Future Work

### Current Implementation
- In-memory storage (data lost on restart)
- Mock MCP connectors for GitHub/Jira (need real API integrations)
- File uploads stored locally (need cloud storage for production)
- No authentication/authorization system
- Frontend uses mock data for initial demo

### Planned Improvements
1. **Database Migration**: Move from MemStorage to PostgreSQL
2. **Authentication**: Implement user authentication and session management
3. **Real MCP Connectors**: Integrate actual GitHub and Jira APIs
4. **Cloud File Storage**: Move uploads to S3 or similar service
5. **Real-time AI Streaming**: Complete WebSocket integration for analysis progress
6. **Model Renderer**: Load actual 3D model files in the scene
7. **Annotation Placement**: Allow users to place annotations in 3D space by clicking

## Deployment Notes
- Workflow: "Start application" runs `npm run dev` on port 5000
- Frontend served via Vite dev server
- Backend Express server on same port with Vite middleware
- Static assets served from `/uploads` directory
- PostgreSQL database available via `DATABASE_URL`

## Architecture Decisions

### Why MCP?
Model Context Protocol provides a standardized way to connect AI systems to external data sources. This allows Claude to access project context (GitHub repos, Jira issues, model metadata) when analyzing 3D models.

### Why In-Memory Storage Initially?
Rapid prototyping and development velocity. The storage interface (`IStorage`) makes migration to PostgreSQL straightforward when needed.

### Why Three.js Instead of React Three Fiber?
Avoided React 18/19 peer dependency conflicts in the current Replit environment. Three.js provides full control and is well-suited for the spatial UI requirements.

### Why Claude Sonnet 4?
Superior technical analysis capabilities, excellent for engineering/CAD review, and strong JSON output reliability for structured suggestions.
