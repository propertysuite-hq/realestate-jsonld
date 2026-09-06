# Real Estate JSON-LD — a standalone Hugo component

A small, theme-independent Hugo component that emits schema.org JSON-LD structured data for rental and property websites.

It provides two partials:

- `jsonld-unit.html` — structured data for an individual rentable unit or listing.
- `jsonld-property.html` — structured data for the property or property business as a whole.

There are no package dependencies and no theme dependency. Copy the partials into any Hugo site and configure the fields you want to expose.

Originally extracted from the [Apartments Hugo theme](https://apartments.propertysuitehq.com/), the component was generalized so it isn't locked to apartment complexes specifically. Single-family rental portfolios, storage facilities, vacation rentals, and other small property businesses can adapt the schema type through configuration.

## Why this matters

Structured data helps search engines understand what a page represents. Providing appropriate schema.org markup can make property and listing information easier for search engines and other systems to interpret than relying on page text alone.

This component focuses on generating the structured data; it does not provide a complete SEO strategy or guarantee rich results.

## Install

Copy these files into your Hugo site's partials directory:

```text
layouts/
└── _partials/
    └── seo/
        ├── jsonld-unit.html
        └── jsonld-property.html
```

No package manager, build step, or theme installation is required.

## Usage

### Unit-level structured data

Call the unit partial from the single-page template used for an individual unit or listing:

```go-html-template
{{ partial "seo/jsonld-unit.html" . }}
```

The partial reads values from the current page's front matter. All fields are optional except the page title.

Example:

```yaml
title: "Two-Bedroom Unit"
summary: "A spacious two-bedroom, one-bath unit with in-unit laundry."
bedrooms: 2
baths:
  full: 1
  partial: 0
occupancy: 4
tags: ["two bedroom", "pet friendly"]
type: "Apartment"   # optional schema.org @type override
```

The unit partial also automatically includes images from the page bundle when page resources of type `image` are available.

### Property-level structured data

Call the property partial once for the site, typically from the base template's `<head>`:

```go-html-template
{{ partial "seo/jsonld-property.html" . }}
```

The property partial reads the property type and contact/address information from the Hugo site configuration.

Example:

```yaml
propertyType: "ApartmentComplex"   # optional schema.org @type override
params:
  contact:
    phone: "555.123.4567"
    address:
      street: "100 Example St"
      city: "Anytown"
      state: "OH"
      zip: "45000"
      country: "US"
```

## Adapting the schema type to your business

Both partials default to apartment-flavored schema types (`Apartment` / `ApartmentComplex`), but both support an override so the component can be adapted to other property businesses.

| Your business | Unit-level `type` | Property-level `propertyType` |
|---|---|---|
| Apartment complex | `Apartment` (default) | `ApartmentComplex` (default) |
| Single-family rentals | `SingleFamilyResidence` | `RealEstateListing` |
| Storage facility | `Accommodation` | `SelfStorage` |
| Short-term/vacation rental | `Apartment` or `House` | `LodgingBusiness` |

Choose a schema.org type that accurately represents your business and the entity being described. The component does not restrict the override to the examples above.

## Testing your markup

After deploying your site, validate the generated structured data with:

- Google's [Rich Results Test](https://search.google.com/test/rich-results)
- The [Schema Markup Validator](https://validator.schema.org/)

Test a live URL or the generated markup and verify that the resulting schema accurately represents the content on the page.

## License

MIT — see [`LICENSE`](LICENSE).
