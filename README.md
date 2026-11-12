# Judo-reisen i Japan – Haralds japanskspill (v1)

Én fil: `index.html` (CSS og JS ligger inni, ingen byggsteg, ingen API-nøkler, virker offline).
Åpne filen i en nettleser. Fremgang (poeng, streak, belte) lagres i nettleserens `localStorage`
på den enheten Harald bruker.

## Slik legger du til ukens tema
1. Bestem hvilke temaer uka skal ha (f.eks. vær, sport, mat, høflige fraser, lengdeenheter).
2. Åpne `index.html` og finn blokken `const WEEK_CONTENT = { ... }` (helt øverst i `<script>`).
3. Legg til en ny nøkkel med ukenummeret (kopier uke 46 som mal):

```js
47: {
  label: "Uke 47",
  topics: ["Vær", "Mat"],                     // ukens temaer, vises på forsiden
  vocab: [                                        // spillet lager spørsmål automatisk av disse
    { no:"regn", jp:"あめ", kanji:"雨", ro:"ame", tema:"Vær" }
  ],
  lengths: [                                      // matte: omgjøring av lengdeenheter (valgfritt)
    { text:"Fra Tokyo til Kyoto er det ca. {v} {u}.", v:476, u:"km" }   // u: km | m | dm | cm | mm
  ],
  questions: [                                    // egne flervalgsspørsmål
    { tema:"Vær", prompt:"Hva betyr dette?", show:{ jp:"あめが ふっています。", ro:"Ame ga futte imasu." },
      choices:["Det regner.","Det snør.","Sola skinner.","Det blåser."],   // FØRSTE = riktig svar (stokkes i spillet)
      explain:"あめ = regn." }
  ]
},
```

* Svaralternativ kan være norsk tekst (`"Det regner."`) eller japansk objekt (`{ jp:"あめ", ro:"ame", kanji:"雨" }`).
* Ha minst 4 ord i `vocab` (gjerne 15+), og alltid 4 svaralternativ per spørsmål.
* Spillet velger automatisk uka som matcher dagens ISO-ukenummer, ellers siste uke før.
  Under ⚙️ Innstillinger kan man velge uke manuelt.
* Romaji skrives i Hepburn med lange vokaler som ō/ū/ā (f.eks. `arigatō`, `sakkā`).

## Spillmekanikk (for den som vil endre)
* Kamp i en by = 4 praktiske situasjoner fra byen (`STOPS[].questions`) + 5 fra ukens tema + 3 ord/kana.
* Riktig svar = waza-ari (2 = ippon). Feil svar = waza-ari til motstanderen. Først til 3 ippon vinner.
* Vinn en ny by → neste belte (hvitt → gult → oransje → grønt → blått → brunt → svart).
* Poeng: 10 per riktig, +5 ved 3 riktige på rad, +50 for seier. Streak teller dager med minst én fullført kamp eller dagens utfordring.
* «Ukens trening» = kamp bare med ukens tema (gir poeng, ikke belte).

## Dagens utfordring
På forsiden ligger knappen **⭐ Dagens utfordring**. Harald får **8 oppgaver** som er de samme hele dagen og nye i morgen (trukket fra datoen, med blanding av kategorier). Spørsmål som er brukt, holdes unna i omtrent to uker.

Oppgavebanken ligger i `DAILY_POOL` i `index.html` (judo, reise, mat, tall/tid/penger, kana, hverdagsfraser og setninger). Dette er egne oppgaver, ikke ukens tema. Første svaralternativ er alltid riktig (stokkes i spillet).

Første gjennomføring om dagen gir poeng (10 per riktig, +5 ved tre på rad, +20 for alle riktige) og teller med på streaken. Etterpå kan han øve på nytt uten ekstra poeng. Når den er ferdig, vises «Kom tilbake i morgen for nye oppgaver!». Fremgangen lagres i `localStorage`.

Hele banken er listet i `dagens-utfordring-oversikt.md` til gjennomgang før publisering.

## Kanji og furigana
Når **Vis kanji** er slått på (standard, under ⚙️), vises kanji sammen med kana som furigana, for eksempel <ruby>空港<rt>くうこう</rt></ruby>. Det gjelder dagens utfordring, bykampene, ukens tema og ordlisten. Romaji styres fortsatt av den egne innstillingen.

Kana-lesing (der oppgaven er å lese hiragana/katakana) vises uten kanji, så svaret ikke gis bort. 🔊 Uttale bruker alltid kana-lesingen, ikke kanji.

## Ønsk reisemål for i morgen
Knappen **✈️ Ønsk deg et reisemål for i morgen** lar Harald velge blant 15 steder i Japan (reise-byene pluss blant annet Nara, Okinawa, Nikkō, Kamakura, Fuji-san, Hakone, Nagoya, Kanazawa og Kobe). Valget lagres i `localStorage` og gjelder **neste dag**. Da inneholder Dagens utfordring omtrent tre oppgaver om stedet, og skjermen viser «I dag reiser du til X!». Uten ønske er utfordringen som før.

## Byinfo
Trykk på en by på kartet (eller på ℹ️ i ønskelisten) for å åpne et infokort: navn med kanji, kana og romaji, noen fakta og nyttige ord. Er byen åpen på reisen, kan du også starte kampen derfra.

Se `oppdatering-kanji-reisemal-oversikt.md` for alle infokort og destinasjonsspørsmål (til gjennomgang før publisering).
