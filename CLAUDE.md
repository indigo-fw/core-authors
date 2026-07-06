# core-authors — CLAUDE.md

Multi-author system with profiles, author pages, and polymorphic content attribution. Decouples editorial identity from user accounts (`cms_authors` has an *optional* `userId` link; `cms_author_relationships` is a polymorphic junction so any content type can have authors).

Surface (schema, components, pages, seed): `module.config.ts` + `bun run indigo map core-authors`.

## Content Type Integration

Enable in `src/config/cms.ts` per type: `postFormFields: { authors: true }`, `authorInJsonLd: true`.

## Key Helpers

Live list: `lib/` (JSDoc'd; map with `bun run indigo map core-authors`). Non-obvious: `syncAuthorRelationships` **replaces** all authors for an object (not additive); use `batchGetAuthorsForObjects` on list pages to avoid N+1; `generateNewsSitemap` includes only articles from the last 2 days (Google News rule).

## Router Endpoints

Live list: `routers/authors.ts` (JSDoc per procedure → `src/generated/procedure-docs.ts` / MCP `search_tools`).

## Wiring

`AuthorPickerPanel` manages its own state, saves via `trpc.authors.syncRelationships`. For new posts, buffers until parent form calls `onSaveRef.current(newPostId)`.

Seed: `seedAuthors()` creates 3 demo authors on `bun run init`.
