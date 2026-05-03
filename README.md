# 🤖 AI CRM System

<div align="center">

![Next.js](https://img.shields.io/badge/Next.js-14-black?style=for-the-badge&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?style=for-the-badge&logo=typescript)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=for-the-badge&logo=postgresql)
![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?style=for-the-badge&logo=prisma)
![Tailwind](https://img.shields.io/badge/Tailwind-CSS-38B2AC?style=for-the-badge&logo=tailwind-css)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**The most complete AI-powered sales CRM — fully automated lead pipeline from Google Sheets to closed deals.**

[🚀 Live Demo](#) · [📖 Documentation](./docs/ARCHITECTURE.md) · [🛠 Setup Guide](./docs/SETUP_GUIDE.html) · [🗺 Workflow Visualizer](./docs/WORKFLOW_VISUALIZER.html)

</div>

---

## ✨ What Makes This Different

Most CRMs just *store* your leads. This one **works them** — automatically.

```
Google Sheets → AI Analysis → Email → WhatsApp → Voice Call → Meet Link → Proposal → Converted
        Everything automated. One lead at a time. Zero manual effort.
```

---

## 🎬 Features

| Feature | Technology | Status |
|---------|-----------|--------|
| 📊 Google Sheets Import | Sheets API + Service Account | ✅ |
| 🧠 AI Lead Analysis | Anthropic Claude (claude-opus-4-5) | ✅ |
| 📧 Personalized Email | Nodemailer + Gmail SMTP | ✅ |
| 📞 Auto Voice Calls | Bland AI (Indian English accent) | ✅ |
| 💬 WhatsApp Messaging | Twilio / Meta Cloud API | ✅ |
| 🎯 AI Lead Scoring (0-100) | Claude AI + Rule Engine | ✅ |
| 🔄 Follow-up Drip Sequences | 4-step: Email→WA→Call→Email | ✅ |
| 📅 Google Meet Auto-scheduling | Google Calendar API | ✅ |
| 📋 AI Proposal Generator | Claude AI + Pricing tiers | ✅ |
| 🗂 Kanban Pipeline Board | Drag & Drop UI | ✅ |
| 📊 Analytics Dashboard | Recharts - Funnel/Pie/Heatmap | ✅ |
| 👥 Team Management | Multi-agent + Lead Assignment | ✅ |
| 🔔 Slack + Telegram Alerts | Webhook APIs | ✅ |
| 💬 Website Chatbot Widget | Embeddable AI bubble | ✅ |
| 📈 Weekly Auto Reports | HTML email + Vercel Cron | ✅ |

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/Khushnoorkm1/Crm-final.git
cd Crm-final

# 2. Install
npm install

# 3. Setup environment
cp .env.example .env
# Fill in your API keys (see docs/SETUP_GUIDE.html)

# 4. Database
createdb ai_crm
npm run db:push

# 5. Run
npm run dev
# Open: http://localhost:3000/dashboard
```

> **⚡ Minimum setup** — Only `ANTHROPIC_API_KEY` + `EMAIL_*` + `DATABASE_URL` needed to start. Bland AI and WhatsApp can be added later.

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────┐
│                    NEXT.JS 14                        │
│  Frontend (App Router)    │    API Routes            │
│  /dashboard/*             │    /api/*                │
│  /proposals/[id]          │    Server-only           │
│  /chatbot-widget          │    Prisma + PostgreSQL   │
└─────────────────────────────────────────────────────┘
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
  Anthropic         Bland AI       Twilio/Meta
  (Claude AI)    (Voice Calls)    (WhatsApp)
        │               │               │
        └───────────────┼───────────────┘
                        ▼
              PostgreSQL + Prisma
```

**Lead Pipeline:**
```
NEW → EMAILED → CALLED → INTERESTED → MEETING_SENT → CONVERTED
                       ↘ NOT_INTERESTED
                       ↘ FOLLOW_UP (drip sequence)
```

---

## 📁 Project Structure

```
ai-crm/
├── prisma/
│   └── schema.prisma          # 10 database models
├── src/
│   ├── app/
│   │   ├── api/               # 22 API endpoints
│   │   ├── dashboard/         # 5 dashboard pages
│   │   ├── proposals/[id]/    # Public proposal page
│   │   └── chatbot-widget/    # Embeddable chat
│   └── lib/
│       ├── lead-processor.ts  # 🎯 Core pipeline engine
│       ├── ai-email.ts        # Claude AI prompts
│       ├── bland-ai.ts        # Voice call integration
│       ├── whatsapp.ts        # WhatsApp (Twilio+Meta)
│       ├── lead-scoring.ts    # 0-100 scoring engine
│       ├── follow-up.ts       # Drip sequence
│       ├── notifications.ts   # Slack + Telegram
│       └── proposal-generator.ts # AI proposals
├── docs/
│   ├── WORKFLOW_VISUALIZER.html  # 🗺 Interactive workflow
│   ├── SETUP_GUIDE.html          # 📖 Complete setup guide
│   ├── ARCHITECTURE.md
│   ├── API_REFERENCE.md
│   ├── DATABASE.md
│   └── DEVELOPER_GUIDE.md
├── .cursorrules               # Cursor AI context
├── .windsurfrules             # Windsurf context
├── CLAUDE.md                  # Claude Code context
└── .github/
    └── copilot-instructions.md
```

---

## 🔧 Environment Variables

```env
# Required
DATABASE_URL="postgresql://..."
ANTHROPIC_API_KEY="sk-ant-..."
EMAIL_HOST / EMAIL_USER / EMAIL_PASS
COMPANY_NAME / NEXTAUTH_URL / NEXTAUTH_SECRET

# Voice Calls
BLAND_AI_API_KEY / BLAND_AI_PHONE_NUMBER / BLAND_WEBHOOK_URL

# WhatsApp (choose one)
WHATSAPP_MODE="twilio" # or "meta"
TWILIO_ACCOUNT_SID / TWILIO_AUTH_TOKEN
# OR: META_PHONE_NUMBER_ID / META_WHATSAPP_TOKEN

# Google
GOOGLE_SERVICE_ACCOUNT_EMAIL / GOOGLE_SERVICE_ACCOUNT_KEY
GOOGLE_SHEETS_SPREADSHEET_ID

# Notifications (optional)
SLACK_WEBHOOK_URL
TELEGRAM_BOT_TOKEN / TELEGRAM_CHAT_ID
```

See `.env.example` for the complete list.

---

## 🗄 Database Models

```prisma
Lead            # Main entity — full lifecycle
EmailLog        # Email history per lead
CallLog         # Calls + recordings + transcripts
WhatsAppLog     # In/outbound WA messages (wamid dedup)
LeadScore       # 0-100 score + grade A-D
FollowUpSequence # Drip steps (pending/sent/skipped)
TeamMember      # Sales agents
Proposal        # AI-generated proposals + pricing
WeeklyReport    # Archived HTML reports
Notification    # Slack/Telegram send log
```

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/leads/import` | Import from Google Sheets |
| `GET` | `/api/leads` | List with filters + pagination |
| `POST` | `/api/leads/score` | AI lead scoring |
| `POST` | `/api/leads/manual-call` | Trigger Bland AI call |
| `POST` | `/api/calls/webhook` | ← Bland AI webhook |
| `POST` | `/api/whatsapp/send` | Send WhatsApp |
| `POST` | `/api/whatsapp/webhook` | ← Inbound WA |
| `POST` | `/api/meetings/create` | Google Meet + email |
| `GET` | `/api/analytics` | Charts data |
| `POST` | `/api/proposals` | Generate AI proposal |
| `POST` | `/api/chatbot` | AI chatbot conversation |
| `GET` | `/api/follow-up` | Cron — daily follow-ups |
| `GET` | `/api/reports/weekly` | Cron — Monday report |

Full reference: [`docs/API_REFERENCE.md`](./docs/API_REFERENCE.md)

---

## 🤖 AI Tool Support

This project has **first-class support** for AI coding assistants:

| File | Tool |
|------|------|
| `.cursorrules` | Cursor |
| `.windsurfrules` | Windsurf |
| `CLAUDE.md` | Claude Code |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `PROJECT_CONTEXT.md` | Any AI tool |

Every AI assistant gets full project context automatically — file structure, conventions, patterns, and common tasks.

---

## 📖 Documentation

| Doc | Description |
|-----|-------------|
| [`docs/SETUP_GUIDE.html`](./docs/SETUP_GUIDE.html) | Interactive step-by-step setup (open in browser) |
| [`docs/WORKFLOW_VISUALIZER.html`](./docs/WORKFLOW_VISUALIZER.html) | Interactive workflow diagram (open in browser) |
| [`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md) | System design + flow diagrams |
| [`docs/API_REFERENCE.md`](./docs/API_REFERENCE.md) | All 22 API endpoints |
| [`docs/DATABASE.md`](./docs/DATABASE.md) | Schema + Prisma queries |
| [`docs/DEVELOPER_GUIDE.md`](./docs/DEVELOPER_GUIDE.md) | Extending, debugging, deploying |

---

## 🚀 Deploy to Vercel

```bash
npm install -g vercel
vercel login
vercel --prod
```

Cron jobs auto-activate from `vercel.json`:
- **Daily 9AM IST** — execute due follow-ups  
- **Monday 9AM IST** — send weekly HTML report

---

## 💰 Cost Estimate

| Service | Cost per 100 leads |
|---------|-------------------|
| Anthropic Claude | ~$0.50–1.00 |
| Bland AI calls | ~$0.90 (10 min avg) |
| Twilio WhatsApp | ~$0.50 |
| **Total** | **~₹250–300** |

---

## 🤝 Contributing

Contributions welcome! Please read the [Developer Guide](./docs/DEVELOPER_GUIDE.md) first.

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## ⭐ Show Your Support

If this project helped you, please consider giving it a ⭐ star on GitHub — it helps others discover it!

---

## 📄 License

MIT License — see [LICENSE](./LICENSE) for details.

---

<div align="center">

**Built with ❤️ using Next.js, Claude AI, and Bland AI**

[⭐ Star this repo](https://github.com/Khushnoorkm1/Crm-final) · [🐛 Report Bug](https://github.com/Khushnoorkm1/Crm-final/issues) · [💡 Request Feature](https://github.com/Khushnoorkm1/Crm-final/issues)

</div>