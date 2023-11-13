# h3labkid 

## x) lue/katso ja tiivistä

### Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation

 - Läpäisytestin päätarkoitus on tunkeutua kohdejärjestelmään ja luoda pysyvä reitti järjestelmän sisään.
 - Metasploit Framework (MSF) on avoimen lähdekoodin työkalu, joka on suunniteltu helpottamaan tunkeutumistestausta. MSF on kirjoitettu Ruby-ohjelmointikielellä.

### Vapaavalintainen läpikävely 0xdf

HTB: Broker

 - Broker on HackTheBoxin tekemä peli, jonka tarkoitus on jakaa tietoisuutta uudesta heikkoudesta ActiveMQ:ssa. ActiveMQ on apachen tekemä javaan pohjautuva viestin välittäjä.
 - CVE-2023-46604 on todentamattoman koodin etäsuorittamisen haavoittuvuus ActiveMQ:ssa, joka sai harvinaisen 10.0 CVSS imact -luokituksen
 - CVSS (Common Vulnerability Scoring System) on pisteytys 0-10 tietoturvaaukkojen vakavuudesta.
 - Hyökkäyksessä käytetään python koodia, jolle annetaan kohteen ip-osoite, porttinumero ja Spring XML Url.
 - Hyökkäys hyödyntää ActiveMQ:n deserialisointihaavoittuvuutta ja käyttää Spring-gadgetia ladatakseen etä-XML-tiedoston, joka pystyy suorittamaan ohjelmia.

### Nyrkkeilysäkki ei kuulu

Metasploitable 2 ja kali samaan verkkoon virtualboxissa. 

- Riku aloitti luomalla uuden DHCP verkon virtualboxiin, jotta kali ja metasploitable voisivat olla samassa verkossa, ilman yhteyttä verkon ulkopuolelle.
- Host-only verkkoa luodessa riku kohtasi virheilmoituksen ”Failed to save host network interface parameter.” Syynä oli, että oletuksena virtualbox hyväksyy linux koneille vain tietyn verkkoalueen. Uudet verkot pitää lisätä myös konffitiedostoon, joka löytyy /etc/vbox/networks.conf
- Virtualboxista voi tämän jälkeen vaihtaa kalin ja metasploitablen verkkoasetukset niin, ettei ne näy internettiin. Asetus löytyy virtualboxissa asetukset sivun network välilehdeltä "Host-only Adapter".
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

Skannasin oman koneeni ip alueella olevat koneet, jolla metasploitable kone myös on.

Käytin komentoa

    db_nmap -sn 192.168.131.5/24

-sn host skannaus, ei tarkastele portteja.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/58b48524-ca9a-49cb-8361-79d50b77b573)

Tarkastin vielä löytämäni ip-osoitteen

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/11d1d5eb-f4b3-4807-9304-d12322204da0)

## e) Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-)

    db_nmap -A -p0- 192.168.131.3

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/910b3d5a-edde-46ac-a8cc-437d2a9cfb3b)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/489e3ad3-843c-4a28-8708-abcc5020cad1)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/3a538df2-2bcb-47fb-8e97-fbe8b8203499)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/dc3ab470-9fa4-46c3-b0f5-e5d89357e058)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/dbe350ab-4eba-4b77-9452-11a0fb6259b8)

### Analyysi 

TCP portissa 21 on avoin portti, jossa pyörii FTP

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c75dea15-7775-43c1-ba19-51a8219f1984)

Pienellä googlailulla ftp portin versionumero on myös vanha.

Portissa 22 on avoin ssh portti

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ba590124-4971-493e-9d89-e3b3f069931d)

Portissa 80 on aikaisemmin curlattu sivusto

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/88432c4e-7e80-456d-896f-0538dbcbca12)

Porteista 3306 sekä 5432 löytyy kaksi sql palvelinta. Ensimmäinen mysql palvelin ja toinen postgresql palvelin
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/3dba99d9-4e70-4382-bd40-c24ef46e160a)
![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/9376fad8-1311-4483-94a3-6e46e9d1cce8)

Lopuksi skannaus myös paljastaa että kohteen käyttöjärjestelmä on Samba 3.0.20 versio Debianista, joka myöskin on vanhentunut.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/2663a212-a09d-409e-8398-d71901b29bbb)

## f) Murtaudu Metasploitablen VsFtpd-palveluun Metasploitilla

Search x voi etsiä sopivan hyökkäyksen hakusanoilla

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/901b6e32-09d6-4feb-8752-436b8a3c02cd)

Valitsin 1 exploitin, sen jälkeen valitsin kohteeksi metasploitable koneen komennolla

    set RHOSTS 192.168.131.3

    exploit

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/d4b6a855-732f-410e-8009-80015fae41dd)

## g) Parempi sessio

Halusin päivittää aikaisemmassa kohdassa saadun session paremmaksi. 
Poistuin saadusta sessiosta ctrl+Z ja hain metasploitista 

    search shell_to_meterpreter

    use 0
    
ja vielä

    sessions -I

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/76440e63-897c-4a6e-9249-607e2cd92601)

    set SESSIONS 1

    run

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c203d6cc-1923-4cd7-a3f0-b688591b9f88)

Ja vielä viimeinen komento, niin saadaan parempi sessio

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e4e7f807-8cb4-481f-bc57-8473d564549d)

## h) Etsi, tutki ja kuvaile jokin hyökkäys ExploitDB:sta.

https://www.exploit-db.com/

### Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)

Löyisin exploit DB:stä samba 3.0.20 heikkouden, eli juuri sen käyttöjärjestelmän heikkouden, joka kohteella on. 

Exploitti käyttää samban komennon suorittamisen haavoittuvuutta suorittamalla ei oletusarvoisen käyttäjänimikartoitus skriptin.

Määrittämällä käyttäjätunnuksen, joka sisältää shell-metamerkkejä, hyökkääjät voivat suorittaa shell komentoja.

Tämän haavoittuvuuden hyödyntämiseen ei tarvita edes todennusta, koska tätä vaihtoehtoa käytetään käyttäjänimien kartoittamiseen ennen todennusta.

## i) Etsi, tutki ja kuvaile hyökkäys 'searchsploit' -komennolla. Muista päivittää.

Löysin komennolla

    searchploit unrealIRCd backdoor

toisen backdoor heikkouden kohteelta.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ba4458b9-3167-43ca-ba0c-fface52e5bed)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/38de4858-54ef-4269-be55-d76caafe18c9)

Tein exploitista tiedoston nykyiseen hakemistoon, jotta voin tarkastella sitä. 

    searchsploit -m 16922

tarkastelin uutta exploittia nano komennolla

    nano 16922.rb

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/89435471-721d-44b5-aa99-583f0ae339b6)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/bb788b43-5b11-4ebe-9eeb-72aa74e6a500)

Hyökkäys käyttää heikkoutta unrealRCD IRC demonissa. Takaovi aukeaa, kun siihen lähettää "AB" ja payload.encoded muuttujan.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/da23bb65-826a-4605-9fc8-aaec64267f52)


## j) Kokeile vapaavalintaista haavoittuvuusskanneria johonkin Metasploitablen palveluun.

Valitsin nikton, joka erikoistuu nettisivustojen heikkouksien skannaamiseen.

    nikto -h 192.168.131.3

Skannaus tulosti paljon dataa, mutta keskityn kiinnostavimpiin löydöksiin

Skannaus löysi tiedoston, jonka sisällä kirjautumistunnuksia

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/533a971d-f774-4017-b870-0f0e0142fce9)

Löysin myös kansion /phpMyAdmin/

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/468a226e-859e-4879-b1e9-ef7d73dc3b92)

/test/ kansiossa nikto kirjoitti, että voi olla, jotakin kiinnostavaa.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/96bded8c-0e5a-44aa-8ca6-54edf1b15dc3)

PHP voi paljastaa jotakin mahdollisesti jotakin sensitiivistä joillain HTTP pyynnöillä

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/6ae24a06-630c-4d98-9866-4d802f254cca)


## k) Kokeile jotain itsellesi uutta työkalua, joka mainittiin x-kohdan läpikävelyohjeessa.

HTB brokerissa ei ollut sellaisia uusia työkaluja, joita varten minulla olisi maali. Valitsin sen sijaan feroxbusterin toisesta artikkelista. HTB: TwoMillionista.

Feroxbuster on työkalu, joka on suunniteltu suorittamaan pakotettua selausta. Pakotettu selaus on hyökkäys, jossa tavoitteena on luetella ja päästä käsiksi resursseihin, joihin verkkosovellus ei viittaa, mutta jotka ovat hyökkääjän käytettävissä.

Latasin ensiksi feroxbusterin, koska se ei tullut kalin mukana

    sudo apt-get install feroxbuster

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/67839504-52bd-4b07-adbb-ac62bcbfd91f)

Feroxbuster löysi mm. phpMyAdmin osoitteen

## Lähteet
 - tehtävät h3labkid https://terokarvinen.com/2023/eettinen-hakkerointi-2023/ 
 - Velu 2022: Mastering Kali Linux for Advanced Penetration Testing 4ed: Chapter 10 - Exploitation https://www.oreilly.com/library/view/mastering-kali-linux/9781801819770/Text/Chapter_10.xhtml#_idParaDest-257
 - 0xdf https://0xdf.gitlab.io/2023/11/09/htb-broker.html
 - c) Nyrkkeilysäkki ei kuulu https://rikumannonen935063021.wordpress.com/
 - d) metasploitin käyttö https://www.kali.org/docs/tools/starting-metasploit-framework-in-kali/
 - g) päivitys meterpreteriin https://infosecwriteups.com/metasploit-upgrade-normal-shell-to-meterpreter-shell-2f09be895646
 - k) HTB: two million https://0xdf.gitlab.io/2023/06/07/htb-twomillion.html
 - k) feroxbuster https://www.kali.org/tools/feroxbuster/
