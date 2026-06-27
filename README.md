# Système Solaire Interactif

## Vue d'ensemble

Ce projet crée une animation interactive du système solaire en utilisant uniquement HTML et CSS. Les planètes orbitent autour du soleil selon des périodes proportionnelles à leurs périodes réelles, créant une visualisation éducative et esthétique.

## Architecture

### Structure HTML

Le système utilise une liste ordonnée (`<ol>`) contenant 10 éléments (`<li>`), un pour chaque objet céleste :

```html
<ol class="solar-system">
  <li class="orbit">
    <div class="label">Sun</div>
    <div class="planet sun"></div>
  </li>
  <!-- Planètes avec structure orbitale -->
</ol>
```

#### Structure d'une planète en orbite

```html
<li class="orbit">
  <div class="label">Earth</div>
  <div class="orbit-ring"></div>  <!-- Anneau visuel -->
  <div class="axis">              <!-- Axe de rotation -->
    <div class="planet earth"></div>
  </div>
</li>
```

**Composants:**
- **`.label`** : Nom de la planète affiché
- **`.orbit-ring`** : Cercle blanc représentant l'orbite
- **`.axis`** : Conteneur tournant qui anime la planète
- **`.planet`** : La planète elle-même (cercle coloré)

### Technique CSS Grid

Le système utilise **CSS Grid** avec une disposition subgrid pour organiser les orbites :

```css
.solar-system {
  display: grid;
  grid-template-rows: repeat(10, auto);
  grid-template-areas:
    "pluto"
    "neptune"
    "uranus"
    ...
    "sun";
}
```

Chaque orbite utilise les zones nommées pour se positionner correctement.

## Paramètres principaux

### Tailles

Les tailles des planètes sont définies via des variables CSS :

```css
--sun-diameter: 8.5dvh;      /* Taille du soleil en viewport height */
--mercury-size: 0.22;         /* Taille relative au soleil */
--jupiter-size: 0.72;         /* Jupiter est la plus grande */
--pluto-size: 0.11;           /* Pluton est la plus petite */
```

### Périodes orbitales

Les périodes sont proportionnelles à la réalité astronomique :

```css
--earth-duration: 5s;         /* Période de base (Terre en 5 secondes) */
--mercury-period: 0.24;       /* Mercure 0.24x la période terrestre */
--saturn-period: 29.46;       /* Saturne 29.46x la période terrestre */
```

La durée réelle d'une orbite = `earth-duration × planet-period`

## Mécanismes d'animation

### 1. Transformation géométrique

```css
.axis {
  transform-origin: center calc(100% - var(--sun-diameter) / 2 - var(--sun-margin));
  animation: orbit var(--planet-duration) linear infinite;
}
```

Le point d'origine est placé au centre du soleil, ce qui permet à la planète de tourner autour.

### 2. Animation d'orbite

```css
@keyframes orbit {
  from { rotate: 0turn; }
  to { rotate: 1turn; }
}
```

Une simple rotation sur un tour complet, sur la durée définie par `--planet-duration`.

### 3. Disposition responsive

L'utilisation de `dvh` (dynamic viewport height) rend le système adaptatif à différentes tailles d'écran :

```css
--sun-diameter: 8.5dvh;  /* S'adapte à la hauteur de l'écran */
```

## Couleurs des planètes

Chaque planète a une couleur spécifique basée sur sa réalité :

- **Soleil** : `#ffd800` (Jaune-or) avec un halo lumineux
- **Mercure** : `#989494` (Gris)
- **Vénus** : `#e1d59d` (Beige)
- **Terre** : `#008bb3` (Bleu)
- **Mars** : `#dfa272` (Rouille)
- **Jupiter** : `#af7045` (Marron-rouges)
- **Saturne** : `#d3b57c` (Beige-ocre)
- **Uranus** : `#77aac4` (Bleu-cyan)
- **Neptune** : `#3a80a4` (Bleu foncé)
- **Pluton** : `#989494` (Gris)

## Effets visuels

### Gradient sur les planètes

```css
.planet {
  background-image: radial-gradient(
    circle at center 80%,
    rgb(255 255 255 / 0.4),
    rgb(255 255 255 / 0) 50%
  );
}
```

Crée un effet de brillance sur chaque planète.

### Halo du Soleil

```css
&.sun {
  box-shadow: 0 0 42px 0 color-mix(in oklch, transparent, var(--sun-color) 50%);
}
```

Produit une lueur rayonnante autour du soleil.

### Arrière-plan

```css
background: linear-gradient(to bottom, #112, #223);
```

Un dégradé du bleu noir au bleu foncé pour un effet d'espace.

## Optimisation de l'affichage

Pour que le système solaire soit complètement visible à l'écran :

```css
.layout {
  min-height: 100dvh;
  display: grid;
  align-items: flex-start;
  justify-items: center;
  padding-top: 5dvh;      /* Espace depuis le haut */
}

.solar-system {
  padding: 1rem 0 0 0;
  overflow: visible;      /* Permet à tout d'être visible */
  gap: calc(var(--sun-diameter) * 0.05);  /* Espacement réduit */
}
```

## Personnalisation

Pour modifier l'affichage :

1. **Agrandir/réduire** : Changer `--sun-diameter`
2. **Accélérer/ralentir** : Modifier `--earth-duration`
3. **Changer la position** : Ajuster `padding-top` dans `.layout`
4. **Modifier les couleurs** : Éditer les `background-color` des classes `.planet`
5. **Changer les tailles relatives** : Modifier `--mercury-size`, `--jupiter-size`, etc.

## Performance

- **Animation fluide** : Utilise la propriété CSS `rotate` et `transform-origin` pour des performances optimales
- **Pas de JavaScript** : Entièrement CSS, donc très léger
- **Responsive** : Utilise `dvh` pour l'adaptabilité mobile et desktop

## Navigateurs compatibles

- Chrome/Edge 90+
- Firefox 88+
- Safari 15+
- Supporte les animations CSS modernes et Grid subgrid

## Résumé technique

| Aspect | Technique |
|--------|-----------|
| Layout | CSS Grid avec subgrid |
| Animation | CSS Keyframes + rotate |
| Responsive | dvh units |
| Palettes | CSS variables (custom properties) |
| Périodes | Proportionnelles à la réalité |
| Dégradés | Radial gradients pour la profondeur |
| Effets | Box-shadow et gradients |
