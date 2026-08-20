# tacedge.org

Static site for TacEdge 2026. Single `index.html`, no build step, no dependencies.

Working on this with Claude Code? Read `CLAUDE.md` first, then `CONTENT.md`.
`CONTENT.md` is the factual source of truth and marks everything still to be
confirmed. It is deliberately kept out of this repository — see `.gitignore`.

## Publishing on GitHub Pages

1. Create a repo (e.g. `platopod/tacedge` or a new `tacedge` org).
2. Push `index.html` and `CNAME` to the default branch.
3. Settings → Pages → Source: Deploy from a branch → `main` / `root`.
4. Settings → Pages → Custom domain: `tacedge.org`. Tick "Enforce HTTPS" once the
   certificate is issued (can take up to an hour).

## DNS at Squarespace

Squarespace is the registrar; DNS records need to point at GitHub. In Squarespace
Domains → DNS Settings, remove the existing A/CNAME records for the apex and `www`,
then add:

    A     @     185.199.108.153
    A     @     185.199.109.153
    A     @     185.199.110.153
    A     @     185.199.111.153
    CNAME www   <your-github-username>.github.io.

Propagation is usually minutes but can take a few hours.

## Registering interest

The site currently asks people to email `a.lenskiy@unsw.edu.au` via a `mailto:`
link that pre-fills the fields. This is deliberate: it captures interest without
putting attendee details into a third-party service before UNSW has said which
tool they want used.

To restore a real form later, the original markup and CSS are in the initial
commit — check with UNSW first which form or registration tool they prefer.

## Migrating to WordPress later

The markup is plain semantic HTML with all CSS in one `<style>` block and the
waterfall in one `<script>` block. To port: the CSS becomes the theme stylesheet,
each `<section>` becomes a page section or block, and the canvas script drops into
the theme footer unchanged.
