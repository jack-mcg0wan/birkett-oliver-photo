# Birkett Oliver Photography — Deployment Guide

## Repo structure

```
/
├── index.html          ← the website
├── photos.json         ← gallery list (edited via CMS)
├── settings.json       ← site settings (edited via CMS)
├── _headers            ← Cloudflare caching rules
├── _redirects          ← Cloudflare redirect rules
├── images/             ← all photos + logo assets
└── admin/
    ├── index.html      ← Decap CMS panel
    └── config.yml      ← CMS config
```

---

## One-time setup (do this once, takes ~20 mins)

### 1. Create the GitHub repo

1. Go to github.com, log in as `jack-mcg0wan`
2. Click **New repository**
3. Name it `birkett-oliver-photo`
4. Set to **Public**
5. Click **Create repository**
6. Upload all files from this folder (drag and drop or use GitHub Desktop)

### 2. Add Oliver as a collaborator

1. In the repo, go to **Settings → Collaborators → Add people**
2. Search for Oliver's GitHub username
3. Set role to **Write**
4. He'll get an invite email — he needs to accept it

### 3. Connect to Cloudflare Pages

1. Log in to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Go to **Pages → Create a project → Connect to Git**
3. Authorise GitHub, select `birkett-oliver-photo`
4. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave blank)*
   - **Output directory:** `/` (or leave blank)
5. Click **Save and Deploy**
6. Cloudflare gives you a `*.pages.dev` URL — test it works

### 4. Connect the custom domain

1. In Cloudflare Pages, go to your project → **Custom domains → Set up a custom domain**
2. Enter `www.birkettoliverphoto.com`
3. Follow the DNS prompts (if domain is already on Cloudflare DNS it's automatic)

### 5. Set up Netlify OAuth (for CMS login)

This lets Oliver log in at `/admin` using GitHub.

1. Go to [netlify.com](https://netlify.com), log in as `jackjosephmcgowan@gmail.com`
2. Create a new site (can be blank — it's just used for auth)
   - **New site → Import from Git → skip** or deploy an empty site
3. Go to **Site settings → Access control → OAuth**
4. Click **Install provider → GitHub**
5. You'll need to create a GitHub OAuth App:
   - Go to github.com → Settings → Developer settings → OAuth Apps → New OAuth App
   - **Application name:** Birkett Oliver CMS
   - **Homepage URL:** `https://www.birkettoliverphoto.com`
   - **Authorization callback URL:** `https://api.netlify.com/auth/done`
   - Copy the **Client ID** and **Client Secret** into Netlify

### 6. Set up Web3Forms (contact form)

1. Go to [web3forms.com](https://web3forms.com)
2. Enter Oliver's email address → get the **Access Key**
3. Open `index.html`, find this line:
   ```html
   <input type="hidden" name="access_key" value="REPLACE_WITH_WEB3FORMS_KEY" />
   ```
4. Replace `REPLACE_WITH_WEB3FORMS_KEY` with the key from Web3Forms
5. Commit and push — Cloudflare will redeploy automatically

### 7. Update the repo name in CMS config

Open `admin/config.yml` and confirm line 8 reads:
```yaml
repo: jack-mcg0wan/birkett-oliver-photo
```
Change if your repo name differs.

---

## How Oliver updates his gallery

1. Go to `https://www.birkettoliverphoto.com/admin`
2. Click **Login with GitHub** — uses his GitHub account
3. Go to **Gallery → Photo List**
4. Add, remove, or drag to reorder photos
5. Click **Publish** — site updates in ~30 seconds

---

## Swapping placeholder content

All `── SWAP ──` comments are in `index.html`:

| What | Where in index.html |
|------|---------------------|
| Oliver's email (footer + form) | search `SWAP email` |
| Instagram URL | search `SWAP: Oliver's Instagram` |
| Marquee text | search `SWAP: edit client names` |
| Hero background image | `.hero-bg { background-image: url('...') }` |

---

## When Oliver wants to change his email address

1. Log in to [web3forms.com](https://web3forms.com) with the account you created
2. Update the email — no code changes needed

---

## Transferring the repo to Oliver later

1. Go to repo **Settings → Danger Zone → Transfer**
2. Enter Oliver's GitHub username
3. Done — he becomes the owner, you can stay as a collaborator
