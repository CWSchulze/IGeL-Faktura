# Änderungshistorie

Alle nennenswerten Änderungen an IGeL-Faktura. Versionierung nach [SemVer](https://semver.org/lang/de/).

## v1.3.0 – 2026-07-10

### Neu
- **GOÄ-Katalog:** das amtliche Gebührenverzeichnis nach Ziffer/Stichwort durchsuchen und ausgewählte Positionen mit einstellbarem **Steigerungsfaktor** direkt als eigene Leistungen übernehmen.
- **Konto-Abgleich:** eine Kontoumsatz-CSV einlesen – Zahlungseingänge werden automatisch offenen Rechnungen zugeordnet (über Rechnungsnummer und Betrag) und lassen sich nach Kontrolle **sammelweise als bezahlt** buchen.
- **Verfahrensdokumentation (GoBD)** direkt im Programm (Hilfe → „Verfahrensdokumentation öffnen"): druck-/PDF-fähig, mit Praxisdaten, Programmversion und Speicherorten vorausgefüllt – für Steuerberater / Betriebsprüfung.
- **Steuernummer / USt-IdNr** als Praxisfeld, das auf der Rechnung gedruckt wird (§14 Abs. 4 Nr. 2 UStG); optionaler **§19-Kleinunternehmer-Hinweis**.

### Rechnung / Umsatzsteuer
- **Mischrechnungen** (steuerfreie + steuerpflichtige Leistungen) werden getrennt und korrekt ausgewiesen; ergänzter Leistungsdatum-Hinweis.
- **Nachdruck** verwendet den zum Ausstellungszeitpunkt gültigen MwSt-Satz (nicht den aktuellen).

### Zahlungserinnerungen
- Mahnungs-**GDT mit festem, einstellbarem Dateinamen** für den T2med-Import (ohne Patienten-/Rechnungsnummer); besseres Karteieintrag-Format (offener Betrag, Frist im deutschen Datumsformat).
- Kein doppeltes Mahnen am selben Tag; datierte PDF-Archivkopie (überschreibt eine frühere Mahnung nicht mehr).
- Im **Client/Server-Betrieb** werden die Erinnerungen am **auslösenden Arbeitsplatz** in dessen T2med eingespielt (wie die Rechnungs-Rückgabe); zentral als „gemahnt" gebucht wird erst, wenn der Arbeitsplatz die tatsächliche lokale Zustellung **bestätigt** hat (kein „gemahnt" ohne Zustellung, auch bei Abbruch/Absturz).

### Stabilität, Sicherheit & GoBD
- Rechnungsbuchung **atomar** – ein Absturz kann keine unvollständige Rechnung mehr hinterlassen; erweiterte Unveränderbarkeit (auch MwSt-Satz, Anschrift, Diagnosen).
- **Downgrade-Schutz:** eine mit neuerer Programmversion geschriebene Datenbank wird nicht mehr von einer älteren geöffnet.
- Im Client/Server-Betrieb wird im LAN ein **Server-Schlüssel erzwungen** (Schutz der Patientendaten); Programm beenden und lokale Server-Einstellungen nur noch am Server-PC.
- Druck meldet **echten Erfolg/Fehler** (falscher/offline Drucker wird erkannt); Einstellungen werden ausfallsicher gespeichert; klare Meldung bei Startproblemen; CSV-Export gegen Excel-Formel-Injektion abgesichert.

## v1.2.1 – 2026-07-10
- **Client/Server:** die lokale PDF-Rechnung wird jetzt mit der **Rechnungsnummer** im Dateinamen gespeichert (`Igelrechnung_<Nr>.pdf`).
- **Rechnungsübersicht:** überfällige offene Rechnungen werden orange als **„fällig"** markiert.
- **„bezahlt" ohne Seiten-Neuaufbau** buchen – die Liste bleibt an Ort und Stelle, so lassen sich mehrere Rechnungen zügig hintereinander abhaken.
- **Zahlungserinnerungen:** eigener Ordner für die Mahnungs-GDT (getrennt von den PDF-Kopien).
- **Client/Server-Versionsabgleich:** unterschiedliche Datenbank-Versionen von Client und Server werden beim Start erkannt und gemeldet.
- **Firewall** wird beim ersten Server-Start automatisch per Windows-Nachfrage freigegeben – die frühere `firewall-freigeben.bat` entfällt.

## v1.2.0 – 2026-07-01
- **Zwei klare Betriebsmodi:** Einzelplatz (Standalone) und Client/Server. Riskante Zugriffe über Netzfreigaben (Datenbank oder Programm auf einem Netzlaufwerk) werden unterbunden.
- **Betriebsmodus** ist während der Nutzung dauerhaft oben rechts sichtbar.
- **Druckerauswahl** robuster: exakter Druckername, scrollbare Liste, Klartext im Log.
- **Beträge** brechen nicht mehr um und laufen nicht über den Text (auch Cent- und große Beträge).
- **Handbuch** erweitert: Diagramm der Betriebsarten sowie ein Abschnitt zu GoBD & Umsatzsteuer (u. a. gemischte MwSt-Rechnungen nach §14 UStG).

## v1.1.1 – 2026-07-01
- Statusanzeige im Einzelplatzbetrieb neutral/statisch („Einzelplatz") statt eines Verbindungs-Symbols.

## v1.1.0 – 2026-07-01
- **Vektor-Briefkopf:** eigener Briefkopf wird als Vektorgrafik in Originalgröße auf die Rechnung gelegt.
- **Rechnungsnummern:** Zähler und angezeigter Nummerntext entkoppelt; das Suffix passt zur tatsächlichen Zahlart (z. B. „EC", „Ü", „*").
- **GDT-Karteieintrag** kompakter (alle Leistungen + Summe in einem Feld).
- PDF-Dateiname enthält die Rechnungsnummer; Empfängeradresse tiefer platziert (für Fensterumschlag).
- Beim Erstellen wird bei fehlender MwSt-Wahl **nachgefragt** (kein Datenverlust); neuer Filter „nur Ausgewählte".

## v1.0.1 – 2026-06-26
- Korrekturen: Tray-Status, Leistungs-Bearbeitung (Position/Filter bleiben erhalten), breiter Briefkopf, „Erstellen"-Knopf oben.

## v1.0.0 – 2026-06-26
- Erste Produktivversion: IGeL-/Privatrechnungen aus T2med (GDT), zentrales Leistungsmenü, GoBD-konforme Rechnungsliste, PDF mit GiroCode, Zahlungserinnerungen, Einzel-/Mehrplatzbetrieb, automatische Datensicherung.
