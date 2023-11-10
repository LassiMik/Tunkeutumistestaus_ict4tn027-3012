# h3labkid 

## x) lue/katso ja tiivistä

### Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation

 - Läpäisytestin päätarkoitus on tunkeutua kohdejärjestelmään ja luoda pysyvä reitti järjestelmän sisään.
 - Metasploit Framework (MSF) on avoimen lähdekoodin työkalu, joka on suunniteltu helpottamaan tunkeutumistestausta. MSF on kirjoitettu Ruby-ohjelmointikielellä.

### Nyrkkeilysäkki ei kuulu

Metasploitable 2 ja kali samaan verkkoon virtualboxissa. 

- Riku aloitti luomalla uuden verkon virtualboxiin, jotta kali ja metasploitable voisivat olla samassa verkossa, ilman yhteyttä verkon ulkopuolelle.
- Host-only verkkoa luodessa riku kohtasi virheilmoituksen ”Failed to save host network interface parameter.” Syynä oli, että oletuksena virtualbox hyväksyy linux koneille avin tietyn verkkoalueen. Uudet verkot pitää lisätä myös konffitiedostoon, joka löytyy /etc/vbox/networks.conf
- Virtualboxista voi tämän jälkeen vaihtaa kalin ja metasploitablen verkkoasetukset niin, ettei ne näy internettiin. Asetus löytyy virtualboxissa asetukset sivun network välilehdeltä.
- Riku vielä varmisti pingaamalla molemmilla hosteilla toiselle, että ne olivat samassa suljetussa verkossa.
  
## a) Asenna Kali virtuaalikoneeseen

 - Aloitin lataamalla Kalin uusimman distron [Täältä](https://www.kali.org/get-kali/#kali-installer-images)
 - Sen jälkeen tein uuden virtuaalikoneen oracle VM virtualbox managerilla, jonka luomiseen käytin äsken lataamaani distroa.
 - Käynnistin virtuaalikoneen ja kävin kalin asennuksen läpi, en vaihtanut muuta, kuin käyttäjänimen, salasanan sekä domaininimen
 - Käynnistin vielä asennuksen jälkeen virtuaalikoneen uusiksi
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/026db60c-be1c-43fc-83a1-4b5f0cdc84fc)

## b) Asenna metasploitable 2 virtuaalikoneeseen

Latasin sourceforgesta metasploitable 2 zippitiedoston.
Seuraavaksi, teen uuden virtuaalikoneen, josta tulee metasploitable 2 virtuaalikone.
Valitsin koneen luonnissa käyttöjärjestelmäksi linux ubuntu 32-bit
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/9f205362-cbb1-4ef9-ade7-d9d6b3c9a540)

Käytin seuraavissa vaiheissa oletusasetuksia kovalevyasetuksiin saakka.
Tässä syötän lataamani tiedoston virtuaalikoneeseen.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/9ccfd456-e1e2-4758-8e6b-c3f43feceaf5)

Valitsin vielä virtualboxin asetuksien network välilehdeltä "Bridged Adapter"
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0da14b9f-839d-4840-a8ae-f1605e7ab6d7)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/13a47487-771e-49d3-a36d-00aba63cd175)

metasploitable 2 on nyt asennettu virtuaalikoneelle.

## c) 


## Lähteet

 - Nyrkkeilysäkki ei kuulu, c) kohta https://rikumannonen935063021.wordpress.com/
