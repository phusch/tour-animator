# Patch für `phusch/tour-navigator-modern`

Dieser Patch ist bewusst klein. Er verändert keine bestehende Navigation oder Routenberechnung.

## 1. Button einfügen

Im aktuellen `index.html` steht im Abschnitt `.actions-card` bereits:

```html
<button id="openKurvigerRouteBtn" class="primary-action" type="button">
  ...
</button>
```

Direkt **danach** diesen Button ergänzen:

```html
<button id="openTourAnimatorBtn" class="share-action" type="button">
  <span>
    <strong>🎬 Tour animieren</strong>
    <span>Aktuelle Route direkt an Tour Animator übergeben</span>
  </span>
  <b>›</b>
</button>
```

## 2. JavaScript ergänzen

Im vorhandenen `<script>`-Block, am besten kurz vor `updateSelectedNav();`, ergänzen:

```js
function openTourAnimator() {
  const validPoints = (Array.isArray(points) ? points : [])
    .filter(p => Number.isFinite(Number(p.lat)) && Number.isFinite(Number(p.lon)))
    .map(p => ({
      name: String(p.name || p.q || ""),
      type: String(p.type || ""),
      lat: Number(p.lat),
      lon: Number(p.lon)
    }));

  if (validPoints.length < 2) {
    alert("Für eine Animation brauche ich mindestens zwei gültige Koordinatenpunkte.");
    return;
  }

  const routeTitle =
    document.querySelector(".route-summary strong")?.textContent?.trim()
    || "Tour Navigator Route";

  localStorage.setItem("tourNavigator.animationRoute", JSON.stringify({
    title: routeTitle,
    source: "tour-navigator-modern",
    geometryMode: "road",
    exportedAt: new Date().toISOString(),
    points: validPoints
  }));

  window.location.href = "https://phusch.github.io/tour-animator/";
}

document.getElementById("openTourAnimatorBtn")
  ?.addEventListener("click", openTourAnimator);
```

## Ergebnis

Danach läuft die Übergabe so:

**Tour Navigator → 🎬 Tour animieren → Tour Animator öffnet mit derselben Punkteliste**

Es ist kein GPX-Download und kein erneutes Einlesen nötig.

## Wichtig

Solange das Animator-Repository noch nicht unter

`https://phusch.github.io/tour-animator/`

veröffentlicht ist, führt der Button natürlich noch ins Leere.
