# Zinstrel News Desk

**Scan → Wire → Queue → Draft**, for Zinstrel's AI music coverage.

1. **Scan now** (sidebar) searches Google News across AI music beats and the trade press, and reads **The AI Musicpreneur** and the **AI Music Newsletter** directly from their sites (see `NEWSLETTERS` to add more). It finds each publisher's own link, reads the article, and has Claude write a one-line summary. Each story also gets an on-topic check, so non-music AI stories are hidden by default. Results save to the **Wire** table in AirMarcus.
2. **Wire** tab: browse by beat. Check stories, then **Queue selected** or **Skip selected**.
3. **Queue** tab: pick **Major Signal**, **Minor Signal**, or **News Brief** for each story you want written, and add an optional editor's note. If an article couldn't be read (a paywall), paste its text into the box. Then click **Draft**.
4. Claude **reports each story out** before writing. It treats the wire item as a tip, uses web search and fetch to find the primary material (the company's announcement, press release, filing, bill text, or original statement), and writes from that, with links to the primary sources rather than "according to [outlet]." Quotes come only from primary material. Each draft comes with reporting notes: editor flags, suggested follow-up interviews, and the primary sources used. Then Claude drafts in house style. The draft goes to the **Lovable CMS** as an unpublished draft. If Lovable isn't configured or rejects it, the draft goes to **Stories** in Airtable instead. Editor flags and word count show in the desk; they stay out of the CMS body.
5. **Drafted** tab: links to everything drafted.

Unused wire items (New or Skipped) older than 10 days are cleared automatically on each scan, so the base doesn't fill up.

## News only, where AI and music collide
Every scanned item passes four gates before it reaches the Wire:
1. **Not a directory, job, or about page.** AI-tool directories such as Quasa.io are blocked outright (see `BLOCKED_DOMAINS` to add more), along with job boards, about pages, and pricing pages.
2. **Not old.** The page's own publish date has to be recent, since Google stamps old pages with today's date.
3. **Has a real music angle.** A music word in the headline, or at least two in the article: music, song, artist, album, vocals, music video, Suno, Spotify, labels by name, and so on. Generic AI products never pass this gate.
4. **Is AI-music news.** Claude checks that it's where AI and music collide (AI-generated or AI-assisted music, music-making tech, AI music videos, AI artists, and the business, law, and licensing around them) and that it reports a specific, recent development.

**Re-check the wire** in the sidebar runs all four gates over what's already there.

## One row per story
When several outlets cover the same event, the Wire shows it once. Claude groups items that report the same announcement, deal, lawsuit, or release, and a headline-overlap check backs it up. The row uses the most authoritative version: the company's own newsroom first, then major outlets (Reuters, Bloomberg, NYT, Billboard, MBW, Variety, The Verge, and others), then music and tech trades, then press wires. Readable, substantive articles break ties. Everyone else goes under **+N outlets** on the row. When a better source covers a story later, it takes over the row. **Re-check the wire** also merges repeats that are already there. Use **Most-covered first** to sort by how widely a story is being covered.

## Atlas
The **Atlas** tab tracks where AI and music collide, by category, one line per item:
- **Research:** new papers on music generation, singing, and music understanding from arXiv
- **Models** and **Datasets:** new and trending music models and datasets on Hugging Face, with likes and downloads
- **Code:** new open-source music-AI projects on GitHub, with stars
- **Rights & Policy, Deals, Funding, and Education & Events:** screened news, one item per story, with how many outlets covered it

Click **Refresh Atlas** whenever you want an update. Every item is screened for an actual AI-music connection, so speech-only papers and generic AI tools are dropped, and each gets a one-line summary. Check items and click **Send to Queue** to write about them; they draft like any wire story. Items older than 45 days are cleared unless queued. Optional: a `GITHUB_TOKEN` secret raises GitHub's rate limit.

The Atlas is built from primary sources directly. It doesn't copy other sites' indexes; aimusic.events, for one, asks that its content and structure not be republished.

## Archive sync
The archive keeps growing on its own. Once per session the desk checks zinstrel.com's sitemap (Signals, Features, Tips & Tools) and the live feed, reads any piece it doesn't have yet, and saves it to the **Archive** table in AirMarcus. Click **📚 Sync archive** once after setup to backfill every zinstrel.com article. After that it stays current automatically.

## Story ideas
The **Ideas** tab suggests stories in Zinstrel's sphere. It draws on everything published on zinstrel.com and Substack (from the site's social feed), the timely stories on the Wire, Google's daily trending searches that touch AI or music, and, when connected, Google Analytics (top pages) and Search Console (the searches that bring readers, plus queries where Zinstrel ranks low). Claude checks for new developments with web search, then pitches each idea with a why-now, a suggested format, a search target, and primary sources to start from. Queue an idea just like a wire story.

Google Trends has no public API, so the desk uses Google's daily trending-searches feed. For keyword-level search data, connect Search Console.

### Connecting Google Analytics and Search Console (optional)
1. In Google Cloud, create a service account (or reuse one) and create a JSON key for it.
2. Enable the **Google Analytics Data API** and the **Google Search Console API** for that project.
3. In GA4, go to Admin → Property access management and add the service account's email as a **Viewer**. Copy the property ID from Admin → Property details.
4. In Search Console, go to Settings → Users and permissions and add the service account's email (Restricted is enough).
5. In Streamlit secrets, add `GA4_PROPERTY_ID`, `GSC_SITE_URL` (for example `sc-domain:zinstrel.com`), and a `[gcp_service_account]` block with the JSON key's fields, at the end of the secrets.

## Zinstrel's archive (continuity and internal links)
Every draft gets the five most related pieces Zinstrel has already published, each with its most relevant passages, plus the opening of a recent Signals issue as a voice reference. Claude links to the most relevant prior piece or pieces in the body, builds on what Zinstrel already established, and flags anything that updates or contradicts earlier coverage. The reporting notes list which prior pieces were linked. They also flag any Zinstrel link that isn't in the archive, so made-up URLs get caught, and flag a draft that links none. In the Queue, **Related Zinstrel coverage** shows what each draft will build on.

The archive is your Substack export (post text and email open rates; the subscriber list is not included). It ships as `zinstrel-archive.enc`, which is encrypted because most posts are paid-only and this repo is public. The app unlocks it with the `ARCHIVE_KEY` secret. Pieces published after the export, including zinstrel.com articles, come in automatically from the site's live feed as titles and summaries. To refresh the full text, export from Substack again and rebuild the file.

Story ideas use the whole archive to avoid repeating coverage, and use open rates to see what this audience responds to.

## Links per story
In the Queue, each story has **Links to include**, for up to 5 links. They save as you type. The draft reads each one, draws on it where relevant, and links to it where it supports a point. If a link isn't usable, the editor flags say why. Leave them blank and Claude finds its own sources.

## Summary
Each draft gets a fresh 1–2 sentence summary. If it repeats the headline or reuses any six-word run from the article, it's rewritten automatically. The summary goes to Lovable's `summary` field and leads the reporting notes.

## Formats
- **Major Signal:** about 300 words; the headline doubles as the email subject.
- **Minor Signal:** a simple header and 2–3 sentences.
- **News Brief:** 300 words minimum, up to about 600 if the story supports it.

## Secrets (Streamlit → Manage app → Settings → Secrets)
See `.streamlit/secrets.toml.example`. Required: `APP_PASSWORD`, `AIRTABLE_TOKEN`, `ANTHROPIC_API_KEY`. For Lovable drafts, also set `LOVABLE_DRAFTS_URL` and `LOVABLE_DRAFTS_KEY`.

## Tuning
Beats and keywords live in `BEATS`, trade sites in `TRADE_SITES`, and format guidance in `FORMATS` and `HOUSE_RULES`, all near the top of `zinstrel_news_desk.py`.
