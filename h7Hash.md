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

Kokeilen vielä murtaa johnilla jonkin zip tiedoston salauksen. 

Asensin testitiedoston

```wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip```

Tiedosto on suojattu salasanalla.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f1c935c0-d337-4aee-917a-b9c5ba44f8b5)

zip tiedostosta pitää tehdä zip.hash tiedosto, jotta john voi murtaa sen. Onneksi johnilla on tapa tehdä se. 

```$HOME/john/run/zip2john tero.zip >tero.zip.hash```

Komento luo uuden tiedoston tero.zip.hash.

Tarkastelin vielä cat komennolla tiedoston sisältöä. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/abd12eec-026e-4174-a48b-3cc1a39e8007)

Seuraavaksi laitan johnin hyökkäämään tiedoston kimppuun.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/edd90be7-66ba-4236-939c-c8fe9fe2bf95)

Ymmärsin että john käyttää valmiiden sanojen salasanatiivisteitä sanakirjasta ja vertailee niitä tiedoston salasanatiivisteeseen. Tämä on vain arvaus. 

Salasana on kätevästi eri värinen kuin muu teksti, joten se osuu silmään

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/8b2cb814-ae23-41e7-bdeb-dc22628a2281)

Kokeilen vielä salasanaa unzip komennolla. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/bf66da67-835f-4efb-b4fc-d1344b8dfdc9)

Ohjelmisto siis toimii vaikka en asentanut kaikkia tarvittavia riippuvaisuuksia.

# c) Ratkaise tiiviste

Tehtävässä annetaan itse tiiviste ```f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f```

Sekä kaksi vihjettä, joista ensimmäinen on: "Käytin hyvin yleistä ja tunnettua tiivistealgoritmia. Sanassa voi olla isoja kirjaimia, mutta ei erikoismerkkejä"

Ja toinen: "on erään tällä tehtäväsivulla olevan yksittäisen sanan tiiviste"

Kokeilin ajaa hashid ohjelmistolla tiivisteen. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/07d5eb07-8356-4a07-b19f-5adf6472b48d)

Ajan hashcatilla ensimmäisen tarjotun vaihtoehdon, eli SHA-256 salauksen

```hashcat -m 0 'f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f' rockyou.txt```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e2effd71-3250-413b-bcbb-20a554f2c8b2)

Hashcat antaa vastauksen "exhausted" eli ei onnistuttu murtamaan tiivistettä. 

Kokeilin vielä GOST R salausta 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ae42412e-a1b6-418b-89fe-29da607af385)

Sama tulos. 

Tässä vaiheessa tajusin, että rockyou.txt käyttää englanninkielisiä sanoja, mutta sivusto on suomeksi. Toinen vihjeistä on, että kyseessä on sana, joka on tehtäväsivulla. Tarvitsen siis uuden sanakirjan.

Kopioin kaiken tekstin, paitsi kommentit sivustolta ja laitoin ne tekstitiedostoon. 

Putkittamalla saan sanakirjasta mieleisen. Aloitin laittamalla jokaisen sanan omalle riville. 

```cat sanakirja.txt|tr ' ' '\n'```

Tiivisteessä ei voi myöskään olla erikoismerkkejä. Poistin erikoismerkit putkittamalla 

```|tr -d '".,()-:'```

Komentoni on valmis. Hashcatin komennossa ei voi putkittaa, joten joudun muokkaamaan sanakirjatiedostoa. Tämän olisi varmaan voinut tehdä helpommin, mutta käytin linuxin redirectoreja. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c636415c-82b9-4adb-8b95-e0fb5dfc37ae)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/5c86a8e8-973e-4cac-a375-5e43bdd765a5)

Aika laittaa uusi sanakirjani testiin. 

```hashcat -m 1400 'f5bc7fcc7f5b3b6af7ff79e0feafad6d1a948b6a2c18de414993c1226be48c1f' sanakirja3.txt```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/73484eee-5cef-4b96-af65-848e17e87344)

Ja uusi sanakirja toimi mahtavasti. Salaus on murrettu. Salasana on "Sertificate". Käytetty salaus oli SHA2-256

# d) Cheatsheet. Kerää kurssilaisten raporteista käteviä tekniikoita. Kerää itse tekniikat ja komennot, älä pelkästään kuvaile. Muista lähdeviitteet. Tee tiivis ja selkeä cheatsheet, josta löydät tarvittavat tiedot lipunryöstössä.

## Tiedustelu

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4ccb0db6-99a7-4515-a330-b51291de04ba)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/3f605582-7f41-4ce3-833b-ce8a56140031)


## Sanakirja 
cewl, lataa sivuston tekstit tiedostoon, voi antaa parametrejä
[dokumentaatio](https://www.kali.org/tools/cewl/) 

tr komennolla pilkkoo. 
 - välilyönnit pois ```tr ' ' '\n'```
 - erikoismerkit pois ```tr -d '".,()-:'```
 - < > uusi tiedosto tr muutoksilla. ```tr ' ' '\n' <1.txt >2.txt```

# Lähteet

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/#h7-hash
 - Karvinen 2022: Cracking Passwords with Hashcat https://terokarvinen.com/2022/cracking-passwords-with-hashcat/
 - Karvinen 2023: Crack File Password With John https://terokarvinen.com/2023/crack-file-password-with-john/
 - Rockyou sanakirja https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
 - tr komennon käyttö https://linux-packages.com/debian/package/pythonpy
 - cewl https://www.kali.org/tools/cewl/
