# h4 NFC ja RFID

## a) Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta?

Luulin olevani suojautunut hyvin. Oikeastaan ainoa suojaus on lompakko, joka estää RFID signaalit. Tämä tosiaan on varmaankin kaikista tärkein, jotta eivät pääse pitkäkyntiset ruuhkassa tekemään lähimaksuveloituksia pankkikortiltani. Kokeilin [NFC toolsilla](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc) lukea lompakkoa. Lompakko suljettuna ei tullut signaalit läpi, kun taas avattuna pystyy lukemaan kortteja taskujen läpi.

Otin tutkailuun oman avainnipun. Löysin avaimistani 4 RFID esinettä, jotka kaikki pystyin lukea NFC toolsilla:

- Abloy Iloq avain
- Hälytysjärjestelmän tagi
- Työpaikan tagi
- Työpaikkaruokalan tagi

Jos avaimet on taskussa, taskun läpi pystyisi lukemaan nuo kaikki neljä esinettä. Mikään näistä ei ole kovin kriittinen esine. Silti on hyvä pohtia miten pystyisi estämään esineiden etäluvun. RFID proof vaatteet? Siinäpä bisnesidea, jos joku lähtee mukaan, voidaan alkaa toteuttamaan.

## b) Tutustu APDU komentojen rakenteeseen (voit käyttää tekoälyä tutustumiseen)

[Wikipedia](https://en.wikipedia.org/wiki/Smart_card_application_protocol_data_unit) kertoo:

APDU rakenne on määritelty [ISO/IEC 7816](https://en.wikipedia.org/wiki/ISO/IEC_7816):ssa

- Älykorteissa **APDU** = Application Protocol Data Unit = keskustelua lukijan ja kortin välillä.
- **APDU command-response pari**
  - **Command APDU** = lukijalta kortille
    - 4 tavun header: CLA, INS, P1, P2
      - CLA = 1 tavu, ilmaisee millainen käsky on kyseessä
      - INS = 1 tavu, kertoo käskyn yksilöllisen toiminnon, kuten: "select", "read", tai "write
      - P1 ja P2 = käskyn parametrit
    - 0-65535 tavua dataa
  - **Response APDU** = kortilta lukijalle
    - Response data = 0-65535 tavua dataa vastauksena
    - 2 pakollista status-tavua: SW1, SW2
      - SW1 ja SW2 = 2 tavua, status määrittelee, onko käsky suoritettu onnistuneesti, hexadesimaalilla ilmaistu 90 00 on yleensä onnistunut suoritus


## c) Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit. (Vinkki, useimmat liittyvät henkilökortteihin)

Luin aluksi uutisia bensavarkaista, kuvitteellisesta kissojen mikrosirun hakkeroinnista (joka on täysin mahdollista), ja taikurista, joka unohti salasanansa ihon alle upotettuun mikrosiruun. Mikään näistä ei ollut riittävän mielenkiintoista. 

Löysin lopulta artikkelin: [Behind Closed Doors: hacking rfid readers](https://i.blackhat.com/Asia-25/Asia-25-Zdunczyk-Behind-Closed-Doors-Bypassing-RFID-Readers.pdf), (kalvot 1-13) jossa kerrotaan miten voidaan sivuuttaa RFID pääsynhallintaa. Tämä ei ole varsinainen uutinen, mutta sitäkin mielenkiintoisempaa luettavaa. Artikkelissa ei keskitytä RFID korttien kloonaamiseen, vaan RFID lukijoiden hakkerointiin.

Joitain tapoja Sebury RFID lukijan hakkerointiin:

- Default admin password = 6-8 merkkiä, factory default: 888888.
- Kortti, joka ohittaa lukijan logiikan. Esimerkissä kortin UID = FFFFFFFF <-- ei voida poistaa järjestelmästä.
- Elektromagneettinen pulssi saattaa joskus nollata lukijan muistin ja avata lukon.





### Lähteet

[https://play.google.com/store/apps/details?id=com.wakdev.wdnfc](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc)

[https://en.wikipedia.org/wiki/ISO/IEC_7816](https://en.wikipedia.org/wiki/ISO/IEC_7816)

[https://en.wikipedia.org/wiki/Smart_card_application_protocol_data_unit](https://en.wikipedia.org/wiki/Smart_card_application_protocol_data_unit)

[https://i.blackhat.com/Asia-25/Asia-25-Zdunczyk-Behind-Closed-Doors-Bypassing-RFID-Readers.pdf](https://i.blackhat.com/Asia-25/Asia-25-Zdunczyk-Behind-Closed-Doors-Bypassing-RFID-Readers.pdf)



---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. [http://www.gnu.org/licenses/gpl.html](http://www.gnu.org/licenses/gpl.html)

Pohjana Tero Karvinen & Lari-Iso Anttila 2025: [Verkkoon tunkeutuminen ja tiedustelu](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/)

Kirjoittanut: <em>Santeri Vauramo</em> 2025
