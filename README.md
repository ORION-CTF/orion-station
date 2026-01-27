Idee um beide Ansätze zu kombinieren:

Ausgangslage - Space Station Szenario

Challange - 1 - OSINT
Ziel: den Namen / Benutzernamen eines Wartungsmitarbeiters herausfinden 
- bevor man ein System hackt, sammelt man Informationen
- Spieler sehen eine öffentlich erreichbare "Infoseite" (Github-gehostet), die weitere Verlinkungen zu Wartungsprotokollen; Blog-/Status-Post oder internen Notizen hat.
- in diesen Daten ist indirekt der Name versteckt (Metadaten beachten: Kommentare, Footer, Commit-Namen, Signaturen)
- ein benutzername wie: orion.maintenance wird gefunden

Challange - 2 - Web Exploitation
Ziel: Zugriff auf interne Wartungskonsole erlangen
- Die Konsole erlaubt es Crew-Mitgliedern 1. Ein Profilbild ("Crew-Badge" als PNG) hochzuladen und 2. Statusinformationen einzusehen
- Ein altes PHP-Modul wird noch für diesen Upload benutzt
- Die Upload-Funktion prüft nur, ob die Datei wie ein PNG aussieht (Magic Bytes Check)
- Angriff: Die Spieler laden eine PNG-Datei hoch, die 1. mit gültigen PNG-Magic-Bytes beginnt und 2. danach PHP-Code enthält
- Durch eine Fehlkonfiguration wird die Datei im Web-Verzeichnis gespeichert und vom Server als PHP interpretiert
- Vorgehen: Spieler loggt sich mit username aus Challange 1 ein, findet PNG-Upload-Funktion und lädt eine preparierte PNG-Datei hoch, die hochgeladene Datei im Browser aufrufen -> Remote Code execution
- der Spieler hat nun Zugriff auf die Server-Datenstruktur z.b. Konfigurationsdateien, Tokens, Hinweise auf das nächste System.

Challange - 3 - Privilege Escalation
Ziel: Vom normalen Benutzer zu Root-Rechten gelangen (um die lebensentscheidende Selbstzerstörung zu stoppen)
Idee: Auch wenn man Code ausführen kann, ist man selten sofort Admin; Das Life-Support-System läuft in einem separaten System/Container
- Fehlkonfiguration: ein Wartungsarbeiter darf bestimmte Programme mit sudo ausführen ohne Passwort 
(z.b. Log-Viewer, Python-Script, Diagnose Tool)
-> Diese Programme erlauben Shell-Escape, Manipulation von Imports oder unkontrollierte Befehle.
Lösung: Spieler loggen sich in das Life-Support-System ein, prüfen ihre rechte, nutzen die Fehlkonfiguration aus,
erlangen Root-Zugriff, lesen eine geschützte Datei
-> Ergebnis: ein Shutdown-Code-Fragement oder Schlüsselteil

Challange - 4 - Logic Bug / API
Ziel: den Selbstzerstörungsmechanismus stoppen
Idee: nicht alles wird "gehackt" - sondern manchmal ist die Logic falsch
- der AI-Core erwartet einen Shutdown-Code, prüft ihn aber falsch
- Spieler muss Code-Fragmente aus den vorherigen Challanges sammeln, analysiert die API, nutzt Logikfehler aus und übermittelt einen manupulierten / unvollständigen Code
-> Ergebnis: Selbstzerstörung gestoppt - ORION gerettet