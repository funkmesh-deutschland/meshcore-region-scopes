# MeshCore Region-Scopes für Deutschland

Dieses Repository enthält die MeshCore-Regionen (Region-Scopes) für Deutschland als eine offene Datei: `regions.geojson`. Jede Region hat einen Scope, einen Namen und ein Gebiet als Polygon.

Die Karte auf [funkmesh.de/wissen/region-scope](https://funkmesh.de/wissen/region-scope) liest diese Datei direkt aus dem Branch `main`. Wer dort seinen Standort setzt, sieht alle Regionen, die an diesem Punkt gelten. Eine Änderung hier ist nach etwa zehn Minuten auf der Karte sichtbar.

Die Datei gehört der Community. Jede Community kann ihre Regionen selbst eintragen und anpassen.

## Aufbau der Datei

`regions.geojson` ist eine GeoJSON-Datei (FeatureCollection). GitHub zeigt sie beim Öffnen als Karte an.

Jede Region steht in genau einer Zeile. Das hält Änderungen übersichtlich: Wer eine Region ändert, ändert eine Zeile.

```json
{
"type": "FeatureCollection",
"features": [
{"type":"Feature","properties":{"scope":"de-msl","name":"Münsterland","ebene":"region","description":"Münsterland: Stadt Münster und die Kreise Borken, Coesfeld, Steinfurt und Warendorf.","url":"https://mcml.info"},"geometry":{"type":"Polygon","coordinates":[[[7.1,51.9],[7.3,51.9],[7.3,52.0],[7.1,51.9]]]}},
{"type":"Feature","properties":{"scope":"local","name":"Lokal","ebene":"sonder","description":"Nur für Tests und direkte Nachbarn."},"geometry":null}
]
}
```

### Angaben zu einer Region

| Feld | Pflicht | Bedeutung |
| --- | --- | --- |
| `scope` | ja | Der Name der Region, so wie er im Repeater und im Companion eingetragen wird. Nur Kleinbuchstaben, Ziffern und Bindestriche, höchstens 30 Zeichen. Jeder Scope darf nur einmal vorkommen. |
| `name` | ja | Lesbarer Name, zum Beispiel "Münsterland". |
| `ebene` | ja | Einordnung der Region, siehe Tabelle unten. |
| `description` | nein | Ein bis zwei Sätze: was die Region umfasst und wofür sie gedacht ist. |
| `url` | nein | Link zur Community oder zu einer Beschreibung der Region. |

### Ebenen

| Wert | Bedeutung | Beispiel |
| --- | --- | --- |
| `deutschland` | Ganz Deutschland | `de` |
| `zone` | Eine der fünf Basis-Zonen | `de-nord` |
| `bundesland` | Ein Bundesland nach ISO 3166-2 | `de-nw` |
| `region` | Region einer Community, eine Landschaft, ein Regierungsbezirk oder eine Metropolregion | `de-msl`, `rhein-main` |
| `kreis` | Ein Landkreis oder eine kreisfreie Stadt | `de-bw-em` |
| `ort` | Eine Stadt oder Gemeinde | `koeln` |
| `sonder` | Scope ohne festes Gebiet | `local`, `europe` |

Die Karte sortiert die Treffer nach Ebene und zeigt die kleinste Region zuerst.

### Gebiet

Das Gebiet steht in `geometry`:

* `Polygon` für eine zusammenhängende Fläche.
* `MultiPolygon` für mehrere Flächen, zum Beispiel Festland und Inseln.
* `null`, wenn die Region kein festes Gebiet hat.

Regeln für die Koordinaten:

* Reihenfolge ist immer Länge, dann Breite, also `[7.1234, 51.9876]`. Das ist die häufigste Fehlerquelle.
* Vier bis fünf Nachkommastellen reichen aus.
* Der letzte Punkt eines Rings ist gleich dem ersten.
* Regionen dürfen sich überlappen. Das ist gewollt, an einem Standort gelten oft mehrere Regionen.
* Grenzen müssen nicht metergenau sein. Ein paar hundert Meter Abweichung spielen für Funk keine Rolle.

## Eine Region eintragen oder ändern

Es gibt drei Wege. Alle führen zur selben Datei.

### Weg 1: Regionen-Editor auf funkmesh.de

Der einfachste Weg für alle. Öffne [funkmesh.de/wissen/region-editor](https://funkmesh.de/wissen/region-editor):

1. Wähle eine bestehende Region aus oder klicke auf "Neue Region".
2. Zeichne die Fläche auf der Karte, verschiebe Punkte oder schneide Stücke aus. Fülle Scope, Name, Ebene und Beschreibung aus.
3. Klicke auf "Eintrag erzeugen". Der Editor zeigt die fertige Zeile für `regions.geojson` und sagt dir, an welcher Stelle sie in die Datei gehört.
4. Öffne hier `regions.geojson`, klicke auf den Stift, füge die Zeile ein oder ersetze die bisherige Zeile der Region und stelle einen Pull Request.

Der Editor speichert selbst nichts. Jeder Vorschlag läuft über einen Pull Request und wird vor der Übernahme geprüft.

Mitglieder von Funkmesh Deutschland mit der Rolle Wissensredaktion haben denselben Editor im Mitgliederbereich. Dort lassen sich Änderungen direkt speichern und eingegangene Vorschläge auf der Karte mit dem bisherigen Stand vergleichen.

### Weg 2: Mit geojson.io zeichnen und hier einreichen

1. Öffne [geojson.io](https://geojson.io).
2. Zeichne die Fläche mit dem Polygon-Werkzeug. Für einen ganzen Landkreis oder eine Gemeinde kannst du die fertige Fläche bei [opendatalab.de](https://opendatalab.de/projects/geojson-utilities/) herunterladen.
3. Kopiere aus dem JSON rechts den Teil `"geometry": { ... }` deiner Fläche.
4. Öffne hier `regions.geojson`, klicke auf den Stift und füge eine neue Zeile nach folgendem Muster ein:

   ```json
   {"type":"Feature","properties":{"scope":"mein-scope","name":"Meine Region","ebene":"region","description":"Kurze Beschreibung."},"geometry":{"type":"Polygon","coordinates":[[[7.1,51.9],[7.3,51.9],[7.3,52.0],[7.1,51.9]]]}},
   ```

5. Achte auf das Komma am Zeilenende. Jede Zeile endet mit einem Komma, nur die letzte Region vor der schließenden Klammer `]` nicht.
6. Speichere mit "Commit changes" und stelle einen Pull Request.

### Weg 3: Nur Angaben ändern

Für einen neuen Namen, eine bessere Beschreibung oder einen Link reicht der Stift auf GitHub: Zeile der Region suchen (Strg+F und den Scope eingeben), Text ändern, Pull Request stellen.

## Vor dem Einreichen prüfen

* Der Scope kommt in der Datei nur einmal vor.
* Der Scope besteht nur aus Kleinbuchstaben, Ziffern und Bindestrichen.
* Die Zeile steht zwischen den anderen Regionen, also zwischen `"features": [` und `]`.
* GitHub zeigt die Datei nach der Änderung weiterhin als Karte an. Erscheint stattdessen eine Fehlermeldung, ist die Datei kein gültiges JSON mehr. Meist fehlt ein Komma oder eine Klammer.
* Die Fläche liegt dort, wo sie liegen soll. Liegt sie vor der Küste Somalias, sind Länge und Breite vertauscht.

Ist die Datei einmal fehlerhaft, zeigt die Karte auf funkmesh.de so lange den letzten gültigen Stand.

## Regeln für Scopes

Die Benennung folgt den Absprachen der deutschsprachigen MeshCore-Community:

* Bundesländer heißen nach ISO 3166-2, zum Beispiel `de-nw` oder `de-by`.
* Die fünf Zonen `de-nord`, `de-ost`, `de-sued`, `de-west` und `de-mitte` liegen in der Größe zwischen Deutschland und den Bundesländern und überlappen sich mit Absicht.
* Regionen der Communities sind frei benannt. Sie müssen nicht unter einem Bundesland hängen.
* `local` und `lokal` sind für Tests und direkte Nachbarn gedacht und dürfen auf keinem Repeater eingetragen sein.

Einzelheiten stehen im Wiki der Community:

* [Deutschlandweite Basis-Regions](https://meshcore-de.fyi/meshcore:allgemeines:regions:basis)
* [Regionale Regions der Communities](https://meshcore-de.fyi/meshcore:allgemeines:regions:reale-regions-in-repeatern)

## Stand der Daten

Der Startbestand umfasst 225 Scopes und wurde im November 2026 aus den beiden Wiki-Seiten oben übernommen.

Die Gebiete sind aus den amtlichen Verwaltungsgrenzen zusammengesetzt (Gemeinden, Kreise, Regierungsbezirke, Bundesländer) und vereinfacht. Bei Regionen ohne amtliche Grenze, zum Beispiel Landschaften oder die Einzugsgebiete einzelner Communities, ist das Gebiet angenähert. Solche Regionen tragen in der Beschreibung den Hinweis "Gebiet angenähert, bitte bei Bedarf anpassen." Die jeweilige Community weiß am besten, wo ihre Region endet, und ist eingeladen, das Gebiet zu korrigieren.

Jede Region ist möglichst eine zusammenhängende Fläche. Kleine Exklaven von Gemeinden und Kreisen, etwa Waldstücke im Gebiet der Nachbargemeinde, zählen zu der Region, in der sie liegen. Aus mehreren Flächen bestehen nur Regionen, die wirklich getrennt sind: Inseln, Bremen mit Bremerhaven oder Gebiete, die durch Wasser oder das Ausland vom Rest getrennt sind.

Ein paar Scopes haben bewusst kein Gebiet, weil es keine feste Abgrenzung gibt: `rheinland`, `fr`, `de-dk`, `dach`, `europe`, `local` und `lokal`.

## Quellen und Lizenz

* Verwaltungsgrenzen: © GeoBasis-DE / BKG 2026, Verwaltungsgebiete 1:250 000 (VG250), [Datenlizenz Deutschland Namensnennung 2.0](https://www.govdata.de/dl-de/by-2-0). Die Grenzen wurden vereinfacht und zu Regionen zusammengefasst.
* Scopes und ihre Bedeutung: Festlegungen der deutschsprachigen MeshCore-Community, gesammelt auf [meshcore-de.fyi](https://meshcore-de.fyi).
* Dieses Repository steht unter der MIT-Lizenz, siehe `LICENSE`. Bei Weitergabe der Gebiete bitte den Hinweis auf GeoBasis-DE / BKG beibehalten.

Das Repository wird von [Funkmesh Deutschland e.V.](https://funkmesh.de) bereitgestellt. Es steht in keiner Verbindung zum MeshCore-Projekt.
