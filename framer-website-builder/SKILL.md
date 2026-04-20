---
name: framer-website-builder
description: Bouw een volledig Framer-ready Figma websiteontwerp vanuit een gedetailleerde briefing. Gebruik deze skill altijd wanneer een gebruiker vraagt om een website te ontwerpen, een Figma bestand te maken voor Framer, of een briefing wil omzetten naar een werkend websiteontwerp. Triggert ook bij: "maak een website", "ontwerp in Figma", "Framer-ready design", "briefing naar design", "website opmaken". De output is een Figma bestand dat direct importeerbaar is in Framer zonder handmatige correcties.
compatibility:
  tools:
    - Figma MCP (vereist)
---

# Framer Website Builder Skill

Zet een gestructureerde klantbriefing om in een volledig Framer-ready Figma websiteontwerp. Het Figma bestand wordt gebouwd volgens strikte Framer-compatibele regels zodat de import in Framer direct bruikbaar is met minimale handmatige aanpassingen.

---

## Stap 1: Briefing verwerken

Extraheer de volgende informatie uit de briefing voordat je begint. Als informatie ontbreekt, gebruik dan verstandige defaults gebaseerd op de sector.

**Verplichte inputs:**
- Bedrijfsnaam en sector
- Doelgroep (wie bezoekt de site, wat zoeken ze)
- Paginastructuur (welke pagina's, welke secties per pagina)
- Primaire CTA (wat moet de bezoeker doen)
- Merkidentiteit (kleuren, lettertypen, tone of voice)
- Referentiesites of stijlrichting
- Taal van de content

**Defaults bij ontbrekende info:**
- Kleuren: extraheer uit bedrijfsnaam/sector context, gebruik nooit willekeurige defaults
- Lettertype: gebruik Google Fonts die beschikbaar zijn in Figma
- Taal: Nederlands tenzij anders aangegeven

---

## Stap 2: Design tokens definiëren

Definieer VOOR het bouwen de volledige token set. Gebruik deze consistent door het hele bestand.

```
Kleuren:
- Primary: [hex]
- Primary Dark: [hex]  
- Secondary: [hex]
- Background: [hex]
- Surface: [hex] (kaarten, secties)
- Text Primary: [hex]
- Text Secondary: [hex]
- Border: [hex]
- White: #FFFFFF

Typografie:
- Heading Font: [naam] (Google Font)
- Body Font: [naam] (Google Font)
- H1: [font] [gewicht] [grootte]px / [regelafstand]px
- H2: [font] [gewicht] [grootte]px / [regelafstand]px
- H3: [font] [gewicht] [grootte]px / [regelafstand]px
- Body Large: [font] [gewicht] [grootte]px / [regelafstand]px
- Body: [font] [gewicht] [grootte]px / [regelafstand]px
- Caption: [font] [gewicht] [grootte]px / [regelafstand]px

Spacing systeem (8px grid):
- xs: 8px
- sm: 16px
- md: 24px
- lg: 32px
- xl: 48px
- 2xl: 64px
- 3xl: 96px
- 4xl: 128px

Breakpoints:
- Desktop: 1440px canvas, content max-width 1200px
- Mobile: 390px canvas

Border radius:
- sm: 4px
- md: 8px
- lg: 16px
- xl: 24px
- pill: 999px
```

---

## Stap 3: Figma bestandsstructuur opzetten

Gebruik de Figma MCP om het bestand op te zetten met de volgende structuur:

**Pagina-indeling in Figma:**
```
Pages:
├── 🎨 Tokens & Components
│   ├── Color Styles
│   ├── Text Styles
│   └── Components (Buttons, Cards, Nav, Footer)
├── 🖥 Desktop
│   ├── Home
│   ├── [Pagina 2]
│   └── [Pagina N]
└── 📱 Mobile
    ├── Home
    └── [Pagina N]
```

---

## Stap 4: Framer-compatibele bouwregels (KRITISCH)

Dit zijn de harde regels die bepalen of de Figma import werkt in Framer. Wijk hier NOOIT van af.

### ✅ Auto Layout — altijd verplicht

- ELKE frame gebruikt Auto Layout. Nooit absolute positionering binnen containers.
- Richting: Vertical voor pagina-secties en kolommen, Horizontal voor rijen en navigatie
- Gap: gebruik altijd een waarde uit het spacing systeem
- Padding: altijd symmetrisch of uit spacing systeem
- Uitzondering: decoratieve overlappende elementen (bv. hero achtergrond) mogen absoluut zijn BUITEN de content container

```
Correct: Frame → Auto Layout (Vertical, gap: 24px, padding: 64px 120px)
Fout: Frame met handmatig gepositioneerde child elements
```

### ✅ Naamgeving — Framer leest dit direct

Gebruik altijd kebab-case, beschrijvende namen zonder speciale tekens:

```
Frames:     page-home, section-hero, section-features, section-cta
Secties:    hero, features, testimonials, pricing, faq, contact, footer
Components: btn-primary, btn-secondary, card-service, nav-desktop, nav-mobile
Tekst:      heading-h1, heading-h2, text-body, text-caption, label-btn
Iconen:     icon-arrow, icon-check, icon-menu
Afbeeldingen: img-hero, img-team, img-product
```

### ✅ Component structuur

Elk herbruikbaar element wordt een Figma Component:

**Verplichte components:**
- `btn-primary` — met hover variant
- `btn-secondary` — met hover variant  
- `nav-desktop` — met logo, links, CTA
- `nav-mobile` — hamburger variant
- `footer` — met links en copyright
- `card-[type]` — voor elke kaartvariant in het design

**Component opbouw:**
```
Component frame → Auto Layout
  ├── Slot voor icon (optioneel)
  ├── Tekst slot
  └── Slot voor decoratie (optioneel)
```

Gebruik Properties voor varianten (niet aparte frames):
- Button: variant = Primary / Secondary / Ghost
- State: Default / Hover / Disabled

### ✅ Tekst — Framer-vriendelijk

- Gebruik ALTIJD Text Styles, nooit handmatige font-instellingen
- Stel text resize in op: Auto Width voor koppen, Fixed Width voor body tekst in kolommen
- Geen tekst met clip/overflow hidden tenzij intentioneel
- Placeholder tekst moet realistisch zijn (geen Lorem Ipsum voor klantpresentaties)

### ✅ Afbeeldingen

- Gebruik Fill voor hero achtergronden (niet Fit)
- Geef elke afbeelding een beschrijvende naam (img-hero-team, niet Rectangle 24)
- Stel clip content in op de container, niet op de afbeelding zelf
- Gebruik aspect ratio constraints: bv. 16:9 voor hero, 1:1 voor avatars

### ✅ Kleuren

- Gebruik uitsluitend gedefinieerde Color Styles, nooit hardcoded hex in elementen
- Maak voor elke kleur een Figma Color Style aan
- Transparantie op een kleur = aparte style (bv. Primary/10, Primary/20)

### ❌ Verboden in Framer-ready design

- Absolute positionering binnen content containers
- Gegroepeerde elementen zonder Auto Layout (gebruik frames)
- Willekeurige namen (Rectangle 24, Frame 87, Group 3)
- Tekst buiten Text Styles
- Geneste groepen dieper dan 3 niveaus zonder duidelijke structuur
- Effects (shadows, blurs) direct op groepen — altijd op frames

---

## Stap 5: Secties bouwen — volgorde en aanpak

Bouw in deze volgorde via Figma MCP:

### 5.1 Styles en Components eerst

1. Maak alle Color Styles aan
2. Maak alle Text Styles aan
3. Bouw de verplichte components (btn, nav, footer, cards)

### 5.2 Desktop pagina's

Bouw elke pagina als een verticale Auto Layout frame (1440px breed, height: hug):

**Standaard sectie-template:**
```
section-[naam] (Auto Layout, Vertical, padding: 96px 120px, gap: 48px, fill: background-color)
  ├── section-header (Auto Layout, Vertical, gap: 16px, alignment: center)
  │   ├── heading-h2 (Text Style: H2)
  │   └── text-body (Text Style: Body Large, max-width: 600px)
  └── section-content (Auto Layout, Horizontal, gap: 32px)
      ├── card-[type] (Component)
      ├── card-[type] (Component)
      └── card-[type] (Component)
```

**Hero sectie specifiek:**
```
section-hero (Auto Layout, Vertical, min-height: 100vh, padding: 160px 120px 96px)
  ├── hero-content (Auto Layout, Vertical, gap: 24px, max-width: 640px)
  │   ├── heading-h1 (H1 style)
  │   ├── text-body (Body Large style)
  │   └── hero-cta (Auto Layout, Horizontal, gap: 16px)
  │       ├── btn-primary (Component instance)
  │       └── btn-secondary (Component instance)
  └── hero-visual (afbeelding of illustratie)
```

### 5.3 Mobile pagina's

- Canvas: 390px breed
- Padding: 24px horizontaal
- Alle horizontale rijen worden verticale stacks
- Nav wordt mobile nav component
- Font sizes: H1 -8px, H2 -6px, Body gelijk
- Sections: padding 64px 24px

---

## Stap 6: Content invullen

Gebruik realistische, sector-specifieke content:

- **Koppen**: Resultaatgericht, actief, specifiek (niet "Wij zijn experts in...")
- **Body tekst**: Kort, scanbaar, voordeel-gericht
- **CTA labels**: Concreet (niet "Meer info" maar "Bekijk ons werk" of "Plan een gesprek")
- **Afbeeldingstitels**: Beschrijvend voor de Framer CMS koppeling

---

## Stap 7: Kwaliteitscheck voor export

Loop voor export deze checklist na via de Figma MCP:

**Structuur:**
- [ ] Alle frames hebben Auto Layout
- [ ] Geen frames genaamd Rectangle, Group, Frame + nummer
- [ ] Alle pagina-frames zijn 1440px (desktop) of 390px (mobile)
- [ ] Component pagina bevat alle gedeelde elementen

**Stijlen:**
- [ ] Alle tekst gebruikt Text Styles
- [ ] Alle kleuren gebruiken Color Styles
- [ ] Geen hardcoded waarden

**Components:**
- [ ] btn-primary en btn-secondary aanwezig
- [ ] nav-desktop en nav-mobile aanwezig
- [ ] footer aanwezig
- [ ] Alle herbruikbare kaarten zijn components

**Content:**
- [ ] Geen Lorem Ipsum
- [ ] Alle afbeeldingsframes hebben beschrijvende namen
- [ ] CTA's zijn concreet en actiegericht

---

## Stap 8: Handoff instructies genereren

Na het bouwen, genereer een korte handoff notitie voor de Framer-import:

```
FRAMER IMPORT INSTRUCTIES — [Projectnaam]

Figma bestand: [link]
Gebouwd op: [datum]

Import volgorde:
1. Importeer eerst de Components pagina
2. Importeer Desktop pagina's per pagina
3. Importeer Mobile pagina's

Na import controleren:
- Responsive breakpoints instellen op 1024px (tablet) en 768px (mobile)
- CMS collections koppelen aan: [lijst van dynamische secties]
- Interacties toevoegen aan: nav (hamburger), btn-primary (hover), cards (hover)
- Fonts controleren: [Heading Font] en [Body Font] — activeer in Framer font manager

Bekende handmatige aanpassingen na import:
- Hero achtergrond: stel parallax scroll in indien gewenst
- Formulieren: vervang placeholder door Framer Form component
- Animaties: voeg scroll-triggered entrance animations toe per sectie
```

---

## Communicatie tijdens het bouwen

Rapporteer voortgang per stap:

```
✅ Stap 1: Briefing verwerkt — [X] pagina's, [Y] secties geïdentificeerd
✅ Stap 2: Design tokens gedefinieerd — [kleurpalet] + [fontpaar]
🔨 Stap 3: Bestandsstructuur aanmaken...
🔨 Stap 4: Components bouwen (1/5)...
```

Meld eventuele keuzes die je hebt gemaakt expliciet:
- "Heb Primary gekozen als #1E3A5F gebaseerd op de sector (zakelijke dienstverlening)"
- "Hero is gebouwd als full-width met tekst links, visueel rechts — aanpasbaar"

---

## Kwaliteitsstandaard

Het bestand is geslaagd als:
1. Import in Framer geeft minder dan 5 handmatige correcties
2. Alle secties zijn responsief instelbaar zonder herbouwen
3. Components zijn herbruikbaar en eenvoudig te wisselen
4. Een junior designer de structuur begrijpt zonder uitleg
