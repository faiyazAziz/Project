# Attendance & Image Quiz (frontend + MantleDB)

## Setup
1. Edit `config.js`: set a long, unique `namespace` and change `adminCode`.
2. Add source URLs to `pageUrls`, or add initial members as `{id:"m001",name:"Name",code:"unique-code"}`.
3. Open with VS Code Live Server or host these static files on GitHub Pages/Netlify/Cloudflare Pages.
4. Admin panel lets you add members, update source URLs, and import an image pool JSON fallback.

## Shared storage layout
- `config`: members, source URLs, image pool.
- `members/<memberId>`: `memberCode`, attendance, scores, `testforimgdone`, `pageQuizCounts`.

The member code is checked on first use on a device and cached in that browser's localStorage. A new device asks for the code once.

## Image selector
The browser scraper preserves the selector and data extraction:
`.wookmark-initialised li.thumbwook a.rel-link img`, reading `data-src` and `alt`.
Browser fetches require the source website to allow CORS. If blocked, use the Admin image pool fallback JSON.

## Critical limitations
MantleDB's unclaimed namespace allows anyone who knows the namespace to read/write it. The admin code is shipped in frontend JavaScript and is not secure; member codes are also stored in the shared database. This is a convenience gate, not authentication. Do not store sensitive data.
This prototype uses per-member documents to reduce write collisions between different users. It still uses read-modify-write for one member, so two devices submitting simultaneously for the same member can race. It is not transaction-safe.
The app checks one attendance per member per India calendar date, selects four unused images, and limits each member to six completed quizzes per source page.
Check MantleDB's current free-tier entry, payload, request and inactivity limits before relying on it. Unclaimed namespaces may be purged after inactivity.
