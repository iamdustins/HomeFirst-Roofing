# HomeFirst Roofing: Lead Forms (Google Sheets + Email) & Best SEO Hosting Guide

This guide covers:
1. **Where your form submissions go** and how to connect both forms on `index.html` directly to a **Google Sheet + Instant Email Alert** (100% Free, keeps our custom glossy design instead of an ugly Google Form embed).
2. **Wix Embed vs. Google Sites Embed vs. Native Free Hosting** — why embedding inside an `<iframe>` hurts SEO, and how to host this exact HTML site for **$0/month** with **100/100 Google SEO** attached to your custom domain.

---

## Part 1: Connecting Your Forms to Google Sheets & Gmail (3-Minute Setup)

Right now, both forms in [`index.html`](./index.html) (`#heroQuickForm` in the top Hero and `#mainBookingForm` at the bottom) are pre-wired with a JavaScript webhook handler (`handleLeadFormSubmit`).

Instead of replacing our glossy Midnight Navy & Terracotta forms with a standard Google Form embed, **your custom forms will send leads directly into a Google Sheet AND email Abraham immediately**.

### Step 1: Create Your Lead Sheet
1. Log into the Google Account you want to own the leads.
2. Go to [Google Sheets](https://sheets.new) and create a new spreadsheet named **`HomeFirst Roofing Leads`**.
3. In **Row 1**, add these exact column headers in cells `A1` through `H1`:
   - `A1`: **Timestamp**
   - `B1`: **Form Source**
   - `C1`: **Full Name**
   - `D1`: **Phone Number**
   - `E1`: **Email Address**
   - `F1`: **Property Address**
   - `G1`: **Service Needed**
   - `H1`: **Notes**

### Step 2: Add the Instant Lead Script
1. Inside your Google Sheet, click **Extensions** → **Apps Script** in the top menu.
2. Delete any code in the editor and paste this exact script:

```javascript
// Replace with Abraham's email (or multiple emails separated by commas)
const NOTIFICATION_EMAIL = "your-email@gmail.com";

function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);

    // 1. Append the lead to the Google Sheet
    sheet.appendRow([
      data.timestamp || new Date().toLocaleString(),
      data.formSource || "Website Form",
      data.fullName || "",
      data.phone || "",
      data.email || "",
      data.address || "",
      data.service || "",
      data.notes || ""
    ]);

    // 2. Send an instant email alert to Abraham / team
    if (NOTIFICATION_EMAIL && NOTIFICATION_EMAIL.includes("@")) {
      const subject = `🔥 New Roof Lead: ${data.fullName} (${data.phone}) - ${data.service}`;
      const body = [
        `New inspection request submitted on HomeFirst Roofing!`,
        `----------------------------------------------------`,
        `Name: ${data.fullName}`,
        `Phone: ${data.phone}`,
        `Email: ${data.email}`,
        `Property Address: ${data.address}`,
        `Service Needed: ${data.service}`,
        `Notes: ${data.notes || "None"}`,
        `Form Used: ${data.formSource}`,
        `Submitted At: ${data.timestamp}`,
        `----------------------------------------------------`,
        `Call them right away from your phone: tel:${data.phone}`
      ].join("\n");

      MailApp.sendEmail(NOTIFICATION_EMAIL, subject, body);
    }

    return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ status: "error", message: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

### Step 3: Deploy as a Web App URL
1. In the top right of the Apps Script window, click the blue **Deploy** button → **New deployment**.
2. Click the gear icon ⚙️ next to "Select type" and choose **Web app**.
3. Set these three fields:
   - **Description**: `HomeFirst Lead Webhook`
   - **Execute as**: `Me`
   - **Who has access**: **`Anyone`** *(Crucial so website visitors can submit without logging in)*
4. Click **Deploy** (Google will ask you to click *Authorize access* → choose your Google account → click *Advanced* → *Go to Untitled project (unsafe)* → *Allow*).
5. Copy the **Web app URL** (it looks like `https://script.google.com/macros/s/AKfycb.../exec`).

### Step 4: Paste the URL into `index.html`
Open [`index.html`](./index.html) and scroll down to **Line 1637**:
```javascript
const GOOGLE_SHEETS_WEBHOOK_URL = "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec";
```
Paste your URL inside the quotes and save! Every single submission from both the top Hero form and the bottom Booking form will immediately populate your Google Sheet and send an email alert.

---

## Part 2: Wix Embed vs. Google Sites Embed vs. Native Free Hosting (Crucial SEO Advice)

You asked a brilliant question:
> *"If I'm just going to be embedding the HTML within a Wix website, wouldn't it make more sense to embed it inside a free Google Site—or what would you suggest for the best SEO and traffic?"*

### Why You Should **NOT** Embed This Inside Wix or Google Sites
When you paste custom HTML into **Wix** or **Google Sites**, both platforms wrap your entire website inside a sandboxed **`<iframe>`** box. That causes three huge problems:
1. **It Kills Your Google SEO**: Google's search crawlers treat an `<iframe>` like an external window. Google will **not** properly index your headlines (`<h1>`, `<h2>`), San Antonio keywords, FAQ schema, or local roofing text for your main domain. To Google, your site looks like a blank page!
2. **Double Scrollbars & Broken Mobile Sticky Buttons**: Because the site is trapped inside a box inside another page, mobile users get weird double-scrolling, and the floating `"Call (210) 251-1454"` bar at the bottom won't stick to the phone screen properly.
3. **Wix Charges $17–$36/month for Zero Benefit**: You would be paying Wix monthly just to trap your already-finished website inside an iframe that hurts your rankings.

---

### The Best Solution: Host Natively on **GitHub Pages** or **Cloudflare Pages** ($0/Month Forever + 100% SEO)

Because we built this website as clean, pure HTML/CSS/JS—and **it is already pushed to your GitHub repository** (`iamdustins/HomeFirst-Roofing`)—you can host it **directly as a real, first-class website** for **$0/month forever** with **zero iframes**.

#### Option A: GitHub Pages (Already 90% Done!)
Since your code is already in GitHub, you can turn it live in 30 seconds:
1. Go to your repo: [`https://github.com/iamdustins/HomeFirst-Roofing/settings/pages`](https://github.com/iamdustins/HomeFirst-Roofing/settings/pages)
2. Under **Build and deployment** → **Branch**, change `None` to **`main`** and click **Save**.
3. In 60 seconds, your website will be live at `https://iamdustins.github.io/HomeFirst-Roofing/`!
4. **To Connect Abraham's Custom Domain**:
   - Type your custom domain (e.g., `homefirstroofingsa.com`) into the **Custom domain** box on that same GitHub Pages settings screen.
   - In your domain registrar (GoDaddy, Namecheap, Squarespace/Google Domains, or Cloudflare), add a `CNAME` record pointing `www` to `iamdustins.github.io` and four `A` records pointing `@` to GitHub's IPs (`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`).
   - Check the **"Enforce HTTPS"** box in GitHub (free SSL certificate included!).

#### Option B: Cloudflare Pages or Netlify (Also 100% Free Forever)
If you buy the domain through **Cloudflare** (which sells `.com` domains at wholesale cost—around $10.44/year with zero markup):
1. Connect your GitHub account to **Cloudflare Pages** (Free).
2. Select the `HomeFirst-Roofing` repository.
3. Click **Deploy**. It automatically connects your domain, gives you enterprise global speed, and every time we push an update to GitHub, the live website updates in 15 seconds.

### Summary Comparison

| Feature | Wix HTML Embed | Google Sites Embed | **GitHub Pages / Cloudflare Pages (Recommended)** |
| :--- | :--- | :--- | :--- |
| **Monthly Hosting Cost** | $17 – $36 / month | $0 / month | **$0 / month Forever** |
| **Google Local SEO Indexing** | ❌ Poor (Trapped in `<iframe>`) | ❌ Poor (Trapped in `<iframe>`) | **✅ 100% Native HTML Indexing** |
| **Page Load Speed** | Slow (Loads Wix bloat first) | Medium (Loads Google wrapper) | **⚡ Instant (< 0.4 seconds)** |
| **Click-to-Call `(210) 251-1454`** | Sometimes blocked by iframe sandbox | Sometimes blocked by iframe sandbox | **✅ Works 100% Natively on iOS & Android** |
| **Custom Domain + Free SSL** | Requires paid plan | Free | **✅ Free** |
