# MeloQuiz (iOS, SwiftUI)

by Ivan Bessala

Erkenne Lieder an den ersten Sekunden und sag den Titel laut. Mit Künstler gibt es Bonuspunkte.

## Starten in Xcode

1. Ordner entpacken und `MeloQuiz.xcodeproj` mit **Xcode 16 oder neuer** öffnen.
2. Links das Projekt **MeloQuiz** anklicken → Target **MeloQuiz** → Reiter **Signing & Capabilities**.
3. Bei **Team** deine Apple-ID auswählen. Falls Xcode meckert, die **Bundle Identifier** ändern (z. B. `de.deinname.melodiequiz`).
4. iPhone per Kabel verbinden, oben als Ziel auswählen und auf ▶︎ drücken.

Am iPhone muss beim ersten Mal der Entwicklermodus aktiv sein (Einstellungen → Datenschutz & Sicherheit → Entwicklermodus).

**Simulator:** Läuft auch, aber die Spracheingabe funktioniert dort oft nicht zuverlässig. Dafür gibt es den Knopf „Lieber tippen“.

## So funktioniert das Spiel

- **Genres** wählen: Pop, Dance & Elektro, Hip-Hop & Rap, R&B & Soul, Deutsch-Pop & NDW, Schlager & Party, Latin, Reggae & Afrobeats (kein Rock, Metal, Jazz, Country).
- **Jahre** wählen: per Jahrzehnt-Knopf oder frei von–bis (1980 bis 2026).
- **Antwort per** wählen: *Sprache*, *Tippen* oder *Beides*. Beim Tippen erscheinen Titelvorschläge aus dem ganzen Katalog (nur Titel, keine Künstler – so wird nichts verraten). Im Modus *Tippen* fragt die App nicht nach Mikrofon-Zugriff.
- Jede Runde startet mit **1 Sekunde**. „Länger hören“ oder eine falsche Antwort schaltet weiter:
  1 → 2 → 3 → 4 → 5 → 6 → 8 → 9 → 10 → 11 Sekunden.
- **Punkte:** Titel = 100 Punkte auf Stufe 1, pro Stufe 10 weniger (mind. 10). Künstler = +50 % Bonus.
- Nur den Künstler zu sagen sichert den Bonus, die Runde ist aber erst mit dem Titel gelöst.
- Die Erkennung ist tolerant („Billy Jean“ zählt für „Billie Jean“) und hört gleichzeitig auf Deutsch und Englisch.

## Musikquelle & Urheberrecht

Die App enthält **keine Musik und keine abgeschriebenen Melodien**, nur Titel, Künstler, Jahr und Genre (`MeloQuiz/Resources/songs.json`). Abgespielt wird über Apple:

- **Mit Apple-Music-Abo:** Das Lied wird über MusicKit ab dem echten Anfang abgespielt. Dafür muss in deinem Apple-Developer-Account für die Bundle-ID der App-Service **MusicKit** aktiviert sein (developer.apple.com → Identifiers → deine App-ID → App Services → MusicKit). Das geht nur mit bezahlter Developer-Mitgliedschaft.
- **Ohne Abo / ohne MusicKit:** Die App nutzt automatisch die offiziellen 30-Sekunden-Hörproben aus dem iTunes Store. Die Ausschnitte beginnen am Anfang der Hörprobe. Das ist bei vielen Liedern der Songanfang, manchmal aber eine Stelle aus der Mitte.

Für den privaten Gebrauch ist das unproblematisch. Wer die App im App Store veröffentlichen will, sollte vorher die Apple-Richtlinien für MusicKit und die iTunes-Search-API prüfen.

## Lieder hinzufügen

In `songs.json` einfach einen Eintrag ergänzen:

```json
{ "id": 2000, "title": "Liedtitel", "artist": "Künstler", "year": 2024, "genre": "pop", "aliases": ["andere Schreibweise"] }
```

Erlaubte Genres: `pop`, `dance`, `hiphop`, `rnb`, `deutsch`, `schlager`, `latin`, `reggae`. Die `id` muss eindeutig sein, `aliases` ist optional.

## Projektaufbau

| Ordner | Inhalt |
|---|---|
| `Models` | `QuizSong`, `Genre` |
| `Services` | Katalog laden, Abspielen (Apple Music / Hörproben), Spracherkennung, Antwortvergleich |
| `Game` | Spielablauf, Punkte, gespeicherte Einstellungen |
| `Views` | Startseite, Spielbildschirm, Ergebnis |
| `Resources` | `songs.json` (3279 Lieder, 1980–2026, Chart-Hits & Radio-Klassiker, mind. 100 pro Genre) |

Die Stufen stehen in `GameEngine.swift` (`static let stages`) und lassen sich dort leicht ändern.
