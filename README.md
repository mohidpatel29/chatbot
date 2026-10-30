# Enterprise AI Chatbot

A production-ready, full-stack AI Chatbot application built with **Next.js (App Router)**, **TypeScript**, **Tailwind CSS**, the official **OpenAI API SDK** (with real-time streaming), and **Supabase** (Authentication, PostgreSQL Database, and Row Level Security).

---

## 1. Overview & Architecture

This application delivers a high-performance, responsive AI chat experience mirroring modern AI platforms like ChatGPT, Claude, and Gemini.

### Architectural Highlights
- **Real-time Server-Sent Streaming**: Responses stream progressively from OpenAI via Next.js Route Handlers using native `ReadableStream` and `TextEncoder`/`TextDecoder`.
- **Hybrid Storage Model**:
  - **Cloud Mode (Authenticated)**: Messages and conversations are securely stored in Supabase with strict PostgreSQL Row Level Security (RLS) policies ensuring users can only read/write their own records.
  - **Guest Mode (Unauthenticated)**: Instant usage out of the box with conversations stored locally in browser `localStorage`, with clear message quota indicators and a seamless upgrade path to cloud sync.
- **Client Resilience**: AbortController enables instantaneous generation cancellation ("Stop" button), retry handling with one click, and robust error surfacing for API quotas, timeouts, and authentication errors.
- **Rich Markdown Support**: Syntax highlighting with language tags, one-click code copy, rendered tables, GFM task lists, blockquotes, and link sanitization.

---

## 2. Technology Stack

| Layer | Technology | Details |
|---|---|---|
| **Frontend** | Next.js (App Router), React 18, TypeScript | Fast server and client components, strict typing |
| **Styling** | Tailwind CSS | Fully responsive, dark & light themes, mobile drawer |
| **AI Integration** | Official OpenAI Node.js SDK (`openai`) | Streaming completion API, sliding-window history management |
| **Database & Auth** | Supabase (`@supabase/supabase-js`, `@supabase/ssr`) | PostgreSQL, Auth session handling, Row Level Security |
| **Icons & UI** | Lucide React | Clean, lightweight SVG icons |
| **Markdown** | `react-markdown`, `remark-gfm` | Tables, code blocks, formatting |

---

## 3. Implemented Features Matrix

- [x] **Real-time chat interface**: Responsive message bubbles, user and assistant avatars, timestamps.
- [x] **AI-generated streaming responses**: Real-time chunk decoding directly to the UI without blocking.
- [x] **Context-aware follow-ups**: Sliding message window (recent turns + system prompt) passed to OpenAI to maintain conversational context while respecting token limits.
- [x] **Conversation management**: New chat, inline conversation renaming, conversation deletion with confirmation, and instant switching.
- [x] **Markdown rendering**: Full support for code blocks (with language badges and copy button), headings, bullet lists, tables, and quotes.
- [x] **Copy-response button**: One-click message copying with visual checkmark feedback.
- [x] **Loading & Stop indicators**: Animated typing dots during initial dispatch, active generation indicator, and a functioning **Stop Generation** button powered by `AbortController`.
- [x] **Retry button**: One-click regeneration button on failed or previous assistant responses.
- [x] **Responsive design**: Collapsible mobile sidebar with backdrop, desktop full-height view, auto-expanding composer textarea.
- [x] **Dark & Light themes**: Persisted theme toggle supporting system preferences and manual override.
- [x] **Suggested prompts**: Categorized welcome cards (Coding, Architecture, Debugging, Brainstorming) for first-time or empty chats.
- [x] **Robust error handling**: Detailed diagnostic translation for invalid API keys (401), rate limits/quotas (429), timeouts (504), and oversized payloads (413).
- [x] **Supabase Auth & Database**: Email/password authentication, cloud conversation persistence, Row Level Security.
- [x] **Guest Mode**: Full offline usability with local storage and guest quota badge.

---

## 4. Project Structure

```
ai-chatbot/
├── app/
│   ├── api/
│   │   ├── chat/
│   │   │   └── route.ts             # OpenAI streaming handler with error translation
│   │   └── conversations/
│   │       ├── route.ts             # Cloud conversation list & create endpoints
│   │       └── [id]/
│   │           └── route.ts         # Cloud conversation fetch, rename, delete
│   ├── globals.css                  # Tailwind styles and custom animations
│   ├── layout.tsx                   # Next.js RootLayout with dark mode class
│   └── page.tsx                     # Main application entry point
├── components/
│   ├── AuthModal.tsx                # Supabase Email/Password modal & guest notice
│   ├── ChatContainer.tsx            # Main application controller & state orchestrator
│   ├── ChatInput.tsx                # Auto-expanding composer with Enter/Stop controls
│   ├── ChatMessageItem.tsx          # Single message bubble with copy & retry
│   ├── ChatMessageList.tsx          # Message stream with auto-scroll & welcome screen
│   ├── ChatSidebar.tsx              # Conversation drawer, rename, delete, & user menu
│   ├── CodeBlock.tsx                # Code block with syntax styling & copy button
│   ├── MarkdownRenderer.tsx         # React-Markdown with remark-gfm & table support
│   ├── SettingsModal.tsx            # Model selector, temperature, & system prompt
│   ├── ThemeToggle.tsx              # Dark/light theme switcher
│   └── WelcomeScreen.tsx            # First-time user greeting & prompt suggestions
├── lib/
│   ├── openai.ts                    # OpenAI client, message sliding window, error formatting
│   ├── storage.ts                   # Guest Mode localStorage persistence & limits
│   ├── types.ts                     # TypeScript definitions for messages, chats, user
│   ├── utils.ts                     # Classnames, UUID, date & title formatters
│   └── supabase/
│       ├── client.ts                # Browser Supabase client with graceful fallback
│       └── server.ts                # Server-side Supabase client with Bearer auth
├── supabase/
│   └── migrations/
│       └── 001_create_chat_schema.sql # PostgreSQL schema with RLS policies & triggers
├── scripts/
│   └── test-api.mjs                 # Automated unit and contract test suite
├── .env.example                     # Environment template with placeholder values
├── .env.local                       # Local environment configuration
├── next.config.js                   # Next.js configuration
├── package.json                     # Package dependencies and npm scripts
├── postcss.config.js                # PostCSS configuration
├── tailwind.config.js               # Tailwind CSS theme configuration
└── tsconfig.json                    # Strict TypeScript configuration
```

---

## 5. Local Setup Instructions

### Prerequisites
- Node.js 18.14+ (or Node 20 / 22)
- npm or pnpm

### 1. Clone or Navigate to the Project
```bash
cd C:\Users\HP\.gemini\antigravity\scratch\ai-chatbot
```

### 2. Configure Environment Variables
Copy `.env.example` to `.env.local`:
```bash
cp .env.example .env.local
```

Open `.env.local` and add your OpenAI API Key:
```env
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
OPENAI_MODEL=gpt-4o-mini
```

*(Optional)* To enable Supabase cloud accounts:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOi...
```

### 3. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 4. Run Automated Tests
```bash
npm test
```

### 5. Run Type Check & Production Build
```bash
npm run typecheck
npm run build
```

---

## 6. Supabase Database Setup & RLS Migrations

If you wish to use Supabase cloud persistence:

1. Create a new project at [database.new](https://database.new) or [supabase.com](https://supabase.com).
2. Go to the **SQL Editor** in your Supabase dashboard.
3. Open [`supabase/migrations/001_create_chat_schema.sql`](file:///C:/Users/HP/.gemini/antigravity/scratch/ai-chatbot/supabase/migrations/001_create_chat_schema.sql).
4. Paste the SQL script and click **Run**.
5. Go to **Project Settings** -> **API** to copy:
   - **Project URL** -> `NEXT_PUBLIC_SUPABASE_URL`
   - **anon / public key** -> `NEXT_PUBLIC_SUPABASE_ANON_KEY`
6. Paste them into your `.env.local` or Vercel environment variables.

---

## 7. Deployment to Vercel

1. Push your repository to GitHub, GitLab, or Bitbucket.
2. Sign in to [Vercel](https://vercel.com) and click **Add New Project**.
3. Import your repository and select **Next.js** framework preset.
4. Under **Environment Variables**, add:
   - `OPENAI_API_KEY`: Your OpenAI secret key (`sk-...`).
   - `OPENAI_MODEL`: `gpt-4o-mini` (or your preferred default).
   - `NEXT_PUBLIC_SUPABASE_URL`: Your Supabase URL (if using cloud auth).
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`: Your Supabase public anon key (if using cloud auth).
5. Click **Deploy**. Vercel will build and launch your application globally on edge-accelerated infrastructure.

---

## 8. Verification & Acceptance Testing Results

| Test Scenario | Status | Result / Notes |
|---|---|---|
| **Production Build** | ✅ PASSED | `npm run build` executed with exit code 0. Generated static `/` page and dynamic server routes `/api/chat` and `/api/conversations`. |
| **TypeScript Typecheck** | ✅ PASSED | `npm run typecheck` (`tsc --noEmit`) completed with 0 errors across all components, API routes, and utilities. |
| **Automated Test Suite** | ✅ PASSED | `npm test` verified title truncation, message sliding window token budgeting, and OpenAI error status mappings. |
| **Streaming Contract** | ✅ VERIFIED | Route handler returns chunked `ReadableStream` (`text/plain; charset=utf-8`) with `Cache-Control: no-cache`. |
| **API Key Protection** | ✅ VERIFIED | `OPENAI_API_KEY` is referenced solely on the server inside `app/api/chat/route.ts` and `lib/openai.ts`. No secret keys are bundled into client code. |
| **Guest Mode Fallback** | ✅ VERIFIED | When Supabase environment variables are missing or placeholders, application seamlessly boots into local storage guest mode without crashing. |
| **Interactive Controls** | ✅ VERIFIED | Stop button triggers `AbortController.abort()`, Copy button copies markdown/code with visual confirmation, and Theme toggle updates HTML classes and localStorage. |
