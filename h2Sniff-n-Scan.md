# h2Sniff-n-Scan


## x) Lue/katso ja tiivistä

### Hoikkala "joohoi" 2020: Still Fuzzing Faster (U fool)

 - Kaikki nettisurffaus koostuu HTTP get pyynnöistä
 - Web fuzzer käyttää samoja get pyyntöjä, mutta muokkaa niitä hieman ja lähettää miljoonia pyyntöjä. Fuzzer yrittää näillä tunnistaa poikkeamia palvelinten vastauksissa.
 - Get pyyntöjen fuzzauksessa jokin osa pyynnöstä vaihdetaan "fuzz" termiksi ja fuzzer kokeilee millä kaikilla termeillä saa vastauksen palvelimelta. Tätä yleensä käytetään HTTP get pyyntöjen parametreissä tai headereissa. Myös post pyyntöjen dataa voi fuzzata.
 - Web fuzzeria voi käyttää moniin eri asioihin, kuten palvelimen aladomainien löytämiseen.

### Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide

 - Port Scanning Basics (termistö)
    - Open
       - Sovellus avoimesti hyväksyy TCP yhteyksiä, UDP datagrammeja sekä SCTP assosiaatioita.
       - Avoimien porttien löytäminen skannauksilla on melkein aina skannauksen päämäärä.
       - Avoimien porttien skannaus myös näyttää mitä palveluita verkostossa on.
    - Closed
       - Vaikka portti on suljettu siihen silti voi saada yhteyden, kun palvelin on käynnissä.
       - Suljetut portit eivät ole turhia. Niiden avulla voi tunnistaa mitä käyttöjärjestelmä palvelimella on. Niitä ei myöskään kannata unohtaa, vaan skannata myöhemmin, koska jokin niistä voidaan avata.
       - Suljetut portit kertovat myös siitä, että niitä ei ole estetty palomuurilla. Palomuuri estää suljettujen porttien skannauksen ja suljetun tilan sijaan ne palauttaisivat "filtered" tilan
    - Filtered
       - Skanneri ei voi selvittää onko portti auki vai kiinni, koska skannerin lähettämä paketti katoaa johonkin matkalla, eikä pääse porttiin asti.
       - Yleensä palomuuri estää pakettien pääsemisen portteihin asti.
       - Filtered portit myös hidastavat skannausta paljon, koska nmap yrittää lähettää uusia paketteja portteihin, mistä se ei saa vastausta.

 - Port Scanning Techniques (tekniikat)
    - -sS 
       - sS eli TCP SYN scan on nmapin oletusasetus. Se on myös suosituin hyvästä syystä. Se ei ikinä suorita loppuun TCP 3-way handshakea vaan lopettaa sen aina kakkosvaiheeseen. sS palauttaa aina eritellyn listauksen    porttien tilasta, onko ne avoimia (open), suljettuja (closed) vai filtteröityjä.
    - -sT
       - sT eli TCP connect scan on oletusasetus silloin, kun SYN skannaus, jota käsittelin ylempänä, ei ole vaihtoehto. SYN skannaus ei ole vaihtoehto silloin, kun käyttäjällä ei ole oikeuksia lähettää raakoja paketteja.
       - Tässä skannauksessa nmap käyttää monissa palvelimissa olevaa ohjelmointirajapintaa "Berkley Sockets API" yhteyden saamiseen.
       - Koska nmapin on luotava yhteys ohjelmointirajapintaan, täysi yhteys on luotava porttien skannausta varten. Tämä on ongelmallista, koska skannuksessa kestää pidempään sekä kohde palvelin saattaa luoda lokitiedostoja skannauksesta.
    - -sU
       - sU eli UDP scan skannaa TCP porttien sijasta UDP portteja. Useimmat palvelinten ylläpitäjät keskittyvät vain TCP porttien suojaukseen, koska niitä on nopeampi ja helpompi skannata.
       - UDP portit ovat yleensä huonommin suojattuja, kuin TCP portit.
       - Toisin kuin TCP skannauksessa UDP portit lähettävät harvemmin vastauksia vaikka ne olisivatkin auki, joka hidastaa nmapin vauhtia.

## a) fuff 

Aloitin tehtävän lataamalla haastebinäärin osoitteesta 
https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/208b5d40-43b6-4d6b-8060-be73a8153dca)

Käynnistin harjoitusmaalin. 

### ffuf asennus

Noudatin ohjeita, jotka löytyivät Tero Karvisen sivuilta

```
$ wget https://github.com/ffuf/ffuf/releases/download/v2.0.0/ffuf_2.0.0_linux_amd64.tar.gz
$ tar -xf ffuf_2.0.0_linux_amd64.tar.gz
$ ./ffuf
```

Jouduin myös asentamaan wget komennon

    sudo apt-get install -y wget

fuff tarvitsee vielä sanalistan, jolla kokeilla mahdollisia osumia. 

```
$ wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
```

### Kahden hakemiston löytäminen harjoitusmaalista

Tehtäväni on löytää aiemmin ladatusta dirfutz-1 maalista kaksi hakemistoa. Admin sivu ja versionhallintaan liittyvä sivu.
Ennen ffuf työkalun käyttöä irrotin virtuaalikoneeni internetistä. 

Kokeilin ensiksi ffuf käyttöä

    ./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4169df66-f2da-40bf-ad85-bc12a49af42e)

Komento tulosti paljon tietoa. Tarkastelin muutamaa tulosta curl komennolla. Kaikkien kokeilemieni sivustojen rakenne oli sama, kuin dirfutz-1 pääsivulla. 
Huomasin myös, että kaikkien sivujen koko tavuissa oli 154.

Seuraavaksi tarkensin ffuf hakua filtteröimällä pois kaikki sivut joiden koko on tasan 154. 

    ./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154

-fs tarkentaa hakua filteröimällä HTTP pyynnön mukaan 

Tällä komennolla sain paljon kiinnostavempia vastauksia.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/be5bced6-90e5-4da0-8d40-3cf213769286)

Nyt, kun vastausten määrä on paljon pienempi tarkastelin tuloksia manuaalisesti curl komennolla. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f5b1947a-d5d4-44c9-9d10-0372f73444d5)































