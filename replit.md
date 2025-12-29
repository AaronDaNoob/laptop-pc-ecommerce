# Club Laptops - E-commerce Platform

## Overview

Club Laptops is a full-stack e-commerce web application for a premium laptop retail store. The platform enables customers to browse and purchase high-performance laptops, book repair appointments, and submit trade-in requests for their used devices. It includes customer-facing pages (home, shop, services, checkout, account) and an admin dashboard for managing orders, repairs, and sell requests.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight client-side routing)
- **State Management**: TanStack React Query for server state, React Context for cart state
- **UI Components**: shadcn/ui component library built on Radix UI primitives
- **Styling**: Tailwind CSS v4 with custom theme variables and CSS-in-JS via class-variance-authority
- **Forms**: React Hook Form with Zod validation
- **Animations**: Framer Motion for micro-interactions

### Backend Architecture
- **Runtime**: Node.js with Express.js
- **Language**: TypeScript with ESM modules
- **API Design**: RESTful JSON API endpoints under `/api/*`
- **Authentication**: Replit Auth integration using OpenID Connect with Passport.js
- **Session Management**: Express sessions stored in PostgreSQL via connect-pg-simple

### Data Storage
- **Database**: PostgreSQL
- **ORM**: Drizzle ORM with drizzle-zod for schema validation
- **Schema Location**: `shared/schema.ts` contains all table definitions
- **Tables**: 
  - `users` and `sessions` (authentication)
  - `repair_appointments` (service bookings)
  - `sell_requests` (trade-in submissions)
  - `orders` (customer purchases)

### Build System
- **Dev Server**: Vite for frontend with HMR
- **Production Build**: Custom build script using esbuild for server bundling, Vite for client
- **Output**: Server compiled to `dist/index.cjs`, client assets to `dist/public`

### Key Design Patterns
- **Shared Types**: Schema definitions in `shared/` are imported by both client and server
- **Storage Abstraction**: `IStorage` interface in `server/storage.ts` enables database operations
- **Auth Isolation**: Authentication logic separated into `server/replit_integrations/auth/`
- **Component Structure**: UI primitives in `components/ui/`, layout components in `components/layout/`, page-specific components alongside pages

## External Dependencies

### Database
- PostgreSQL database via `DATABASE_URL` environment variable
- Drizzle Kit for schema migrations (`npm run db:push`)

### Authentication
- Replit Auth (OpenID Connect provider)
- Requires `ISSUER_URL`, `REPL_ID`, and `SESSION_SECRET` environment variables

### Third-Party Libraries
- **UI**: Radix UI primitives, Lucide icons, Embla Carousel
- **Utilities**: date-fns for date formatting, zod for validation, nanoid for ID generation
- **Development**: Replit-specific Vite plugins for dev banner and error overlays

### Static Assets
- Product and service images stored in `attached_assets/generated_images/`
- Custom fonts loaded from Google Fonts (Rajdhani, Space Grotesk)