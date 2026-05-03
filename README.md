# Kill Yr Substack

A browser extension that automatically redirects Substack articles to an archiving service of your choice, including articles on custom domains.

## How It Works

The extension uses a three-tier approach to block Substack traffic:

**Tier 1: Known Substack domains.** All `*.substack.com/p/*` article URLs and `substack.com/@*` profile URLs are intercepted at the network level via `declarativeNetRequest`. The HTTP request to Substack never fires. The page never loads.

**Tier 2: Learned custom domains.** Once a custom domain Substack is detected (see Tier 3), the domain is cached and added as a `declarativeNetRequest` rule. Every subsequent visit to that domain is blocked at the network level, same as Tier 1. No traffic to Substack.

**Tier 3: First-visit detection.** The first time you hit a custom domain Substack, the page loads so the content script can sniff for platform fingerprints:

1. `<meta name="generator" content="Substack">` tag
2. Stylesheets or assets from `substackcdn.com`
3. Scripts from `substack.com` or `substackcdn.com`
4. Substack's React `AppFrame` component
5. RSS feed links pointing to `substack.com`
6. Substack-specific class names paired with subscribe forms

If detected, the page redirects to archive.is and the domain is cached for future visits (promoting it to Tier 2).

The popup shows a toggle to pause/resume redirects and a list of learned custom domains. Individual domains can be removed if a false positive is cached.

## Picking Your Archive Service

Kill Yr Substack 1.2.x adds a preference for where its redirects land. `archive.is` (the default) or Ghost Archive. To switch, head to the options page (`chrome://extensions` on Chromium, `about:addons` on Firefox), find Kill Yr Substack, and open preferences.

Why offer the choice? Some folks have an understandable issue with `archive.is` after [the maintainer DDoSed a blog that was investigating the site's ownership](https://arstechnica.com/tech-policy/2026/02/wikipedia-might-blacklist-archive-today-after-site-maintainer-ddosed-a-blog/). Their behavior is nasty and I don't agree with it.

`archive.is` stays the default tho. It's the most seamless link-to-archived-page experience I've come across. Just one click. And if the page has never been captured before the site grabs it on the spot.

Ghost Archive gets close. It searches for the URL first, and if nothing's been archived it shows an "Archive it now?" button. One extra click, but you still (eventually) get to where you were trying to go.

Pick whichever option makes you feel less gross.

## Install

### Chrome / Chromium

**From CRX:**
1. Download `kill-yr-substack.crx`
2. Go to `chrome://extensions/`
3. Enable **Developer mode** (top right)
4. Drag `kill-yr-substack.crx` onto the page

**From ZIP (unpacked):**
1. Download and extract `kill-yr-substack-X.Y.Z.zip`
2. Go to `chrome://extensions/`
3. Enable **Developer mode** (top right)
4. Click **Load unpacked** and select the extracted folder

**From source:**
1. Go to `chrome://extensions/`
2. Enable **Developer mode** (top right)
3. Click **Load unpacked** and select this repository directory

### Firefox

**From XPI:**
1. Download `kill-yr-substack-X.Y.Z.xpi`
2. Go to `about:addons`
3. Click the gear icon, select **Install Add-on From File...**
4. Select the downloaded XPI file

## Permissions

The extension requests `<all_urls>` host permission because custom domain Substacks can live on any domain. The content script runs lightweight DOM queries and bails immediately on non-Substack pages.

## License

MIT
