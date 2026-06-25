<div align="center">

# ✅ Taskify

### Plan Goals. Build Habits. Get Things Done.

[![React Native](https://img.shields.io/badge/React%20Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactnative.dev/)
[![Expo](https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white)](https://expo.dev/)
[![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com/)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

<br/>

> **Taskify** is a productivity app that turns your ambitions into actionable plans — with AI-generated goal roadmaps, habit tracking, and smart timezone-aware reminders delivered right to your phone.

</div>

---

## ✨ What is Taskify?

Taskify helps you go from *"I want to..."* to *"I did it."*

Tell it your goal. It builds the plan — habits, tasks, deadlines, and all. Then it keeps you on track with smart reminders that follow your timezone, no matter where you are.

---

## 🚀 Core Features

| Feature | Description |
|---|---|
| 🎯 **Goal Planning** | Set goals with due dates and structured milestones |
| 🔁 **Habit Tracking** | Build daily/weekly habits tied to your goals |
| ✅ **Task Management** | Step-by-step task plans generated automatically |
| 🔔 **Smart Notifications** | Timezone-aware push reminders via Expo |
| 🤖 **AI Goal Planner** | GPT-4o-mini generates your full plan from a single prompt |

---

## 🤖 AI Goal Planner

> Just describe your goal. Taskify does the rest.

The AI planner accepts a natural-language goal prompt and returns a fully structured plan — no setup, no guesswork.

```
POST /ai/goal-plan
```

**What you get back:**

```json
{
  "goalTitle": "Run a 5K in under 30 minutes",
  "note": "Consistency is key — aim for 3 runs per week.",
  "suggestedGoalDueDate": "2025-09-01",
  "habits": ["Morning run", "Stretch daily", "Track calories", "Sleep by 10PM"],
  "tasks": [
    { "title": "Download a couch-to-5K app", "dueDate": "2025-06-15" },
    { "title": "Buy proper running shoes", "dueDate": "2025-06-16" }
  ]
}
```

**Under the hood:**

- 🧠 Powered by `gpt-4o-mini`
- 📋 Returns **4–6 habits** and **8–12 tasks**
- 📅 Outputs ISO dates (`YYYY-MM-DD`) — future only
- 🕐 Times in 12-hour format (`09:00 AM`)
- 🔒 Validates authentication before every call
- ✅ Enforces strict JSON-only responses

---

## 🔔 Notification System

Taskify's notification pipeline is built to be reliable, timezone-aware, and duplicate-safe.

```
┌─────────────────────────────────────────────────┐
│              Every 5 Minutes (Cron)             │
│                  notificationCron.ts            │
└────────────────────┬────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────┐
│            notificationService.ts               │
│  • Fetches due reminders from Supabase          │
│  • Sends Expo push notifications                │
│  • Logs results → notification_logs             │
│  • Deduplicates to prevent repeat sends         │
└────────────────────┬────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────┐
│            notificationScheduler.ts             │
│  • Converts local times → UTC                   │
│  • Saves to upcoming_notifications              │
│  • Cancels stale reminders on update            │
│  • Recalculates if timezone changes             │
└─────────────────────────────────────────────────┘
```

**Reminder types handled:**

- 🎯 Goal reminders
- ⏰ Goal due-date alerts
- 🔁 Habit reminders
- ✅ Task due reminders

**Cron schedule** (configurable via `CRON_SCHEDULE` env var):
```
*/5 * * * *   →   runs every 5 minutes in UTC
```

---

## 🗂 Project Structure

```
taskify/
├── 📱 Frontend (React Native + Expo)
│   ├── App.js                     # Startup, fonts, notification handler
│   ├── RootNavigation.tsx         # App navigation
│   ├── src/                       # Screens and app logic
│   ├── aiGoalPlanApi.ts           # Calls backend AI endpoint
│   ├── AiGeneratingScreen.tsx     # AI goal generation UI
│   ├── useCoverImagePreloader.ts  # Preloads cover images
│   └── store/                     # Global state with Zustand
│
└── 🖥 Backend (Express + TypeScript)
    ├── index.ts                   # Server bootstrap
    ├── ai.ts                      # AI goal generation route
    ├── notificationScheduler.ts   # Schedules reminders
    ├── notificationService.ts     # Sends push notifications
    └── notificationCron.ts        # Cron job (every 5 min)
```

---

## 🛠 Tech Stack

**Frontend**
- [React Native](https://reactnative.dev/) + [Expo](https://expo.dev/)
- [Zustand](https://zustand-demo.pmnd.rs/) — lightweight global state

**Backend**
- [Express](https://expressjs.com/) — REST API server
- [node-cron](https://github.com/node-cron/node-cron) — scheduled jobs
- [TypeScript](https://www.typescriptlang.org/) — end-to-end type safety

**Database & Auth**
- [Supabase](https://supabase.com/) — auth, user profiles, notification storage

**AI & Notifications**
- [OpenAI GPT-4o-mini](https://openai.com/) — goal plan generation
- [Expo Push Notifications](https://docs.expo.dev/push-notifications/overview/) — mobile delivery

---

## ⚙️ Environment Variables

```env
# OpenAI
OPENAI_API_KEY=your_openai_api_key

# Supabase
SUPABASE_URL=your_supabase_url
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# Cron (optional — defaults to every 5 minutes)
CRON_SCHEDULE=*/5 * * * *
```

---

## 🧑‍💻 Getting Started

**1. Clone the repo**
```bash
git clone https://github.com/your-username/taskify.git
cd taskify
```

**2. Install dependencies**
```bash
# Frontend
cd frontend && npm install

# Backend
cd ../backend && npm install
```

**3. Set up environment variables**
```bash
cp .env.example .env
# Fill in your keys
```

**4. Start the backend**
```bash
cd backend && npm run dev
```

**5. Start the app**
```bash
cd frontend && npx expo start
```

---

<div align="center">

Made with 💜 using React Native, Expo, and a little AI magic.

</div>
