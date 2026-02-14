# Blur — Scoping Document

**Status:** Scoped
**Date Created:** 2026-01-22
**Last Updated:** 2026-02-14
**Decision:** Parked
**Source Chat:** https://claude.ai/chat/caf60fe0-796b-4f1d-9459-3aca4e34c645

---

## Problem Statement

Existing RSVP speed reading apps are either buried in features, broken, or have friction that gets in the way of just reading fast. Andy wants to upload an epub of a Ray Dalio book and read it in several hours at 600–1000+ WPM. Every app he tested (speed-reader.com, Reedy, others) fails on UX — clunky interfaces, no library, poor controls, bad chunking.

## Proposed Solution

A brutally minimal RSVP speed reader. No accounts, no social features, no gamification. Just a personal library and a clean reading interface that gets out of the way.

**Domain:** blur.is

## Target Platform

PWA (Progressive Web App) — installable, works offline, all data in IndexedDB. Share sheet integration for iOS would be ideal but may require a native wrapper later.

---

## Core Screens

### 1. Library View
- Dark background (#0A0A0A)
- Grid of book covers (2 columns mobile, 4+ desktop)
- Each book shows: cover image, title, progress indicator
- FAB to upload new book
- Tap book → open reader, resume from last position
- Long press → delete option

### 2. Reader View
- True black background (#000000)
- Crosshairs: thin off-white lines (#E8E8E8, ~1px, ~40% opacity) intersecting at screen centre
- Word displayed in clean sans-serif (Inter/SF Pro), off-white, 48–72px
- **ORP (Optimal Recognition Point):** Letter ~30% into the word coloured red (#FF3B30), positioned at crosshair intersection — eye never moves
- Speed controls: minus/plus buttons for increments, aqua slider for big jumps (100–1500 WPM range)
- Tap anywhere → pause/play
- Swipe right from left edge → return to library

### 3. Settings
- Text size (small/medium/large)
- Default speed
- Speed increment for +/- buttons
- Chunking intensity (subtle/normal/strong)

---

## Chunking Logic

Punctuation affects timing to give natural pauses:

| Intensity | Comma/semicolon | Full stop/question | Paragraph break |
|-----------|----------------|-------------------|-----------------|
| Subtle    | 1.25×          | 1.5×              | 1.75×           |
| Normal    | 1.5×           | 2.0×              | 2.5×            |
| Strong    | 2.0×           | 3.0×              | 4.0×            |

---

## Colour Palette

| Element | Hex |
|---------|-----|
| Background (library) | #0A0A0A |
| Background (reader) | #000000 |
| Text (primary) | #E8E8E8 |
| Text (secondary) | #888888 |
| ORP letter | #FF3B30 |
| Slider / accent | #32ADE6 |
| Crosshairs | #E8E8E8 @ 40% |

---

## MVP Scope

### Phase 1: Core
- Library view with book grid
- Upload epub files
- Epub text extraction
- Reader view with crosshairs and ORP
- Play/pause on tap
- Speed slider (100–1500 WPM)
- Plus/minus speed buttons
- Auto-save reading position
- Resume from last position
- Basic PWA (installable, offline)
- Settings: text size, default speed, speed increment

### Phase 2: Polish
- PDF support (practice for Suppertime recipe ingest)
- Chunking (punctuation pauses) with settings
- Bookmarks
- Book deletion
- Progress indicators (reader + book cards)
- Cover extraction from epub

### Phase 3: Nice to Have
- Share sheet integration (share epub/pdf to Blur)
- Keyboard shortcuts for desktop
- Import from URL
- Reading stats

---

## Technical Notes

- **Apple Books DRM:** Books purchased from Apple Books Store have FairPlay DRM — cannot be shared to Blur. DRM-free epubs (manually loaded, or from publishers like Pragmatic Bookshelf, Leanpub) work fine.
- **PDF extraction** can be messy (columns, headers, footers). May need heuristics. Could punt on PDF for MVP.
- **Epub edge cases:** complex formatting, images, tables — need strategy for non-text content.
- **Performance:** 100k word book = ~100k array items. Should be fine in memory but worth testing on mobile.

## Evaluation Criteria

- [ ] Does Andy personally have this problem? **Yes — actively looking for a good speed reader**
- [ ] Would Andy use this daily/weekly? **Weekly for book reading**
- [ ] Can v1 ship in under 2 weeks? **Yes — Phase 1 is a weekend build**
- [ ] Does it fit under Dopamine Labs? **Yes — personal productivity tool**
- [ ] Does it overlap existing products? **No**
- [ ] Clear monetisation path? **Freemium or one-time purchase. Small market though.**
- [ ] Vitamin or painkiller? **Vitamin — nice to have, not urgent**
- [ ] Reduces executive function load? **Yes — removes friction from reading**

## Open Questions

- PWA vs native iOS? Share sheet integration pushes toward native wrapper.
- PDF support priority — is it worth the complexity for MVP?
- Market size — is this a personal tool or a business?
