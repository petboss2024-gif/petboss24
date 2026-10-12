# Petboss 24

Pet-Sitters First, Pets Always. The single source for everything visual at Petboss 24: marketing pieces, the app and the landing page. Master version, approved October 2026.

## Voice

- **Boss attitude:** confident, minimalist, professional, with light humor that never overdoes it.
- Public name **Petboss 24** (only the P is capital). Handle `@petboss24`. File prefix `PB24`.
- Marketing in English, for the SF Bay Area. Contact: text or WhatsApp, (650) 773-2089.

## Light and dark

The brand has two modes and both apply everywhere: app, landing page and marketing. Today the app and landing page run in dark; light comes later. Marketing picks the mode per piece. The orange field is part of the brand too.

## Foundations

- **Orange acts, violet highlights.** Same rule in light and dark. Orange is for actions: primary buttons and anything the user taps to move forward. Violet is for offers, promos and news: small, punctual, at most one per screen, never a button.
- **Secondary text:** #B3B3B3 on dark, #6B6B6B on light, for dates, descriptions and hints.
- **Text on orange:** white, bold, 18px or larger. Small text goes on white or dark, never on orange.
- **Type:** Raleway for headlines, Poppins for everything else.
- **Shape:** radius 14px; 8px for pieces nested inside a container; pills only on chips. Flat: no shadows, depth comes from the surface color.
- **Spacing:** 4, 8, 16, 24.

## Logo

Full logo on orange, on white or on dark. Dog head alone in white or orange, for avatars, the app icon and small spaces. Choose by ground, never recolor, never place on violet.

## Components

Button (primary and secondary) and Chip. Styles in `components/bundle.css`, classes start with `pb-`.

To use them outside the Design System viewer, load [`tokens.css`](tokens.css) first (CSS variables and `@font-face` for the fonts in `fonts/`), then `components/bundle.css`. Dark is the default; `data-theme="light"` switches to light. The component previews already link both files.

Fonts: Poppins and Raleway, SIL Open Font License 1.1 ([`fonts/OFL.txt`](fonts/OFL.txt)).
