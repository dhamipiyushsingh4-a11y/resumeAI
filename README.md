# ResumeAI — Full-stack ATS Resume Builder

ResumeAI is a Next.js full-stack starter for creating job-targeted resumes. It includes all 20 named template choices, an editable resume builder, Gemini-powered resume tailoring and cover letters, a keyword-overlap analyzer, account registration/login, MongoDB persistence, saved-resume management, and browser PDF export.

## Included features

- **20 templates:** Modern Pro, Corporate Elite, Minimal Classic, Elegant Edge, Nova CV, Prime Resume, Zenith, Aura CV, Fresh Start, Career Launch, FirstStep, Tech Talent, Executive Pro, Skyline, Vision CV, Prestige, NextGen, Impact, Elevate, Apex Resume.
- **AI tailoring:** paste a job description and ask Gemini to prioritize relevant information already present in the candidate's resume. The prompt explicitly prohibits inventing credentials or experience.
- **Resume editor:** profile/contact details, summary, skills, multiple experience entries, education entries, projects, certifications and achievements.
- **ATS analyzer:** basic keyword-overlap estimate and resume completeness checklist. It is not an employer ATS score and cannot guarantee screening results.
- **Accounts:** email/password registration and login, bcrypt password hashing, signed JWT bearer tokens.
- **Cloud storage:** save, list, load, update and delete the signed-in user's resumes in MongoDB.
- **Cover letter:** Gemini-generated editable cover letter based on resume data and the job description.
- **PDF:** use Print / Export PDF and choose Save as PDF in the browser print dialog.

## Requirements

- Node.js 18.17 or newer (Node 20 LTS recommended)
- npm
- MongoDB Atlas database
- Gemini API key for AI features

## Run locally

1. Extract the ZIP and open the project folder in VS Code.
2. Create `.env.local` in the project root by copying `.env.example`.
3. Fill in `MONGODB_URI`, `JWT_SECRET`, and `GEMINI_API_KEY`.
4. Install and run:

   ```bash
   npm install
   npm run dev
   ```

5. Open http://localhost:3000.
6. For a production build, run `npm run build`.

Do not commit `.env.local`, API keys, database credentials, or JWT secrets. The `.gitignore` excludes local environment files.

## MongoDB Atlas setup

Create a free/paid cluster in MongoDB Atlas, create a database user, and configure network access for your deployment. Copy the Node.js connection string and replace the username/password. URL-encode special characters in the database password. Avoid leaving broad `0.0.0.0/0` access enabled for a production database unless you understand the exposure and have chosen it deliberately.

## Gemini setup

Create a key in Google AI Studio and add it as `GEMINI_API_KEY`. The key is used only in server API routes; do not prefix it with `NEXT_PUBLIC_`. The AI route uses the `@google/genai` SDK and the `gemini-3.8-flash` model identifier. If that model is not enabled for your Google account/region, change the model identifier in both AI route files to a model currently available in your AI Studio project.

## GitHub upload

Create a GitHub repository named `ResumeAI`. Upload the contents of this folder (the `app`, `lib`, `package.json`, `tsconfig.json`, `next.config.js`, README, etc.). Do not upload `node_modules`, `.next`, `.env.local`, or API keys.

Or use Git from the project directory:

```bash
git init
git add .
git commit -m "Build ResumeAI full-stack resume builder"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ResumeAI.git
git push -u origin main
```

## Deploy to Vercel

1. Sign in to Vercel with GitHub and import the `ResumeAI` repository.
2. Keep the framework preset as Next.js.
3. In Project Settings → Environment Variables, add `MONGODB_URI`, `JWT_SECRET`, and `GEMINI_API_KEY`. Add `NEXT_PUBLIC_APP_URL` with your deployed URL if needed.
4. Deploy. After changing environment variables, redeploy the project.
5. Every push to the connected branch triggers a new deployment.

## API routes

- `POST /api/auth/register` — create account `{name,email,password}`
- `POST /api/auth/login` — sign in `{email,password}`
- `POST /api/ai/generate` — tailor resume `{jobDescription,resume}`
- `POST /api/ai/cover-letter` — create cover letter `{jobDescription,resume}`
- `GET /api/resumes` — list signed-in user's saved resumes
- `POST /api/resumes` — save a resume
- `GET /api/resumes/:id` — load an owned resume
- `PUT /api/resumes/:id` — update an owned resume
- `DELETE /api/resumes/:id` — delete an owned resume

All resume routes require `Authorization: Bearer <JWT>`.

## Security and launch checklist

This is a deployable starter, not a security-audited commercial SaaS. Before opening it to the public, add persistent rate limiting/abuse controls to AI endpoints, email verification, password reset, stronger request/schema validation, monitoring, privacy policy and account/data deletion controls. Consider moving JWTs from browser-managed bearer tokens to secure, HttpOnly cookies and adding CSRF protections. Review generated resumes and cover letters for accuracy before applying.

## ATS limitations

Template choices are distinct branded styles within a text-first, single-column resume structure; they are not 20 separate drag-and-drop layout engines. Browser print is used for PDF export; DOCX export is not included. The keyword analyzer is a simple text-overlap estimate, not a real ATS simulation. ATS systems vary, so no score or template guarantees a pass.
