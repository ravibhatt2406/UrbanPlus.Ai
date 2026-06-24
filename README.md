# UrbanPulse AI
> **"Design, Evaluate, and Afford the Future City."**

UrbanPulse AI is a tactical urban planning digital twin platform that helps citizens, urban designers, and municipalities simulate walkable 15-minute city infrastructures, estimate carbon footprint displacements, and evaluate family budget sustainability.

---

## Core Product Vision

UrbanPulse AI consists of three interconnected spatial and budget systems sharing the same neighborhood engine:
1. **Accessibility Intelligence:** Measures walking access to daily necessities (Healthcare, Groceries, Education, Parks, Transit, and Pharmacies) using live Overpass and Nominatim APIs.
2. **AI Future Planner:** Generates realistic 2035 urban interventions using Gemini, placing solar EV charging stations, modular clinics, and net-zero schools onto maps while dynamically adjusting walking scores.
3. **Smart Living Advisor:** Analyzes household financial sustainability across a 10-city Indian cost index. Translates local walking transit scores into direct monthly budget transportation discounts.

---

## Repository Structure

```
├── public/                 # Static assets & map markers
├── supabase/
│   └── schema.sql          # Supabase PostgreSQL tables & indexing
└── src/
    ├── app/
    │   ├── layout.tsx      # Persistent layout, Header Navigation & Context Wrapper
    │   ├── page.tsx        # HOME page (explains platform, modules, & CTA)
    │   ├── analysis/       # Neighborhood Analysis (baseline scoring & map simulation)
    │   ├── future-planner/ # AI 2035 Future Planner (Gemini scenario simulator)
    │   ├── living-advisor/ # Smart Living Advisor (demographics & AI Financial Coach)
    │   ├── urban-score/    # Urban Sustainability Center (Master consolidated USI score)
    │   └── api/
    │       ├── score/          # Nominatim Geocoding + Overpass score calculation
    │       ├── future-plan/    # 2035 plan generation (Gemini Content API)
    │       ├── intervene/      # Baseline tactical interventions
    │       └── financial-coach/# AI Budget coach (Gemini Content API)
    ├── components/
    │   ├── Map.tsx         # Shared Leaflet Map (simulated and future marker overlays)
    │   ├── ScoreRadarChart.tsx # Custom Recharts spider radar comparison chart
    │   └── Navigation.tsx  # Responsive glassmorphic navbar
    ├── context/
    │   └── UrbanPulseContext.tsx # Global state engine syncing sub-pages
    └── utils/
        ├── category.ts     # Category colors and labeling
        ├── cityCosts.json  # Household cost index for 10 target Indian cities
        └── sustainabilityEngine.ts # Living budget and Master index math
```

---

## Interconnection Logic (Ecosystem Sync)

* **Access -> Budget:** When a neighborhood's walking Transit Accessibility score is high, the family's Transportation cost decreases by up to 35% (lower reliance on cars/cabs).
* **Future Transit -> Budget:** Simulating a **Future Transit Stop** on the map awards an additional 15% discount on monthly commuting expenses.
* **Future EV Hub -> Carbon & Budget:** Simulating a **Future EV Hub** reduces household transit cost by 10% and upgrades the neighborhood's Carbon Score.
* **Future Park -> Carbon & Equity:** Placing a park offset increases regional Carbon offset ratings (+10 points) and lifts the community park accessibility equity index.

---

## API Architecture

### 1. Score API (`GET /api/score?address=<query>`)
Queries Nominatim to extract coordinates, runs an Overpass query for nodes/ways within a 1.2km radius, and maps walking distance decays.
* **Returns:** JSON object containing coordinates, overall walkability index, category breakdowns, and a lists of coordinates/types.

### 2. Future Plan API (`POST /api/future-plan`)
Queries Gemini API to identify spatial gaps, placing 3 to 5 realistic 2035 assets (clinics, parks, charging hubs) with coordinate offsets.
* **Returns:** Recommendations, suggestedAmenities coordinates, carbon offsets, and equity gains.

### 3. Financial Coach API (`POST /api/financial-coach`)
Queries Gemini API with family size, income, consumption lifestyle, and calculated expenditure items.
* **Returns:** savingsAdvice, costReductionTips, alternative affordable suburbs, transit suggestions, and future projections.

---

## Deployment Guide

### Next.js Frontend (Vercel)
1. Install project dependencies:
   ```bash
   npm install
   ```
2. Configure environmental keys in `.env.local` or Vercel dashboard:
   ```env
   GEMINI_API_KEY=your_gemini_api_key
   ANTHROPIC_API_KEY=your_anthropic_api_key_if_using_claude
   ```
3. Run the development server locally:
   ```bash
   npm run dev
   ```
4. Build the production package:
   ```bash
   npm run build
   ```

### Database Setup (Supabase)
1. Initialize a new project on the [Supabase Dashboard](https://supabase.com).
2. Open the **SQL Editor** in your Supabase console.
3. Copy the contents of [`supabase/schema.sql`](file:///c:/Users/User/OneDrive/Desktop/devpost/supabase/schema.sql) and click **Run**.
4. Enable RLS and customize Auth hooks as required (Clerk or Supabase Auth compatible).

---

## Testing Guide

### 1. Manual Testing Workflow
* **Search test:** Search for "Jaipur, Rajasthan". Ensure map zooms to coordinates and radar displays data.
* **Map click simulation:** Click on the map in `/analysis`. Select "Healthcare" and click "Add Pin". Confirm the healthcare accessibility score rises immediately.
* **AI Future Planner:** Navigate to `/future-planner`. Click "Generate 2035 Future Plan". Confirm that future markers appear in a glowing blue pulse.
* **Living Advisor:** Shift parameters on `/living-advisor`. Check that expenses (e.g. food) scale with family size. Request advice and verify the AI coach returns tips.
* **Urban Sustainability Score:** Check `/urban-score`. Verify that the Master Index recalculates based on formula weights.

### 2. API Validation
Verify backend routes using curl or Postman:
```bash
# Verify geocoder scoring
curl "http://localhost:3000/api/score?address=Jaipur"
```

---

## Accessibility Compliance

* **Color contrast:** Neon highlights utilize dark backdrops (`bg-zinc-950`) to maintain a text contrast ratio of > 4.5:1 (WCAG AA).
* **Keyboard navigation:** Standard HTML tags (buttons, inputs) utilize active focus rings (`focus:ring-cyan-500`) for visual tracking.
* **Semantic HTML:** Implements landmarks (`<nav>`, `<main>`, `<header>`, `<section>`, `<h1>` to `<h5>`) for assistive screen readers.

---

## Hackathon Demo Script (3-Minute Presentation)

1. **Introduction (30s):**
   * *Presenter:* "Meet UrbanPulse AI. Most city dashboards show statistics, but they fail to connect accessibility to family affordability. We help answer: Is my neighborhood walkable, how can we improve it by 2035, and can families afford to live here?"
2. **Module 1 & 2: Analysis & AI Future (90s):**
   * *Action:* Navigate to `/analysis`, search `Jaipur, Rajasthan`. Show the Overpass points on the Leaflet map and the radar chart.
   * *Presenter:* "Jaipur gets a baseline walkability index of 58. Let's transition it into the future."
   * *Action:* Navigate to `/future-planner`, click **Generate 2035 Future Plan**. Projections load, glowing futuristic markers appear on the map.
   * *Presenter:* "AI identified healthcare and transit deserts. It planted modular medical domes and EV charging links, lifting the target accessibility index to 82."
3. **Module 3: Family Affordability & Sustainability Center (60s):**
   * *Action:* Navigate to `/living-advisor`, change family size to 4, lifestyle to Moderate, and click **Ask Financial Coach**. Tips populate.
   * *Presenter:* "Our Smart Living Advisor shows how this 2035 upgrade benefits family wallets. Because transit access improved, monthly transport bills dropped by 45%. The AI coach projects a ₹2,50,500 savings surplus in 12 months!"
   * *Action:* Navigate to `/urban-score` and highlight the final Master USI score of 78.
   * *Presenter:* "By linking accessibility, family budgets, equity, and carbon displacements, UrbanPulse AI designs the sustainable future city you can actually afford."
