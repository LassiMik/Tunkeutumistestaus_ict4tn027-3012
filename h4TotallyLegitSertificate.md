# h4 Totally Legit Sertificate

## x) Lue/katso ja tiivistä.

### OWASP 2021: OWASP Top 10:2021

OWASP eli Open Worldwire Application Security Project on voittoa tavoittelematon järjestö,
joka tuottaa vapaasti saatavilla olevaa tietoa verkkosovellusten tietoturvasta.

#### A01:2021 – Broken Access Control

 - Access control tarkoittaa, että vain oikeutetut käyttäjät voivat käyttää rajattuja toimintoja nettisivulla, esim admin.
 - Jos access controllissa on aukkoja, tunnistamattomat käyttäjät voivat käyttää näitä samoja toimintoja haitallisesti.
 - Yleisiä heikkouksia access controllissa on paljon.
 - Access control on tehokasta vain luotetussa palvelinpuolen koodissa tai palvelimettomassa API:ssa,
jossa hyökkääjä ei voi muokata access control tarkastusta tai metatietoja.

#### A10:2021 – Server-Side Request Forgery (SSRF)

 - SSRF-virheitä ilmenee aina, kun verkkosovellus hakee etäresurssia vahvistamatta käyttäjän antamaa URL-osoitetta
 - Sen avulla hyökkääjä voi pakottaa sovelluksen lähettämään muotoillun pyynnön odottamattomaan kohteeseen
 - Hyökkääjät voivat käyttää SSRF:ää hyökätäkseen verkkosovellusten palomuurien, palomuurien tai verkon ACL-luetteloiden takana suojattuihin järjestelmiin.
 - Hyökkääjät voivat käyttää paikallisia tiedostoja tai sisäisiä palveluita saadakseen arkaluontoisia tietoja, kuten file:///etc/passwd ja http://localhost:28017/

### PortSwigger academy

#### Access control vulnerabilities and privilege escalation
 - Access control on rajoitusten soveltamista sille, kuka tai mikä on valtuutettu suorittamaan toimintoja tai käyttämään resursseja. Verkkosovellusten yhteydessä access control on riippuvainen todennuksesta ja istunnonhallinnasta.
 - Käyttäjän todennus: Varmistaa, että käyttäjä on kuka sanoo olevansa.
 - Istunnonhallinta: tunnistaa mitkä peräkkäiset HTTP pyynnöt ovat saman käyttäjän tekemiä.
 - Access control: määrittää saako käyttäjä tehdä mitä on yrittämässä tehdä.

#### Server-side template injection

 - Template enginet on suunniteltu luomaan verkkosivuja yhdistämällä kiinteät mallit volatiilisiin tietoihin.
 - Server-side template injection on, kun hyökkääjä pystyy käyttämään alkuperäistä mallisyntaksia lisäämään haitallisen hyötykuorman malliin, joka sitten suoritetaan palvelinpuolella.
 - Server-side template injection voi tapahtua, kun käyttäjän syöte viedään suoraan templateen sen sijaan, että se välitettäisiin tietoina.

#### Server-side request forgery (SSRF)

 - Server-side request forgery on verkkotietoturvahaavoittuvuus, jonka avulla hyökkääjä voi saada palvelinpuolen sovelluksen tekemään pyyntöjä tahattomaan paikkaan.
 - Tyypillisessä SSRF-hyökkäyksessä hyökkääjä saattaa saada palvelimen muodostamaan yhteyden sisäisiin palveluihin organisaation infrastruktuurissa.
 - Muissa tapauksissa he saattavat pystyä pakottamaan palvelimen muodostamaan yhteyden hyökkääjän valitsemiin ulkoisiin järjestelmiin. Tämä voi vuotaa arkaluonteisia tietoja, kuten käyttäjätietoja.
 - SSRF-hyökkäykset käyttävät usein hyväkseen luottamussuhteita eskaloidakseen hyökkäyksen haavoittuvasta sovelluksesta ja suorittaakseen luvattomia toimia

#### Cross-site scripting

 - Cross-site scripting, joka tunnetaan myös nimellä XSS on nettisivujen tietoturvahaavoittuvuus, jonka avulla hyökkääjä voi vaarantaa käyttäjien vuorovaikutuksen haavoittuvan sovelluksen kanssa
 - Cross-site scripting heikkoudet sallivat yleensä hyökkääjän naamioitua toiseksi käyttäjäksi, suorittaa mitä tahansa toimintoja, joita käyttäjä voi suorittaa, ja päästä käsiksi mihin tahansa käyttäjän tietoihin.
 - Jos kyseisellä käyttäjällä on etuoikeutettu käyttöoikeus sovellukseen, hyökkääjä saattaa saada täyden hallinnan sovelluksen kaikista toiminnoista ja tiedoista.

### Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking

 - Webgoat on web tunkeutumistestauksen harjoitteluun suunniteltu sivu. Webgoattiin on tahallaan haudattu haavoittuvuuksia.
 - Webgoat on java pohjainen sivu
 - Sivusto vaatii toimivan java runtime enviromentin pyöriäkseen. Javan voi ladata komennolla

       sudo apt-get install openjdk-17-jre

Karvinen myös suosittelee jonkin palomuurin lataamista koneelle, esimerkissä ufw tulimuuri
```
sudo apt-get install ufw
sudo ufw enable
```
## Lähteet 

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/
 - OWASP Broken access control https://owasp.org/Top10/A01_2021-Broken_Access_Control/
 - OWASP Server-Side Request Forgery (SSRF) https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/
 - PortSwigger Access control vulnerabilities and privilege escalation https://portswigger.net/web-security/access-control
 - PortSwigger Server-side template injection https://portswigger.net/web-security/server-side-template-injection
 - PortSwigger Cross-site scripting https://portswigger.net/web-security/cross-site-scripting


