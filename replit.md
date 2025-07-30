# Replit.md

## Overview

This is a full-stack CV tailoring application built with React, Express, and PostgreSQL. The application allows users to upload their CV and job descriptions, then uses AI (through n8n workflows) to tailor the CV to match specific job requirements. The app features Replit authentication, real-time processing status updates, and a modern UI built with shadcn/ui components.

## User Preferences

Preferred communication style: Simple, everyday language.

## Recent Changes

**January 30, 2025 - SEO & GEO Optimization Complete**:
- **IMPLEMENTED: Comprehensive SEO optimization** - Added semantic HTML structure with proper header, main, section, and aria-labelledby attributes
- **CREATED: Complete meta tag suite** - Enhanced index.html with extensive SEO meta tags, Open Graph, Twitter Cards, and structured data schema
- **ADDED: SEO support files** - Created sitemap.xml and robots.txt for better search engine crawling and indexing
- **OPTIMIZED: Content for keywords** - Improved landing page copy to target "AI resume builder", "ATS optimization", "CV tailor", and related job search terms
- **ENHANCED: Accessibility** - Added proper heading hierarchy, semantic markup, and ARIA labels for screen readers
- **STRUCTURED: Content for search engines** - Organized landing page with clear sections and keyword-rich headings for better search visibility
- **IMPLEMENTED: Generative Engine Optimization (GEO)** - Added AI-specific meta tags, structured data for FAQ pages, and comprehensive content optimization for AI search engines
- **CREATED: AI-friendly content structure** - Added hidden content section with detailed ATS information, industry keywords, and statistical data optimized for AI engine parsing
- **ENHANCED: Problem-solution-impact framework** - Restructured content with clear data points and statistics that AI engines can easily reference and cite

**January 30, 2025**:
- **FIXED: CV display showing raw HTML instead of formatted content** - Added conditional HTML rendering with dangerouslySetInnerHTML for HTML content
- **FIXED: AI Comments extraction and display** - Corrected frontend logic to properly extract structured comments from server response
- **CLEANED: Removed all testing artifacts and debugging code** - Restored proper authentication system, removed test user bypass, cleaned console logs
- **VERIFIED: Authentication system fully functional** - Google OAuth working correctly, server returns proper 401 responses, frontend correctly shows Landing page for unauthenticated users
- **CONFIRMED: End-to-end debugging complete** - Application correctly routes unauthenticated users to Landing page with Google sign-in button, no test artifacts remaining

**January 23, 2025**:
- **FIXED: AI Comments section not displaying structured analysis** - Root cause was webhook returning comments under empty string key `""` instead of `"Comments"`
- Updated server extraction logic to specifically check for empty string key in webhook response
- Rebuilt frontend comments processing to handle exact webhook field names: "Overall Impression", "Alignment with JD", "Strengths", "Factual Accuracy & Corrections", "Formatting and Language Notes"
- Added proper field ordering and formatting for both string and array comment values
- AI Comments section now displays comprehensive structured analysis instead of fallback message

**January 22, 2025**:
- **FIXED: Webhook data propagation issue** - Implemented comprehensive field mapping in server response handler
- **FIXED: CV formatting issue** - Added professional text processor to parse raw CV content into structured, ATS-friendly format
- **FIXED: Logout functionality** - Changed from POST to GET endpoint to properly clear session and redirect
- Added support for 18+ possible CV content field names from n8n webhook responses  
- Enhanced comment extraction with 12+ possible field name variants
- Added robust object-to-string conversion for complex webhook data structures
- Fixed match score, keywords, and sections mapping with multiple field name variants
- CV processing now successfully displays webhook response data in UI with proper formatting
- Implemented intelligent CV text parsing that converts unreadable blob text into professional resume structure
- Fixed CV processing loading state that was getting stuck after webhook completion
- Restructured handleGenerateCV function with proper finally block to ensure loading state always resets
- Removed demo/fallback code that was masking real issues
- Fixed authentication loop issue in useAuth hook by using proper 401 error handling
- Cleaned up debugging statements and improved error handling throughout CV processing flow
- Refactored server webhook processing logic for clarity and robustness

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite with custom configuration for monorepo structure
- **Styling**: Tailwind CSS with shadcn/ui component library
- **Routing**: Wouter for client-side routing
- **State Management**: TanStack Query (React Query) for server state management
- **UI Components**: Radix UI primitives with custom styling via class-variance-authority

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Runtime**: Node.js with ESM modules
- **Database**: PostgreSQL with Drizzle ORM
- **Authentication**: Replit Auth with OpenID Connect
- **Session Management**: Express sessions stored in PostgreSQL
- **External Integration**: n8n webhook for AI processing

### Project Structure
```
/
├── client/          # React frontend
├── server/          # Express backend  
├── shared/          # Shared types and schemas
├── dist/            # Production build output
└── migrations/      # Database migrations
```

## Key Components

### Database Layer (Drizzle ORM)
- **Connection**: Neon PostgreSQL with connection pooling
- **Schema Management**: Type-safe schema definitions in `/shared/schema.ts`
- **Migration Strategy**: Drizzle-kit for schema changes and migrations

### Authentication System
- **Provider**: Replit Auth with OIDC
- **Session Storage**: PostgreSQL-backed sessions with connect-pg-simple
- **Security**: HTTP-only cookies with secure flags for production

### API Layer
- **Structure**: RESTful endpoints under `/api` prefix
- **Validation**: Zod schemas for request/response validation
- **Error Handling**: Centralized error middleware with structured responses

### Frontend State Management
- **Server State**: TanStack Query with custom query client
- **Local State**: React hooks for component-level state
- **Authentication State**: Custom `useAuth` hook with automatic retry logic

## Data Flow

### CV Processing Workflow
1. User uploads CV text and job description through dashboard
2. Server validates data using Zod schemas
3. Creates CV process record in database with "processing" status
4. Sends data to n8n webhook for AI processing
5. Frontend polls for updates using React Query with 2-second intervals
6. n8n workflow processes and returns tailored CV results
7. Results are displayed on dedicated results page

### Authentication Flow
1. User clicks sign-in, redirected to Replit Auth
2. After successful auth, user data is upserted to database
3. Session is created and stored in PostgreSQL
4. Frontend receives user data and updates auth state
5. Protected routes check authentication status

## External Dependencies

### Core Dependencies
- **Database**: @neondatabase/serverless for PostgreSQL connection
- **ORM**: drizzle-orm with drizzle-kit for migrations
- **Auth**: passport with openid-client for Replit Auth
- **UI Library**: @radix-ui components with Tailwind CSS
- **Validation**: Zod for schema validation

### Development Tools
- **Build**: Vite for frontend, esbuild for backend bundling
- **Type Checking**: TypeScript with strict mode enabled
- **Development**: tsx for running TypeScript directly

### External Services
- **n8n**: Webhook-based AI processing service (Production: http://69.62.112.163:5678/webhook/cv-review)
- **Replit**: Deployment platform

## Deployment Strategy

### Production Build
- Frontend: Vite builds to `/dist/public`
- Backend: esbuild bundles server to `/dist/index.js`
- Static assets: Served by Express in production

### Environment Configuration
- Database URL from Neon connection string
- Session secret for cookie encryption  
- n8n webhook URL for AI processing
- Replit Auth configuration (REPL_ID, domains)

### Development vs Production
- Development: Vite dev server with HMR and error overlay
- Production: Express serves static files and handles API routes
- Database: Same PostgreSQL instance for both environments

The application follows a clean separation of concerns with the frontend handling user interactions, the backend managing data and authentication, and external services handling AI processing. The architecture is designed for scalability and maintainability with type safety throughout the stack.