# GEOGUESS
Einfache Geoguessing Spiel mit Google Street View

*Google API Key wird benötigt*

[https://mksdc.github.io/geoguess](https://mksdc.github.io/geoguess/)


## Spielablauf
So läuft das Spiel ab: Du landest an einem zufälligen Ort in Street View (ohne Adresse und Strassennamen) und kannst dich umschauen und herumlaufen. Mit „Zur Karte“ wechselst du auf die Weltkarte, klickst deinen Tipp an und bestätigst. Danach erscheinen der richtige Ort (grün), dein Tipp (gelb) und eine gestrichelte rote Linie dazwischen, zusammen mit der Entfernung in km und den Punkten. Es gibt 5 Runden mit maximal 5000 Punkten pro Runde, ähnlich wie bei GeoGuessr.

## API
Was du brauchst: einen Google Maps API-Schlüssel. Den bekommst du in der Google Cloud Console. Dort legst du ein Projekt an, aktivierst die „Maps JavaScript API“ und richtest Billing ein. Das ist Pflicht, aber Google gewährt ein monatliches Gratiskontingent, das für privates Spielen normalerweise reicht. 

## Versionen

### geoguess.html
Den Schlüssel gibst du beim Start im Spiel ein, er wird dann lokal im Browser gespeichert.

### geoguess_api.html
Den Schlüssel trägst du in der HTML-Datei ein. Die Datei kannst Du lokal hosten, aber niemals im Internet. Ein öffentlicher API key verursacht Kosten.
```
161 // >>> HIER DEINEN GOOGLE MAPS API-SCHLÜSSEL EINTRAGEN <<<
162 const API_KEY = "....";
```

## Funktionsweise
Wie der zufällige Ort gefunden wird: Vor jedem Spiel (auf dem Start- und dem Endbildschirm) wählst du, wo du landen willst:

| Spielart | Was passiert |
|---|---|
| Zufall | zufälliger Punkt in grossen Regionen mit guter Street-View-Abdeckung (Liste REGIONS), nächstes Panorama im Umkreis von 50 km. Führt oft auf Landstrassen in der Natur. |
| Städte (Standard) | zufällige Stadt aus der Liste CITIES (rund 130 Städte weltweit), Punkt bis 8 km vom Zentrum, nächstes Panorama im Umkreis von 1 km. Innenstädte, Vororte, Dörfer am Stadtrand. |
| Eher Städte | etwa 70 % der Runden wie „Städte“, 30 % wie „Zufall“ |
| Städte und Umgebung | wie „Städte“, aber bis 40 km vom Zentrum entfernt |

Die Suche nach dem Panorama läuft über StreetViewService.getPanorama(), nur offizielle Google-Aufnahmen. Die gewählte Spielart wird lokal im Browser gespeichert.

Die Spielarten stehen in der Liste MODES oben im Skript (share = Anteil der Stadt-Runden, spread = max. km vom Zentrum). Die Listen CITIES und REGIONS kannst du ebenfalls anpassen, zum Beispiel nur Schweizer Städte.

## Screenshots

### Anfang
![Anfang](screenshots/geoguess1.png)

### Wo bin ich?
![Wo bin ich?](screenshots/geoguess2.png)

### Tip abgeben
![Tip abgeben](screenshots/geoguess3.png)

### Auflösung
![Auflösung](screenshots/geoguess4.png)



## Google API Key

Die Google Cloud Console findest du unter https://console.cloud.google.com. Du meldest dich dort mit deinem normalen Google-Konto an.

Für das Spiel gehst du dann so vor:

Projekt anlegen: Oben links neben dem Google-Cloud-Logo auf die Projektauswahl klicken, dann auf „Neues Projekt“. Du kannst es zum Beispiel „Geoguess“ nennen.
Billing einrichten: Im Menü (☰) auf „Abrechnung“ gehen und ein Rechnungskonto mit Kreditkarte verknüpfen. Ohne das funktioniert die Maps API nicht, auch wenn du im Gratiskontingent bleibst.
API aktivieren: Im Menü „APIs & Dienste“ → „Bibliothek“ öffnen, nach „Maps JavaScript API“ suchen und auf „Aktivieren“ klicken.
Schlüssel erstellen: Unter „APIs & Dienste“ → „Anmeldedaten“ auf „Anmeldedaten erstellen“ → „API-Schlüssel“ klicken. Der Schlüssel beginnt mit AIza…. Diesen kopierst du ins Spiel.

Zur Sicherheit kannst du den Schlüssel danach einschränken. Klick ihn in der Liste an und wähle unter „API-Einschränkungen“ nur die Maps JavaScript API aus. Die Einschränkung auf Websites lässt du am besten weg, solange du die Datei lokal von deinem Computer aus öffnest. Sonst lehnt Google den Schlüssel ab.

Die Oberfläche ändert sich ab und zu ein wenig, die Menüpunkte können also leicht anders heissen. Die Grundstruktur bleibt aber gleich.