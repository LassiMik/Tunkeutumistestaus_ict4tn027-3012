# h6Attaaack

## x) Lue/katso/kuuntele ja tiivistä.

### Yehoshua and Kosayev 2021: Antivirus Bypass Techniques, luku: Chapter 1: Introduction to the Security Landscape

 - Kappaleessa käsitellään tietokoneviruksia ja niiden tyyppejä, tietokoneen suojausjärjestelmiä, Antiviruksen alkeita sekä miten ohitetaan antivirus.
 - Tietokonevirus tarkoittaa koodia, hyötykuormaa tai tiedostoa, jonka tarkoitus on päästä kohdejärjestelmään sekä aiheuttaa vahinkoa kohteeseen. Tietokoneviruksella päätarkoitukset on antaa tunkeutujalle täysi pääsy kohteen järjestelmään, varastaa tietoja, kuten salasanoja tai salata tiedostot ja pyytää lunnaita. Tarkoituksia on monia ja niissä on rajana vain tunkeutujan mielikuvitus.
 - Antivirukset eivät ole täydellisiä ja artikkelissa onnistuttiin lataamaan tiedosto, joka loi "reverse shellin" kohdejärjestelmään. Reverse shell avaa satunnaisen portin kohdejärjestelmästä, joka ottaa yhteyden tunkeutujan palvelimeen.

### Halonen, Rajala ja Ollikainen 2023: PhishSticks Youtube Channel, kahdeksan videota, yhteensä noin 15 min

 - PhishSticks kanavalla demotaan Bad Usb:tä, joka näyttää usb tikulta, mutta tökätessään se hakee windows powershellin avulla hyötykuorman kohdejärjestelmään
 - Powershellin avaus tapahtuu niin nopeasti, että se voi jäädä huomaamatta.
 - Hyötykuorma, jonka bad usb latasi on "keylogger" eli se nauhoittaa kaikki napinpainallukset, mitä kohdejärjestelmässä tapahtuu.
 - Keyloggerin avulla tunkeutuja yrittää saada arkaluontoista tietoa, kuten käyttäjätunnuksia ja salasanoja

### Halonen, Rajala ja Ollikainen 2023: PhishSticks Git Repository, sivut:

#### README.md

 - readmessä käydään viikko kerrallaan PhishSticksin kehitys läpi.
 - Viikko 39 ja 40 käytettiin itse feikki usb tikun tekemiseen. Alusta, jossa koodi pyörii on "digispark" ja se käärittiin muoviseen koteloon, jotta se näyttäisi oikealta usb tikulta.
 - Viikolla 41 windows defender otti kiinni ensimmäiset yritykset keyloggerista. Ongelma saaatiin kuitenkin nopeasti korjattua. Windows defender osaa lukea koodin sisällön ja ei tykkää, jos siellä lukee sanoja, kuten "keylogger".
 - Viikolla 48 projekti oli valmis ja se esiteltiin HelSec tapahtumassa.

#### Revshell

 - USB tikun sisällä on "company raport" tiedosto, jos tiedosto avataan kohdekoneella se lataa raport.txt tiedoston sekä netcat-ohjelmiston, jota tunkeutujat käyttävät yhteyden luomiseen sekä portin avaamiseen.
 - raport.txt:n ainoa tarkoitus on hämätä käyttäjää. 

#### Mitigations

 - Helpoin tapa estää tikun toimiminen on poistaa powershellin käyttö koneelta. USB:n sisällä oleva digispark tarvitsee powershelliä saadakseen hyötykuormat kohdejärjestelmään.
 - Toinen tapa on lisätä powershell windowsin palomuuriasetuksiin.
 - Myös tapa estää USB tikun toimiminen on poistaa käytöstä windows run. 

#### Installing Windows 10 on a virtual machine

 - Käyn tarkemmin windowsin asennuksen virtuaalikoneelle kohdassa a)
 - Noudatin kohdassa a) tämän kappaleen oppeja

### MITRE Att&ck Frequently Asked Questions: Part 1. General.

 - MITRE:n hyökkäysmatriksi on jaettu kolmeen osaan. Tactics, techniques ja procedures.
 - Tactics edustavat hyökkäyksen syytä. Miksi tunkeutuja tekee jotain? Esimerkiksi käyttäjätietojen saaminen.
 - Techniques edustavat miten tunkeutuja pääsee päämääräänsä. Mitä tunkeutumistekniikoita hän käyttää?
 - Procedures ovat erityinen toteutus, jota hyökkääjä on päättänyt käyttää kohteeseensa saavuttaakseen päämääränsä. Proceduret vaihtelevat riippuen aiemmista kohdista. 

## a) The OS pwns you.

Latasin windows .iso tiedoston [täältä](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) Valitsin english(Great Britain) ja 64-bittinen versio.

Seuraavaksi siirryin virtualboxin puolelle. Tein uuden virtuaalikoneen painamalla "new" 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0c5e014f-e873-4575-8585-abfeaac47d41)

Syötin .ison ja nimesin koneen. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/47eb60ec-b87a-4f36-8db7-16c786617213)

Määritin vielä ramin ja cpu määrät. 

Käynnistäessä virtuaalitietokonetta windows kysyy kysymyksiä kielestä sekä rahasta ja ajasta. Vastasin näihin kaikkiin ja painoin "install now". Valitsin "custom install" koska koneella ei aikaisemmin ole ollut windowsia. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1e3e47e9-6392-4462-b1d7-988887650629)

Asennusten jälkeen windows pyytää microsoft tiliä kirjautumiseen. Valitsen "domain join instead".
Täytin vielä nimen, salasanan sekä salakysymykset.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/05fda4a1-75c0-4abf-ad1c-d35350b79ea6)

Nyt windows on asennettu. Suljen windowsin hetkeksi, jotta voin vaihtaa windowskoneen samaan verkkoon, kuin kali 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/5692e845-a771-4e27-ae66-1bc0942b597b)

Kokeilin vielä koneitten yhteyttä pingaamalla molemmilta

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/360d06f8-846c-4f30-9476-ecf133c2c340)

Jostain syystä kali ei voinut pingata windows konetta. Sama ongelma oli ohjeistuksessa, jota seurasin tehtävää varten. Windows kuitenkin pystyi pingaamaan kalia, joten yhteys oli.


## b) Trustme.lnk.

## d) PageRank.

## c) Attaaack!


# Lähteet

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

 - https://learning.oreilly.com/library/view/antivirus-bypass-techniques/9781801079747/B17257_01_Epub_AM.xhtml#_idParaDest-18

 - https://www.youtube.com/@phishsticks_pentest/videos

 - Windows asennus - https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md
