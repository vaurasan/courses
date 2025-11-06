Santeri Vauramo 2024<br><br>


# Linuxin asentaminen virtuaalikoneeseen

Tässä osiossa kerron Linuxin asentamisesta virtuaalikoneeseen. En ole aiemmin tehnyt vastaavaa, paitsi asentanut Nindento 64 emulaattorin Windows PC:lle, mutta siinä ei ollut mielestäni mitään ihmeellistä, joten käyn hyvin tarkasti läpi Tero Karvisen ohjeita joka vaiheessa.
<br>
Oma host kokoonpanoni on seuraavanlainen:<br>

(huom. kokeilin ensin Teron tapaa tuoda kuvia tänne, mutta kun se ei toiminut aloin oikomaan kuvakaappaustyökalulla ctrl+c, ctrl+v:tä hyväksi käyttäen, tällaiselle aloittelijalle huomattavasti nopeampi tapa, mutta toki hyvä olisi osata molemmat)<br><br>
Tässä vaiheessa huomaan, että ainakaan minulla nämä kuvat eivät toimi vaikka pitäisi kaiken järjen mukaan. En kuitenkaan siihen tuhlaa enempiä aikoja vaan kirjoitan tietokoneeni specsit tähän tekstinä:(myöhemmin huomasin, että kaikki kuvat toimii, mutta jätän tekstit tähän kuitenkin varmuuden vuoksi)<br>

Suoritin	AMD Ryzen 9 5900X 12-Core Processor 3.70 GHz<br>
Asennettu RAM	32,0 Gt<br>
Järjestelmätyyppi	64-bittinen käyttöjärjestelmä, x64-suoritin<br>
Versio	Windows 11 Pro<br>
Versio	23H2<br>
Käyttöjärjestelmän koontiversio	22631.4037<br>
Käyttökokemus	Windows Feature Experience Pack 1000.22700.1027.0<br><br>


### Debian

Aloitan lataamalla [debian-live-12.6.0-amd64-xfce.iso] tiedoston (https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-12.6.0-amd64-xfce.iso) omalle koneelleni h1 työkansioon. Tämänhetkinen uusin versio on 12.6.0.<br><br>

### Virtuaalikone

Seuraavaksi lataan Virtualbox virtuaalikoneen osoitteesta: https://www.virtualbox.org/wiki/Downloads.<br>
Alan asentaa virtuaalikonetta asennusohjelmalla, valittuani kohdekansion, asennusohjelma varoittaa, että nettiyhteys katkeaa hetkeksi joten tallennan tämän kirjoitelman varmuuden vuoksi.<br><br>
![virtuaaliasennus1](images/h1a1.png)<br><br>

Varoituksesta huolimatta, pysyn rauhallisena ja jatkan asennusta.<br>
Vaan enpä jatkakaan, kun seuraava herja ilmestyy ruudulle:<br><br>
![image2](images/h1a2.png)<br><br>

Pikainen Googlaus, tai pikemminkin Duckaus tuo minut tälle sivustolle: https://www.sysnettechsolutions.com/en/fix-python-win32api-virtualbox/<br>
Ohjetta noudattaen, menen python.org/downloads sivulle, josta lataan uusimman Python version 3.12.5 ja suoritan asennustiedoston järjestelmänvalvojana.<br>
Tämän jälkeen step 3:n mukaan avaan PowerShellin järjestelmänvalvojana ja kirjoitan komentoriville: py -m pip install pywin32.<br><br>
Seuraavanlainen virhe tapahtuu: ![pywinerror](images/h1a3.png)<br><br>

Kokeilen poistaa ja uudelleenasentaa Pythonin koneeltani samaisella asennusohjelmalla, jonka juuri latasin. Nyt uudestaan PowerShelliin ja sama litania komentoriville, ja homma onnistui.<br><br>
![image](images/h1a4.png)<br><br>

Tässä vaiheessa vaaditaan tietokoneen uudelleenkäynnistys, joten teen sen.<br>
Nyt takaisin PowerShelliin järjestelmänvalvojana ja komentoriville ohjeen mukaan: python.exe -m pip install --upgrade pip.<br>
Tulee virheilmoitus:<br><br>
![image](images/h1a5.png)<br>
Tästä huolimatta yritän nyt asentaa Virtualboxia ja hämmästyksekseni ei enää tule samaa herjaa mikä tuli aiemmin, asennus menee läpi muitta mutkitta.<br>

Seuraavaksi virtuaalikoneen luontiin.<br>
Teron ohjeiden mukaan ylävalikosta Machine - New, aukeaa luonti-ikkuna, valitaan Expert Mode ja aletaan syöttämään haluttuja tietoja:<br>
Jostain syystä en pysty laittamaan täppää kohtaan Skip Unattended Installation, pakko mennä näillä korteilla mitkä on jaettu.<br><br>
![image](images/h1a6.png)<br><br>

Ohjeen mukaan Hardwareen Base Memory 4000MB. Sitten Create Virtual Hard Disk Now ja laitetaan Size 60GB. Valitaan VDI (VirtualBox Disk Image) ja painetaan Finish. Nyt näkyy vasemmalla luotu virtuaalikone offline-tilassa.<br><br>
![image](images/h1a7.png)
<br>

### Linuxin asentaminen juuri luotuun virtuaalikoneeseen

- Valitaan virtuaalikone aktiiviseksi vasemmalta ja mennään Settings.<br>
- Storage välilehti auki, Controller: IDE-kohdasta valitaan CDROM Empty.<br>
- Optical Drive kohtaan haetaan CD-levyn kuvaketta klikkaamalla Virtual Optical Disk File ja haetaan aiemmin ladattu debian-live-12.6.0-amd64-xfce Linuxin  asennusohjelma ja painetaan Choose. Nyt meillä on virtuaalikone ja virtuaalinen CD syötetty koneeseen sisälle.<br>
- Tuplaklikataan virtuaalikonetta, nyt virtuaalikoneeseen muuttui tila Offlinesta -> Running, sekä Boot menu aukesi erilliseen ikkunaan. Tämä näyttää hieman erilaiselta kuin ohjeessa, mutta näillä mennään.<br><br>
![image](images/h1a8.png)<br>
- Live Systemin kohdalla painoin Enteriä ja odottelin hetken. Nyt avautui Linux työpöytä asentamatta Linuxia, varsin merkillistä.<br>
- Testataan toimivuus, Applications menusta Web Browser ja kokeillaan nettisivua, näyttää toimivan:<br><br>
![image](images/h1a9.png)<br>
Kuva: https://www.mtv.fi/<br>
- Homma pelittää, joten aletaan asentamaan Linuxia, valitaan Install Debian työpöydältä.<br>
- Asennuskieleksi American English<br>
- Location Finland Helsinki<br>
- Keyboard Model, Generic 105-key PC, Finnish Default ja testataan laatikossa näppäimistön toimivuus.<br>
- Partitions välilehdeltä valitaan Erase Disk, jätetään tsekkaamatta Encrypt system koska kyseessä on viruaalikone, muuten ehdottomasti kannattava täppä. Boot loader location jätetään oletusarvoonsa eli "Master Boot Record...", muuten ei homma pelitä jostain syystä.<br>
- Users välilehdelle oma nimi normaalisti, käyttäjätunnus käyttäjätunnus käyttäen ASCII kirjaimia ja aloitus pienellä kirjaimella, tietokoneen nimeksi jokin tunnistamaton ei oma nimi - tähä laitoin "taulu", seuraavaksi laitetaan vahva salasana salasanakenttään. Jätetään pois täppä kohdasta Log in automatically...<br>
- Painetaan Next ja asennus alkaa.<br>
<br>
Muutaman minuutin odottelun jälkeen asennus valmistui.<br>

### Asennuksen jälkeen

Pääsin kirjautumisikkunaan, syötin valitsemani käyttäjätunnuksen ja salasanan ja nyt olen työpöydällä. Web-selaimella taas testi, kaikki näyttää toimivan, olen siis onnistuneesti asentanut Debianin, JES!<br><br>

Nyt mennään ohjeiden mukaan superkäyttäjän oikeuksin päivittelemään Debian, Applications: Terminal Emulatorista komennolla: sudo apt-get update<br>
Seuraavaksi päivitetään ohjelmat komennolla: sudo apt-get -y dist-upgrade<br><br>
Sitten asennetaan palomuuri: sudo apt-get -y install ufw, ja laitetaan se päälle: sudo ufw enable ![image](images/h1a10.png)
<br><br>
Bootataan virtuaalikone, Log Out -> Restart<br>
Homma sitä myöten valmis.<br>
Note to self: epähuomiossa menin Scaled modeen mikä näytti aivan järkyttävältä, tästä pääsin eroon painamalla oikea Ctrl + c.
<br><br>
Vielä lopuksi haluan Debianin toimimaan järkevämmällä resoluutiolla, joten menen Teron ohjeiden mukaan laittamaan asiat kuntoon VirtualBoxin lisäosalla. Syötetään kuvitteellinen CD levy asemaan menemällä Devices -> Insert Guest Additions CD image -> VBox_GAs... -> avatan Terminal Emulator ja mennään media/*käyttäjä*/VBox -> komento: sudo bash VBoxLinuxAdditions.run<br>
Tämän jälkeen ruudulla tapahtuu asioita, bootataan virtuaalikone. Nyt on parempi resoluutio ja kaikki pelittää ja pystyn Devices välilehdeltä ottamaan Shared Clipboardista bidirectional asetuksen käyttöön ja voin copy pastettaa hostin ja virtuaalikoneen välillä tekstiä!<br><br>

![valmis](images/h1a11.png)

## Lähteet

CDImage.debian.org. Debian download. Luettavissa: https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/. Luettu 23.8.2024<br>
Karvinen, T. 2021. Install Debian on Virtualbox - Updated 2023. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu 23.8.2024<br>
Karvinen, T. Linux Palvelimet 2024 alkusyksy. Luettavissa https://terokarvinen.com/linux-palvelimet/. Luettu 23.8.2024<br>
stackoverflow. Shortcut to exit scale mode in VirtualBox. Luettavissa: https://stackoverflow.com/questions/10716899/shortcut-to-exit-scale-mode-in-virtualbox#10716934. Luettu 23.8.2024<br>
Sysnettech. 2016-2024. How to Fix Missing Dependencies Python Core / win32api in VirtualBox. Luettavissa: https://www.sysnettechsolutions.com/en/fix-python-win32api-virtualbox/. Luettu 23.8.2024<br>
VirtualBox. 2023. Download VirtualBox. Luettavissa: https://www.virtualbox.org/wiki/Downloads. Luettu 23.8.2024<br><br>

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com
