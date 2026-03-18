# CLAUDE.md

## Project Overview

AI-powered Traditional Chinese Medicine (TCM) tongue diagnosis web application. Users upload or capture tongue photographs, and Google Gemini 3 Pro analyzes them to provide TCM syndrome diagnoses with dietary, lifestyle, and herbal recommendations.

## Tech Stack

- **Framework**: React 19 with TypeScript
- **Build Tool**: Vite 5
- **Styling**: Tailwind CSS (CDN, configured in `index.html`)
- **AI Service**: Google Generative AI SDK (`@google/genai`) — Gemini 3 Pro Preview
- **Icons**: Lucide React

## Project Structure

```
├── App.tsx                    # Main component, state management, layout
├── index.tsx                  # React DOM entry point
├── index.html                 # HTML template + Tailwind config
├── types.ts                   # TypeScript interfaces & enums
├── components/
│   ├── Header.tsx             # Navigation bar
│   ├── Footer.tsx             # Footer with medical disclaimer
│   ├── UploadSection.tsx      # Image upload/capture UI
│   └── ResultSection.tsx      # Analysis results display
└── services/
    └── geminiService.ts       # Gemini API integration
```

## Commands

```bash
npm run dev       # Start dev server on port 3000
npm run build     # Production build to dist/
npm run preview   # Preview production build
```

## Environment Variables

- `API_KEY` — Required. Google Gemini API key. Exposed to client via Vite's `define` in `vite.config.ts`.

## Key Conventions

- **Functional components** with `React.FC` typing and `useState` hooks
- **No external state management** — state lives in `App.tsx`
- **Strict TypeScript** — `strict: true`, no unused variables/parameters
- **Tailwind CSS only** — no CSS files; custom `tcm` color palette (greens) and fonts (`Noto Serif SC`, `Inter`) defined in `index.html`
- **PascalCase** for components/types, **camelCase** for variables/functions
- **Chinese UI text** — primary interface language is Chinese; variable names in English
- **Components in `/components`**, services/utilities in `/services`**, shared types in `types.ts`

## Data Flow

1. User uploads image → `handleImageSelected()` stores base64 + preview
2. User clicks analyze → `startAnalysis()` calls `geminiService.analyzeTongueImage()`
3. Gemini returns structured JSON (`TCMAnalysis`) validated against a schema
4. `ResultSection` displays diagnosis, recommendations

## Key Types (types.ts)

- `TCMAnalysis` — structured analysis result (visualFeatures, diagnosis, recommendations)
- `AnalysisStatus` — enum: `IDLE`, `ANALYZING`, `SUCCESS`, `ERROR`

## No Test Suite

No testing framework is configured. No test files exist.

## Notes for AI Assistants

- This is a **frontend-only** app — no backend, no database
- The Gemini API call uses structured output with JSON schema validation and a thinking budget of 1024 tokens
- The API prompt in `geminiService.ts` is written in Chinese
- Medical disclaimer in Footer is important — do not remove
- Tailwind config lives inside `index.html` `<script>` tag, not in a separate config file
