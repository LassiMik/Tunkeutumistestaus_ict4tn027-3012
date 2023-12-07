# h7 Hash

# x) Lue/katso/kuuntele ja tiivistä.

## Karvinen 2022: Cracking Passwords with Hashcat

 - Hashcattia varten tarvitaan iso sanakirja. Esimerkissä käytetään "Rockyou" nimistä sanakirjaa. Sanakirja sisältää yli 14 miljoonaa sanaa. Sanakirja koostuu oikeista salasanoista, jotka ovat vuotaneet julkisuuteen.
 - hashid on salasanahashin tunnistamiseen tarkoitettu ohjelma. Komento ```hashid -m hash``` tulkitsee mitä salausta hashissa on käytetty.
 - Hashcat osaa nyt tunnistuksen sekä sanakirjan avulla murtaa salasanan.
 - Komennolla ```hashcat -m (hashid numero) 'hash' rockyou.txt(sanakirja) -o solved(jos haluaa tallentaa salasanan uuteen tiedostoon)```

## Karvinen 2023: Crack File Password With John

 - Hashcat on salasanoja varten, mutta John the ripper on zip tiedostojen salausta varten.
 - Jotta john voi murtaa mitään, tiedostosta pitää tehdä zip.hash tiedosto. Johnilla on komento tätä varten. ```/john/run/zip2john tiedosto.zip >tiedosto.zip.hash```
 - zip.hash tiedoston john voi murtaa ```/john/run/john tiedosto.zip.hash```
 - Murtaminen antaa zip tiedoston salasanan. Nyt tiedoston voi purkaa unzipillä.

# a) Hashcat. Asenna Hashcat ja testaa sen toimivuus ratkaisemalla tiiviste.

# b) John. Asenna Jumbo John ja testaa sen toimivuus murtamalla jonkin tiedoston salasana.

# c) Ratkaise tiiviste

# d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä.

# Lähteet

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h7-hash
 - Karvinen 2022: Cracking Passwords with Hashcat https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
 - Karvinen 2023: Crack File Password With John https://terokarvinen.com/2023/crack-file-password-with-john/
 - Rockyou sanakirja https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
