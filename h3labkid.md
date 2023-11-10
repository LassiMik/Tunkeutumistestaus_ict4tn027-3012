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


## Lähteet

 - Nyrkkeilysäkki ei kuulu, c) kohta https://rikumannonen935063021.wordpress.com/
