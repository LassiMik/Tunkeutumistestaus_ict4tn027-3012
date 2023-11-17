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
```
sudo apt-get install openjdk-17-jre
```
Karvinen myös suosittelee jonkin palomuurin lataamista koneelle, esimerkissä ufw tulimuuri
```
sudo apt-get install ufw
sudo ufw enable
```
Seuraavaksi ohjeessa ladataan itse webgoat komennolla
```
wget https://github.com/WebGoat/WebGoat/releases/download/v2023.4/webgoat-2023.4.jar
```
Ja viimeiseksi käynnistetään webgoat
```
java -Dfile.encoding=UTF-8 -Dwebgoat.port=8888 -Dwebwolf.port=9090 -jar webgoat-2023.4.jar
```
## a) Totally Legit Sertificate

### zap sekä javan asennus

Aloitin lataamalla zap-proxyn [täältä](https://www.zaproxy.org/download/)
Valisin "cross platform package". 
Paketti tulee zip tiedostona, joten jouduin purkamaan sen vielä unzip komennolla.

Koska zap käyttää javaa latasin myös java runtime enviromentin komennolla
```
sudo apt-get install openjdk-17-jre
```

Avasin zap proxyn komennolla
```
java -jar ZAP_2.14.0/zap-2.14.0.jar
```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0d1a70af-13aa-4b39-894d-3dd11aeef1a5)

### proxyn asentaminen hakukoneeseen

Zap options kohdassa kirjoitin hakuun "network"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/d0e0328e-23b5-4a8d-ab43-e6dd64ecd974)

Tallensin sertifikaatin koneelleni. 

Seuraavaksi menin hakukoneelleni, joka tässä tapauksessa on firefox ja avasin asetukset.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/529dc231-5aa8-42a1-a1a1-8f417ced7f11)

Asetuksissa laitoin "manual proxy configuration" ja syötin portin, jossa zap proxy pyörii.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c55eecc2-d32d-43c0-a572-58145cd4320b)

Jotta zap toimisi vielä lisäsin aikaisemmin tallentamani sertifikaatin firefoxiin, firefoxin asetuksista.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/03bae4a8-7d42-4f9f-8b27-6512b61bed39)

Import ja valitsin sertifikaatin, jonka latasin.

Nyt kaikki pitäisi olla kunnossa. Testasin proxyn toimimista navigoimalla aiemmin ladatun metasploitable 2 virtuaalikoneen 
sivustolle. 
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/963e3bf9-09c6-461d-860b-8c93852804d4)

Zap proxyyn tulostuu tietoa.

## b) Kettumaista. Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxyksi siihen.

Firefoxin oikeasta valikosta, josta asetuksetkin löytyivät valitsin "add-ons and themes" 
Hain "foxyproxy" hakukentästä.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/54b86196-dcb6-4e60-a230-6e00b061850f)

Latasin "FoxyProxy Standard" 
Avasin foxyproxyn asetukset chromen ylävalikosta 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/3c05cc53-baba-4e06-8dcd-6ce773645747)

Lisäsin ZAP-proxyn foxyproxyyn 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c5ba8c47-6832-4dd7-ab42-5fc24f261612)

## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu

### Insecure Direct Object Reference (IDOR)

Tehtävän tarkoitus on etsiä carlos nimisen käyttäjän salasana. Sivustossa on heikkous. Sivusto tallentaa käyttäjän chatti logit suoraan palvelimen tiedostoihin ja hakee ne staattisilla urleilla.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7a136993-3100-406d-92cc-2d946cb14036)

### Path traversal

#### d) File path traversal, simple case

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/198df745-0a5b-4ebc-bef6-ee916e3b4a90)

#### e) File path traversal, traversal sequences blocked with absolute path bypass

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/43fc1cae-e0e7-410b-b929-66604069dfc4)

#### f) File path traversal, traversal sequences stripped non-recursively

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/478cfe29-30d8-4be8-b0fa-598fca289201)

### Server Side Template Injection (SSTI)

#### g) Server-side template injection with information disclosure via user-supplied objects


### Server Side Request Forgery (SSRF)

#### h) Basic SSRF against the local server

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0385d636-aade-4154-8be6-18f19f0d061b)

### Cross Site Scripting (XSS)

#### i) Reflected XSS into HTML context with nothing encoded

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/24314acb-4968-422b-ae8d-9f2b8c6d686a)

#### j) Stored XSS into HTML context with nothing encoded

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/65ea17da-569b-41b9-b712-bbb6674c64de)


## k) Asenna Webgoat 2023.4. (Uusi versio, jossa on eri tehtäviä kuin vanhemmissa)
Latasin webgoatin [Täältä](https://github.com/WebGoat/WebGoat/releases) . Käynnistin webgoatin komennolla 
```
java -jar webgoat-2023.4.jar
```
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e482ca04-143d-4c64-a2b6-605efe64c085)

Kohtasin kuitenkin ongelman käynnistyksessä. Webgoatin default porttinumero on sama, kuin zapissa. Korjasin ongelman vaihtamalla portin.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6be50e10-729b-4386-915a-8b5849eb61fa)

Webgoat toimii nyt.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/26a428bd-86f1-45c9-80f2-e162bab463bb)

### m) (A1) Broken Access Control (WebGoat 2023.4)

#### Hijack a session

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/b5e8b8d9-038a-44d6-ac1b-eafbafed5227)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ed38ab66-8967-4f00-a1c1-518f5c47204c)

Voidaan päätellä, että session id eli ensimmäinen lukusarja ennen "-" on 82 päättyinen.

"-" jälkeinen numerosarja on kinkkisempi. 5 viimeistä numeroa ovat erilaiset ja oikea loppu on lukujen 69892 ja 70036 väliltä.

#### Insecure Direct Object References (4) 

Ensimmäisessä osiossa annettiin valmiiksi käyttäjänimi "tom" ja salasana "cat", joilla piti kirjautua sisään.

##### Toinen osio:

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/eca6768a-13cf-4e3b-9561-876cc9965efa)

View profile nappia painettaessa ZAP nappasi get pyynnön. Get pyynnön vastauksessa on kaksi parametria, joita etsimme.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7a6f1cc8-85d2-4767-82e6-4e2f95b225db)

Role: 3 ja userId: 2342384

##### kolmas osio:

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/cf1b8556-7c9a-4aae-a374-f9960f4d1061)

Tehtävässä pitää syöttää url, joka osoittaa tom cat profiiliin. Kaikki sivuston osoitteet ovat olleet /IDOR/..., joten syötin WebGoat/IDOR/profile/<aiemman tehtävän userId>

##### neljäs osio:

Tehtävänä on käyttää aikaisempien osioiden temppuja ja löytää uusi käyttäjä järjestelmästä userId:n avulla. En osaa käyttää ZAPin fuzzeria tai muuta, joka automaattisesti syöttää numeroita ja korottaa niitä yhdellä. 
Syötin manuaalisesti isompia numeroita ja aloitin aiemmin löydetystä userId:stä. En kerennyt kokeilla montaa kunnes vastaan tuli käyttäjä "Buffalo Bill"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c730164d-9fc3-44d5-acd0-ab33ac32dd5d)

Buffalo Billin userId on 2342388 

#### Missing Function Level Access Control (3)

##### 1:

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c4408a7d-ec41-44de-8f10-7c42cde21385)

Inspect element 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/abaa8bc0-cc09-49ec-92a8-a6fc48da6c22)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6400cecc-c0db-419b-94a1-42f1e94aab33)

ja

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6e52b1bd-2f09-49a3-8129-26f1c6c41e2b)

##### 2:

Vaihdoin aikaisemmassa tehtävässä saadun osoitteen post pyyntöön

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/45fdda02-90e0-4166-b324-b017a0ccf6b6)

Palvelin vastasi "unsupported media type" 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/390eca5b-702e-4e49-91a1-fe93f8207546)

Vaihdoin pyynnön tyypiksi GET sekä content-typeksi "application/json"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/8ee4a2f4-de26-477c-9894-532985869475)

Tässä vielä GET pyyntö 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/18625ffb-71d2-438b-952c-51d26f2cfda7)

En osaa edetä, palvelin antaa 500 internal server erroria jokaiseen yritykseen.


#### Spoofing an Authentication Cookie (1)

Aloitin kirjautumalla annetuilla tunnuksilla "admin" "admin"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4b23fa34-5cbd-47bd-b6c0-4641c1e78838)

Syötin koodisarjan [base64 decoderiin](https://www.base64decode.org/)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7232b782-f2db-493a-9759-03906cd043c4)

base64 decoder palauttaa hexadesimaali arvoja 

Käytin [hexadesimal decoderia](https://cryptii.com/pipes/hex-decoder)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/47ef0c79-7387-45f8-9abe-93ea1d8db392)

tekstin lopussa admin lukee väärinpäin

Vaihdoin adminin tilalle "tom" ja toistin saman koodaus ketjun toisin päin. Lisäsin POST pyyntöön uuden spoof_auth cookien

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/dbf16219-9fe3-4079-9d33-b7a38fe6a2d6)

### n) (A7) Identity & Auth Failure (WebGoat 2023.4)

#### Authentication Bypasses (1)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/274ebe3e-7bbc-496c-8a9b-9b58ac9ed7ce)

post pyyntö jonka bodyssä "secQuestion0" ja "secQuestion1"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/be363684-ab42-4fab-8cd1-7ef584aedbef)

sec questionit vaihdettu "secQuestionA" ja "secQuestionB"

### Insecure Login (1)


## Lähteet 

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/
 - OWASP Broken access control https://owasp.org/Top10/A01_2021-Broken_Access_Control/
 - OWASP Server-Side Request Forgery (SSRF) https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/
 - PortSwigger Access control vulnerabilities and privilege escalation https://portswigger.net/web-security/access-control
 - PortSwigger Server-side template injection https://portswigger.net/web-security/server-side-template-injection
 - PortSwigger Cross-site scripting https://portswigger.net/web-security/cross-site-scripting
 - Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/
 - base64 decoder https://www.base64decode.org/
 - hex decoder https://cryptii.com/pipes/hex-decoder
 - Webgoat tehtävissä käytetty Max Integrations youtubekanavan videoita apuna https://www.youtube.com/@MaxInteg


