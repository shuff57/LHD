# LHD

The source of **[lefthanddev.com](https://lefthanddev.com)**: software written with the other hand.
It is the front page for everything built here, mostly tools for a statistics class, a CS lab and
a weld shop.

The whole site is one static page. There is no build step and no framework.

## What is in this repo

| Path | What it is |
|---|---|
| `index.html` | The entire page: markup, CSS and the small scripts for the theme toggle, filters and scroll reveals. |
| `fonts/` | Self-hosted WOFF2 files (Bricolage Grotesque, Caveat, JetBrains Mono). |
| `img/` | WebP screenshots for the project cards. |
| `favicon*`, `apple-touch-icon.png` | Icons. There are light and dark SVG favicons. |
| `robots.txt` | Allows everything. |

The contact form is not part of this repo. The page loads it from `contact.lefthanddev.com/embed.js`.

## Run it locally

Any static file server works:

```sh
python3 -m http.server 8741
```

Then open <http://localhost:8741>.

## Add or change a project

Each project is an `<article class="prob">` in `index.html`. Copy a neighbouring one, then:

1. Set `data-cat` to `class`, `make` or `lab`. The filter buttons use it.
2. Number it in `<span class="n">`. The numbers run in one sequence from the problem set into the
   "Loose-leaf" list below it, so adding a card means renumbering every card after it.
3. Update the project count in two places: the hero button ("Browse the N projects") and the
   "All N" filter button.
4. Link the live site and the source repo in the card's `links` line.

A card with a screenshot uses an `<img>` from `img/` (WebP, with `width` and `height` set). A card
without one uses a drawn `spec` panel instead, so copy an existing `print spec` card.

Projects that are source only go in the Loose-leaf list and link straight to their repository.

A project belongs on the page when it has a public `*.lefthanddev.com` address. Check that the
address answers before linking it.

## Deploy

The site is a Cloudflare Pages project named `lefthanddev`, served at `lefthanddev.com`. It is not
connected to Git, so **pushing to `main` does not publish it**. Deploy the committed tree by hand:

```sh
dir=$(mktemp -d)
git archive HEAD ':(exclude)README.md' ':(exclude).gitignore' | tar -x -C "$dir"
npx wrangler pages deploy "$dir" --project-name lefthanddev --branch main
```

Exporting `HEAD` first means only committed site files go live: no `README.md`, and no local
scratch files such as `.wrangler/` or `.playwright-cli/`.
