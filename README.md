# Real Estate JSON-LD — a standalone Hugo component

Two small partials that emit valid [schema.org](https://schema.org) JSON-LD
structured data for rental/property sites — one for an individual unit
(`jsonld-unit.html`), one for the property/business as a whole
(`jsonld-property.html`). No theme dependency; drop into any Hugo site.

Originally extracted from the [Apartments Hugo theme](https://propertysuitehq.com/themes/apartments),
generalized so it isn't locked to "apartment complexes" specifically —
single-family rental portfolios, storage facilities, vacation rentals, or
any small property business can use it by overriding the `@type`.

## Why this matters

Search engines (and AI assistants that cite search results) use structured
data to understand what a page is actually about — a listing with proper
`Apartment`/`ApartmentComplex` markup is more likely to show up correctly
in rich results (bedroom/bathroom counts, etc.) than one that's just plain
HTML text.

## Install

Copy `layouts/_partials/seo/jsonld-unit.html` and
`layouts/_partials/seo/jsonld-property.html` into your site's own
`layouts/_partials/seo/` folder.

## Usage

**Unit-level** — call from your unit/listing single-page template:

```go-html-template
{{ partial "seo/jsonld-unit.html" . }}
```

Front matter it reads (all optional except the page title):

```yaml
title: "Two-Bedroom Unit"
summary: "A spacious two-bedroom, one-bath unit with in-unit laundry."
bedrooms: 2
baths:
  full: 1
  partial: 0
occupancy: 4
tags: ["two bedroom", "pet friendly"]
type: "Apartment"   # optional override — see below
```

**Property-level** — call once, site-wide, typically in your base template's
`<head>`:

```go-html-template
{{ partial "seo/jsonld-property.html" . }}
```

Site config it reads:

```yaml
propertyType: "ApartmentComplex"   # optional override — see below
params:
  contact:
    phone: "555.123.4567"
    address:
      street: "100 Example St"
      city: "Anytown"
      state: "OH"
      zip: "45000"
```

## Adapting the schema type to your business

Both partials default to apartment-flavored types (`Apartment` /
`ApartmentComplex`), but accept an override:

| Your business | Unit-level `type` | Property-level `propertyType` |
|---|---|---|
| Apartment complex | `Apartment` (default) | `ApartmentComplex` (default) |
| Single-family rentals | `SingleFamilyResidence` | `RealEstateListing` |
| Storage facility | `Accommodation` | `SelfStorage` |
| Short-term/vacation rental | `Apartment` or `House` | `LodgingBusiness` |

Full list of valid types: [schema.org/Accommodation](https://schema.org/Accommodation)
and [schema.org/LocalBusiness](https://schema.org/LocalBusiness).

## Testing your markup

After deploying, validate with Google's
[Rich Results Test](https://search.google.com/test/rich-results) or the
[Schema Markup Validator](https://validator.schema.org/) — paste in your
live URL.

## License

MIT — see `LICENSE`.
