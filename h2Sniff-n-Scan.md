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

Aloitin tehtävän lataamalla haastebinäärin Tero Karvisen sivustolta 

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

Löysin ensimmäisen maalihakemiston. Versionhallintasivun. 

Seuraavaksi etsin admin sivun. ffuf tulosteessa oli vain yksi admin tulos ja kokeilin sitä

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e08a410b-ba1a-4c4d-bc45-2f7402adb550)

Toinenkin maali löytyi.

## b) Fuffme

Tätä tehtävää varten asensin paikallisen harjoitusmaalin Tero Karvinen sivuston ohjeiden avulla

```
$ sudo apt-get update
$ sudo apt-get install docker.io git ffuf
$ git clone https://github.com/adamtlangley/ffufme
$ cd ffufme/
$ sudo docker build -t ffufme .
```

Latasin vielä tehtävään liitetyt hakusanatiedostot ffuf varten

```
$ mkdir $HOME/wordlists
$ cd $HOME/wordlists
$ wget http://ffuf.me/wordlist/common.txt
$ wget http://ffuf.me/wordlist/parameters.txt 
$ wget http://ffuf.me/wordlist/subdomains.txt
```
Irrotin vielä koneeni internetistä tehtävän ajaksi.

### Basic content discovery 

Ensimmäinen tehtävä on vain kokeilu, että ffuf toimii kuin pitäisikin. Tehtävässä annetaan valmiiksi komento ja tehtävässä pitää löytää kaksi tiedostoa. class ja development.log 

    ffuf -w $HOME/wordlists/common.txt -u http://localhost/cd/basic/FUZZ

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/38f4f327-190c-47ef-92cc-04f75170fc39)

### Content Discovery With Recursion

Seuraavassa tehtävässä opetellaan -recursion komento. Komento käskee hakua tehdä aina uusi haku jos ensimmäinen haku löytää kansion. -recurision skannaa kaikki tiedostot, mitä eteen tulee. Oli ne kuinka syvällä. 
Myös tässä tehtävässä annetaan valmiiksi komento.

    ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/basic/FUZZ

Tämäkin komento toimi, kuten pitikin. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/54984743-0783-4ae2-a371-1fcb24602542)

### Content Discovery With File Extensions

Seuraavassa tehtävässä käytetään komentoa 

    ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ

-e komennolla tarkennetaan mitä tiedostoja etsitään, tässä tapauksessa .log tiedostoja

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/eb0779e1-91d9-41f2-b63f-3005325d573e)

### No 404 Status

Tehtävässä opetetaan -fs komento. En selitä tätä komentoa, sillä selitin sen a) kohdassa. 

    ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/55567454-26e7-4c1c-80f3-c14d853b34fa)

Liikaa turhia weppisivuja. fs komennolla filteröidään turhat pois

    ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669
    
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4226672b-427e-4833-a6dc-6d13ff37cc64)

Löytyi secret tiedosto. 

### Param Mining

Tässä tehtävässä opetellaan mitä tehdä, jos vastaan tulee virhetieto "required parameter missing". 

    ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1

Komennossa korvattiin aikaisempi hakulista common.txt, parametrien metsästämiseen tarkoitetulla parameters.txt tiedostolla.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/30bb4224-e6f4-4a4c-8408-a29ae97ce363)

### Rate Limited

Jotkin palvelimet ovat rajanneet, kuinka monta pyyntöä voi lähettää sekunnissa. 
Tämä voidaan korjata komennolla

    ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://ffuf.test/cd/rate/FUZZ -mc 200,429

-p 0.1 aiheuttaa applikaation pysähtymään 0.1 sekunniksi pyyntöjen välissä.
-t 5 tekee 5 versiota ffuf:ista. Yhdessä komennot siis tekevät maksimissaan 50 pyyntöä sekunnissa. 
-mc 200,429 näyttää vain pyynnöt jotka palauttivat HTTP statukset 200 tai 429 

Ilman pyyntöjen määrän rajoitusta

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ad01f1d0-eb2c-4d81-b488-85cd1b4accd7)

Rajoituksen kanssa 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/13e87326-d1cb-42ff-a582-5216414af65f)

### Subdomains - Virtual Host Enumeration

Tässä tehtävässä kerrotaan että fuff ei tarvitse välttämättä tiedostoa, mistä se saa hakusanoja. 

    seq 1 1000 | ffuf -w - -u http://localhost/cd/pipes/user?id=FUZZ

seq 1 1000 putkittaa kaikki luvut 1 - 1000 ffuf hakusanoiksi.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/555cfc23-2b2b-4dd9-b850-b696e762e571)

657 tärppäsi.

## Porttiskannaa paikallinen kone (127.0.0.2 tms), sieppaa liikenne snifferillä, analysoi.

Tehtävää varten suljin aikaisemman docker instanssin komennolla

    sudo systemctl stop docker

Tehtävää varten käynnistin myös wiresharkin. 

Selvitin myös virtuaalikoneeni ip:n tehtäviä varten

    hostname -I

Koneellani on kaksi eri yksityistä ip-osoitetta.
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/fe02ad4e-d5bf-45d6-9f40-6ba4a64b4470)


### c) nmap TCP connect scan -sT

    sudo nmap -sT 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6819d1f2-7e90-4aae-80de-20a2034a3ee8)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4b4caa5b-4f5a-427f-90d7-0ac2562cf7d8)

Wiresharkin tulosteesta näkyy source ja destination samana ip-osoitteena, koska skannasin oman koneeni. 
Tämän lisäksi osoitteesta 10.0.2.15, joka on koneeni toinen yksityinen osoite, on lähtenyt pyyntöjä myös.

Infokentässä olevia portteja tarkastelemalla huomataan, miten yhteys etenee. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/b9105c48-65bb-4872-b6f7-465ddac412ae)

nmap ensin lähettää syn pyynnön palvelimelle porttiin 5405, jonka jälkeen palvelin vastaa [RST, ACK]
RST tarkoittaa reset ja ACK tarkoittaa acknowledgement. 

Palvelin lähettää RST paketin takaisin sovellukseen (tässä tapauksessa nmap), kun se saa odottamattoman paketin.
En osaa sanoa, miksi palvelin lähettää myös ACK paketin.

### d) nmap TCP SYN scan -sS

    sudo nmap -sS -p80 172.17.0.1

Tarkensin myös nmap skannausta vain yhteen tiettyyn porttiin analysoinnin helpottamiseksi

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/9f87eef2-dcaf-42f7-bede-edbdd13bb246)

Tulosteen info kohdassa huomataan, että nyt nmap lähetti paketin vain palvelimen porttiin 80.
Muuten tuloste näyttää samalta ylemmän tehtävän kanssa

### e) nmap ping sweep -sn

    sudo nmap -sn 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/d7dfae0e-265d-40b7-a3e1-d65d6c56f0ed)

Tulosteessa näkyy uusi ip-osoite. 8.8.4.4. 
ip-osoite kuuluu googlen DNS palvelimelle. Protokolla keskustelussa on myös DNS.

Wireshark tulosteessa ei myöskään näy, että kukaan olisi yrittänyt ottaa yhteyttä portteihin

Jos sn komentoa käytetään yksinään, se saa Nmapin tekemään isäntähaun ja tulostamaan sitten käytettävissä olevat isännät, jotka vastasivat skannaukseen.

### f) nmap don't ping -Pn

    sudo nmap -Pn -p80 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4d293f8f-8c87-46e9-8371-b300a1a71280)

Tulosteessa yhdistyy kaksi aikaisempaa näkymää edellisistä tehtävistä. Ensiksi nmap on yhteydessä googlen dns palvelimeen, jonka jälkeen nmap lähettää tcp paketin palvelimen osoitteeseen.
Paketin saamisen jälkeen palvelin lähettää RST, ACK paketin lähettäjän osoitteeseen.

Pn ohittaa isännän etsintävaiheen kokonaan.

### nmap version detection -sV

    sudo nmap -sV -p80 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/abe4cef6-82bf-4c22-95ee-53f1897efafc)

Tuloste näyttää identtiseltä viime tehtävän tulosteen kanssa.

sV eli Version detection sallii nmapin selvittää hostin versionumerot esim avoimesta palvelimesta.

### nmap output files -oA foo

    sudo nmap -oA foo -p80 172.17.0.1

oA kertoo nmapille, että skannaus tulee tallentaa -oA (tiedostonimi) tiedostoon. Jos tiedostoa ei ole nmap tekee sen nimisen tiedoston

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/98fd00a9-0e16-4e78-b25e-9dc0d4826cbe)

Komento myös teki kolme eri foo tiedostoa eri tiedostotyypeillä

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/933d80c9-bf8f-42e3-aea1-9cfca583f629)

#### foo.gnmap

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0aae54e2-5ef8-4c13-87fa-7db37f66c825)

.gnmap tiedosto näyttää hyvin samalta nmap tulosteeseen. tiedostosta käy ilmi mitä internet protokollaa käytettiin skannauksessa.

#### foo.nmap

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f34c79bf-aec2-4ba9-b332-5354df9dc523)

.nmap tiedosto näyttää samalta, kuin nmapin tuloste komentoriville skannauksen jälkeen. Tämä tiedostotyyppi on paras selkeästi luettavan skannauksen lopputuloksen tarkasteluun.

#### foo.xml 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/541a2fd7-4bae-4811-b707-c168cac94dc5)

.xml on markup kieli, kuten HTML. XML tiedostojen tarkoitus on siirtää dataa. Tämä tiedosto sopii parhaiten skannauksen lopputuloksen jakamiseen.

### i) nmap ajonaikaiset toiminnot

nmapin suorituksen aikana skannaukseen voi vaikuttaa. 
Koska harjoittelumaali (oma kone) skannaukset ovat niin nopeita, en pysty demoamaan ajonaikaisia toimintoja.
 - v/V
   lisää / vähennä moninaisuutta
 - d/D
   lisää / vähennä debuggaus tasoa
 - p/P
   käynnistä / sammuta pakettien jäljitys
 - ?
   tulostaa ohjeluettelon

### j) Ninjojen tapaan. Piiloutuuko nmap-skannaus hyvin palvelimelta?

Asensin apachen

    sudo apt-get install apache2

Ja skannasin oman koneeni komennolla

    sudo nmap -A 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/69387a3b-1cce-4e86-b8f6-8e16f4d665cf)

wiresharkkiin tulostuu paljon get pyyntöjä, joiden lähde on koneeni ja kohde palvelin. 
Get pyyntöjen lisäksi nmap pyytää palvelimelta "Options / HTTP /1.1" moneen otteeseen.

Options pyyntöjen tarkoitus on kerätä tietoa. Optionssin avulla palvelin luovuttaa tietoja siitä mitkä kaikki asetukset ovat tuettuja palvelimella. 

Get pyyntöjen seassa on omituinen pyyntö
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/3d7dcc5b-99c5-40e6-86cc-fad816726233)

Get pyynnöt ovat tapa pyytää tietoa. Get pyynnön jälkeisen kauttaviivan jälkeen tulee se tieto, mitä pyydetään. 
Tässä tapauksessa nmap pyytää palvelimelta "nmaplowercheck16..."

Etsin hieman tietoa ja ilmeisesti kyseinen get pyyntö on yksi nmapin lukuisista skripteistä, mitä se käyttää selvittääkseen kohteesta jotain syvempää kuin avoimia portteja. 

"nmaplowercheck16..." on todiste siitä, että palvelinta ollaan tutkittu nmapin avulla. 

### k) UDP-skannaus -sU

    sudo nmap -sU 172.17.0.1

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c0453907-fc19-4b3c-9648-1752dabb5fdc)

nmap lähettää palvelimelle UDP pyyntöjä, mutta saa vastauksen ICMP protokollana. Pyynnöt palauttavat "Destination unreachable"

Toisin kuin TCP skannauksissa, UDP skannauksissa nmap lähettää pyynnöt samasta portista. 

### l) Miksi UDP-skannaus on hankalaa ja epäluotettavaa?

Toisin kuin TCP skannauksessa UDP portit lähettävät harvemmin vastauksia vaikka ne olisivatkin auki, joka hidastaa nmapin vauhtia ja tekee skannaamisesta epäluotettavaa.

--reason flagilla pääsee kurkkaamaan nmapin kulissien taakse, nmap näyttää enemmän informaatiota skannauksesta. 

snifferi kuten wireshark auttaa myös seuraamaan udp skannauksen etenemistä. 

## Lähteet 

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

 - a) tehtävän haastebinääri https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/?fromSearch=ffuf#your-turn---challenge
 - Harjoitusmaali dockerilla https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/


 - nmap runtime interaction https://nmap.org/book/man-runtime-interaction.html

 - HTTP options request https://reqbin.com/Article/HttpOptions

 - --reason flag https://geek-university.com/the-reason-flag/








