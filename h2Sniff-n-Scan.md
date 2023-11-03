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
     
