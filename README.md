# Converso - Real-time AI Teaching Platform

Converso is a modern SaaS application that provides real-time voice-based AI tutoring sessions. Students can create personalized AI companions for various subjects, engage in interactive voice conversations, and track their learning journey.

## Project Overview

Converso enables users to:
- Create custom AI teaching companions for different subjects and topics
- Engage in real-time voice conversations with AI tutors
- Track learning progress and session history
- Bookmark favorite companions
- Manage subscriptions with tiered access levels

## Core Functionalities

### 1. AI Companion Management
- **Create Companions**: Users can create personalized AI teaching companions with:
  - Custom name and subject (Maths, Science, Language, History, Coding, Economics)
  - Specific topics to learn
  - Voice preferences (Male/Female, Casual/Formal)
  - Estimated session duration
- **Companion Library**: Browse and search through all available companions with filtering by subject and topic
- **Companion Limits**: Tiered subscription system controls how many companions users can create (3, 10, or unlimited for Pro plan)

### 2. Real-time Voice Sessions
- **Voice Conversations**: Interactive voice-based learning sessions powered by VAPI AI
- **Live Transcripts**: Real-time transcription of conversations displayed during sessions
- **Session Controls**: 
  - Start/End session buttons
  - Microphone mute/unmute functionality
  - Visual feedback with Lottie animations during active sessions
- **Session History**: Automatic tracking of completed sessions

### 3. User Journey & Profile
- **My Journey Page**: Personal dashboard showing:
  - Total lessons completed
  - Number of companions created
  - Bookmarked companions
  - Recent session history
  - User-created companions
- **Progress Tracking**: Visual statistics and organized accordion sections for different data categories

### 4. Search & Discovery
- **Subject Filtering**: Filter companions by subject category
- **Topic Search**: Search companions by topic or name
- **Popular Companions**: Homepage displays popular and recently completed sessions

### 5. Authentication & Authorization
- **Clerk Integration**: Secure authentication with sign-in/sign-up
- **User Profiles**: Integrated user management with profile images and information
- **Subscription Management**: Clerk-based pricing table for subscription tiers

### 6. Error Monitoring
- **Sentry Integration**: Comprehensive error tracking and monitoring
- **Source Maps**: Configured for better debugging in production

## Technology Stack

### Frontend
- **Next.js 15.4.6** - React framework with App Router
- **React 19.1.0** - UI library
- **TypeScript 5** - Type-safe development
- **Tailwind CSS 4** - Utility-first CSS framework
- **Radix UI** - Accessible component primitives (Accordion, Select, Label, Slot)
- **Lottie React** - Animation library for visual feedback
- **Lucide React** - Icon library

### Backend & Services
- **Supabase** - PostgreSQL database and backend services
- **Clerk** - Authentication and user management
- **VAPI AI** - Real-time voice AI platform for conversations
- **Express 5.1.0** - Server framework (if needed for custom endpoints)

### Form Management & Validation
- **React Hook Form 7.62.0** - Form state management
- **Zod 4.1.5** - Schema validation
- **@hookform/resolvers** - Zod integration for React Hook Form

### Monitoring & Observability
- **Sentry** - Error tracking and performance monitoring
- **OpenTelemetry** - Distributed tracing support

### Development Tools
- **ESLint** - Code linting
- **Turbopack** - Fast bundler for development
- **PostCSS** - CSS processing


## Getting Started

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd SaaS-Application
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   # or
   pnpm install
   ```

3. **Set up environment variables**

   Create a `.env.local` file in the root directory with the following variables:

   ```env
   # Clerk Authentication
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
   CLERK_SECRET_KEY=your_clerk_secret_key

   # Supabase Database
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key

   # VAPI AI
   NEXT_PUBLIC_VAPI_WEB_TOKEN=your_vapi_web_token

   # Sentry (Optional)
   SENTRY_DSN=your_sentry_dsn
   SENTRY_ORG=jsmpro
   SENTRY_PROJECT=jsm_converso
   SENTRY_AUTH_TOKEN=your_sentry_auth_token
   ```

4. **Set up Supabase Database**

   Create the following tables in your Supabase database:

   ```sql
   -- Companions table
   CREATE TABLE companions (
     id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
     name TEXT NOT NULL,
     subject TEXT NOT NULL,
     topic TEXT NOT NULL,
     voice TEXT NOT NULL,
     style TEXT NOT NULL,
     duration INTEGER NOT NULL,
     author TEXT NOT NULL,
     created_at TIMESTAMP DEFAULT NOW()
   );

   -- Session history table
   CREATE TABLE session_history (
     id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
     companion_id UUID REFERENCES companions(id),
     user_id TEXT NOT NULL,
     created_at TIMESTAMP DEFAULT NOW()
   );

   -- Bookmarks table
   CREATE TABLE bookmarks (
     id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
     companion_id UUID REFERENCES companions(id),
     user_id TEXT NOT NULL,
     created_at TIMESTAMP DEFAULT NOW(),
     UNIQUE(companion_id, user_id)
   );
   ```

5. **Configure Clerk**

   - Set up your Clerk application at [clerk.com](https://clerk.com)
   - Configure authentication providers
   - Set up subscription plans and features:
     - `3_companion_limit` feature
     - `10_companion_limit` feature
     - `pro` plan (unlimited companions)

6. **Configure VAPI AI**

   - Sign up at [vapi.ai](https://vapi.ai)
   - Get your web token
   - The application uses 11Labs for voice synthesis and Deepgram for transcription

### Running the Development Server

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to see the application.

### Building for Production

```bash
npm run build
npm start
```

## Key Features Explained

### Voice AI Integration

The application uses VAPI AI to power real-time voice conversations:
- **Voice Provider**: 11Labs with multiple voice options
- **Transcription**: Deepgram Nova-3 model
- **AI Model**: GPT-4 for conversation intelligence
- **Real-time Events**: Call start/end, speech detection, message transcription

### Database Schema

The application uses Supabase (PostgreSQL) with three main tables:
- **companions**: Stores AI companion configurations
- **session_history**: Tracks user learning sessions
- **bookmarks**: User bookmarked companions

### Authentication Flow

Clerk handles all authentication:
- Protected routes via middleware
- User session management
- Subscription and feature checks
- User profile integration

### Subscription Tiers

- **Free Tier**: Limited companion creation (if configured)
- **3 Companion Limit**: Feature flag for 3 companions
- **10 Companion Limit**: Feature flag for 10 companions
- **Pro Plan**: Unlimited companions

## Development Notes

- The project uses Next.js App Router with Server Components by default
- Server Actions are used for database operations (marked with `'use server'`)
- TypeScript strict mode is enabled
- ESLint and TypeScript errors are ignored during builds (configured in `next.config.ts`)
- Turbopack is used for faster development builds

## Deployment

The application is configured for deployment on Vercel with:
- Automatic Sentry source map uploads
- Vercel Cron Monitor integration
- Optimized builds with Turbopack
