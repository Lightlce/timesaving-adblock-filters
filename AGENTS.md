# AGENTS.md

## Purpose

This repository maintains adblock-style content filter lists focused on reducing distracting or compulsive Instagram browsing.

The current repo is intentionally small:

- `README.md` explains end-user setup and links to the published raw filter URLs.
- `CONTRIBUTING.md` gives lightweight contribution rules.
- `lists/InstagramAntiDoomScrolling.txt` hides high-engagement surfaces like feed, search, and reels.
- `lists/InstagramAntiDistractions.txt` hides lower-level distractions like notes, markers, pop-ups, and clutter.

## Working Style For Agents

Prefer targeted, minimal edits. This repo is essentially a curated set of selector lists, so even small changes can affect the usability of Instagram's web UI.

When making changes:

- Preserve the header metadata in each list file:
  - `! Title`
  - `! Description`
  - `! Homepage`
  - `! Version`
- Keep one filter rule per line.
- Keep existing comment sections such as `! Filters for mobile` and `! Desktop-specific filters`.
- Update the `! Version` field whenever a list file changes.
- Update `README.md` if a new list is added or the user-facing behavior changes materially.

## Repo-Specific Conventions

The filter files use Adblock cosmetic filter syntax with very specific Instagram DOM selectors. Many selectors are long class chains, which means they are likely brittle and should be edited carefully.

Guidelines:

- Avoid broad selectors unless the intent is clearly to hide an entire surface.
- Prefer adding a narrowly-scoped selector over rewriting a working block.
- Do not reformat selector lines just for style; diff clarity matters more than aesthetics here.
- Treat comments like `OUT OF DATE` as cues to verify and refresh selectors rather than delete them blindly.
- Preserve file naming style for new lists: descriptive `PascalCase` names ending in `.txt`.

## Validation Expectations

There is no automated test suite in this repository.

Validation is therefore manual:

- Check that each filter file still follows adblock list formatting.
- Review for accidental duplicate or malformed rules.
- If behavior changes are introduced, manually verify the affected Instagram surface in a browser when possible.
- Confirm that changes do not obviously break the README's described purpose for each list.

## Safe Change Boundaries

Safe tasks for an agent:

- Add or refine individual cosmetic filter rules.
- Refresh stale selectors.
- Improve documentation and contribution guidance.
- Add a new filter list plus matching README documentation.

Tasks that deserve extra caution:

- Large rewrites of existing selector blocks.
- Removing rules without understanding what UI they suppress.
- Renaming list files or changing published raw URLs.
- Making assumptions about desktop coverage where the file already notes drift or staleness.

## Current Observations

Based on the current files:

- `InstagramAntiDoomScrolling.txt` is organized into mobile filters plus a small desktop-specific section.
- `InstagramAntiDistractions.txt` also separates mobile and desktop rules, and explicitly marks the desktop block as out of date.
- `CONTRIBUTING.md` expects one filter list per pull request and one filter per line.

## First Things To Check Before Editing

1. Which list matches the requested behavior change.
2. Whether the change targets mobile, desktop, or both.
3. Whether a nearby selector already covers the same UI.
4. Whether the README description should change after the edit.
