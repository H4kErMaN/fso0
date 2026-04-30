# Uuden muistiinpanon luominen sivulla `/notes`

Käyttäjä on sivulla `https://studies.cs.helsinki.fi/exampleapp/notes`, kirjoittaa tekstikenttään ja painaa **Tallenna**-nappia.

```mermaid
sequenceDiagram
    participant selain as Selain
    participant palvelin as Palvelin

    Note right of selain: Käyttäjä kirjoittaa tekstikenttään ja klikkaa Tallenna-nappia.<br/>Selain lähettää lomakkeen sisällön POST-pyyntönä palvelimelle.

    selain->>palvelin: POST https://studies.cs.helsinki.fi/exampleapp/new_note<br/>Content-Type: application/x-www-form-urlencoded<br/>body: note=<käyttäjän kirjoittama teksti>

    Note left of palvelin: Palvelin lisää uuden muistiinpanon palvelimella<br/>olevaan muistiinpanojen listaan ja vastaa<br/>uudelleenohjauksella (HTTP 302) osoitteeseen /notes.

    palvelin-->>selain: HTTP 302 Found<br/>Location: /exampleapp/notes

    Note right of selain: Selain noudattaa uudelleenohjausta ja tekee<br/>uuden GET-pyynnön sivulle /notes.

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/notes
    palvelin-->>selain: HTML-dokumentti (notes-sivu)

    selain->>palvelin: GET /exampleapp/main.css
    palvelin-->>selain: main.css

    selain->>palvelin: GET /exampleapp/main.js
    palvelin-->>selain: main.js

    Note right of selain: Selain alkaa suorittaa main.js-koodia,<br/>joka hakee JSON-muodossa olevat muistiinpanot palvelimelta.

    selain->>palvelin: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    palvelin-->>selain: [{ "content": "...", "date": "..." }, ...]

    Note right of selain: Selain suorittaa tapahtumankäsittelijän, joka<br/>renderöi muistiinpanot (myös juuri lisätyn) sivulle.
```

## Selitys lyhyesti

1. **Lomakkeen lähetys:** Tallenna-napin painaminen aiheuttaa lomakkeen `submit`-tapahtuman. Lomakkeen `action` on `/new_note` ja `method` on `POST`, joten selain lähettää tekstikentän sisällön palvelimelle POST-pyyntönä.
2. **Palvelimen toiminta:** Palvelin lisää uuden muistiinpanon muistissaan olevaan listaan ja vastaa uudelleenohjauksella (statuskoodi 302) osoitteeseen `/exampleapp/notes`.
3. **Uudelleenohjaus:** Selain seuraa uudelleenohjausta ja lataa `/notes`-sivun uudelleen, mukaan lukien CSS- ja JavaScript-tiedostot.
4. **Muistiinpanojen haku:** Sivun JavaScript-koodi hakee muistiinpanot erillisellä GET-pyynnöllä osoitteesta `/data.json` ja renderöi ne — myös juuri lisätyn — sivulle.
