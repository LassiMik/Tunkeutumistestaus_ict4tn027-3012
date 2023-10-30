# h1hackerwarmup

## x ) lue ja tiivistä

### Santos et al the art of hacking
 - Tiedustelu on onnistuneen tunkeutumisen tärkein osa. Tiedustelun tarkoitus on kartoittaa hyökättävän kohteen heikkoudet.

### Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains
 - Kyber tappoketju on systemaattinen prosessi, jonka avulla vihollinen kohdistaa ja saa aikaan haluttuja vaikutuksia.
 - Kyber tappoketju koostuu seuraavista seitsemästä vaiheesta:
   - Tiedustelu
     - Kohteen tutkiminen sekä mahdollisten heikkousten löytäminen. Tähän aiheeseen kuuluu esimerkiksi kohteen työntekijöiden sähköpostiosotteiden saaminen.
   - Aseistaminen
     - Etäkäyttötroijalaisen sekä aiemmassa vaiheessa huomatun heikkouden yhdistäminen käytettäväksi hyökkäysmenetelmäksi.
   - Jakelu
     - Hyökkäysmenetelmän siirto kohdeympäristöön. Esimerkkejä siirtomenetelmistä ovat sähköpostin liitteet, web sivustot sekä usb-tikut
   - Hyväksikäyttö
     - Kun ase on toimitettu kohdejärjestelmään, ase laukaisee tunkeilijan koodin. Useimmiten hyväksikäyttö kohdistuu sovelluksen tai käyttöjärjestelmän haavoittuvuuteen.
   - Asennus
     - Etäkäyttötroijalaisen tai takaoven asentaminen uhrijärjestelmään mahdollistaa hyökkääjän säilyttää pääsy uhrijärjestelmään.
   - Komento ja ohjaus
   - Tavoitteisiin liittyvät toimet
     - Vasta nyt, kuuden ensimmäisen vaiheen jälkeen, tunkeilijat voivat ryhtyä toimiin alkuperäisten tavoitteidensa saavuttamiseksi.



## a) Over The Wire: Bandit kolme ensimmäistä tasoa (0-2)

Tehtävän alussa sivusto kertoo helpot ohjeet miten päästä eteenpäin tehtävissä, jos jää jumiin. Sivusto muistuttaa esimerkiksi man komennon käytöstä, joka kertoo jokaisen komennon käyttötavan. 

### Tehtävä 0

Tehtävässä pitää opetella käyttämään SSH komentoa ja ottaa etäyhteys komennon avulla tehtäväsivulle. Tehtävän annossa annetaan nettiosoite, johon yhteys otetaan sekä käyttäjätunnus ja salasana.

Käytin SSH komentoa etäyhteyden saamiseen.

        ssh bandit0@bandit.labs.overthewire.org -p 2220

ssh komennossa laitoin annetun käyttäjätunnuksen @ palvelimen osoite ja -p tarkentaakseni portin johon yhdistän.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f93fbbac-cd6f-46ac-a1ae-e5080f17d2c0)

Pääsin palvelimelle.

## tehtävä 1 

Tämän tehtävän tavoitteena on tarkastella ls komennolla bandit0 käyttäjän tiedostohakemistoa. Tiedostohakemiston readme tiedostosta löytyy salasana, jolla kirjaudutaan bandit1 käyttäjälle.
Aloitin tehtävän komennolla ls, jonka avulla löysin readme tiedoston. Seuraavaksi tarkastelin readme tiedoston sisältöä cat komennolla.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/fe98081f-8381-437e-aec2-9363914c9cd7)

readme tiedoston sisältä löysin bandit1 salasanan, jolla kirjaudun seuraavalle käyttäjälle. Kopioin salasanan leikepöydälle.

Kirjauduin bandit1 käyttäjälle

        ssh bandit1@bandit.labs.overthewire.org -p 2220

Ja syötin löytämäni salasanan. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6b276eab-0d2e-4b68-bdf4-0a450eeada39)

Pääsin käyttäjälle.

## tehtävä 2

Päästäkseni seuraavalle tasolle, pitää bandit1 kotihakemistosta lukea bandit2 salasana, joka on "-" nimisessä tiedostossa. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/2a202016-3b04-451b-8148-437ed32facf5)

Yritin avata tiedoston cat komennolla, mutta sain virheilmoituksen. Tiedoston nimi aiheuttaa ongelmia cat komennon kanssa.

Jouduin googlettamaan, miten tiedosto avataan. Pienen googlettelun jälkeen sain tiedoston auki.

        cat ./-

Avattaessa tiedostoja, joiden nimet aiheuttavat konflikteja cat komennon kanssa pitää avata tarkentamalla tiedoston sijainti, eikä pelkällä nimellä.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c96ec6ce-a461-45ba-8ac4-d9a40ce35df9)

Sain luettua tiedoston sisällön.

## b) Challenge.fi 2021 yhden tehtävän ratkaisu

Valitsin tehtäväksi "Hack weblogin part 1" nimisen tehtävän.

Aloitin tehtävän käymällä läpi kaikkien saatavilla olevien sivustojen lähdekoodin. Lähdekoodista en huomannut mitään heikkouksia. Seuraavaksi kokeilin "forgot your password" sivua ja yritin palauttaa käyttäjän "admin" salasanan. Sivu ei antanut erroreita ja sain selville käyttäjänimen lisäksi myös käyttäjänimeen sidotun sähköpostiosoitteen "admin@hackd.example"

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/cc0d156d-6a5d-4ed7-8adb-da93ae1bc8b4)

Jäin jumiin tähän kohtaan. Kokeilin tarkastella palautussivun lähdekoodia, jossa ei ollut oikeastaan mitään, koska sivu on staattinen nettisivu pelkällä yläpuolella olevalla tekstillä. Kokeilin myös analysoida kirjautumisen sekä salasanan palautuksen verkkoliikennettä wiresharkilla, mutta myöskin tuloksetta.

En tiedä miten päästä tehtävästä eteenpäin. Yritin etsiä netistä tietoa, mutta kaikki vastaukset oli luokkaa "tämä on laitonta, älä edes kokeile". Yritin selvittää mm. miten php skriptejä voi katsella nettisivun kautta ilman admin oikeuksia. 

## c) PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

Tehtävän esittelyssä annettiin tieto, että sivustolla on SQL injektio heikkous. Kategorioita valitessa sivusto käyttää seuraavaa SQL hakua

    SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Aloitin tehtävän tekemisen käymällä kaikki kategoriat läpi. Huomasin, että selaimen hakukenttään tulostui suoraan kategorian filtterin tiedot. Pystyin myös vaihtamaan filttereitä ilman, että painoin linkkejä sivulta, vaan vaihdoin hakukentän tietoja. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/98cb8a2d-3c6b-417c-ba7c-75c71d3d4b99)

Tämän huomattuani, kokeilin ensimmäiseksi kirjoittaa hakukenttään: 
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/454e45ff-a092-47d8-ab7d-f47260a159f5)

Kun tämä ei toiminut luin portswiggerin dokumenttia sql injektioista, josta löysin suoraan vastauksen tehtävään. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7c6651eb-3823-4f36-9703-f6e74454e73f)

OR 1=1 hakukentässä palauttaa aina arvon "true", joten SQL näyttää kaikki tuotteet kategoriassa. Hakukentän lopussa olevat "--" kommeotoi loppu SQL haun pois. Eli tässä tapauksessa released=1 haun. 


## d) linuxin asennus

Olen asentanut virtuaalikoneen jo aikaisemmalla "linux -palvelimet" kurssilla. Käytän virtuaalikoneeen käynnistämiseen oraclen VM virtualbox manageria. Virtuaalikoneella minulla on debian 12 käyttöjärjestelmä.

## e) porttien skannaus

Tässä osiossa asennan nmap nimisen työkalun, jota käytän porttien skannaamiseen. 

    sudo apt-get install nmap

selvitin myös oman virtuaalikoneeni ip-osoitteen 

    hostname -I

Seuraavaksi kokeilen porttiskannata oman virtuaalikoneeni

    sudo nmap 10.0.2.15

nmap komentoa voi käyttää vain sudo oikeuksilla. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/81b282b7-5f68-478a-affa-543a2fd17db2)

Pelkkä nmap komenento skannaa vain kohteen 1000 tavallisinta tcp porttia. Virtuaalikoneellani 999 niistä on kiinni ja yksi auki.
Koneella on auki tcp-portti 80 ja service kohdalla lukee "http" (Hypertext Transfer Protocol).

Virtuaalikone käyttää porttia 80 internettiin yhdistämistä varten.

## f) kaikkien porttien skannaus

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c6a2c92e-dac1-4673-805d-e6bed7140e89)

Skannasin kaikki virtuaalikoneeni portit, skannaus antaa saman lopputuloksen, eli portin 80. Loput 65535 ovat auki. 
Skannaus myös kesti huomattavasti aikaisempaa skannausta pidempään. Aikaisempi skannaus, jossa katsottiin vain 1000 porttia kesti 0.03 sekuntia,
kun taas 65535 portin skannauksessa kesti 0.62 sekuntia.
Muun kuin oman koneen skannauksessa saattaa kestää huomattaviakin aikoja.

## g) laaja porttiskannaus 

Selvitin ensin mitä tehtävänanossa annettu komento nmap -A tekee tarkastelemalla nmapin man sivua.

    man nmap

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/23e7ec59-743a-4c3f-b5a0-6ce55646a2e8)

Kun tiesin mitä komento nmap -A tekee, käytin sitä omaan localhostiini

    sudo nmap -A 10.0.2.15

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7502739a-8753-444e-8c4c-e549f2cb7f68)

-A laajentaa skannausta selvittämään myös kohteen käyttöjärjestelmän, omassa tapauksessani linux 2.6.32. -A myös näyttää, että tcp-portissa 80 pyörii apachen verkkopalvelin, jonka versio on 2.4.54. Skannaus selvitti myös apache verkkopalvelimen verkkosivun headerin sekä titlen. Viimeiseksi skannaus myös näyttää network distance (DS) eli network hop distance. Joka käytännössä tarkoittaa kuinka kaukana kohde on omasta koneesta, millä skannaus tehtiin. Omassa tapauksessani network distance on 0 eli localhost. Yhteyden ei tarvinnut edetä yhtäkään "hoppia". Kun taas jos skannaisin koneen, joka on ethernet yhteyden päässä, "hops" olisi 1. 

## i) Avointen lähteiden tiedusteluun sopiva weppisivu: crt.sh 

crt.sh on nettisivu, joka näyttää https (Hypertext Transfer Protocol Secure) sertifikaatin saaneiden nettisivujen kaikki alidomainit ja myös niiden osoitteet. Crt.sh työkalu on siitä käytännöllinen, että sivustojen kehittäjät saattavat tehdä saman osoitteen alle alidomainin, jota käyttävät eri ominaisuuksien testaamiseen. Yleensä testisivustojen tietoturvallisuuteen käytetään vähemmän huomiota, kuin varsinaisen nettisivun turvallisuuteen. 

crt.sh sivustolla voi hakea tiedusteltavan kohteen nimellä. Esimerkiksi, jos hakuun kirjoittaa "google", sieltä näkyy kaikki googlen alidomainen osoitteet. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ea2c7fe0-323b-495c-87c2-db9a65900c48)


## lähteet

 - miten avata tiedosto, jonka nimi on "-" https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal

 - SQL injektiot https://portswigger.net/web-security/sql-injection#retrieving-hidden-data

 - Miten skannata nmapilla kaikki portit https://www.techtarget.com/searchsecurity/feature/How-to-use-Nmap-to-scan-for-open-ports

 - nmap man sivu

 - selitystä nmap tulosteesta https://nmap.org/book/osdetect-fingerprint-format.html
