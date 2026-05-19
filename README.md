# Cold Mine — Author Website

## How to preview locally (no setup needed)

The fastest way to see the site:

1. Unzip this folder anywhere on your computer
2. Open the unzipped folder
3. **Double-click `index.html`** — it'll open in your default browser

That's it. You'll see the home page. Click around the nav (Home, The Book, Author, Gallery, Contact) to view each page. All five pages work this way.

Note: the contact form's AJAX submission won't work from a `file://` URL (browser security), but it will work fine once deployed to Cloudflare Pages. For a fully working local preview including forms, run a tiny web server in the folder:

```bash
cd coldmine
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

---

## Project structure

```
coldmine/
├── index.html          Home page
├── the-book.html       Book detail page (front cover, blurb, Amazon link)
├── author.html         Author bio with photo
├── gallery.html        Image gallery with click-to-expand modal
├── contact.html        Contact form (Formspree) with thank-you modal
├── css/
│   └── style.css       All styles
├── js/
│   └── site.js         Mobile nav, fade-ins, newsletter handler, gallery modal
└── images/
    ├── hero.jpg              Hero background (Gemini-merged scene)
    ├── hero-1200.jpg         Mid-size responsive variant
    ├── hero-800.jpg          Mobile responsive variant
    ├── cover.jpg             Book front cover (used on book page)
    ├── full-cover.jpg        Full wrap-around cover (gallery)
    ├── coldmine-logo.png     Title logo (used as wordmark in hero & book page)
    ├── author.jpg            Author headshot
    ├── author-at-kgf.jpg     Author at the KGF sign
    ├── chainaadu.jpg         Spent rock tailings near Kolar
    ├── oakley-shaft-1908.jpg Historic Oakley Shaft photo (1908)
    ├── favicon.ico           Multi-size favicon
    ├── favicon-{16,32,48}.png
    └── apple-touch-icon.png
```

## Before going live — final checklist

1. **Set the Amazon link.** Find and replace `AMAZON_URL_HERE` across all `.html` files. Currently appears in `index.html` and `the-book.html`.

2. **Verify the Formspree form.** The contact form posts to `https://formspree.io/f/xjgzgovn`. On the first real submission, Formspree sends a verification email to the address tied to your account. Click the link in that email or future submissions won't be delivered. Test the form once after deploy.

3. **Wire up the newsletter form** (or remove the section). Open `index.html`, find `<form class="newsletter-form">`, change `action="#"` to your provider's endpoint:
   - **Buttondown** (recommended for writers): `https://buttondown.email/api/emails/embed-subscribe/YOUR_USERNAME`
   - **Mailchimp**: from your audience settings
   - **ConvertKit**: from your form embed code

   Then remove the `e.preventDefault()` line in `js/site.js` inside the `newsletterForm` handler.

4. **Privacy Policy and Terms links** in the footer currently go to `#`. Either write those pages, point to template generators, or remove the links.

5. **Update the README** (you're reading it) with anything you've customized further.

## Local preview

```bash
cd coldmine
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deploying to Cloudflare Pages (with Namecheap domain)

You said your domain `coldminebook.com` is registered at Namecheap.

1. Push this folder to a private GitHub repo.
2. Sign in at https://dash.cloudflare.com/
3. **Workers & Pages → Create → Pages → Connect to Git**. Pick the repo.
4. Build settings:
   - **Build command:** *(blank)*
   - **Build output directory:** `/`
5. Click **Save and Deploy**. You'll get a `coldminebook.pages.dev` URL within 30 seconds.
6. Once it works there, **Custom domains → Set up a custom domain → coldminebook.com**.
7. Cloudflare gives you two CNAME records. Log in to Namecheap → Domain List → Manage → Advanced DNS. Delete any default A/CNAME records, then add Cloudflare's records.
8. Wait 5–60 minutes for DNS propagation. Cloudflare will auto-issue an SSL cert in that window.

Every push to the `main` branch redeploys.

## Customizing

All design tokens (colors, fonts, spacing) live at the top of `css/style.css` in the `:root` block.

Typography: **Cinzel** for display headings, **Cormorant Garamond** for body. Both loaded free from Google Fonts.

## Image notes

- Hero image is served responsively via `srcset` (800/1200/2000px). Browsers pick the right one.
- Page weight on the homepage is roughly 700KB on desktop, 250KB on mobile. Both well within fast-load territory.
- Gallery images use `loading="lazy"` so they only download as the user scrolls down.

## Tech credit

Built collaboratively with Claude. Hero composition generated with Gemini. Title logo and favicon designed in Canva. Historic photos courtesy of the public domain. Kolaramma Temple imagery from Wikimedia Commons (CC BY-SA 3.0).
