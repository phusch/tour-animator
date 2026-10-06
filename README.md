# Tour Animator V0.1.3

Kleine GitHub-Pages-App für animierte Routenvideos aus dem **Tour Navigator**.

## Ziel

- Route mit einem Klick aus `https://phusch.github.io/tour-navigator-modern/` übernehmen
- automatisch eine Straßenlinie aus den Tour-Navigator-Wegpunkten erzeugen
- Route mit bewegtem Motorradmarker und automatischer Kamera animieren
- Ausgabe wahlweise 16:9 Full HD oder 9:16 Hochformat
- nur drei Eingaben: Format, Dauer, Kartenstil
- Video direkt im Browser erzeugen
- GPX-Datei als alternative Quelle

## Technische Idee

Beide Apps liegen unter `https://phusch.github.io/...` und haben damit denselben Web-Origin.  
Der Tour Navigator legt die aktuelle Punkteliste vor dem Öffnen des Animators in `localStorage` ab.  
Der Tour Animator liest die Daten anschließend direkt aus.

Verwendeter Schlüssel:

`tourNavigator.animationRoute`

Payload:

```json
{
  "title": "Route des Grandes Alpes",
  "source": "tour-navigator-modern",
  "geometryMode": "road",
  "exportedAt": "2026-10-06T08:00:00.000Z",
  "points": [
    {"name":"Start","lat":48.7,"lon":9.3,"type":"start"}
  ]
}
```

## GitHub Pages

Neues Repository anlegen, empfohlen:

`tour-animator`

Dann den **Inhalt dieses Ordners** in das Repository laden und unter  
**Settings → Pages → Deploy from a branch → main / root** veröffentlichen.

Die Zieladresse ist dann:

`https://phusch.github.io/tour-animator/`

## Browser

Für die reine Vorschau funktioniert die App in modernen Browsern.  
Für zuverlässige Videoaufzeichnung in Full HD ist **Chrome oder Edge am Mac/PC** die beste Wahl.

Die Aufnahme versucht zuerst MP4/H.264. Falls der Browser das nicht anbietet, wird WebM erzeugt.

## Karten/Routing

- Straße: OpenFreeMap / MapLibre GL JS
- Satellit: Esri World Imagery
- Straßen-Rekonstruktion: öffentlicher OSRM-Routingdienst

Wenn das Routing nicht erreichbar ist, verbindet die App die übergebenen Wegpunkte direkt.

## Tour Navigator anbinden

Siehe Datei `TOUR_NAVIGATOR_PATCH.md`.


## Änderung V0.1.3

- Kameradrehung und hektisches Nachführen deaktiviert
- statische nordorientierte Gesamtansicht während der gesamten Animation
- nur noch eine einzige wachsende Routenlinie
- Motorradposition und gezeichnete Route verwenden dieselbe Distanzinterpolation
- Video-Vorbereitung bewegt die Karte nicht mehr entlang der Route

Stops / Tagesetappen / Shaping Points sind bewusst noch nicht Bestandteil dieser Reparaturversion.


## V0.1.3

- neue Ansichtsmodi: Gesamtübersicht, Sanft folgen, Nah dran
- Zoom: Weit, Mittel, Nah
- Standard: Sanft folgen + Mittel
- Kamera bleibt immer nordorientiert und wird weich interpoliert
