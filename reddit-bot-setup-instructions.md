# Reddit Bot About Page Setup Instructions
**For:** Reddit App Creation at reddit.com/prefs/apps
**Bot:** Forgely Outreach Bot (Script bot)
**Status:** ✅ Page created, 🔴 Not yet deployed

---

## Reddit App Setup - Required URLs

### About URL (Required for Reddit App)
**URL:** `https://forgelyroy.com/reddit-outreach-bot`
**Status:** 🔴 Currently 404 (Not Live Yet)
**Fix Required:** Deploy to production

### Redirect URI (Required for OAuth)
**URL:** `https://forgely3d.com/reddit-callback` (or your chosen callback URL)
**Status:** 🟢 Already exists (you can use any callback URL you control)

### About URL (Optional for App Profile)
**URL:** `https://forgely3d.com/reddit-bot` (same as above, or separate page)
**Status:** 🟢 Could create this on forgely3d.com instead

---

## Current Status

### Reddit Bot About Page in Forgely Roy Repo

**Location:** `app/routes/reddit-outreach-bot.tsx`
**Content:**
- Hero section with bot description
- Features section (automated outreach, compliance, transparency)
- Contact information (Bill Davis, email, Discord)
- Legal/compliance disclaimer
- FAQ section

**Commit:** `749f8cd feat(reddit-bot): Add Reddit B2B outreach bot about page`

### Deployment Status

**Current Status:** 🔴 NOT DEPLOYED
**Test Result:** `curl https://forgelyroy.com/reddit-outreach-bot` returns 404

**Reason:** Route exists in code but site hasn't been rebuilt/deployed since commit

---

## How to Fix (3 Options)

### Option 1: Deploy Forgely Roy Site (Recommended for Roy Store)
**Action:** Deploy/update forgelyroy.com to make `/reddit-outreach-bot` live
**Owner:** Tyler (tdawg012)
**Steps:**
1. Pull latest main branch to forgely-roy repo
2. Run `npm install` (if dependencies updated)
3. Run `npm run build`
4. Deploy to production (Oxygen/Vercel/Netlify)
5. Test: `curl https://forgelyroy.com/reddit-outreach-bot` should return 200

**Time Required:** 30-60 minutes
**About URL for Reddit:** `https://forgelyroy.com/reddit-outreach-bot`

---

### Option 2: Deploy to Forgely3d.com (Alternative)
**Action:** Create bot about page on forgely3d.com instead
**Owner:** Tyler (tdawg012) or Bill (if forgely-roy is separate)
**Steps:**
1. Create route in forgely-roy repo: `app/routes/reddit-bot.tsx`
2. Copy content from `app/routes/reddit-outreach-bot.tsx`
3. Commit and push to main
4. Build and deploy to production
5. Test: `curl https://forgely3d.com/reddit-bot` should return 200

**Time Required:** 30-60 minutes
**About URL for Reddit:** `https://forgely3d.com/reddit-bot`

---

### Option 3: Use Temporary About URL (Fastest)
**Action:** Use an existing about page or create a simple GitHub Pages page
**Owner:** Bill (sitaga)
**Steps:**
1. Create simple markdown file with bot info
2. Commit to ember-ops repo: `docs/reddit-bot.md`
3. Enable GitHub Pages on ember-ops repo
4. Use: `https://forgely3d.github.io/ember-ops/reddit-bot`

**Time Required:** 10-15 minutes
**About URL for Reddit:** `https://forgely3d.github.io/ember-ops/reddit-bot`

---

## Reddit App Creation Instructions (Once About URL is Live)

### Step 1: Go to Reddit App Creation
**URL:** https://www.reddit.com/prefs/apps
**Login:** Use your Reddit account (sitaga or separate bot account)

### Step 2: Create Script App
**App Name:** Forgely Outreach Bot
**App Type:** Script
**Description:** Automated B2B outreach for print farms, makerspaces, and 3D printing businesses
**About URL:** `https://forgelyroy.com/reddit-outreach-bot` (or whichever option you chose)
**Redirect URI:** `https://forgely3d.com/reddit-callback` (or your chosen callback)
**Icon:** Upload Forgely logo (optional)

### Step 3: Configure Scopes
**Required Scopes:**
- `read` — Read Reddit comments, posts, and user info
- `identity` — Get your Reddit username
- `privatemessages` — Send and receive private messages (DMs)
- `submit` — Post comments to threads (optional, for automated commenting)

### Step 4: Save and Get Credentials
**After Creating App:**
1. Copy **Client ID** (e.g., `abcd1234`)
2. Copy **Client Secret** (e.g., `xyz-9876-super-secret`)
3. Click **Create App** or **Edit** button
4. Look for **Script** section or **Authorized** button

### Step 5: Authorize and Get Refresh Token
**Manual Authorization (Required for Script Bot):**

1. Build authorization URL:
```
https://www.reddit.com/api/v1/authorize?client_id=YOUR_CLIENT_ID&response_type=code&state=RANDOM_STRING&redirect_uri=YOUR_REDIRECT_URI&duration=permanent&scope=read identity privatemessages submit
```

2. Visit the URL in your browser
3. Click "Allow"
4. Copy the `code` from the redirected URL
5. Exchange code for refresh_token using PRAW or curl

**Using PRAW (Python):**
```python
import praw

# Create Reddit instance
reddit = praw.Reddit(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET",
    user_agent="Forgely Outreach Bot by /u/sitaga",
    redirect_uri="YOUR_REDIRECT_URI"
)

# Get refresh token
print("Visit:", reddit.auth.url(scopes=["read", "identity", "privatemessages"], state="random_string"))

# After authorization, get refresh token
token_info = reddit.auth.authorize("random_string")
print("Refresh token:", token_info["refresh_token"])
```

### Step 6: Save Credentials to `.env.reddit`
**File:** `/home/openclaw/.openclaw/workspace/.env.reddit`

**Contents:**
```
REDDIT_CLIENT_ID=abcd1234
REDDIT_CLIENT_SECRET=xyz-9876-super-secret
REDDIT_REFRESH_TOKEN=abc123xyz789
REDDIT_USER_AGENT=Forgely Outreach Bot by /u/sitaga (for Forgely B2B outreach)
REDDIT_REDIRECT_URI=https://forgely3d.com/reddit-callback
```

**Security Note:** `.env.reddit` should be in `.gitignore` (never commit secrets to GitHub)

---

## After Setup

### Test Reddit Connection
**Run:** `python3 scripts/reddit-b2b-outreach/reddit-outreach.py --action test`

**Expected Output:**
```
✅ Reddit connection successful
✅ Authenticated as: /u/sitaga
✅ Read access granted
✅ Private messages access granted
```

### Test Reddit Outreach (Dry Run)
**Run:** `python3 scripts/reddit-b2b-outreach/reddit-outreach.py --action post --prospect printlooper --dry-run`

**Expected Output:**
```
DRY RUN MODE - No actual posts will be made
✅ Loaded prospect: printlooper
✅ Thread ID: 1sjr7gv
✅ Comment prepared
✅ Rate limiting: 60 second delay
[Would post comment to r/3DPrintFarms/comments/1sjr7gv]
```

### Test Reddit Outreach (Actual)
**Run:** `python3 scripts/reddit-b2b-outreach/reddit-outreach.py --action post --prospect printlooper`

**Expected Output:**
```
✅ Loaded prospect: printlooper
✅ Thread ID: 1sjr7gv
✅ Posting comment...
✅ Comment posted successfully
✅ Comment ID: abc123xyz
✅ Link: https://reddit.com/r/3DPrintFarms/comments/1sjr7gv/comment_id
✅ Waiting 60 seconds before next post...
```

---

## Discord Integration for Response Tracking

### Webhook Configuration
**File:** `.env.analytics`

**Add:**
```
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN
```

**How to Get Discord Webhook:**
1. Go to Discord server settings → Integrations → Webhooks
2. Create new webhook
3. Select channel: `#background-worker`
4. Copy webhook URL
5. Add to `.env.analytics`

### Test Discord Webhook
**Run:** `python3 -c "import requests; requests.post('YOUR_WEBHOOK_URL', json={'content': 'Test webhook for Reddit automation'})"`

**Expected Output:** Message appears in `#background-worker` channel

---

## Next Steps

### Immediate (Tonight - 15 min)
1. [ ] **Bill:** Choose deployment option for about page (Option 1, 2, or 3)
2. [ ] **Tyler:** Deploy chosen option (30-60 min)
3. [ ] **Bill:** Test about URL: `curl https://forgelyroy.com/reddit-outreach-bot` (or chosen URL)
4. [ ] **Bill:** Create Reddit app (5-10 min)
5. [ ] **Bill:** Save credentials to `.env.reddit` (2 min)

### Tomorrow Morning (8:00 AM)
6. [ ] **Ember:** Test Reddit connection and outreach script (10 min)
7. [ ] **Ember:** Test Discord webhook (5 min)
8. [ ] **Bill:** Authorize Reddit app and get refresh_token (5 min)
9. [ ] **Bill:** Manual Reddit outreach (6 replies) if automation blocked (15 min)

---

## Success Criteria

- [ ] Reddit about page is live and accessible (200 status)
- [ ] Reddit app created and configured with correct scopes
- [ ] Credentials saved to `.env.reddit` (client_id, client_secret, refresh_token)
- [ ] Reddit connection tested successfully
- [ ] Discord webhook configured and tested
- [ ] First Reddit comment posted (test or manual)

---

**Status:** ✅ About page created in forgely-roy repo
**Next:** Deploy to production (Tyler's task)
**Reddit App:** Ready to create once about URL is live

**Time to Deploy:** 30-60 minutes (depending on option chosen)
**Time to Create Reddit App:** 10-15 minutes
**Total Time:** 40-75 minutes from now to Reddit automation ready
