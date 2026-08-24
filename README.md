# select-search

Geeft elke `<select>` met veel opties een zoekveld, voor webapplicaties waarvan de dropdowns geen
zoekfunctie hebben. Een native dropdown zoekt alleen op de *eerste* letters, dus "Poule" vinden in
"Restaurant Poule & Poulette B.V." lukt niet. Met dit script wel.

Het originele `<select>` blijft bestaan en blijft de bron van waarheid: het wordt enkel visueel
verborgen. Een keuze zet `selectedIndex` en vuurt native `input`- en `change`-events, dus de
applicatie merkt geen verschil met een gewone selectie en het formulier verstuurt hetzelfde als
voorheen.

## Installeren

Werkt hetzelfde in Chrome en Edge.

1. Installeer Tampermonkey:
   [Chrome](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
   of [Edge](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd).
2. Zet eenmalig **Allow user scripts** aan. Ga naar je extensiepagina
   (`chrome://extensions` of `edge://extensions`), klik bij Tampermonkey op *Details* en zet die
   schakelaar om. Zonder dat draait geen enkel userscript.
3. Open de [installatielink](https://raw.githubusercontent.com/glgoose/select-search/main/dist/select-search.user.js).
   Tampermonkey toont zijn installatiescherm in plaats van de broncode.
4. Klik op installeren.

Staat je browser onder beheer van je werkgever, dan kan het zijn dat extensies alleen van een
goedgekeurde lijst mogen komen. Vraag dat na bij je IT-dienst voor je het probeert.

### Zonder extensie: de bookmarklet

Mag je geen extensies installeren, gebruik dan de bookmarklet. Die vraagt niets van je browser en
werkt overal, maar je moet hem per pagina zelf aanklikken.

1. Neem de inhoud van [`dist/bookmarklet.txt`](dist/bookmarklet.txt) over.
2. Toon de bladwijzerbalk, rechtsklik erop en kies *Bladwijzer toevoegen*.
3. Naam: `Zoek in dropdown`. URL: de gekopieerde tekst.
4. Klik de bladwijzer aan op de pagina met de dropdown. Nog eens klikken doet een herscan.

Websites met een strikte Content-Security-Policy blokkeren bookmarklets. Werkt hij niet, dan is de
extensie de enige route.

## Gebruik

- Typen filtert op **substring**, niet alleen op het begin, en negeert accenten. `genee` vindt
  `Genée`.
- Meerdere woorden mogen in willekeurige volgorde: `poule rest` vindt
  `Restaurant Poule & Poulette B.V.`
- Pijltjes navigeren, Enter kiest, Escape sluit zonder te wijzigen, Tab kiest en gaat verder.
- Boven de 200 treffers stopt het met tekenen en toont het hoeveel er nog zijn. Verfijn dan je
  zoekterm.

Dropdowns met minder dan 10 opties blijven onaangeroerd.

## Als er iets misgaat

**Alt+klik** op een zoekveld zet voor dat ene veld het originele keuzemenu terug, voor als een
specifiek veld zich vreemd gedraagt. Zet `data-nosearch` op een `<select>` om hem permanent over te
slaan.

**Er gebeurt niets op de pagina.** Controleer in het Tampermonkey-dashboard of de URL van de pagina
onder de `@match` van het script valt. Zit het formulier in een `<iframe>` van een ander domein, dan
kan het script daar niet bij: dat is een beperking van de browser, niet van het script.

## Zelf aanpassen

`src/select-search.js` bevat de volledige logica, zonder dependencies. `build.sh` genereert daaruit
het userscript en de bookmarklet in `dist/`.

```sh
./build.sh
```

Wijzig je iets, hoog dan `@version` op in `src/userscript-header.txt`. Zonder verhoging krijgt
niemand de update binnen.

`test/fixture.html` bootst een echte applicatie na: 850 opties en een checkbox die de optielijst
vervangt, met een formulier dat toont welke waarde er werkelijk verstuurd wordt.

```sh
python3 -m http.server 8731
# open http://localhost:8731/test/fixture.html
```

## Licentie

MIT
