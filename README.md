# 🏠 NEXUS — Smart Campus & PG Platform | Indore

A multi-sided AI marketplace connecting Students, Working Professionals, PG Owners, and Colleges — with 24/7 AI agents that negotiate, detect fraud, and optimize pricing automatically.

---

## 🗺️ What This System Does

| Who | What They See | What They Can Do |
|-----|--------------|-----------------|
| **Students** | PG listings, college listings, roommate board | Post bed vacancy, get AI-matched PGs, receive negotiated offers |
| **Professionals** | PG listings, roommate board | Post bed vacancy, get AI-matched PGs |
| **PG Owners** | Student/professional enquiries, offer board | List rooms, see AI pricing suggestions, receive negotiated offers |
| **Colleges** | Student profiles seeking admission | List courses/seats, respond to student inquiries |

**Privacy Rules (enforced at DB level):**
- College listings → visible to Students only
- Student profiles (college-seeking) → visible to Colleges only
- Roommate/bed vacancy posts → visible to all logged-in users
- PG listings → visible to Students and Professionals only

---

## 🤖 AI Agents (run 24/7 via GitHub Actions)

### 1. Pricing Intelligence Agent
- Fetches all PG listings from Supabase
- Uses Claude AI to analyze rent vs. area market rates
- Flags overpriced/underpriced listings
- **Generates counter-offers** displayed publicly on site to attract tenants

### 2. Fraud Detection Agent  
- Scans every listing for fraud signals (vague address, impossibly low price, copied descriptions)
- Scores 0–100, flags suspicious listings for admin review
- Auto-hides listings with score > 80

### 3. Vacancy Monitor Agent
- Detects listings vacant 30+ days
- Generates personalized Hinglish WhatsApp nudges for PG owners
- Auto-pauses listings vacant 60+ days

### 4. Negotiation Agent ⭐ NEW
- Runs every 6 hours
- Reads enquiries where student offered a price
- Claude AI crafts a counter-offer between student's offer and owner's ask
- Posts the deal as a **public "Live Offer"** on the site
- If owner accepts → marks as deal, charges platform commission
- Commission: **₹500 flat + 5% of first month rent** per successful deal

### 5. Roommate Matching Agent ⭐ NEW
- Matches bed-vacancy posts with similar profiles (area, budget, gender, profession)
- Sends compatibility scores and suggested connections

---

## 📁 Project Structure

```
nexus/
├── index.html              ← Frontend (deploy to Netlify)
├── agents/
│   ├── pricing_agent.py    ← Pricing intelligence
│   ├── fraud_agent.py      ← Fraud detection  
│   ├── vacancy_agent.py    ← Vacancy monitoring
│   ├── negotiation_agent.py← AI deal negotiation ⭐ NEW
│   ├── roommate_agent.py   ← Roommate matching ⭐ NEW
│   └── run_all_agents.py   ← Master runner
├── api/
│   └── commission.py       ← Commission calculation logic
├── db/
│   └── schema.sql          ← Complete Supabase schema
├── docs/
│   └── STARTUP_GUIDE.md    ← Step-by-step launch guide
└── .github/
    └── workflows/
        ├── agents_nightly.yml   ← Runs all agents at 2 AM IST
        └── agents_frequent.yml  ← Negotiation agent every 6h
```

---

## 🚀 Quick Start (15 minutes)

### Step 1: Supabase Setup
1. Go to [supabase.com](https://supabase.com) → New Project
2. Open SQL Editor → paste contents of `db/schema.sql` → Run
3. Copy your `Project URL` and `anon key` from Settings → API

### Step 2: Get Claude API Key
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create API key (free tier: $5 credit on signup)

### Step 3: GitHub Setup
1. Fork/push this repo to your GitHub
2. Go to Settings → Secrets → Actions
3. Add these secrets:
   ```
   SUPABASE_URL=https://xxxx.supabase.co
   SUPABASE_KEY=your_anon_key
   ANTHROPIC_API_KEY=sk-ant-xxxx
   ```

### Step 4: Deploy Frontend
1. Go to [netlify.com](https://netlify.com) → New Site → Import from GitHub
2. Build command: (leave empty)
3. Publish directory: `.` (root)
4. Done! Your site is live.

### Step 5: Enable GitHub Actions
- Go to your repo → Actions tab → Enable workflows
- Agents will now run automatically 24/7!

---

## 💰 Revenue Model

| Revenue Stream | Amount | When |
|---------------|--------|------|
| Deal commission | ₹500 + 5% of 1st month rent | Per successful PG booking |
| PG Owner listing (premium) | ₹299/month | Featured placement |
| Roommate matching premium | ₹99/month | Unlimited matches |
| College listing | ₹499/month | Per active college |

**Projected Month 1 (conservative):**
- 20 PG deals × avg ₹1,500 commission = ₹30,000
- 10 premium listings × ₹299 = ₹2,990
- **Total: ~₹33,000/month**

---

## ➕ Adding New Services Later

**You do NOT need to rebuild from scratch.** Just:
1. Create a new agent file in `agents/` (copy any existing one as template)
2. Add new tables to Supabase via SQL Editor
3. Add a new section in `index.html` for the UI
4. Add the new agent to `.github/workflows/agents_nightly.yml`
5. Push to GitHub → it runs automatically!

Example future services you could add:
- `tiffin_agent.py` — connect students with home-cooked tiffin services
- `tutor_agent.py` — match students with tutors
- `job_agent.py` — internship/part-time jobs for students
- `laundry_agent.py` — on-demand laundry pickup

---

## ⚠️ Things to Do Before Launch

- [ ] Replace demo Supabase URL with your own in `.env`
- [ ] Set up Row Level Security (RLS) policies in Supabase (see `db/schema.sql`)
- [ ] Add real WhatsApp Business API or Twilio for nudge messages
- [ ] Set up Razorpay/Cashfree for commission collection
- [ ] Register business (GST not required below ₹20L/year turnover)
- [ ] Create refund/dispute policy page
- [ ] Add Google Analytics to index.html

---

## 🆘 Support

Built with: Supabase (DB) + Claude AI (agents) + GitHub Actions (scheduling) + Netlify (hosting)

All free tiers until you scale. Estimated cost at 1000 users/month: **~₹0** (within free tiers).
