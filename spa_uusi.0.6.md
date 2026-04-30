# Uuden muistiinpanon luominen SPA-versiossa

Käyttäjä on sivulla `https://studies.cs.helsinki.fi/exampleapp/spa`, kirjoittaa tekstikenttään ja painaa **Tallenna**-nappia.

```mermaid
sequenceDiagram
    participant selain as Selain
    participant palvelin as Palvelin

    Note right of selain: Käyttäjä kirjoittaa tekstikenttään ja klikkaa Tallenna-nappia.<br/>Sivun spa.js-koodin tapahtumankäsittelijä estää lomakkeen<br/>oletuskäyttäytymisen (event.preventDefault), jolloin selain<br/>EI tee tavallista lomakelähetystä eikä sivu lataudu uudelleen.

    Note right of selain: Tapahtumankäsittelijä luo uuden muistiinpano-olion,<br/>lisää sen selaimen omaan muistiinpanojen listaan,<br/>renderöi listan uudelleen sivulle ja lähettää uuden<br/>muistiinpanon palvelimelle JSON-muodossa POST-pyynnöllä.

    selain->>palvelin: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa<br/>Content-Type: application/json<br/>body: { "content": "...", "date": "..." }

    Note left of palvelin: Palvelin lisää uuden muistiinpanon palvelimella<br/>olevaan muistiinpanojen listaan ja vastaa<br/>statuskoodilla 201 Created. Tämän pyynnön<br/>yhteydessä ei tehdä uudelleenohjausta.

    palvelin-->>selain: HTTP 201 Created<br/>body: { "message": "note created" }

    Note right of selain: Selain ei tee mitään palvelimen vastauksen perusteella —<br/>näkymä päivitettiin jo aiemmin paikallisesti.
```
