# Protecting what we sell, and consistency

Public content names the problem and shows the outcome. It does not explain how the solution works.

**Safe to publish:** the problem and what it costs, the outcome we deliver, who it is for, proof it works, price or how pricing works, how to start.

**Never publish unless the project file says otherwise:**
- Methods and step-by-step processes that someone could follow without us
- Prompts, scoring models, thresholds, weights, classification logic, routing rules
- Internal architecture, vendor stack, field mappings, API shapes, package names
- Internal decision IDs, ticket numbers, TODOs, "per D-013" style references
- Customer data, even when anonymized, unless approved for that piece

**Boundary test, run on every piece before it ships:**

| # | Question | If yes |
|---|---|---|
| B1 | Could a competitor or a client's in-house team use this to rebuild what we deliver? | Rewrite |
| B2 | Does it name more than one protected element from the project's list in the same piece? | Rewrite |
| B3 | Does it describe how something works instead of what it produces? | Rewrite |

**Public means reachable.** A page that is live and crawlable is public, whether or not it is advertised. Check robots.txt, the sitemap and noindex before calling anything "unlisted." Documentation, demos, sign-in pages and app UI visible before login follow the public rules in full. Detail belongs behind login or under NDA, and gets marked as such.

**If something protected already went live:** pull it, record the first live date from hosting or deploy logs, and tell whoever handles IP and legal. Do not edit the evidence away first.

---

Most audit findings were not bad writing. They were two surfaces saying different things. Each project keeps a **locked facts register** in its `WRITING.md`. Before publishing, check the text against it.

Lock these at minimum:
- Company, product and tier names, spelled and capitalized one way
- Positioning line and tagline (one version, with the date it was locked)
- Prices and what each includes
- Counts (features, integrations, languages, team size) and which number is current
- Contact addresses and booking links
- Supported platforms and what is not built yet
- Who the author is and the titles used

One number or name, everywhere: site, app, billing page, emails, decks, LinkedIn bio, meta descriptions, alt text. When a fact changes, update the register first, then search every surface.

Also check:
- Own articles that argue against what our own product or nav does
- Seed data, demo content and sample records that tell stories unrelated to the project
- Placeholder text, test copy, raw status codes ("collecting_data") and dev links in anything a customer can see
