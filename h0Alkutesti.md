# h0 Alkutesti

Tehtävän pohjana toimii aikaisemmalla linux - palvelimet kurssilla asennettu debian linux virtuaalitietokone

## wireshark asennus

Kokeilen ohjelmaa nimeltä wireshark tehtävän suorittamiseen

    aloitin päivittämällä tiedostot komennolla sudo apt-get update

Seuraavaksi latasin wiresharkin komennolla

    sudo apt-get install wireshark

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/28214eb1-2080-41c9-b8fa-dbace1494005)

Törmäsin seuraavaan varoitukseen yrittäessäni käyttää wiresharkkia

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ecd6979e-7569-48f7-a00e-3c932db5342c)

Toimin ohjeiden mukaan ja sain wiresharkin auki

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1dcecd7a-4326-4abc-84fa-476c42a7cc4f)

Valitsin valikosta "any" kohdan ja menin firefox selaimella youtubeen

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/09827eff-5a86-46c5-985d-b66fbd98fb24)

Wireshark on kuvan ottamisen hetkellä tulostanut jo 3158 riviä verkkoliikennettä virtuaalikoneeseen ja virtuaalikoneesta ulos. Source on ip osoite josta tieto tulee tai johon tieto lähtee. Kuvasta näkee, että virtuaalikone kommunikoi youtuben kanssa, sillä yksi ip-osoite lähtettää tiedon, jonka jälkeen samaan ip-osoitteeseen tulee vastaus. Toinen ip-osoitteista on mitä luultavammin virtuaalikoneeni ip-osoite sekä toinen youtuben ip-osoite.

Protocol kertoo ip osoitteen tietoliikenneprotokollan, kuvasta näkyy 
 - TCP (Transmission Control Protocol) sekä 
 - QUIC (Quick UPD Internet Connections

Löysin myös hakuselaimen lähettämän verkkoliikennepyynnön 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/85ddd3c5-8cc5-4e97-97cb-c42291eb748b)

Pyyntö lähettää tiedon ip-osoitteeseen 8.8.8.8 ja käyttää DNS tietoliikenneprotokollaa. Length kertoo myös pyynnön pituuden sekä info lisäkohta kertoo lisätietoa kyseisestä liikenteestä. 

Pari riviä alempaa löysin vastauksen omaan verkkoliikennepyyntööni 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f164253d-55e0-423c-a35b-f2a7aaa3753c)

## Loppusanat

En ole aikaisemmin käyttänyt wiresharkia. Testieni perusteella wiresharkilla voi ainakin monitoroida kaikkea oman tietokoneen verkkoliikennettä. Wiresharkia käynnistäessä valikossa oli monia eri vaihtoehtoja, joten monitoroitia voi varmaankin rajata tarkemmaksi tarvittaessa. 

## Lähteet

 - wiresharkin lataus https://askubuntu.com/questions/700712/how-to-install-wireshark

 - wiresharkin käyttö https://www.wireshark.org/docs/wsug_html_chunked/ChCustCommandLine.html

