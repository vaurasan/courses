# h3 Hello Web Server

#### Oma Host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core Processor 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3600MHz, CL 16, 1.3  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF Gaming Gold        | ATX 80 Plus Gold      |
| Kotelo   | Phanteks Enthoo Pro       |  Full Tower      |

Käyttöjärjestelmä: Windows 11 Pro 23H2

## x) Lue ja tiivistä

The Apache Software Foundation 2023: Apache HTTP Server Version 2.4 Documentation: [Name-based Virtual Host Support](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)<br>
- Name-based virtual hostingissa palvelin luottaa asiakkaan ilmoittamaan isäntänimeen HTTP-otsikoissa
- Name-based virtual hostingissa moni isäntä voi käyttää samaa IP-osoitetta
- Name-based virtual hosting usein yksinkertaisempaa, kuin IP-based Virtual Host
- Tulisi aina käyttää Name-based virtual hostingia, jos suinkin laitteisto ei muuta vaadi
- Historialliset syyt eivät enää päde IP-based Virtual hostingiin<br>

Karvinen 2018: [Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/)<br>
Tätä on vaikea tiivistää, kun on kohtuullisen tiivis sivu jo valmiiksi:
- Yleensä käytössä on yksi IP-osoite ja monta web-sivua hostattavana, Apachen kanssa voi pitää useita verkkotunnuksia yhdellä IP-osoitteella
- Asennetaan apache2 ja konfiguroidaan web palvelin
> $ sudo apt-get -y install apache2<br>
> $ echo "Default"|sudo tee /var/www/html/index.html
- Lisätään uusi Name Based Virtual Host
> $ sudoedit /etc/apache2/sites-available/pyora.example.com.conf<br>
> $ cat /etc/apache2/sites-available/pyora.example.com.conf<br>
> $ sudo a2ensite pyora.example.com
- Uudelleenkäynnistetään eli restartataan apache2
> $ sudo systemctl restart apache2
- Luodaan web sivu normaalikäyttäjänä
> $ mkdir -p /home/xubuntu/publicsites/pyora.example.com/<br>
> $ echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html
- Viimeisenä koeajo
> $ curl -H 'Host: pyora.example.com' localhost<br>
> $ curl localhost

## a) Testaa, että weppipalvelimesi vastaa localhost-osoitteesta. Asenna Apache-weppipalvelin, jos se ei ole jo asennettuna.

*Aloitus 5.9.2024 klo 12:55*<br><br>
Apache2 ehdin asentaa eilen tunnilla<br>
> sudo apt-get install apache2<br>

Laitan seuraavan komennon, jotta apache2 käynnistyy, kun käyttöjärjestelmä käynnistyy:<br>
> sudo systemctl enable --now apache2<br>

![apache2now](images/h31.png)<br>

Seuraavaksi kokeillaan localhostia komentokehotteessa:
> curl http://localhost<br>

Localhost toimii komentokehotteessa:<br>

![toimii](images/h32.png)<br>

Kokeillaan vielä Firefoxilla:<br>

![toimii2](images/h33.png)<br>

Myös Firefoxilla toimii.<br>

*Klo 13:04 valmis, aikaa kului 9min*

## b) Etsi lokista rivit, jotka syntyvät, kun lataat omalta palvelimeltasi yhden sivun. Analysoi rivit (eli selitä yksityiskohtaisesti jokainen kohta ja numero, etsi tarvittaessa lähteitä).

*Aloitus 5.9.2024 klo 13:10*<br><br>
Yritetään etsiä lokista tietoja: "sudo journalctl" ei näytä ainakaan vielä tietoja.<br>

![imagejournalctl](images/h34.png)<br>

Kokeillaan hakea lokit toista kautta:<br>

> sudo tail /var/log/apache2/access.log<br>

"tail" näyttää lokit tiedoston loppupäästä.<br>

![imagevarlog](images/h35.png)
<br>
Lokien analysointi onkin hieman hankalampi asia, yritän lonkalta ensin ja sitten haen tietoa internetistä.<br><br>
Vasemmalla näkyy 127.0.0.1, tämä on localhost IP-osoite.<br>
Päivämäärän jälkeen lukee GET, en tiedä mitä tässä tarkoittanee, paitsi jonkun asian hakemista.<br>
Sen jälkeen /icons/openlogo-75.png, selkeästi joku kuvatiedosto logosta.<br>
"Mozilla/5.0", olisikohan Firefoxin versio, tai tuo "Firefox/115.0".<br>

Arvailut sikseen, aika etsiä tietoa. Kohtalaiset ohjeet löysin täältä: https://www.sumologic.com/blog/apache-access-log/.<br>
Lisäksi käytin myös tätä: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status.<br>
Sekä: https://disjoint.ca/til/2016/05/17/how-to-determine-the-file-size-of-a-remote-http-object/.<br>

Eli ensimmäisenä **127.0.0.1** IP osoite, josta pyyntö tuli.<br>
Tässä välissä itsestäänselvyytenä **päivämäärä**, sekä **aika** ja **aikavyöhyke**.<br>
**GET / HTTP/1.1** pyynnön tyyppi, sekä "GET" ja "HTTP1.1" välissä se mitä pyydetään.<br>
**200** HTTP vastauksen status koodi. "200" viittaa onnistuneeseen pyyntöön. Kyseessä on "GET" tyyppinen pyyntö, jolloin 200 tarkoittaa, että resurssi on onnistuneesti haettu ja toimitettu.<br>
**6040** asiakkaalle palautetun objektin koko byteinä, eli tavuina, tässä tapauksessa 6040 tavua, eli 6040 bytes.<br>
**http://localhost** viittaa osoitteeseen, josta pyyntö on tullut.<br>
**Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0** on ympäristö, josta käyttäjä käyttää kyseistä palvelua. Tässä tapauksessa Mozilla Firefox Linuxilla.<br>

*Valmis klo 13:49, aikaa kului 39min*<br>

Unohdin katsoa vielä "sudo tail /var/log/apache2/error.log":lla virhelokit. Teen sen nyt *6.9.2024 klo 8:30*<br>

![imageapacheerrorlog](images/h36.png)<br>

https://stackify.com/apache-error-log-explained/ täältä löytyy tietoa näistä virhelokeista.<br>
https://httpd.apache.org/docs/2.4/mpm.html myös täältä.<br>
https://httpd.apache.org/docs/current/logs.html<br>

Kuvassa näkyy:<br>
**mpm_event** eli Multi-Processing Modules, "notice" eli ei vakava virhe, mutta lähinnä ilmoitusasia<br>
**pid** eli prosessin id numero<br>
**tid** eli "thread id"<br>
**core** eli ydin<br>

*Valmis klo 8:44 aikaa kului 14min*

## c) Etusivu uusiksi. Tee uusi name based virtual host. Sivun tulee näkyä suoraan palvelimen etusivulla http://localhost/. Sivua pitää pystyä muokkaamaan normaalina käyttäjänä, ilman sudoa. Tee uusi, laita vanhat pois päältä. Uusi sivu on hattu.example.com, ja tämän pitää näkyä: asetustiedoston nimessä, asetustiedoston ServerName-muuttujassa sekä etusivun sisällössä

*Aloitus 6.9.2024 klo 9:01*<br>

Komennolla "echo "hattu.example.com"|sudo tee /var/www/html/index.html" lähdetään päällekirjoittamaan index.html tiedostoa.<br>

![image](images/h37.png)<br>

Lisätään Name Based Virtual Host:<br>

> sudoedit /etc/apache2/sites-available/hattu.example.com.conf<br>

Lisätään tiedot:<br>

![imageconffi](images/h38.png)<br>

Ajetaan "sudo a2ensite hattu.example.com" ja käynnistetään apache2 uudestaan komennolla "sudo systemctl restart apache2"<br>

![imagea2ensite-restart](images/h39.png)<br>

Kokeillaan tehdä web sivu normaalina käyttäjänä:<br>

> mkdir -p /home/xubuntu/publicsites/hattu.example.com/<br>

"Permission denied"<br>
![image-denied](images/h311.png)<br>

Vaihdoin "xubuntu" nimen "santeri":ksi. Nyt kansioiden luonti onnistui. Täytyy vain käydä vielä muokkaamassa .conf tiedostoon xubuntujen tilalle santeri<br>

> sudoedit /etc/apache2/sites-available/hattu.example.com.conf<br>

> echo hattu > /home/santeri/publicsites/hattu.example.com/index.html<br>

Nyt voidaan testata web sivua:<br>

> curl -H 'Host: hattu.example.com' localhost<br>

Ei oikeutta tähän resurssiin<br>

![imagedenied-2](images/h312.png)<br>

Luon uudestaan kansion: "mkdir -p /home/santeri/publicsites/hattu.example.com/" ja varmuudeksi "echo hattu > /home/santeri/publicsites/hattu.example.com/index.html"<br>

Edelleen sama error, ei oikeutta.<br>

Ajoin uudelleen samat komennot tuolta alusta lähtien, nyt "curl -H 'Host: hattu.example.com' localhost", ohjeen "Default" tekstiin kirjoitin "hattu", nyt komento antaa seuraavan:<br>
![imagehattu](images/h313.png)<br>

Ja "curl localhost" seuraavaa:<br>
![image-curl.local](images/h314.png)<br>

*Tauko klo 9:46, aikaa käytetty 45min*<br>
*Jatkuu klo 10:04*<br>

Tuossa tauolta palatessa katsoin seuraavaa videota: https://www.youtube.com/watch?v=1CDxpAzvLKY. Videon innoittamana menin "sudoedit /etc/hosts" ja lisäsin sinne localhostin jälkeen seuraavat rivit:<br>
> 127.0.1.1 hattu<br>
> 127.0.0.1 hattu.example.com<br>

![imagehosts](images/h315.png)<br>


Nyt näyttää toimivan (muutin localhostin Defaultiksi välissä)<br>

![image-toimii](images/h316.png)<br>

![imagefox1](images/h317.png)<br>

![imagefox2](images/h318.png)<br>

*Valmis klo 10:14, aikaa meni yhteensä 55min*

## e) Tee validi HTML5 sivu

*Aloitus klo 10:17*<br>

Tähän tietenkin Karvisen (2012) ohjeet: https://terokarvinen.com/2012/short-html5-page/<br>

Menen kansioon /home/santeri/publicsites/hattu.example.com/ ja sieltä muokkaan "micro edit index.html"<br>

![image-html5](images/h319.png)
<br>
Nyt hattu.example.com näyttää tältä:<br>

![imagefox3](images/h320.png)<br>

*Valmis klo 10:28, aikaa meni 11min*

## f) Anna esimerkit 'curl -I' ja 'curl' -komennoista. Selitä 'curl -I' muutamasta näyttämästä otsakkeesta (response header), mitä ne tarkoittavat

*ALoitus klo 10:30, kahvin keiton lomassa luen lisää curleista*<br>

https://curl.github.io/curl-cheat-sheet/http-sheet.html Kätevä muistikartta "curl":n käytöstä.<br>
https://curl.se/docs/tutorial.html <- Täällä myös hyvin selitetty asioita.<br>

"curl" palauttaa web-palvelimen pääsivun:<br>

![image-curl](images/h321.png)<br>

"curl -I" palauttaa web-sivun yksityiskohtaisia tietoja:<br>

![imagecurl-I](images/h322.png)<br>

**Date** = päivämäärä<br>
**Server** = palvelin, versio ja käyttöjärjestelmä<br>
**Last-Modified** = viimeksi muokattu pvm, klo<br>
**Etag** = Entity Tag, eli kyseisen resurssin identifiointitunnus. Auttaa vähentämään esimerkiksi kuormitusta ja turhia päällekkäisiä päivityksiä: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag<br>
**Accept-Ranges** = kertoo tässä tapauksessa, että range requestit vastaanotetaan tavuina: https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests<br>
**Content-Length** = messagen pituus tavuina: https://reqbin.com/req/curl/b3tqmhxa/post-request-with-content-length-header<br>
**Vary** = en aivan täysin ymmärrä tämän toimintaa, mutta se toimii jonkunlaisena validaattorina: https://www.smashingmagazine.com/2017/11/understanding-vary-header/<br>
**Content-Type** = kertoo minkälaista sisältöä sivu sisältää. Tässä tapauksessa sekä tekstiä, että html:ää.<br>

Kokeilin vielä käydä muuttamassa web-sivun tekstiä, jotta saan ETagin muuttumaan:<br>

![imageetag](images/h323.png)<br>

*Valmis klo 11:09, aikaa kului 39min*<br>

Bonus, "curl -i" palauttaa sekä "curl":n, että "curl -I":n (tätä riviä muokattu myöhemmin keskeneräisen lauseen vuoksi)

![imagecurliii](images/h324.png)


## m) Vapaaehtoinen, suosittelen tekemään: Hanki GitHub Education -paketti

Tämän hankin GitHubin ohjeiden mukaan.

## edit) 12.9.2024

Jälkikäteen tajusin, kuinka kohdan e) HTML5-sivu validoidaan oikein.<br>
Tässä kuvassa HTML-koodia muokattu niin, että validaattori päästää läpi:<br>

![imagevalidator](images/h325.png)<br>

Validaattori löytyy osoitteesta: [https://validator.w3.org/](https://validator.w3.org/).<br>
Välilehdellä "Validate by Direct Input" pystyy syöttämään suoraan HTML-koodin tekstikenttään.


## Lähteet

Fizpatrick, S. 2020. Understanding the Apache Access Log: View, Locate and Analyze. Luettavissa: https://www.sumologic.com/blog/apache-access-log/. Luettu 5.9.2024<br>
Karvinen, T. 2012. Short HTML5 page. Luettavissa: https://terokarvinen.com/2012/short-html5-page/. Luettu 6.9.2024
Karvinen, T. 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 5.9.2024<br>
The Apache Foundation. 2024. Name-based Virtual Host Support. Luettavissa: https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 5.9.2024

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024
