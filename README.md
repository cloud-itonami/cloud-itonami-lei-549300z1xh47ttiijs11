# cloud-itonami-lei-549300z1xh47ttiijs11

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by ALICORP SAA.**

Public-register record for **ALICORP SAA** (Peru), keyed by its ISO 17442 Legal
Entity Identifier, per ADR-2607110300 (`cloud-itonami-lei-corporate-tos-catalog`,
`com-junkawasaki/root`). Read-only reference/archive repository — not a governed
Advisor/Governor actor.

## Company

- **Legal name**: `ALICORP SAA` (GLEIF also holds an earlier spelling under `PREVIOUS_LEGAL_NAME`)
- **LEI**: `549300Z1XH47TTIIJS11` — entity status ACTIVE, registration ISSUED, `FULLY_CORROBORATED`
- **Jurisdiction**: PE (Peru); registered office in the Constitutional Province of Callao (`PE-CAL`)
- **National id**: RUC `20100055237` (Registro Único de Contribuyentes, SUNAT)
- **Legal form**: ELF `PKRY` — Sociedad Anónima Abierta
- **Entity created**: 1992-10-09; LEI first issued 2013-04-10

Each of these is cited to a public register in `facts/catalog.edn` and checked
live by the gate below. None of them is asserted here on its own authority.

## Data

- `facts/catalog.edn` — 56 verified public-register citations, each with the claim the URL is relied on for.
- `80-data/public/tos.journal.edn` — a document retrieval with source URL, retrieval date and sha256. **Read the correction below before using it.**

### Correction: what `tos.journal.edn` actually holds

The journal records `:tos/doc-type :privacy-policy` for a document fetched from
`https://www.alicorp.com.pe/privacy-policy` on 2026-07-25. **That document is not
a privacy policy.** The archived text (9,862 chars, sha256 `0c4552d3…`) contains
the marker `404 | Alicorp Consent Details` — it is the site's 404 page, carrying
the cookie-consent banner that page renders. Measured again 2026-08-20: the same
URL still answers **HTTP 404**.

The capture itself is sound — the recorded sha256 matches the recorded text
exactly — so the journal is left as retrieved rather than rewritten. What is
wrong is the label, and the earlier version of this README that repeated it.
Nothing in `facts/catalog.edn` depends on that document, and the company's own
site is not cited there: it is not a register.

Finding a genuine published Alicorp privacy policy and archiving it is
outstanding work, not something this repository currently has.

## Verifying the citations

```sh
nbb tools/verify_citations.cljk facts/catalog.edn --min 50
```

For every row in `facts/catalog.edn` the gate performs a live `GET`, requires
HTTP 2xx, and requires `:cite/expect-substring` to appear in the response body.

Exit codes distinguish three outcomes that must never share one:

| exit | meaning |
| --- | --- |
| `0` | every citation was checked and every one held |
| `1` | a citation is **wrong** — the body no longer carries the claim (`DRIFT`) |
| `2` | the gate **could not answer** — catalog missing, unparseable, empty, or fewer rows than `--min` |

`--min` is a floor, not a target: it exists so that a truncated or half-written
catalog cannot report a pass by checking two rows and finding both fine.

### What the gate is actually testing

Three independent authorities are cited — GLEIF, Colombia's **RUES** (the
national commercial register the chambers of commerce administer, republished on
Colombia's open-data portal) and Ecuador's **SRI** (the national tax authority).
The second and third were not chosen because they agreed; they are the
registration authorities GLEIF itself names on this entity's children, reached by
following GLEIF's own `RA` pointers. Two joins carry the weight:

- **NIT 900102580-3.** GLEIF lists Alicorp Colombia S.A.S as a direct child,
  validated at `RA000101` — which GLEIF resolves to the Bogotá Chamber of
  Commerce. Asked for NIT 900102580, RUES returns `ALICORP COLOMBIA S.A.S` *and*
  gives its chamber as `BOGOTA`. Both the parent–child edge and the chamber are
  corroborated by a register that does not source them from GLEIF.
- **RUC 0992711523001.** GLEIF lists Inbalnor S. A. (EC) as a direct child,
  validated at `RA000691` = the SRI. Asked for that RUC, the SRI returns
  `INBALNOR S.A.`, status `ACTIVO`, principal activity *fabricación de alimentos
  preparados para animales acuáticos* — aquafeed, which is the line Alicorp runs
  through Vitapro, itself listed as another child of the same LEI. A tax
  authority with no LEI field corroborates both the edge and the business.

**Status alone cannot carry these rows.** Measured 2026-08-20, every one of the
three authorities answers 2xx for a *wrong* identifier: RUES returns HTTP 200 and
an empty array, the SRI returns HTTP 204 (and HTTP 200 with a *different real
company* for another valid RUC), and GLEIF returns HTTP 200 with `"total":0`. So
it is the substring, not the status code, doing the work — which is the property
the gate exists to hold.

**Some rows identify; some only describe.** Repointing all 31 subject-record URLs
at a real sibling LEI made 24 rows `DRIFT` and left 7 standing — jurisdiction PE,
status ISSUED, the managing LOU, and so on, all of which are true of the sibling
too. Those are attribute rows, and they mean something only because the identity
rows in the same block pin *which* entity is being described. The gate still
fails as a whole, but `facts/catalog.edn` says which rows are which rather than
letting a reader mistake one for the other.

`facts/catalog.edn` also records, in its header, the sources deliberately *not*
cited and why — including SUNAT's own RUC lookup, which is the citation this
repository would most like to have: GLEIF names it as the authority that
validated this entity, but a `GET` returns only the search form, so there is no
substring a body check could honestly assert.
