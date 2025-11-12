# h4 Wifi

#### Oma host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3200MHz  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF       | ATX 80 Plus      |
| Kotelo   | Phanteks Enthoo Pro       |  Full Tower      |

- Käyttöjärjestelmä: Windows 11 Pro 25H2
- VMware® Workstation Pro 25H2 25.0.0.24995812
- WiFiChallenge Lab v2.3 Debian GNU/Linux 12 (bookworm)

### a) Tutustu [wifi challenge lab 2.1](https://lab.wifichallenge.com/) harjoitus ympäristöön ja käytä tarvittaessa hyväksesi jo olemassa olevia ohjeita.

Latasin **VMWaren** [https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion):stä ja **VMware v2.3 Direct ZIP**:n [https://lab.wifichallenge.com/README](https://lab.wifichallenge.com/README):stä harjoituksia varten. Luin ohjeet samaisesta README:sta.

Itse haasteet löytyvät sivulta: [https://lab.wifichallenge.com/challenges](https://lab.wifichallenge.com/challenges).

Piti katsella ohjeita jonkun verran: [https://www.aircrack-ng.org/doku.php?id=airmon-ng](https://www.aircrack-ng.org/doku.php?id=airmon-ng).

Joitakin hyödyllisiä komentoja tehtävissä:

```bash
sudo cat "/esimerkki/kansio.txt"
sudo airmon-ng #listaa langattomat interfacet
sudo airmon-ng check #näyttää prosessit, jotka voi häiritä prosessia
sudo airmon-ng check kill #lopettaa häiritsevät prosessit
sudo airmon-ng start wlan1 #monitor mode päälle
sudo airmon-ng stop wlan1 #pois päältä
sudo airodump-ng wlan1 -w scan --manufacturer #kuunnellaan kanavia

sudo iwconfig #näyttää langattomien interfacejen statuksen, tällä löytyi esimerkiksi tieto, että wlan60 käyttää 5GHz taajuutta
```


### b) Kirjoita raportti siitä mitä opit ja mitkä asiat yllättivät sinut kun tutustuit harjoitukseen.

**Opin**:

- Käyttämään **Aircrack-ng**:tä [https://www.aircrack-ng.org/doku.php?id=Main](https://www.aircrack-ng.org/doku.php?id=Main]


**Yllätykset**:



### c) Miten suhtautumisesi WLanin turvallisuuteen muuttui sen jälkeen kun teit harjoitukset?











### Lähteet

[WiFi Challenge Lab](https://lab.wifichallenge.com/)

[https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)

[https://lab.wifichallenge.com/README](https://lab.wifichallenge.com/README)

[https://lab.wifichallenge.com/challenges](https://lab.wifichallenge.com/challenges)

[https://www.aircrack-ng.org/doku.php?id=Main](https://www.aircrack-ng.org/doku.php?id=Main]

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html

Pohjana Tero Karvinen & Lari-Iso Anttila 2025: Verkkoon tunkeutuminen ja tiedustelu

Kirjoittanut: <em>Santeri Vauramo</em> 2025
