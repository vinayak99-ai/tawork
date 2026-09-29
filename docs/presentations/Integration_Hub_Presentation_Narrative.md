# Integration Hub — Presentation Narrative

Speaker narrative for a 45-minute senior-management presentation on transfer-agent
integration with middle- and back-office systems. Each section below is the "story" for one
of the three source slides — text-only, bullet-point, written to be read or presented
verbatim.

---

## Slide 1 · The Story
### We Mapped Every Data Point, Not Just the Systems
*Source slide: "Transfer Agent Data Flow — Front, Middle & Back Office"*

- **We started at the data level, not the system level.** Before drawing any box-and-arrow
  architecture, we mapped the complete set of data points that actually move between the
  transfer agent and everything around it — front office in, middle and back office out.
- **Three front-office sources feed the TA today.** Customer onboarding (KYC), the client
  order portal, and AML/financial-crimes screening — and two of those three are two-way
  exchanges, not one-directional feeds, which matters for how we design the integration.
- **Out of the TA, we catalogued the real data types in motion.** Orders, order corrections,
  payments/cash, dividends, tax info, customer info, holdings, and reconciliation — this is
  illustrative, not exhaustive; the full inventory goes further.
- **Every connection is explicitly labeled one-way or two-way.** That distinction isn't
  cosmetic — it determines whether a new TA integration only needs to push data out, or has
  to support a live round trip.
- **This is the foundation everything else in this deck stands on.** Every requirement,
  problem statement, and business outcome we'll walk through next traces back to this map.

> **The message to leadership:** before we ask for a Hub, we did the discipline of knowing
> exactly what data exists and where it moves today.

---

## Slide 2 · The Story
### We Went Down to the File Level, Not Just the Concept
*Source slide: "FDIT Specific Data Points Analysed"*

- **We inventoried every individual interface, file by file.** Every TA2000-to-ITACC file,
  and the reverse, is documented on its own line — not summarized away into a generic
  "data exchange" box.
- **Four attributes captured per file, every time.** What it carries, whether it's PII, a
  transaction, or fund-level data, how often it runs (EOD, EOD batch, real-time batch, EOM),
  and any operational nuance that makes it non-trivial.
- **Some of these nuances are business-critical, not cosmetic.** The Same-Day Orders file
  (MAUI) runs in real-time batch specifically because the Portfolio Manager needs that
  information immediately — a live dependency, not a reporting nice-to-have.
- **Compliance-relevant nuance is called out by name.** Tax withholding carries a cost-basis
  requirement; Blue Sky reporting needs customer type and domicile — exactly the details that
  break a generic integration if they're not designed in from the start.
- **Every PII-bearing interface is explicitly flagged, not implied.** Payment Hub exchanges
  and TA2000-to-FREP (our N-MFP SEC regulatory reporting) both carry PII and are marked as
  such — so data-governance requirements are visible on day one, not discovered later.

> **The message to leadership:** this is the granularity a real integration effort requires —
> and we already have it. Any future TA gets evaluated against this exact inventory, not a
> simplified version of it.

---

## Slide 3 · The Story
### This Is the Ask — Built on What We Already Know
*Source slide: "Integration Hub — Common Component"*

- **We're already managing three live TA providers, with a fourth built in from day one.**
  Superstate, Centrifuge, and FT Offering are onboarded today; the model explicitly reserves
  a slot for a future TA — extensibility isn't an afterthought, it's a starting requirement.
- **We support two fund types today and left room for more.** 40 Act (registered) and
  Private (registration-exempt) are both in scope now, with future fund types designed in
  rather than bolted on later.
- **The problems we're solving are concrete, not abstract.** Every new TA or fund type
  currently means bespoke integration against multiple formats and multiple existing
  systems; PII handling is inconsistent across those integrations; and there's no single,
  reusable definition of the "TA capabilities" a new provider has to meet.
- **We mapped every downstream system with the same rigor as slide 1.** Fund
  accounting/payments/pricing/dividends (FFIO), AML, financial crimes compliance,
  KYC/onboarding, regulatory and investor reporting, and custody/settlement — plus room for
  future systems.
- **The business outcome is specific and measurable.** Eliminate bespoke integration per TA
  per system, enforce one consistent enterprise standard for PII handling, and reduce time
  to market for every new fund launch.

> **The message to leadership:** slides 1 and 2 are the evidence; this slide is the ask — one
> well-understood integration pattern that scales across every TA and fund type we have
> today, and every one we add tomorrow.
