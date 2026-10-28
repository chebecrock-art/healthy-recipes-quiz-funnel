# Healthy Recipes for a Better You - Quiz Funnel Sales Page

A high-converting, accessible, mobile-first quiz funnel sales page for the digital cookbook **"Healthy Recipes for a Better You"** by Google Deepmind team principles.

Built with **plain HTML5, CSS3, and Vanilla JavaScript** in a single `index.html` file with zero build step and zero runtime dependencies (except Google Fonts).

---

## 📁 Project Structure

```text
healthy-recipes-quiz-funnel/
├── index.html             # The complete standalone quiz funnel (HTML + CSS + JS)
├── README.md              # Documentation & configuration guide
└── assets/
    └── cover.png          # High-resolution book cover hero image
```

---

## 🚀 How to Run Locally

Because this is a pure static single-page application, no Node or compiler setup is required.

### Option 1: Double-click
Simply open `index.html` in Google Chrome, Microsoft Edge, Safari, or Mozilla Firefox.

### Option 2: Local HTTP Server
Using any local static file server:
- **Python** (if installed):
  ```bash
  python -m http.server 8000
  ```
- **Node.js `npx serve`** (if installed):
  ```bash
  npx serve .
  ```
- **VS Code / Cursor / Antigravity**:
  Right-click `index.html` and choose **"Open with Live Server"**.

Then navigate to `http://localhost:8000`.

---

## ⚙️ How to Edit Configuration, Price & Checkout URL

Open `index.html` in any text editor and scroll to the `<script>` section (around line 720). The top section contains the `CONFIG` object:

```javascript
const CONFIG = {
  // Base Hotmart payment checkout link
  baseCheckoutUrl: 'https://pay.hotmart.com/L108217514F',

  // Regular and discount display prices
  regularPrice: '$20.00',
  specialPrice: '$9.99',
  specialPriceNumeric: 9.99,

  // Configurable 7-day money back guarantee block (disabled by default)
  // Set to true to display the guarantee reassurance badge
  GUARANTEE_ENABLED: false,

  // Session storage key for persisting progress across page refreshes
  storageKey: 'healthy_recipes_quiz_data_v1',

  // Duration in milliseconds for the analyzing screen (3.4 seconds)
  analyzingDurationMs: 3400
};
```

### Profile Tracking Parameter
All CTA buttons automatically append the winning profile ID to the checkout URL in the same tab:
- `https://pay.hotmart.com/L108217514F?src=profile_energy`
- `https://pay.hotmart.com/L108217514F?src=profile_busy`
- `https://pay.hotmart.com/L108217514F?src=profile_family`
- `https://pay.hotmart.com/L108217514F?src=profile_foodie`

---

## 🌍 How to Edit Copy & Switch to Portuguese (i18n)

All headlines, subheadings, questions, answer options, profile descriptions, recommendations, chapter names, benefits, and FAQ answers are centralized inside the single `COPY` object in `index.html`.

To translate or adapt to Portuguese (or another language):
1. Duplicate `COPY.en` to `COPY.pt` inside the `COPY` object.
2. Set `const copy = COPY.pt;` instead of `const copy = COPY.en;`.

Example Portuguese snippet:
```javascript
const COPY = {
  pt: {
    brand: "Receitas Saudáveis",
    cookbookBadge: "E-book Digital",
    welcome: {
      badge: "Quiz Rápido de 1 Minuto",
      title: "Descubra o Plano de Alimentação Saudável Ideal Para Sua Vida",
      subtitle: "Responda a 7 perguntas rápidas e receba seu pacote personalizado de receitas, além de um desconto especial no final.",
      btnStart: "Iniciar o Quiz",
      timeNote: "Leva menos de 1 minuto"
    },
    // ...
  }
}
```

---

## 📊 How to Add Analytics & Conversion Tracking

The funnel includes an event dispatcher function: `trackEvent(eventName, eventData)` that logs to `console.log` by default and is pre-wired to:
- **Google Tag Manager (`window.dataLayer`)**
- **Google Analytics 4 (`window.gtag`)**
- **Meta Pixel (`window.fbq`)**

### Tracked Events
1. `quiz_start`: Triggered when the user clicks "Start the Quiz".
2. `question_answered`: Triggered on each question with `question_index`, `question_id`, and `answer_id`.
3. `quiz_complete`: Triggered when the user finishes all 7 questions with `winning_profile`, `match_score`, and score breakdown.
4. `cta_click`: Triggered when any CTA button is clicked with `location` (`hero_offer`, `final_cta`, or `sticky_bar`), `profile`, and destination URL.

### Activating GA4 or Meta Pixel
Near the top of `index.html` inside the `<head>` tag, uncomment the marked placeholders and insert your actual measurement ID or Pixel ID:
- Replace `G-XXXXXXXXXX` with your Google Analytics 4 Measurement ID.
- Replace `YOUR_PIXEL_ID` with your Meta Pixel ID.

---

## 🧠 Scoring Logic

The quiz computes real profile affinity points across 4 distinct user profiles:
- **The Energy Seeker (`energy`)**: Focuses on vitality, quick breakfasts, morning smoothies, nutrient bowls, and sustained daytime energy.
- **The Busy Achiever (`busy`)**: Focuses on under-15-minute meals, meal prepping, simple pantry staples, and rapid cleanup.
- **The Family Chef (`family`)**: Focuses on crowd-pleasing dinners, wholesome poultry, meat, gentle seafood, and healthy treats for kids and adults.
- **The Curious Foodie (`foodie`)**: Focuses on bold spices, global plant-forward bowls, inventive marinades, and fresh culinary discoveries.

**Tie-Breaking Rule**: In the event of an exact points tie between two or more top profiles, the user's primary goal in **Question 1** determines the winning profile.

**Session Persistence**: Progress is stored in `sessionStorage`. If the user accidentally refreshes during Question 4, the page restores their exact question and selected answers.
