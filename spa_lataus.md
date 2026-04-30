# SPA-version lataaminen sivulla `/spa`

Käyttäjä menee selaimellaan osoitteeseen `https://studies.cs.helsinki.fi/exampleapp/spa`.

```mermaid
sequenceDiagram
    participant selain as Selain
    participant palvelin as Palvelin

    Note right of selain: Käyttäjä kirjoittaa osoitepalkkiin URL-osoitteen<br/>https://studies.cs.helsinki.fi/exampleapp/spa<br/>ja selain lähettää GET-pyynnön palvelimelle.

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/spa
    palvelin-->>selain: HTML-dokumentti (spa-sivu)

    Note right of selain: Selain alkaa parsia HTML-dokumenttia ja huomaa,<br/>että se viittaa CSS- ja JavaScript-tiedostoihin.<br/>Selain hakee ne palvelimelta.

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    palvelin-->>selain: main.css

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/spa.js
    palvelin-->>selain: spa.js

    Note right of selain: Selain alkaa suorittaa spa.js-koodia.<br/>Koodi tekee HTTP GET -pyynnön muistiinpanojen<br/>JSON-dataan osoitteeseen /data.json.

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    palvelin-->>selain: [{ "content": "...", "date": "..." }, ...]

    Note right of selain: Kun JSON-data on saapunut, selain suorittaa<br/>tapahtumankäsittelijän, joka renderöi muistiinpanot<br/>sivulle DOM-puuta manipuloimalla.
```

## Ero tavalliseen `/notes`-versioon

SPA-versiossa palvelimelta haettava JavaScript-tiedosto on eri (`spa.js` eikä `main.js`), ja **lomakkeen lähettäminen tapahtuu jatkossa ilman sivun uudelleenlatausta** — koodi lähettää uuden muistiinpanon palvelimelle JSON-muodossa ja päivittää näkymän selaimessa itse, sen sijaan että koko sivu ladattaisiin uudelleen palvelimelta.

Itse SPA-sivun **alkulataus** näyttää kuitenkin pyyntöjen tasolla hyvin samanlaiselta kuin tavallisen `/notes`-sivun lataus: HTML, CSS, JS ja lopuksi data.json.
