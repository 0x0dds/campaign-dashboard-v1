# Google Ads API Setup Guide — PearlView Dental

**Purpose:** Enable the Triage Orchestrator agent to pull Google Ads data programmatically on a twice-daily schedule.

**Time required:** ~30 minutes of your time, plus 1–3 business days for Google to approve the developer token.

---

## Step 1: Get Your Google Ads Customer ID (CID)

You already have this — it's the 10-digit number in the top-right of ads.google.com when you're logged in. Format: `XXX-XXX-XXXX`.

**Action:** Paste your CID to me when ready.

---

## Step 2: Create a Google Cloud Project + Enable the API

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Sign in with the **same Google account** that has access to your Google Ads account
3. Click **"Select a project"** → **"New Project"**
   - Name: `PearlView Ads Automation` (or whatever you like)
   - Click **Create**
4. Once created, make sure it's selected, then:
   - Go to **APIs & Services → Library**
   - Search for **"Google Ads API"**
   - Click it → Click **Enable**

---

## Step 3: Create OAuth2 Credentials

1. Go to **APIs & Services → Credentials**
2. Click **"+ CREATE CREDENTIALS" → "OAuth client ID"**
3. If prompted for a consent screen:
   - User type: **External** (or Internal if you have Google Workspace)
   - App name: `PearlView Ads Automation`
   - User support email: your email
   - Developer contact: your email
   - Click **Save and Continue** through the remaining screens (no scopes needed to add manually)
   - Add yourself as a **test user** if External
4. Back on Credentials → Create OAuth client ID:
   - Application type: **Desktop app**
   - Name: `PearlView CLI`
   - Click **Create**
5. **Download the JSON file** — click the download icon next to your new credential
6. Save it somewhere safe (e.g., `~/credentials/google-ads-oauth.json`)

---

## Step 4: Get a Developer Token

1. Sign in to [Google Ads](https://ads.google.com)
2. Click the **Tools icon** (wrench) → **Setup → API Center**
   - If you don't see API Center, you may need a **Manager (MCC) account**. Create one for free at [ads.google.com/home/tools/manager-accounts/](https://ads.google.com/home/tools/manager-accounts/), then link your PearlView account to it.
3. In the API Center, you'll see your **Developer Token**
4. It starts as **Test Account** access (limited to your own account — which is all we need)
5. Copy the developer token

**Note:** Test-level access works for reading your own account. If Google requires "Basic" access, there's a short application form. This usually approves in 1–3 business days.

---

## Step 5: Generate a Refresh Token

I'll run this step for you on this machine once you give me the OAuth JSON and developer token. What happens:

1. I run the Google Auth flow
2. A browser window opens asking you to sign in and grant access
3. You click "Allow"
4. Google gives back a refresh token that I store securely
5. From then on, I can pull data silently — no browser needed

---

## Step 6: Give Me the Credentials

Once you have these three things, paste or tell me:

| Item | What it looks like |
|---|---|
| **Customer ID (CID)** | `123-456-7890` |
| **Developer Token** | A string from the API Center |
| **OAuth JSON file path** | Where you saved the downloaded JSON |

I will:
- Store them in a secure config at `~/.hermes/profiles/triage-orchestrator/`
- Set up the Python `google-ads` library
- Run a test query to verify access
- Build the automated data pipeline

---

## What Happens After Setup

Once connected, I will:

1. **Pull data twice daily** (07:00 and 17:00 CDT) using GAQL queries
2. **Respect the R1 integrity gate** — all queries explicitly set date windows, never pulling contaminated pre-Aug-11 data
3. **Store raw pulls** as timestamped files so we have an audit trail
4. **Update the dashboard** with fresh numbers
5. **Write the vault note** per the playbook

The queries I'll run cover:
- Campaign metrics (spend, clicks, impressions, CTR, CPC, conversions)
- Keyword-level performance with Quality Score components
- Geographic performance by zip/location
- Conversion action inventory and settings
- Campaign settings (to check R5 config drift)
- Auction insights (competitive data)

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "API Center" not visible | You need an MCC account — create one and link PearlView to it |
| "Application not verified" warning | Normal for test mode — click "Advanced" → "Go to PearlView CLI (unsafe)" |
| Developer token stuck in "Test" | Apply for Basic access in the API Center — it's a short form |
| OAuth error after setup | Refresh token may have expired — I'll re-run the auth flow |
| "Request had insufficient authentication scopes" | The OAuth consent screen needs the `google-ads` scope — I'll handle this in the auth flow |
