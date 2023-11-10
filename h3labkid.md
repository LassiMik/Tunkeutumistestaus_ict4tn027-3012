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


![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/13a47487-771e-49d3-a36d-00aba63cd175)

metasploitable 2 on nyt asennettu virtuaalikoneelle.

## c) Tee koneille virtuaaliverkko, jossa

### Kali saa yhteyden Internettiin, mutta sen voi laittaa pois päältä

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/91d57734-c9cd-430c-ba1e-da6570476b9b)

Kali on yhteydessä internettiin.
Tarvittaessa kalin voi ottaa pois internetistä 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ad84784d-ed72-49a4-879f-a586299af2bc)

### Kalin ja Metasploitablen välillä on host-only network

Loin kalille kaksi eri network adapteriasetusta. 
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/44b3725d-3478-4e31-82cf-32dde16f5db4)

sekä host-only 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/5a0fa95b-4c94-49a2-825c-ddf1e69e7626)

Muutin myös metasploitablen adapteriasetuksia

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e74b10b1-31e2-4248-929a-d7172981c044)

Nyt metasploitablen ei pitäisi enää olla yhteydessä internettiin.

### Osoita eri komennoilla, että Internet-yhteys katkeaa

metasploitable koneella ajoin seuraavan komennon, sen ip-osoitteen selvittämiseksi

    ifconfig

Kali koneella pingasin metasploitable konetta 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/02a8e948-3044-4c32-a7cc-25bd09a75b79)


Metasploitable kone:

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/9fcf4df9-97f9-4303-ac53-4269dbd7e51f)

Koneet ovat yhteydessä.

Metasploitable kone ei myöskään ole yhteydessä ulkomaailmaan.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/5ce88212-b847-4b1f-b9ce-e8836164e875)

## d) Etsi Metasploitable porttiskannaamalla (db_nmap -sn)

aloitin käynnistämällä kali koneella metasploit frameworkin komennolla

    sudo msfdb init

    msfconsole














## Lähteet

 - Nyrkkeilysäkki ei kuulu, c) kohta https://rikumannonen935063021.wordpress.com/
 - metasploitin käyttö d) https://www.kali.org/docs/tools/starting-metasploit-framework-in-kali/
 -
