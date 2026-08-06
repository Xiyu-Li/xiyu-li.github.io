# Personal academic site — setup guide

Quarto website, deployed via GitHub Pages. Edit markdown, push, site rebuilds automatically.

---

## 1. Create the repository

Repository name matters. For a URL of `https://xiyuli.github.io`, the repo **must** be named exactly `xiyuli.github.io`. Any other name gives you `https://xiyuli.github.io/reponame/` instead.

1. github.com → New repository
2. Name: `xiyuli.github.io`
3. Visibility: **Public** (required for Pages on free accounts)
4. Do not initialize with a README

## 2. Push these files

```bash
cd path/to/this/folder
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/xiyuli.github.io.git
git push -u origin main
```

## 3. Turn on Pages

Repo → **Settings** → **Pages** → under "Build and deployment", set **Source** to **GitHub Actions**.

Do this *before* worrying about the first build failing. If the workflow ran before you changed this setting, re-run it from the Actions tab.

## 4. Wait for the build

**Actions** tab → watch "Publish site". First run takes ~2 minutes (it installs Quarto). Green check means live at `https://xiyuli.github.io`.

---

## Editing

| To change | Edit |
|---|---|
| Bio, interests, contact | `index.qmd` |
| Papers and abstracts | `research.qmd` |
| Courses | `teaching.qmd` |
| CV link | `cv.qmd`, and drop the PDF at `files/cv.pdf` |
| Colors, fonts, spacing | `styles.scss` |
| Nav menu, site title | `_quarto.yml` |

Commit and push; the site rebuilds itself. No local Quarto install needed, though installing it locally lets you run `quarto preview` to see changes live before pushing.

**Math works out of the box.** Write `$\mu$` inline or `$$...$$` for display. Useful for abstracts with real notation.

---

## 5. Search Console — the part that was broken before

Now that you control `<head>`, verification is trivial.

1. Search Console → Add property → **URL prefix** → `https://xiyuli.github.io`
2. Choose the **HTML tag** method, copy the `content="..."` token
3. Add to `_quarto.yml` under `format: html:`

```yaml
    include-in-header:
      - text: |
          <meta name="google-site-verification" content="YOUR_TOKEN_HERE" />
```

4. Push, wait for the build, click **Verify**
5. **URL Inspection** → paste the URL → **Request Indexing**

Also worth adding, since GitHub Pages does not generate one automatically for Quarto: after your first build, submit `https://xiyuli.github.io/sitemap.xml` under **Sitemaps**. Quarto generates this when `site-url` is set in `_quarto.yml` — it already is.

---

## 6. Custom domain (optional, ~$12/year)

Recommended. It survives graduation and every future job move.

1. Buy `xiyuli.com` at Cloudflare Registrar (at-cost pricing) or Namecheap
2. At your registrar, add these DNS records:

```
A     @    185.199.108.153
A     @    185.199.109.153
A     @    185.199.110.153
A     @    185.199.111.153
CNAME www  YOURUSERNAME.github.io
```

3. Repo → Settings → Pages → **Custom domain** → enter `xiyuli.com` → Save
4. Wait for the DNS check, then tick **Enforce HTTPS**
5. Update `site-url` in `_quarto.yml` to the new domain
6. Add the new domain as a separate Search Console property

---

## The thing that actually determines whether Google finds you

None of the above substitutes for inbound links. A new domain with zero referring pages ranks nowhere regardless of how correctly it is configured.

- [ ] Email the econ department administrator; ask for your site URL on the PhD students directory page
- [ ] Google Scholar profile, with the site URL in it
- [ ] IDEAS/RePEc author registration
- [ ] SSRN author page
- [ ] Site URL on the first page of every working paper

The `.edu` directory link is worth more than every other item combined.
