## Societas – How to Add or Edit Articles

This guide explains how to manage your articles using `articles.json` without touching the core code.

The site now reads articles from **two places**:

1. The original `posts` array inside `index.html` (the code you had before)
2. The new `articles.json` file at the project root

All articles are merged together and then sorted by date. Over time, you can move everything into `articles.json` and even delete the old `posts` array if you want. The site will keep working either way.

---

### 1. Structure of `articles.json`

Open `articles.json`. It looks like this (shortened example):

```json
[
  {
    "id": 4,
    "slug": "legalised-corruption-lobbying-in-america",
    "category": "politique",
    "categoryLabel": "Politics & Society",
    "image": null,
    "title": "Legalised Corruption: The Case Against Lobbying in America",
    "subtitle": "Short subtitle goes here.",
    "date": "March 2026",
    "dateISO": "2026-03-01",
    "readTime": "12 min read",
    "excerpt": "Short teaser for the article.",
    "featured": true,
    "bodyBlocks": [
      { "type": "paragraph", "text": "First paragraph…" },
      { "type": "heading2", "text": "Section heading" },
      { "type": "paragraph", "text": "More text…" },
      { "type": "conclusion", "text": "Optional italic conclusion." }
    ]
  }
]
```

**Fields:**

- `id` – a number. Make it unique (5, 6, 7, …).  
- `slug` – URL‑friendly string, all lower‑case, words separated by `-` (no spaces).  
- `category` – internal key used for filters. Use one of:
  - `politique`
  - `cinema`
  - `law`
  - `sport`
- `categoryLabel` – how the category is displayed to readers (e.g. `"Politics & Society"`).  
- `image` – if you have an image URL or base64 string, put it here; otherwise set to `null`.  
- `title` – main article title.  
- `subtitle` – optional italicised subtitle shown under the title.  
- `date` – human‑readable date string (e.g. `"March 2026"`).  
- `dateISO` – machine date, format `YYYY-MM-DD` (for correct sorting).  
- `readTime` – e.g. `"8 min read"`.  
- `excerpt` – 1–2 sentence teaser used on the home page and blog list.  
- `featured` – `true` or `false` (you can use this later if you want to highlight certain essays).  
- `bodyBlocks` – ordered list of blocks that make up the article body.

**Block types inside `bodyBlocks`:**

- `"paragraph"` – normal paragraph text.  
- `"heading2"` – large section heading.  
- `"heading3"` – smaller sub‑heading.  
- `"blockquote"` – pulls text into a styled quote block.  
- `"conclusion"` – styled final section (italic block at the end).

Example block list:

```json
"bodyBlocks": [
  { "type": "paragraph", "text": "Intro paragraph…" },
  { "type": "heading2", "text": "First big idea" },
  { "type": "paragraph", "text": "Explaining the idea…" },
  { "type": "blockquote", "text": "A key sentence you want to highlight." },
  { "type": "conclusion", "text": "A closing reflection or takeaway." }
]
```

---

### 2. Adding a New Article

1. Open `articles.json`.  
2. Inside the outer `[...]` array, add a **comma** after the last article object, then paste a new object with the same structure as above.  
3. Choose:
   - A new `id` (not used before).  
   - A new `slug` (unique, lower‑case, `-` instead of spaces).  
4. Fill in `title`, `subtitle`, `date`, `dateISO`, `readTime`, `excerpt`, `category`, `categoryLabel`.  
5. Write your full article in `bodyBlocks`.  
6. Save the file and redeploy to Vercel.

Your new article will automatically:

- Appear in **Latest Essays** on the home page (if it is one of the newest by `dateISO`).  
- Appear in **All Essays** on the blog page.  
- Respect the **category filters** (Politics, Cinema, Law, Sport).  
- Open in the article view at `/#article/your-article-slug`.

---

### 3. Migrating Existing Articles from the JS Code

Right now, you also have an old `posts` array defined in the `<script>` inside `index.html`. The new system **merges**:

- everything from that `posts` array, plus  
- everything from `articles.json`.

You have two options:

1. **Gradual migration (safest for a beginner)**  
   - Leave the existing `posts` array alone for now.  
   - Each time you create a *new* article, only add it to `articles.json`.  
   - The site will show both old and new content together.

2. **Full migration (move everything into `articles.json`)**  
   - For each existing article in the `posts` array:
     - Copy its data into a new object in `articles.json`.  
     - Make sure `id` and `slug` are unique.  
     - Optionally copy the body text into `bodyBlocks`.
   - After you are confident everything looks good, you can remove the old `posts` array from `index.html`.  
   - The code is already written so that if `posts` does not exist, it simply uses `articles.json` alone.

**Important notes when migrating:**

- Avoid duplicates: don’t keep **two** entries with the same slug or the same id.  
- If two articles share the exact same `dateISO`, their order between each other may be arbitrary. This usually doesn’t matter, but if you care, give the newest article a slightly later date (e.g. `"2026-03-02"` vs `"2026-03-01"`).

---

### 4. How URLs and Sharing Work

- When you open an article, the URL hash becomes:

  `/#article/your-article-slug`

- The **“Share this article”** button uses the current URL, so if you have set the `slug` correctly, people can bookmark or share links to a specific essay and it will still work after deployment.

---

### 5. Common Mistakes to Avoid

- **Missing commas in JSON** – every article object must be separated by a comma, except the very last one. If you see a “JSON parse error” in the browser console, check commas and quotes carefully.  
- **Wrong category key** – `category` must be exactly one of: `politique`, `cinema`, `law`, `sport`. If you mistype it (e.g. `"politics"`), the article will not show up under the correct filter.  
- **Invalid dates** – keep `dateISO` in the form `YYYY-MM-DD`. The human `date` field can be any text.  
- **HTML tags in `text`** – `bodyBlocks` are plain text; you don’t need `<p>` or `<h2>` tags. The system wraps them for you based on the `type`.

If you get stuck, you can always:

- Temporarily remove the last article you added from `articles.json` and see if the site loads again.  
- Use the browser console to check for JSON errors or typos.

---

### 6. Quick Template to Copy

When you want to add a new article, copy this template into `articles.json` and fill it out:

```json
{
  "id": 999,
  "slug": "your-article-slug",
  "category": "politique",
  "categoryLabel": "Politics & Society",
  "image": null,
  "title": "Your article title",
  "subtitle": "Optional subtitle.",
  "date": "Month Year",
  "dateISO": "2026-04-01",
  "readTime": "10 min read",
  "excerpt": "Short teaser text for home and blog lists.",
  "featured": false,
  "bodyBlocks": [
    { "type": "paragraph", "text": "Intro paragraph…" },
    { "type": "heading2", "text": "First big idea" },
    { "type": "paragraph", "text": "Developing the idea…" },
    { "type": "conclusion", "text": "A closing reflection or takeaway." }
  ]
}
```

You can keep this guide (`ARTICLES_GUIDE.md`) in your repo as a reference. It does not affect the site’s code or behaviour.

