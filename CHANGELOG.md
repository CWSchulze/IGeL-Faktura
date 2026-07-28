# Änderungshistorie

Alle nennenswerten Änderungen an IGeL-Faktura. Versionierung nach [SemVer](https://semver.org/lang/de/).

## v1.8.0 – 2026-07-28

### Sammelrechnung: Behandlungen über mehrere Termine vormerken (Stufe 2)
- **Laufende Behandlungen parken und später gemeinsam abrechnen.** Im Express kann jede Leistung
  eines T2med-Patienten **vorgemerkt** werden (Knopf „Vormerken") – z. B. jede Sitzung einer
  Impf- oder Plasmaserie am Behandlungstag. Preis und Datum werden dabei **eingefroren**.
- Der Bereich **„Laufende Behandlung"** zeigt die bereits erfassten Sitzungen des Patienten; ein
  Klick auf **„Jetzt abrechnen"** fasst sie zu **einer** Rechnung mit je Sitzung eigenem Datum
  zusammen (PDF + Archiv + GDT wie bei jeder Rechnung). Vorgemerkt ≠ gebucht: erst das Abrechnen
  erzeugt die revisionssichere Rechnung.
- Neuer Menüpunkt **„Vormerkungen"** (mit Zähler): Übersicht aller Patienten mit offenen Serien –
  damit nichts vergessen wird – plus ein **Editor**, in dem sich Datum und Preis je Sitzung bis
  zum Abrechnen korrigieren und einzelne Sitzungen entfernen lassen.
- Vormerken braucht die **T2med-Patientennummer** (eindeutiger Serien-Schlüssel).

### Leistungsdatum je Position (Sammelrechnung, Stufe 1)
- **Jede Rechnungsposition kann ein eigenes Leistungsdatum tragen** (Vorgabe = Rechnungsdatum).
  Damit lassen sich mehrterminige Behandlungen – Impf- oder Plasma-/PRP-Serien – korrekt als
  **eine** Rechnung mit **je Sitzung datierten** Positionen abbilden.
- Im Erfassungs- und Express-Formular steht je Leistung ein kleines Datumsfeld; weicht ein Datum
  vom Rechnungsdatum ab, weist das **PDF** je Position das Leistungsdatum aus (§12/§14-konform),
  sonst bleibt die gewohnte Kurzform.

### Storno vollständig in T2med rückgemeldet
- **Storno-Rechnungen sind jetzt druckbar.** Das PDF trägt einen deutlichen Hinweis
  „STORNIERT – diese Rechnung ist ungültig“, damit ein Ausdruck nie mit einer gültigen Rechnung
  verwechselt wird.
- **Storno schreibt eine GDT-Rückmeldung in die Patientenakte** – analog zur Rechnung, nur mit
  **negativen Eurobeträgen** und dem Vermerk „Storno zu Rechnung &lt;Nr&gt;“. So ist der Vorgang
  auch in T2med nachvollziehbar (fester Dateiname `Storno.gdt`, ohne PDF-Anhang). Im
  Client/Server-Betrieb wird die Rückmeldung am **auslösenden Arbeitsplatz** geschrieben – dort,
  wo dessen T2med liest.
- Mahnungs- und Storno-GDT teilen sich denselben **dauerüberwachten Rückmelde-Ordner**; T2med muss
  beide Dateinamen (`Mahnung.gdt`, `Storno.gdt`) in seinem Ordner-Import führen (siehe In-App-Hilfe,
  Abschnitt „T2med / GDT-Anbindung“).

### Robustheit / GDT
- **Wahlweise nummeriertes GDT-Dateischema** (Einstellungen → Arbeitsplatz → „GDT-Dateischema"):
  Neben den festen Dateinamen (Vorgabe) kann IGeL-Faktura die Austauschdateien nun auch mit
  **hochzählender Endung** (`…001…999`) schreiben **und lesen** – passend zu T2meds Standard-Im-/
  Export. Das umgeht das Überschreiben komplett. Vorgabe bleibt der feste Dateiname; nur umstellen,
  wenn T2med entsprechend konfiguriert ist.
- **GDT-Kennungen (Felder 8315/8316) sind jetzt zentral einstellbar** (Einstellungen → Praxis →
  „GDT-Kennungen für T2med“) – für Rechnung, Storno und Mahnung gemeinsam. Vorgabe bleiben die
  erprobten Werte `T2MEDIGEL`/`IGELT2MED`; ändern nur, wenn das T2med-Gerät andere IDs erwartet.
- **GDT-Dateien werden atomar und serialisiert geschrieben**, damit T2med nie eine halb
  geschriebene Datei liest. Holt T2med eine Datei nicht ab, weist ein Hinweis darauf hin, statt
  sie stillschweigend zu überschreiben.
- **Zeichensatz-Code 3** (GDT-Feld 9206) wird als **CP1252/„ANSI“** interpretiert (Windows-üblich)
  – Sonderzeichen wie €, Anführungszeichen und Gedankenstrich gehen beim Lesen/Schreiben nicht
  mehr verloren.

### Bedienung & In-App-Hilfe
- **Die In-App-Hilfe ist jetzt durchsuchbar.** Ein schwebendes Suchfeld (oben rechts, per
  **Strg+F** oder Knopf) hebt alle Fundstellen hervor; mit **F3 / Shift+F3** (oder Enter /
  Shift+Enter) springt man vor- und rückwärts von Treffer zu Treffer, mit **Esc** schließt es.
- **Kritische Fehler hängen die Anwendung nicht mehr auf:** Statt im Hintergrund weiterzulaufen,
  zeigt das Programm den Fehler an und beendet sich sauber – kein „Zombie"-Prozess mehr, der nur
  über den Task-Manager zu beenden war.

## v1.7.0 – 2026-07-16

### IGeL-Faktura läuft jetzt auch unter Linux
Passend zu T2med – das Server und Arbeitsplätze über **Windows und Linux** hinweg beliebig
kombinieren lässt – läuft IGeL-Faktura jetzt auch unter Linux. Server und Arbeitsplätze dürfen
gemischt sein (z. B. ein Linux-Server mit Windows-Arbeitsplätzen).

- **Unterstützte Systeme:** Windows 11 · Ubuntu 22.04 / 24.04 LTS.
- **Drucken** unter Linux über das Betriebssystem (CUPS/`lp`); unter Windows unverändert
  (mitgeliefertes SumatraPDF).
- **Server-Autostart** jetzt auch unter Linux (systemd-Dienst): Der Server startet – wie bisher
  unter Windows – **ohne Anmeldung** beim Hochfahren und wird über die Weboberfläche verwaltet.
- **Automatische Server-Suche** im Netz und die Selbstheilung bei IP-Wechsel funktionieren
  plattformübergreifend.
- **Auto-Update schützt gemischte Netze:** Ein Arbeitsplatz aktualisiert sich nur von einem Server
  mit **gleichem Betriebssystem** – ein Windows-Programm wird also nie versehentlich durch ein
  Linux-Paket ersetzt (und umgekehrt).

### Behoben
- **Auto-Update unter Linux:** Ein bereits beendeter Aktualisierungs-Prozess wurde als „läuft noch"
  fehlgedeutet, wodurch der Updater endlos wartete. Wird jetzt korrekt als beendet erkannt.

## v1.6.1 – 2026-07-15

### Behoben
- **Server-Suche findet den Server jetzt auch auf PCs mit mehreren Netzwerken zuverlässig.**
  Auf einem Arbeitsplatz mit gleichzeitig aktivem **VPN, WLAN und LAN** ging die Suchanfrage
  bisher nur über eine Netzwerkkarte (die des Standard-Wegs) hinaus – lag das Praxisnetz nicht
  darauf, wurde der Server nicht gefunden. Die Suche fragt jetzt über **alle** Netzwerkkarten
  des PCs gleichzeitig.

### Hinweis
- Der **Server-Port** bleibt (nur am Server) einstellbar – als Notausgang, falls 8765 belegt
  ist. Niemand muss ihn kennen: Arbeitsplätze erhalten ihn automatisch über die Server-Suche.

## v1.6.0 – 2026-07-15

### Arbeitsplätze finden den Server automatisch – keine IP-Eingabe mehr
- **Server-Suche im Praxisnetz:** Im Arbeitsplatz-Dialog gibt es neben der Server-Adresse den
  Knopf **„Im Netz suchen …"** – er findet laufende IGeL-Server automatisch (Zero-Configuration,
  wie bei Netzwerkdruckern) und zeigt sie mit **Praxisnamen** an; ein Klick übernimmt die
  Adresse und stellt die Rolle auf „Arbeitsplatz". IP-Adressen und Ports muss niemand mehr
  kennen oder abtippen. *(Grenze: über VPN/Router hinweg funktioniert die automatische Suche
  technisch nicht – dort bleibt die manuelle Adresseingabe.)*
- **Selbstheilung bei Adress-Wechsel:** Jeder Server trägt eine **stabile Kennung**. Bekommt der
  Server-PC eine neue IP (z. B. durch den Router/DHCP), findet ein Arbeitsplatz „seinen" Server
  beim Start automatisch wieder und aktualisiert die gespeicherte Adresse – statt „Server nicht
  erreichbar" zu melden.
- Die **Firewall-Freigabe** für die Server-Erkennung wird automatisch zusammen mit der
  bestehenden Server-Regel angelegt (weiterhin nur **eine** Windows-Admin-Nachfrage).
- Die Erkennungs-Antwort enthält keine Patientendaten (nur Praxisname, Kennung, Port, Version);
  der Datenzugriff bleibt durch die Arbeitsplatz-Freigabe geschützt.

## v1.5.1 – 2026-07-15

### Behoben (Feld-Feedback: Verknüpfung/Tray liefen ins Leere)
- **Tray und Verknüpfung richten sich jetzt nach dem wirklich laufenden Server** – nicht mehr
  nach der gespeicherten Einstellung. Der Server hinterlegt beim Start seinen tatsächlichen
  Port in einer Laufzeitdatei; das Steuerungs-Tray liest sie, und die Desktop-Verknüpfung
  „IGeL-Server verwalten" wird bei **jedem Serverstart** auf die echte Adresse aufgefrischt
  (bisher blieb der Port vom Einrichtungszeitpunkt eingebrannt – z. B. 8765 statt 8795).
  Auch eine Port-Änderung in den Einstellungen zieht die Verknüpfung sofort nach.
- **Das Eingabefeld „Server-Bindeadresse" ist entfernt** – es lud zu Verwechslungen ein
  (Server-IP? Subnetz-Maske?) und konnte den Server vom eigenen PC aus unerreichbar machen.
  Der Server ist jetzt **immer in allen Netzwerken dieses PCs erreichbar** (einschließlich
  `localhost` am Server-PC selbst); eine früher eingetragene feste IP wird beim Start
  automatisch ignoriert. Wer den Zugriff auf bestimmte Netze beschränken will, nutzt die
  Windows-Firewall – die Patientendaten schützt ohnehin die Arbeitsplatz-Freigabe.

## v1.5.0 – 2026-07-14

### Server: Autostart & saubere Trennung von Dienst und Steuerung
- **Server-Autostart (headless).** Der Server lässt sich jetzt so einrichten, dass er
  **automatisch beim Windows-Start** läuft – auch **ohne Anmeldung** (echter Server-Betrieb). Das
  Programm legt dabei selbst eine geplante Aufgabe an – der Windows-Aufgabenplaner muss **nicht
  mehr von Hand** konfiguriert werden.
  - **Ein-/ausschalten direkt in der Server-Verwaltung:** Knopf **„Autostart einrichten/entfernen"**
    im **Einstellungen → Server**-Tab der Weboberfläche (bzw. im Menü des Steuerungs-Tray).
    Einmalig mit Windows-Admin-Nachfrage. **Nur am Server-PC selbst** möglich – ein Arbeitsplatz
    kann den Server-Autostart nicht ändern.
- **Server ist jetzt ein reiner Hintergrund-Dienst; die Steuerung ist ein eigenes Programm.**
  Bisher hing das Tray-Icon am Server-Prozess – beim Autostart (Systemstart) gab es deshalb gar
  kein Icon. Jetzt:
  - Der **Server läuft immer headless** (nur im Hintergrund, kein Fenster/Icon).
  - Ein **separates, optionales Steuerungs-Tray** („**IGeL-Server steuern**") zeigt den Status
    (Apfel grün = Server läuft / rot = nicht erreichbar) und bietet: Verwaltung öffnen, Logs,
    Autostart ein/aus, Server beenden. Es startet **keinen** zweiten Server, sondern steuert den
    laufenden – funktioniert also auch beim Autostart-Server. Beim manuellen Start in der Rolle
    „Server" erscheint es automatisch; **läuft der Server bereits, öffnet ein Doppelklick auf das
    Programm einfach die Steuerung** (statt „läuft bereits").
  - Die Einrichtung legt zwei Desktop-Verknüpfungen an – **„IGeL-Server verwalten"** (Weboberfläche)
    und **„IGeL-Server steuern"** (Tray) – und trägt das Steuerungs-Tray zusätzlich in den
    **Windows-Autostart** ein: Nach der **Anmeldung** erscheint das Icon automatisch, während der
    Server selbst schon seit dem **Systemstart** läuft. Der **Server-Tab** der Einstellungen zeigt
    den Autostart-Status.

### Aufräumen & Deinstallation
- Der Server **räumt den Update-Zwischenspeicher selbst auf**: Beim Start werden gecachte
  Update-Pakete älterer Versionen (je ~240 MB im Windows-Temp-Ordner) automatisch gelöscht.
- Die Anleitung enthält jetzt einen **Deinstallations-Abschnitt** (erst „Autostart entfernen",
  dann Ordner löschen) – inklusive Hinweis auf die **Aufbewahrungspflicht** der Rechnungsdatenbank
  (GoBD) vor dem Löschen.

### Sicherheit
- Abhängigkeiten aktualisiert (**Pillow ≥ 12.3.0**, **setuptools ≥ 83.0.0**) zur Behebung
  gemeldeter Schwachstellen; die mitgelieferte Komponentenliste (SBOM) ist wieder ohne Befund.

## v1.4.4 – 2026-07-13

### Behoben
- **Automatisches Update funktioniert jetzt wirklich.** In v1.4.0–v1.4.3 brach die Aktualisierung
  eines Arbeitsplatzes durch einen internen Programmfehler **immer sofort** ab – noch bevor etwas
  geladen wurde; die Meldung „Update fehlgeschlagen" erschien unabhängig von Netz oder Server.
  Behoben und mit einem Test abgesichert.
  - **Wichtig für den Umstieg:** Ein Arbeitsplatz mit v1.4.0–v1.4.3 kann sich **nicht selbst** auf
    v1.4.4 aktualisieren. Diese eine Version bitte noch **manuell** installieren (Programmordner
    ersetzen, der Ordner `daten` bleibt erhalten) – **zuerst den Server, dann die Arbeitsplätze**.
    Ab v1.4.4 läuft das automatische Update.
- Der Aktualisierungsvorgang schreibt jetzt ein eigenes Protokoll (`daten\logs\igel-update.log`),
  damit ein Problem beim Ersetzen der Dateien nachvollziehbar ist.

## v1.4.3 – 2026-07-13

### Diagnose & Stabilität
- **Verständliche Fehlerseite statt „Internal Server Error":** Tritt ein unerwarteter Fehler
  auf, zeigt das Programm jetzt eine klare Diagnose-Seite mit dem **konkreten Fehler** und dem
  **Speicherort des Protokolls** – so lässt sich ein Problem (z. B. auf einem neu eingerichteten
  Rechner) sofort einordnen und melden, statt vor einer leeren Meldung zu stehen. Die Daten
  bleiben dabei unberührt.
- **Umgebungs-Protokoll beim Start:** Version, Betriebsart, alle Datenpfade und Schreibrechte
  werden beim Start ins Protokoll geschrieben – das erleichtert die Ferndiagnose erheblich.

## v1.4.2 – 2026-07-12

### Behoben
- **Automatisches Update lief immer in „Update fehlgeschlagen":** Der Server stellte das
  Update-Paket erst beim Abruf zusammen (bei der Programmgröße dauert das), während der
  Arbeitsplatz schon nach kurzer Zeit abbrach – sowohl über VPN als auch direkt am Server.
  Jetzt bereitet der **Server das Paket bereits beim Start vor** (zwischengespeichert), und der
  Arbeitsplatz **lädt es geduldig mit Fortschrittsanzeige** herunter, statt vorzeitig aufzugeben.
- **Klarere Meldung, wenn ein Update doch scheitert:** statt „bitte später erneut versuchen" nennt
  der Arbeitsplatz jetzt den **konkreten Grund** (Server nicht erreichbar / Arbeitsplatz noch
  nicht freigegeben / Zeitüberschreitung / zu wenig Speicherplatz) und den Pfad zum Protokoll.

### Sicherheit
- **Update-Paket enthält garantiert keine Patientendaten:** Das an die Arbeitsplätze
  ausgelieferte Programm-Paket schließt jede Datenbank und alle lokalen Einstellungen zuverlässig
  aus – auch dann, wenn die Datenbank per Einstellung an einen anderen Ort gelegt oder ein
  Sicherungsordner versehentlich im Programmordner angelegt wurde.

## v1.4.1 – 2026-07-12

### Behoben
- **Konto-Abgleich – Rechnungsnummern:** werden jetzt **formatunabhängig** im Verwendungszweck erkannt – nicht nur `2026/001`, sondern auch zusammenhängende Nummern wie `M4006260049` oder `K400520260096`. Damit greift die 🟢-Ampel (Nummer erkannt) auch bei diesen Formaten statt immer nur „vermutet".
- **„Arbeitsplatz noch nicht freigegeben":** Der Arbeitsplatz zeigt jetzt klar **„bitte am Server unter ‚Arbeitsplätze' freigeben"** und verbindet automatisch, sobald er freigegeben ist – statt der irreführenden Meldung „Server nicht erreichbar".
- **Doppelstart verhindert:** „IGeL-Faktura läuft bereits" (je Betriebsart) – keine mehrfachen Server-Instanzen/Tray-Icons mehr; Server und Arbeitsplatz auf einem PC (zum Testen) bleiben möglich.
- **Arbeitsplätze-Liste zeigt den Stationsnamen** (Sprechzimmer, Anmeldung …) statt nur des Geräte-Tokens – so ist erkennbar, welcher Arbeitsplatz sich anmeldet. Freigaben bleiben nach einem Server-Neustart erhalten.
- Beim Start wird **zuerst das automatische Update** angeboten (der zusätzliche „gleiche Version empfohlen"-Hinweis entfällt); die Version des Arbeitsplatzes ist im Fenster/Menü sichtbar. Der alte Server-Schlüssel entfällt vollständig (durch die Freigabe ersetzt).

## v1.4.0 – 2026-07-12

### Konto-Abgleich
- **Englische Spaltenüberschriften** werden erkannt (Date / Value / Amount / Purpose …) – der Bank-Export läuft ohne Umbenennen; bei 0 Treffern nennt eine klare Meldung die gefundenen Spalten.
- **Farb-Ampel** je Zahlung: 🟢 über die Rechnungsnummer erkannt, 🟠 nur über Name + Betrag vermutet.
- **Sammelüberweisungen:** stehen mehrere Rechnungsnummern im Verwendungszweck und die Summe passt, werden alle betroffenen Rechnungen zugeordnet und vorgehakt (z. B. wenn eine Mutter die Rechnungen mehrerer Kinder zusammen überweist).

### Client/Server – Rollen & Bedienung
- **Explizite Betriebsart** (Einzelplatz / Server / Arbeitsplatz), direkt im Programm über die native Menüleiste **„Dieser Arbeitsplatz"** wählbar – nicht mehr implizit über die Server-Adresse. Zurück auf Einzelplatz jederzeit möglich.
- **Arbeitsplatz-Freigabe statt Schlüssel:** neue Arbeitsplätze werden am Server unter **„Arbeitsplätze"** freigegeben (einzeln oder zeitweise automatisch „zulassen"); jeder Arbeitsplatz merkt sich ein eigenes Geräte-Token. Kein Schlüssel mehr zum Abtippen.
- **Native Menüleiste** am Fenster: die lokalen Einstellungen dieses PCs sind **immer** erreichbar – auch ohne Serververbindung. Das Client-Fenster zeigt klar „Arbeitsplatz · Server …".
- **Automatisches Update:** ist der Server neuer, aktualisiert sich ein Arbeitsplatz beim Start auf Wunsch selbst vom Server – die lokalen Daten und Einstellungen bleiben erhalten.
- Einstellungen und angezeigte Funktionen richten sich jetzt nach der Rolle: ein reiner **Server** zeigt keine Arbeitsplatz-Einstellungen (GDT/Drucker/Mahnungs-GDT) mehr, ein **Arbeitsplatz** verstellt nicht mehr versehentlich die Server-Konfiguration.

### Behoben
- Server-Start bricht nicht mehr an einem Sonderzeichen ab, wenn die Ausgabe umgeleitet ist (kein Fenster/Konsole).
- **`--help`** erscheint jetzt als Fenster (die fensterlose App hat keine Konsole).

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
