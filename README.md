# ForgeMee ⚒️

> Some legends are forged.

Curated roadmap of free resources to become an **AI Engineer**, **Data Scientist**, or **ML Engineer** — from Python basics to production AI systems.

**Live:** [forgemee.vercel.app](https://forgemee.vercel.app)

Every resource is free or has a substantial free tier. No affiliations or sponsorships.

## Structure

Three tracks (Data Scientist · ML Engineer · AI Engineer) share the early phases and diverge at Phase 3:

| Phase | Content |
|---|---|
| 0 | Foundations — Python, mathematics, SQL |
| 1 | Data & Classical ML |
| 2 | Deep Learning — up to the transformer |
| 3 | Specialization Stacks — tracks diverge |
| 4 | Production & Ops — MLOps, serving, evals |
| 5 | Practice & Portfolio |
| 6 | Reading & Staying Current |

Features: track filtering, per-track progress tracking (free account), completion certificates (self-tracked, no formal value), community suggestions with a Wall of Fame.

## Stack

Static site, no build step.

- `index.html` — page markup + all app logic (rendering, filtering, progress, auth, certificates)
- `resources.js` — the resource catalogue (single source of truth; see field schema in its header comment)
- `style.css` — custom styles on top of Tailwind CDN
- `suggestions.html` — suggestion form (EmailJS, client-side validation, honeypot + rate limiting)

Services: Firebase Auth + Firestore (progress persistence), EmailJS (suggestion form), jsPDF (certificates), canvas-confetti. All loaded from CDN.

## Data model

Each entry in `resources.js`:

```js
{
  id: 'unique-slug',
  name, description, url,
  type: 'course' | 'book' | 'youtube' | 'platform' | 'guide',
  level: 'beginner' | 'intermediate' | 'advanced',
  tracks: ['ds', 'ml', 'ai'],      // subset
  phase: 0-6,
  subgroup: 'python',              // optional: labelled band within a phase
  alt_group: 'key',                // optional: mutually exclusive alternatives ("pick one")
  optional: true,                  // optional: checkable but excluded from progress totals
}
```

Progress counts each `alt_group` as one slot (done if any member is done) and skips `optional` resources.

## Local development

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

Firebase and EmailJS keys in the HTML are public client-side identifiers; access is controlled by Firestore security rules (documented in the config comment in `index.html`).

## Contributing

Suggest a resource via the [form](https://forgemee.vercel.app/suggestions.html) or by email. Accepted suggestions are added to `resources.js` and credited on the Wall of Fame.

## Deployment

Pushed to `main` → auto-deployed by Vercel.
