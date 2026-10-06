# Vizbee Roku docs map (developer.vizbee.tv)

**Fetch rule:** append **`.html`** to any path; the bare path returns a "Loading …" shell.
Roku appears under the **Omni**, **HomeSSO**, and **Continuity receiver** sections rather
than a single `/continuity/roku/` tree, so **derive exact paths** by fetching
`https://developer.vizbee.tv/continuity/overview/intro.html` (and `…/omni/overview/intro.html`)
and reading the `href=".../roku/..."` links.

## What to look for
- **HomeSSO · Roku** — TV sign-in: Setup, SDK Initialization, Handle Sign In Request, Handle
  Start Video during Sign In, UX guide, conceptual guide.
- **Continuity receiver (Roku)** — accept casts / deep-linked video handoff on the TV.
- **Omni · Roku** — if the channel is built on the Vizbee Omni framework.

## SDK source
Take the exact Roku SDK package, **version**, and install steps from the live doc page; do
not hard-code a version here.
