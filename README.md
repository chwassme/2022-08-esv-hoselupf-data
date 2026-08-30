# ESV "Hoselupf" Data Repository

## Einführende Anmerkungen

Dieses Repo enthält die gescrapten und geparsten Daten von Bergkranz-, Teilverband- und Kantonal-Schwingfeste aus den Jahren 2015 bis und mit Juli 2022, die auf ESV.ch veröffentlicht wurden (Stand: Juli 22) und von Bajour und SRF geparst, verarbeitet und ergänzt wurden. Falls gewünscht, kann die TrueSkill Berechnung nachgereicht werden (dazu bitte ein issue erfassen).

Diese Daten wurden im Rahmen einer Datenanalyse in Zusammenarbeit mit SRF Data, SRF Impact und bajour.ch für eine Vorberichterstattung vor dem ESAF 2022 aufbereitet.


- [SRF 10vor10 | Daten zeigen: Einheimische Schwinger werden bevorzugt](https://www.srf.ch/play/tv/redirect/detail/d3fcb608-384f-4dbc-a055-b3924bf54712)
- [SRF News | Wenns um den Muni geht, haben es die Gäste schwerer](https://www.srf.ch/news/schweiz/datenanalyse-zum-schwingsport-wenns-um-den-muni-geht-haben-es-die-gaeste-schwerer)
- [SRF Impact | Nationalsport Schwingen – Daten belegen unfaire Einteilung an Schwingfesten](https://youtu.be/vbDpDDqHBv0)
- [bajour.ch | Wenns um den Muni geht, wird im Schwingen «gemischelt»](https://bajour.ch/a/SkL5VZZlqlzutHIR/datenanalyse-schwingen)

## Haftungsausschluss / Disclaimer

Die veröffentlichten Informationen wurden sorgfältig zusammengestellt, erheben jedoch keinen Anspruch auf Aktualität, Vollständigkeit oder Richtigkeit. Sie stammen ursprünglich aus PDFs, die auf der Webseite ESV.ch veröffentlicht wurden. Für Schäden, die durch die Verwendung dieses Skripts oder der daraus gezogenen Informationen entstehen, wird keine Haftung übernommen. Dies gilt auch für Inhalte Dritter, die über dieses Angebot zugänglich sind.

## Kontakt

Bei Fragen oder Unklarheiten bitte an [Christian Wassmer](https://github.com/chwassme), [Julian Schmidli](https://github.com/jlsmdl), [Lukas Frischknecht](https://github.com/lukasfrischknecht) oder [Samuel Hufschmid](https://github.com/shufschmid) wenden.

## Daten

### Allgemeine Felder

- `Schwingerlevel`: Kranzlevel des eingeteilten Schwingers * Kantonal- bzw. Gäuverbandskranzschwinger ** Teilverbandskranzschwinger *** Eidg. Kranzschwinger

### Rohdaten

- Die PDFs wurden mittels eines R-Scripts von https://esv.ch/ranglisten/ runtergeladen:
  - `data/raw/statistk`: Enthält jeden Kampf aller Schwinger und aller Gänge
  - `data/raw/rangliste/pdf`: Schlussrangliste im PDF-Format
  - `data/raw/rangliste/txt`: per R-Script extrahierter Text aus dem Ranglisten-PDF 

### Verarbeitete Daten

Aus den Statistik-PDFs wurde je eine **Kämpfe**- und eine **Schlussranglisten**-Datei extrahiert mittels Python-Script (thanks to [John Templon](https://github.com/jtemplon/)). 

Aus diesen Dateien wurde mittels eines Java-Programms die **Zwischenrangliste**, angereicherte **Notenblätter** sowie Daten für die weitere Analyse (`fight_data.csv`) extrahiert. 

**Kämpfe**

`data/processed/bouts/`, z.B. `3341-2019-06-10_Stoos-Schwinget-ST_results_bouts.csv`
- `result`: Result des Kampfes aus Sicht `placed_fighter`, `+` (gewonnen) `-` (gestellt/unentschieden) `o` (verloren)
- `name`: Gegnerischer Schwinger
- `level`: Aktueller Schwingerlevel des Gegners
- `points`: Punkte aus Sichter `placed_fighter`
- `placed_fighter`: Gesetzter Schwinger
- `fight_round`: Gang

**Schlussrangliste**

`data/processed/results`, z.B. `3002-2018-08-12_Bernisch-Kantonales_Schwingfest-ST_results_places.csv`
- `place`: Schlussrang
- `name`: Schwinger
- `points`: Anzahl Punkte
- `level`: Aktueller Schwingerlevel
- Enthält nicht korrekt geparste Zeilen, z.B. "Berni,-Kantonaler Schwingerverband,,Schwingerverband"

**Zwischenrangliste**

`data/processed/rankings`, z.B. `1500-ranking.txt`, enthält die Rangliste nach jedem Gang
- `Gang`
- `Rang`
- `Schwinger`
- `Level`
- `Punkte`

**Notenblätter**

Mit Zusatzinformationen angereicherte Notenblätter: `data/processed/scoresheets`, z.B. `1500-scoresheets.csv`. 

Pro Schwinger ein Notenblatt mit folgenden Zusatzinformationen [TODO] 

**fight_data.csv**

Daten von über 40k Kämpfen, jeweils aus Sicht des Schwingers und des Gegners mit 46 Attributen:

- `festId`: Eindeutige Identifikation des Schwingfests, Anlass-Parameter auf ESV.ch (https://esv.ch/ranglisten/?anlass=5327)
- `schwingfestName`: Schwingfeste-Name, extrahiert aus der PDF-Datei, z.B. 5354-2022-05-15_97_Schwyzer_Kantonales_Schwing-_und_Aelplerfest-ST.pdf, ohne Datum und festId
- `jahr`: Jahr extrahiert aus Datum
- `datum`: Datum, extrahiert aus dem Dateinamen
- `organisierenderVerband`: Organisierender Verband, bzw. Verbandsmitglied mit Stichentscheid in Einteilungsjury
- `festType`: Festtyp: K - Kantonales Schwingfest, T: Teilverbandsschwingfest, B: Bergkranzschwingfest
- `fighterName`: Eingeteilter Schwinger
- `level`: Kranzlevel des eingeteilten Schwingers * Kantonal- bzw. Gäuverbandskranzschwinger ** Teilverbandskranzschwinger *** Eidg. Kranzschwinger
- `fighterVerbandId`: Verband des eingeteilten Schwingers
- `finalRank`: Schlussrang des eingeteilten Schwingers am Ende des Schwingfests
- `finalPoints`: Punktetotal des eingeteilten Schwingers am Ende des Schwingfests
- `nextRoundFightResult`: Ergebnis (+ - o) des eingeteilten Schwingers aus seiner Sicht gegen den 'oppNameNextRound' in Gang `round + 1`
- `nextRoundFightPoints`: Punkte des eingeteilten Schwingers aus seiner Sicht gegen den 'oppNameNextRound' in Gang `round + 1`
- `curRank`: aktueller Zwischenrang nach Gang 'round'
- `curPoints`: aktuelle Punktezahl nach Gang 'round'
- `round`: Referenzrunde, die Rangierungen und Punkte beziehen sich auf diese Anzahl geschwungene Gänge pro Schwinger
- `oppNameNextRound`: Gegnerischer Schwinger des eingeteilten Schwingers ('fighterName') im nächsten Gang `round + 1`
- `oppLevel`: "Kranzlevel" des gegnerischen Schwingers
- `oppVerbandId`: Verband des gegnerischen Schwingers
- `oppFinalRank`: Schlussrang des gegnerischen Schwingers am Ende des Schwingfests
- `oppFinalPoints`: Punkte des gegnerischen Schwingers am Ende des Schwingfests
- `oppNextRoundFightResult`: Ergebnis (+ - o) des gegnerischen Schwingers aus seiner Sicht gegen 'fighterName'
- `oppNextRoundFightPoints`: Punkte des gegnerischen Schwingers aus seiner Sicht gegen 'fighterName'
- `oppCurRank`: Schlussrang des gegnerischen Schwingers am Ende des Schwingfests
- `oppCurPoints`: Punktetotal des gegnerischen Schwingers am Ende des Schwingfests
- `curRankDiff`: Differenz des Rangs (ohne Unterränge) nach Gang 'round' zwischen Gegner und Schwinger (oppNameNextRound MINUS fighterName): negative Zahl bedeutet Schwinger ('fighterName') hat einen stärkeren Gegner erhalten
- `curPointsDiff`: Differenz der Punkte nach Gang 'round' zwischen Gegner und Schwinger (oppNameNextRound MINUS fighterName): positive Zahl bedeutet Schwinger ('fighterName') hat einen stärkeren Gegner erhalten
- `kampfTyp`: II: Duell zweier Einheimischer Kämpfer, EI: Kampf eines Gastes gegen einen Einheimischen, IE: Einheimischer gegen einen Gast, EG: Gastschwinger vom gleichen Verband, EV: Gastschwinger untersch. Verbände
- `kranzQuoteAlle`: Anteil der Schwinger anhand der Schlussrangliste (RL-Datei), die einen Kranz erhalten haben, auch ausgeschiedene Schwinger berücksichtigt
- `kranzQuoteOhneUnfaelle`: Anteil der Schwinger anhand der Schlussrangliste (RL-Datei), die einen Kranz erhalten haben, ohne wegen Unfall ausgeschiedene Schwinger
- `kranzLimit`: Anzahl Punkte, die am Fest für einen Kranz reichen
- `kranz`: K für Schwinger hat Kranz geholt
- `schlussgangKandidat`: S für Schwinger war nach Runde 5 (aka vor der letzten Runde) auf Rang 2 oder besser klassiert
- `curRankGroup`: TOP vs OTHERS: TOP hat Kranz geholt, OTHERS sind alle anderen (keine Berücksichtigung des Jahresrankings)
- `oppKranz`: K für Gegner hat Kranz geholt
- `oppCurRankingGroup`: TOP vs OTHERS aus Sicht des Gegners
- `minPointsForKranz`: Anzahl Punkte die nach der aktuellen Runde auf einem Kranz-Platz nötig waren
- `pointsBehindKranz`: Anzahl Punkte hinter der Kranz-Limite aus Sicht des Schwingers
- `oppPointsBehindKranz`: Anzahl Punkte hinter der Kranz-Limite aus Sicht des Gegners
- `firstRankPoints`: Anzahl Punkte des aktuell führenden Schwingers
- `secondRankPoints`: Anzahl Punkte des aktuell zweitplatzierten Schwingers
- `pointsBehindLeading`: Punkte hinter dem Führenden (positive Zahl: Rückstand, negative: Vorsprung)
- `opponentsPointsBehindLeading`: gegnerischer Schwinger Punkte hinter dem Führenden (positive Zahl: Rückstand, negative: Vorsprung)
- `pointsBehindSecond`: Punkte hinter dem Zweitplatzierten (positive Zahl: Rückstand, negative: Vorsprung)
- `opponentsPointsBehindSecond`: gegnerischer Schwinger Punkte hinter dem Zweitplatzierten (positive Zahl: Rückstand, negative: Vorsprung)
- `sameCurRankingGroup`: 1 für gleiche Ranking-Group (TOP gegen TOP, OTHERS gegen OTHERS), 0 für alle anderen Kämpfe


### Zusatzdaten

- [TODO]: weitere Dateien aus `./esv/esv-results/data`

## Probleme

Da die Daten in nicht-maschinenlesbarer Form vorlagen (insb. vor 2015) und es keine eindeutige Kennzeichnung von Schwingern gibt (keine IDs; Schwinger haben z.T. gleichen Vor- und Nachnamen, u.a. auch am gleichen Schwingfest) entstanden diverse Probleme bei der Datenverarbeitung. Diese Probleme werden hier nicht abschliessend beschrieben.

### Parser-Fehler: solved / ignored 

- Schwingfeste ignoriert, generell sind die Daten vor 2015 in einem anderen Format, deshalb keine bouts/places-Dateien
- 1258-2011-08-21_Schwaegalpschwinget_ST: `ä`h Bruno (NOS)`
- 1271-2012-06-24_Schwarzsee_ST: `imon (BE) K  8.7`

### Schwingfeste: Parser error

Folgende Feste konnten nicht exportiert werden:
- 3459-2016-05-22_95_Urner_Kant_Schwingfest-ST
- 3582-2019-05-30_Baselstaedtischer_Schwingertag-ST
- 4069-2018-05-21_Glarner-Buendner_Kantonalschwingfest-ST
- 4451-2019-05-05_Thurgauer_Kantonalschwingfest-ST

### Namens-Duplikate

- im gleichen Schwingfest gibts nicht unterscheidbare Schwinger, Total 41 betroffene Fälle, wobei circa 2/3 unterscheidbar sind anhand Kranz-Level

```
3002;Röthlisberger Simon; **
3261;Heinzer Stefan; * *
3261;Zimmermann Martin; * **
3378;Koller Martin; * *
3430;Heinzer Stefan; * *
3430;Zimmermann Martin; * **
3448;Odermatt Andreas; *
3578;Heinzer Stefan; * *
3715;Heinzer Stefan; ** *
3715;Schuler Alex; ***
3715;Stucki Christian; *** *
3754;Heinzer Stefan; * *
3754;Schuler Alex; ***
3789;Bucher Thomas;
3905;Gwerder Andreas; ** *
4045;Heinzer Stefan; ** *
4068;Heinzer Stefan; ** *
4099;Schuler Alex; ***
4114;Fankhauser Reto; ** *
4114;Thalmann Adrian;
4126;Heinzer Stefan; ** *
4126;Schuler Alex;  ***
4161;Heinzer Stefan; ** *
4187;Fankhauser Reto; ** *
4345;Rohrer Marco; **
4347;Schuler Alex; ***
4345;Rohrer Marco;**
4405;Gisler Silvan ;2002),
4405;Rohrer Marco; **
4405;Schuler Alex; ***
4437;Gisler Silvan ;2002),  
4437;Heinzer Stefan; ** *
4437;Schuler Alex; ***
4718;Schmid Dominik; **
5141;Koller Silvan; *
5266;Schmid Dominik; **
5354;Schuler Alex; *** *
5506;Bucher Thomas; *
5506;Schuler Alex; *** *
5506;Thalmann Adrian;
5575;Gisler Silvan;
5593;Gisler Silvan;
```

und weitere, die mit Nummern gehandelt werden, aber dem falschen zugewiesen werden:

```
3381,Gasser Dominik 2,Lungern
3381,Ming Christian 2,Lungern
3381,Gasser Dominik 1,Lungern
3459,Ming Christian 2,Lungern
3459,Gasser Dominik 2,Lungern
3459,Gasser Dominik 1,Lungern
3489,Hermann Lukas 1,Oberwil BL
3580,Hermann Lukas 1,Oberwil BL
3582,Hermann Lukas 1,Oberwil BL
3677,Gasser Dominik 2,Lungern
3677,Ming Christian 2,Lungern
3685,Hermann Lukas 1,Oberwil BL
3754,Ming Christian 2,Lungern
3754,Gasser Dominik 2,Lungern
3771,Hermann Lukas 1,Oberwil BL
3860,Ming Christian 2,Lungern
4045,Gasser Dominik 1,Lungern
4045,Gasser Dominik 2,Lungern
4045,Herger Simon 1,Schattdorf
4068,Gasser Dominik 2,Lungern
4094,Gasser Dominik 2,Lungern
4099,Gasser Dominik 1,Lungern
4099,Gasser Dominik 2,Lungern
4100,Hermann Lukas 1,Oberwil BL
4126,Gasser Dominik 1,Lungern
4161,Gasser Dominik 2,Lungern
4345,Gasser Dominik 1,Lungern
4345,Gasser Dominik 2,Lungern
4347,Gasser Dominik 1,Lungern
4437,Gasser Dominik 1,Lungern
4449,Gasser Dominik 1,Lungern
4449,Gasser Dominik 2,Lungern
4655,Hermann Lukas 1,Oberwil BL
4984,Gasser Dominik 1,Lungern
4984,Gasser Dominik 2,Lungern
4985,Gasser Dominik 1,Lungern
5325,Gasser Dominik 1,Lungern
5325,Burch Jonas 2,Sarnen
5354,Gasser Dominik 1,Lungern
5354,Betschart Fabian 2,Muotathal
5509,Gasser Dominik 1,Lungern
5547,Gasser Dominik 1,Lungern
5593,Burch Jonas 2,Sarnen
5605,Gasser Dominik 1,Lungern
```

### Zeilenumbruch bei Von Niederhäusern Marcel (2001): noch nicht behoben
- PDFs Von Hand korrigiert mittels "Wondershare PDFElement" (Gratisversion), Original weg kopiert davor
  - 3052-2017-07-16_Fete_Romande2-ST: java.lang.IllegalStateException: No rank for Von Niederhäusern Marcel (2001) in round 5
  - 3072-2019-07-14_Fete_Romande2-ST: java.lang.IllegalStateException: No rank for Von Niederhäusern Marcel (2001) in round 4
  - 3071-2018-07-08_Fete_Romande2-ST: java.lang.IllegalStateException: No rank for Von Niederhäusern Marcel (2001) in round 4
- noch offen sind diese: 3616, 3892, 4034, 4470, 4464

### No fighter level
- 5494-2021-07-25_Thurgauer_Kant_Schwingfest-ST: Exception for Lenherr Jonas (2005): java.lang.IllegalStateException: No rank for Bernhardsgrütter Stefan (2005) in round 5

### RL aber kein ST File
- 4451: failed to export enriched score-sheets for 4451-2019-05-05_Thurgauer_Kantonalschwingfest-RL
- 4069: failed to export enriched score-sheets for 4069-2018-05-21_Glarner-Buendner_Kantonalschwingfest-RL
- 3582: failed to export enriched score-sheets for 3582-2019-05-30_Baselstaedtischer_Schwingertag-RL
- 3459: failed to export enriched score-sheets for 3459-2016-05-22_95_Urner_Kant_Schwingfest-RL
