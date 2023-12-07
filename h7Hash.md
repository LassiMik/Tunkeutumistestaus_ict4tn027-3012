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

Hashcatin asennukseen ja käyttöön käytin Tero Karvisen [ohjetta](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)

Asensin tarvittavat ohjelmat komennolla ```sudo apt-get -y install hashid hashcat wget```

Tein uuden kansion hashcattia varten sekä latasin suuren sanakirjan. 

```wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz```

Tiedosto tulee .tar.gz muodossa. Muutetaan se tekstitiedostoksi. 

```
tar xf rockyou.txt.tar.gz
rm rockyou.txt.tar.gz
```

Kokeilen vielä toimiiko ohjelmat. 
Käytän testissä samaa hashia, kuin ohjeissa. ```6b1628b016dff46e6fa35684be6acc96```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/a9a3f53a-b631-4854-89fb-ff9f772f13ef)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/585ba1cf-92d9-4636-ba13-5d775678a076)

Kaikki toimii. 

# b) John. Asenna Jumbo John ja testaa sen toimivuus murtamalla jonkin tiedoston salasana.

Seuraan john the ripperin asennuksessa Tero Karvisen laatimia [ohjeita](https://terokarvinen.com/2023/crack-file-password-with-john/) 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/58f00602-14c9-4580-89f4-d64b4d63650c)

zlib-gst aiheutti jostakin syystä ongelmia asentaessa. Onnistuin asentamaan kaikki muut. 

zlib-gst käytetään johnin zippi ominaisuutta varten. Toimiikohan ilman tätä?

Asensin vielä itse johnin. 

```git clone --depth=1 https://github.com/openwall/john.git```

configuroin vielä johnin 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/722229f6-b18d-40e5-bfa7-a451701dd148)

Komento loi "make" komennon 

```make -s clean && make -sj4```

Ajoin vielä make komennon, joka compiloi koodin. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/5b4a0e35-694b-403a-ab12-a09fc3cd1bf3)

John on nyt toimintavalmiina. 

# c) Ratkaise tiiviste

# d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä.

# Lähteet

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h7-hash
 - Karvinen 2022: Cracking Passwords with Hashcat https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
 - Karvinen 2023: Crack File Password With John https://terokarvinen.com/2023/crack-file-password-with-john/
 - Rockyou sanakirja https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
