# Compass — AI Persona Career Counsellor

## 1. Project Title
Compass — AI Persona-Based Career Counsellor Web App (Gemini API)

## 2. Problem Statement
Students get inconsistent, one-dimensional career advice — a single generic answer that doesn't reflect the different lenses a real advisory team would apply (technical readiness, placement readiness, academic path, entrepreneurial path). There's no single tool that lets a student ask one question and instantly compare how different kinds of career advisors would answer it.

## 3. Objective
Build a single-file, frontend-only web app that lets a student ask a career question and get a response from one or several AI "personas," each with a distinct role, audience, and constraints, generated live by the Gemini API — with multiple personas answered in **one** API call rather than one call per persona.

## 4. Personas
| Persona | Focus |
|---|---|
| Technical Career Counsellor | AI/ML, programming, software development, technical skills, projects |
| HR & Placement Counsellor | Resume, interviews, employability, recruitment, placement prep |
| Academic & Research Counsellor | Higher studies, MS/M.Tech, PhD, research, certifications |
| Entrepreneurship Counsellor | Startups, business ideas, freelancing, product development |

Each persona gives a meaningfully different answer to the same question because each is driven by its own Prompt Card (see `PROMPT_CARD.md` and the `PERSONAS` array in `index.html`), not just a different label on the same advice.

## 5. Prompt Cards
Full six-element Prompt Cards (Role, Audience, Context, Format, Constraints, Language) for all four personas are in [`PROMPT_CARD.md`](./PROMPT_CARD.md), and are embedded directly in the `PERSONAS` constant at the top of the `<script>` block in `index.html`.

## 6. Technology Used
- HTML5, CSS3, vanilla JavaScript — **one file**, no build step, no framework
- Google Fonts (Source Serif 4, Inter) loaded via CDN
- Google **Gemini API** (`generateContent`) called directly from the browser

## 7. Gemini API Integration
- Endpoint: `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`
- Model is selectable in the UI (`gemini-2.0-flash` by default; `gemini-1.5-flash` / `gemini-1.5-flash-8b` as fallbacks in case a key doesn't have access to 2.0).
- **Single request for multiple personas:** when the user selects more than one persona, every selected persona's full Prompt Card is embedded into one combined prompt, and Gemini is asked to return **one** JSON object covering all of them — not one request per persona.
- Structured output is enforced with `generationConfig.responseMimeType: "application/json"` and a `responseSchema`, so the response reliably parses into `{ responses: [{ personaId, response, mainRecommendation, priority, suggestedAction }] }`. The last three fields feed the comparison table without a second API call.
- **API key handling:** the user pastes their own Gemini API key into the app at runtime. It is stored only in the browser's `localStorage` and sent only to Google's API — it is never hard-coded in this file and must never be committed to the repo.

## 8. Application Screenshots
_Add 2–3 screenshots here before submitting:_
- Persona selection + question screen
- Single-persona response
- Multi-persona responses + comparison table

## 9. How to Run the Application
1. Clone this repo.
2. Open `index.html` directly in any modern browser (Chrome/Edge/Firefox) — no server or build step needed.
3. Open the **"Gemini API connection"** panel and paste in your own Gemini API key (get one free at [aistudio.google.com/apikey](https://aistudio.google.com/apikey)).
4. Select one or more personas, type a question (or click one of the example chips), and click **Get Career Advice**.

## 10. Sample Questions
1. "Should I prepare for placements or pursue higher studies?"
2. "I know Python but do not have any projects. What should I do?"
3. "Should I become an AI Engineer, Data Scientist or Software Developer?"

Test each question with: one persona, multiple personas, and different persona combinations.

## 11. Sample Outputs
_Paste 2–3 example screenshots or transcripts here after testing, e.g. the Technical vs HR vs Academic responses to Question 1._

## 12. Team Members
_Add name(s) and roll number(s) here._

## Notes on API Key Security
- No real API key is committed to this repository.
- The key field is a `password`-type input, kept only in the browser's local storage, and is never printed to the console or included in any request body other than the direct call to `generativelanguage.googleapis.com`.
