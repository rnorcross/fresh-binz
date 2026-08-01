# Fresh Binz — Landing Page

Static one-page site: logo, pricing selector, and CTAs that send customers to Stripe Payment Links. No build step, no dependencies — deploys anywhere.

## Files

- `index.html` — the landing page (all CSS/JS inline)
- `thanks.html` — post-checkout confirmation page (set as your Stripe redirect)
- `assets/` — logo files

## 1. Set up Stripe (about 15 minutes)

Create **3 Products** in the Stripe Dashboard (Product catalog → Add product):

| Product | Prices |
|---|---|
| Monthly Bin Cleaning | $20/month (1 bin) · $30/month (2 bins) — both **Recurring, monthly** |
| Bi-Weekly Bin Cleaning | $30/month (1 bin) · $40/month (2 bins) — both **Recurring, monthly** (you clean twice a month, but billing stays simple: one charge per month) |
| One-Time Deep Clean | $40 — **One-off** |

Then create **5 Payment Links** (Payment Links → + New), one per price. For each link:

- **Collect customers' addresses** → Shipping address (this is the service address — you need it to find the bins)
- **Collect phone number** → ON (you text confirmations)
- **After payment** → "Don't show confirmation page" → redirect to `https://YOUR-DOMAIN.vercel.app/thanks.html`
- Optional: add a custom field "Trash pickup day" (dropdown Mon–Fri) so scheduling info arrives with the payment

Copy each link URL into the `STRIPE_LINKS` object at the top of the `<script>` in `index.html`:

```js
const STRIPE_LINKS = {
  monthly_1bin:  "https://buy.stripe.com/...", // $20/mo
  monthly_2bin:  "https://buy.stripe.com/...", // $30/mo
  biweekly_1bin: "https://buy.stripe.com/...", // $30/mo
  biweekly_2bin: "https://buy.stripe.com/...", // $40/mo
  onetime_1bin:  "https://buy.stripe.com/..."  // $40 one-time
};
```

Until you paste real links, the buttons safely fall back to a phone call so you never lose a lead.

## 2. Push to GitHub

```bash
# from inside this folder
git init
git add .
git commit -m "Fresh Binz landing page"
# create an empty repo at github.com/new named fresh-binz, then:
git remote add origin https://github.com/YOUR_USERNAME/fresh-binz.git
git branch -M main
git push -u origin main
```

## 3. Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) → **Add New → Project**
2. Import the `fresh-binz` GitHub repo
3. Framework preset: **Other** (it's plain static HTML) — no build command, no output directory needed
4. Deploy. You'll get a live URL like `fresh-binz.vercel.app`

Every future `git push` to `main` auto-deploys. To use a custom domain (e.g. `freshbinz.com`), add it under Project → Settings → Domains and follow the DNS instructions.

## Editing later

Everything lives in `index.html`. Prices/plan copy are in the `PLANS` object in the script; colors are CSS variables at the top of the `<style>` block.
