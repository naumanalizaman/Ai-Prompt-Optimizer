# AI Prompt Optimizer

A full-stack web app that transforms rough prompts into optimized, structured instructions using Anthropic Claude. Built with Next.js 15, TypeScript, PostgreSQL, and Tailwind CSS.

## Features

- **AI Prompt Optimization** - Claude-powered optimization adds structure, context, and clarity
- **Goal-Based Optimization** - Optimize for creative writing, technical docs, marketing, coding, and more
- **Multi-Model Support** - Target Claude 3.5 Sonnet, Opus, GPT-4, and more
- **Before/After Comparison** - See exactly what changed and why
- **Reusable Templates** - Save and reuse your best prompt patterns
- **Optimization History** - Track all past optimizations with search and pagination
- **Usage Limits** - Free tier (10/day) and Premium (unlimited) plans
- **Authentication** - Email/password + Google OAuth via NextAuth.js
- **Firebase Integration** - Firestore database + Firebase Auth support
- **Responsive Design** - Mobile-first, clean, premium UI

## Tech Stack

- **Framework**: Next.js 15 (App Router)
- **Language**: TypeScript
- **Database**: PostgreSQL + Prisma ORM (primary), Firebase Firestore (alternative)
- **Authentication**: NextAuth.js (Credentials + Google OAuth), Firebase Auth (alternative)
- **AI**: Anthropic Claude API
- **Styling**: Tailwind CSS
- **Deployment**: Vercel-ready

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL database
- Anthropic API key

### Setup

1. **Clone and install**:
```bash
npm install
```

2. **Set up environment variables**:
```bash
cp .env.example .env.local
```
Edit `.env.local` with your values:
```env
DATABASE_URL="postgresql://user:password@localhost:5432/prompt_optimizer"
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with-openssl-rand-base64-32"
ANTHROPIC_API_KEY="sk-ant-your-key"
```

3. **Set up the database**:
```bash
npx prisma db push
```

4. **Run the development server**:
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Deployment (Vercel)

1. Push to GitHub
2. Import project in Vercel
3. Add environment variables in Vercel dashboard
4. Set up a managed PostgreSQL database (e.g., Supabase or Neon) and add its connection string as `DATABASE_URL` in Vercel. The Prisma schema is synchronized during each Vercel build.
5. Add `NEXTAUTH_URL` (your production URL) and a strong `NEXTAUTH_SECRET` in Vercel. Firebase configuration variables do not replace `DATABASE_URL` for registration.
6. Deploy

## Project Structure

```
src/
├── app/                    # Next.js App Router pages & API routes
│   ├── api/               # API endpoints (optimize, history, templates, usage)
│   ├── auth/              # Login & register pages
│   ├── dashboard/         # Dashboard, history, templates pages
│   ├── optimize/          # Main optimizer interface
│   ├── pricing/           # Pricing page
│   └── layout.tsx         # Root layout
├── components/            # React components
│   ├── ui/               # Reusable UI primitives
│   ├── layout/           # Header, Footer
│   └── optimize/         # Optimizer-specific components
├── lib/                   # Shared utilities
│   ├── auth.ts           # NextAuth configuration
│   ├── ai.ts             # Claude API integration
│   ├── prisma.ts         # Prisma client
│   ├── utils.ts          # Helper functions
│   └── validators.ts     # Zod schemas
└── types/                 # TypeScript types
```

## Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm start` - Start production server
- `npx prisma studio` - Open database GUI

## Firebase Integration

The project includes Firebase Firestore as an alternative database backend.

### Setup Firebase

1. **Create a Firebase project** at [console.firebase.google.com](https://console.firebase.google.com)

2. **Enable Firestore Database**:
   - Go to Firestore > Create database
   - Choose production or test mode
   - Select your preferred location

3. **Get your config**:
   - Go to Project Settings > General > Your apps
   - Copy the Firebase config object

4. **Add to environment variables**:
   ```env
   NEXT_PUBLIC_FIREBASE_API_KEY="your-api-key"
   NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="your-project.firebaseapp.com"
   NEXT_PUBLIC_FIREBASE_PROJECT_ID="your-project-id"
   NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET="your-project.firebasestorage.app"
   NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID="your-sender-id"
   NEXT_PUBLIC_FIREBASE_APP_ID="your-app-id"
   NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID="your-measurement-id"
   ```

5. **Deploy Firestore rules**:
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase deploy --only firestore:rules
   ```

### Using Firestore

The project includes Firestore-backed API routes:

- `/api/history-firestore` - CRUD operations for prompts
- `/api/templates-firestore` - CRUD operations for templates
- `/api/usage-firestore` - Usage tracking

### Firestore Collections

- **prompts** - Stores optimized prompts with metadata
- **templates** - Reusable prompt templates
- **usage** - Daily usage tracking per user

### Security Rules

Firestore security rules are in `firestore.rules`. They ensure:
- Users can only access their own data
- Public templates are readable by all authenticated users
- Usage records are private to each user
