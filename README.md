# Passwort-Tippexperiment

Statische Web-App (eine HTML-Datei + Wortliste), misst, wie schnell und fehlerfrei Kennwörter zweier Policies abgetippt werden.

## Policies

| | Policy 1 – Passphrase | Policy 2 – Komplex, 8 Zeichen | Policy 2 – Komplex, 12 Zeichen |
|---|---|---|---|
| Aufbau | 6 Diceware-Wörter (deutsche Liste, 7776 Wörter, nur a–z), jedes Wort zufällig mit grossem oder kleinem Anfangsbuchstaben, ohne Trennzeichen | 8 Zeichen aus 94 druckbaren ASCII-Zeichen; jede Klasse (Gross, Klein, Ziffer, Sonderzeichen) mindestens einmal | wie links, 12 Zeichen |
| Entropie | 6 × (log2 7776 + 1) = **83,55 Bit** | Inklusion-Exklusion über die Klassenbedingung: **51,32 Bit** | **78,14 Bit** |
| Ziel ≥ 76 Bit | erreicht | **nicht erreichbar** – rot markiert | erreicht |

Die Länge von Policy 2 wird auf der Startseite per Schalter (8 / 12 Zeichen) gewählt, gilt für den ganzen Durchgang und wird im Browser gespeichert (`pwtyping.p2len.v1`). Die Statistik wertet 8- und 12-Zeichen-Kennwörter getrennt aus.

Hinweise zur Entropie: Bei Policy 1 sind Wortgrenzen ohne Trennzeichen theoretisch mehrdeutig; der Überschätzungsfehler liegt weit unter 1 Bit. Bei Policy 2 werden gültige Kennwörter per Rejection Sampling gleichverteilt erzeugt, die angezeigte Entropie ist also exakt.

## Ablauf

1. Start → 10 Kennwörter, je 5 pro Policy, Reihenfolge zufällig (Fisher-Yates mit `crypto.getRandomValues`).
2. Kennwort wird angezeigt, Eingabefeld hat den Fokus. Einfügen (Paste/Drop) ist blockiert.
3. Messung: Lesezeit = Anzeige bis erste Taste; Tippzeit = erste Taste bis korrektes Enter. Falsche Eingabe zählt als Versuch, Feld wird geleert, die Tippzeit läuft weiter.
4. Nach jedem Kennwort: Tippzeit, Lesezeit, Entropie (mit 76-Bit-Flag), Versuche → „Weiter“.
5. Nach 10 Kennwörtern: Auswertung pro Policy (Ø/Median Tippzeit, Lesezeit, Zeichen/s, Versuche, Erstversuch-Quote, Entropie).
6. Durchgänge werden in `localStorage` (`pwtyping.runs.v1`) gespeichert; „Statistik“ auf der Startseite zeigt alle Durchgänge, mit JSON-Export und Löschen. Kennwörter selbst werden nicht gespeichert.

## Betrieb

Reine statische Dateien, kein Build, keine Abhängigkeiten ausser Google Fonts (mit System-Fallback). Für GitHub Pages: `index.html` und `wordlist.js` ins Repo, Pages auf den `main`-Branch (Root) zeigen lassen.
