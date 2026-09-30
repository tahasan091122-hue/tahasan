# Portfolio Website + Admin Panel

A premium animated portfolio website with a password-protected Admin Panel that edits
every page — no code, no database software.

Put it online once, then make it yours from the panel: your name, your photos, your
services, your reviews, your colours, your logo.

**Open `START-HERE.html` first.**

---

## Folder

```
portfolio-website/
├── START-HERE.html        Open this first
├── README.md              This file
└── website/               The whole website — this is what you upload
    ├── admin/             The Admin Panel
    │   ├── index.html
    │   └── app.js
    ├── assets/
    │   ├── css/style.css  Design system (shared by every page)
    │   ├── css/admin.css  Admin Panel styling
    │   ├── js/render.js   Shared page renderer
    │   ├── js/main.js     Animations, slider, viewer, form
    │   ├── js/boot.js     Loads your saved content
    │   └── img/           Bundled artwork
    ├── netlify/functions/
    │   └── api.js         The server: login, save, upload, inbox
    ├── index.html  services.html  portfolio.html
    ├── about.html  testimonials.html  contact.html
    └── netlify.toml  _redirects  robots.txt  sitemap.xml
```

---

## How it works

Every page ships as a **finished HTML file**, so it loads instantly and search engines see the
real text. When a page opens it quietly asks the server whether the Admin Panel has saved
anything newer, and redraws itself if so. One renderer (`assets/js/render.js`) builds those
pages both ahead of time and in the browser, so the two can never drift apart.

Saving is handled by one small server function (`netlify/functions/api.js`). Content, images,
contact messages and your login live in **Netlify Blobs** — storage that comes free with the
site, with nothing to install and no database to manage.

---

## Your login

```
Username:  admin
Password:  ChangeMe123!
```

**Everyone who has these files knows that password**, so the panel refuses to save anything
until you replace it. Your first sign-in goes straight to Admin Account; set your own password
there and everything unlocks. New passwords are stored only as a one-way scrypt hash and cannot
be read back by anyone.

Two ways to reach the panel: the **Admin Panel** button in the website footer under *Get in
Touch*, or `your-site.netlify.app/admin/`.

---

## The Admin Panel

| Section | What you can do |
|---|---|
| **Dashboard** | Everything at a glance, with a shortcut into each section |
| **Home Page** | Hero text, rotating roles, intro, photo, floating cards, stats, every section heading, about teaser, tools, marquee, closing CTA |
| **My Work** | Add / edit / remove projects — a screenshot, a title, a short description |
| **About Me** | Photo, opening line, story paragraphs, signature, page CTA |
| **Services** | Add / edit / remove services — name, tag, icon, description, included list, note |
| **Testimonials** | Add / edit / remove reviews — name, role, service tag, text, avatar initials and colours |
| **Contact Information** | Email, phone, WhatsApp, social links, contact page wording |
| **Inbox** | Every message sent through the contact form, with delete |
| **Images / Media** | Upload, browse, copy paths, delete. Every image field has a **Choose** button |
| **Theme / Colors** | Four colour pickers, six presets, live preview — saved and applied site-wide |
| **Website Settings** | Brand name, **logo image**, browser tab icon, logo initials, browser title, footer text, content reset |
| **Admin Account** | Change your username and password |

### How the sections connect

- Services → Services page, home page cards, footer list, **and** the contact form dropdown
- My Work → the gallery, plus the four featured cards on the home page
- Testimonials → Testimonials page (featured + grid) and the home page slider
- Contact info → Contact page, footer, hero social icons, every `mailto:` / `tel:` / WhatsApp link
- Theme → every accent colour, gradient and glow across all six pages

---

## Security

- Password hashed with **scrypt** and a per-account salt; compared in constant time
- Session cookie is `HttpOnly` + `SameSite=Lax` + `Secure`, signed with HMAC-SHA256, 8-hour expiry
- Changing your username or password invalidates every existing session
- 5 wrong passwords → 5-minute lockout
- Every write endpoint checks the session; anonymous requests get `401`
- Uploads: extension **and** file signature checked, SVGs containing scripts rejected, 3.5 MB cap
- Saved content is filtered to known keys, so nothing unexpected can be injected
- Everything typed in the admin is escaped on output; long-form copy allows only
  `<strong> <em> <b> <i> <br> <a>` and strips `javascript:` and `on*` handlers
- The function's own source is blocked from the web in `_redirects`
- `robots.txt` plus an `X-Robots-Tag` header keep `/admin` and `/api` out of search engines

---

## Design system

| | |
|---|---|
| **Palette** | `#070A12` base · cyan `#35E0F0` → violet `#7B5CFF` → ember `#FF7A59` (all editable in Theme) |
| **Type** | Space Grotesk (headings) · Inter (body) |
| **Motion** | GSAP + ScrollTrigger reveals, count-up stats, magnetic buttons, spotlight cards, 3D portrait tilt, marquee |
| **Cursor** | 12px dot + 40px trailing ring; grows over interactive elements; native cursor on touch devices |
| **Responsive** | desktop → tablet (980px) → mobile (680px), admin panel included |

---

## Still to replace with your own material

Nothing reads as "placeholder" to a visitor, but this content is still sample text.
All of it is editable in the Admin Panel.

| What | Where in the admin |
|---|---|
| Hero and About photos | Home Page / About Me → Choose → upload |
| Project screenshots, titles and descriptions | My Work |
| The 9 sample client reviews | Testimonials |
| Your name, tagline and intro | Home Page and Website Settings |
| Stats (250 / 120 / 7 / $1.2M) | Home Page → Stats counters |
| Email, phone, WhatsApp, social links | Contact Information |
| Service descriptions and pricing notes | Services |

---

## Good to know

- **Re-deploying never erases your content.** Pushing new files to GitHub replaces the code only;
  everything you saved in the Admin Panel lives separately.
- **Restoring the original text** — Website Settings → Reset content. Your images, theme and
  login are kept.
- **A change not showing?** Refresh once with `Ctrl + Shift + R`. The storage occasionally takes a
  few seconds to catch up.
- **Contact messages** — every submission is stored and shown in the Inbox.
