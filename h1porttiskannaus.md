# h1porttiskannaus

## x ) lue ja tiivistä

Santos et al: 
Tiedustelu on onnistuneen tunkeutumisen tärkein osa. Tiedustelun tarkoitus on kartoittaa hyökättävän kohteen heikkoudet. 

## a) linuxin asennus

Olen asentanut virtuaalikoneen jo aikaisemmalla "linux -palvelimet" kurssilla. Käytän virtuaalikoneeen käynnistämiseen oraclen VM virtualbox manageria. Virtuaalikoneella minulla on debian 12 käyttöjärjestelmä.

## b) porttien skannaus

Tässä osiossa asennan nmap nimisen työkalun, jota käytän porttien skannaamiseen. 

    sudo apt-get install nmap

selvitin myös oman virtuaalikoneeni ip-osoitteen 

    hostname -I

Seuraavaksi kokeilen porttiskannata oman virtuaalikoneeni

    sudo nmap 10.0.2.15

nmap komentoa voi käyttää vain sudo oikeuksilla. 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/81b282b7-5f68-478a-affa-543a2fd17db2)

Pelkkä nmap komenento skannaa vain kohteen 1000 tavallisinta tcp porttia. Virtuaalikoneellani 999 niistä on kiinni ja yksi auki.
Koneella on auki tcp-portti 80 ja service kohdalla lukee "http" (Hypertext Transfer Protocol).

Virtuaalikone käyttää porttia 80 internettiin yhdistämistä varten.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c6a2c92e-dac1-4673-805d-e6bed7140e89)

Skannasin kaikki virtuaalikoneeni portit, skannaus antaa saman lopputuloksen, eli portin 80. Loput 65535 ovat auki. 
Skannaus myös kesti huomattavasti aikaisempaa skannausta pidempään. Aikaisempi skannaus, jossa katsottiin vain 1000 porttia kesti 0.03 sekuntia,
kun taas 65535 portin skannauksessa kesti 0.62 sekuntia.
Muun kuin oman koneen skannauksessa saattaa kestää huomattaviakin aikoja.

## d) laaja porttiskannaus 

Selvitin ensin mitä tehtävänanossa annettu komento nmap -A tekee tarkastelemalla nmapin man sivua.

    man nmap

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/23e7ec59-743a-4c3f-b5a0-6ce55646a2e8)

Kun tiesin mitä komento nmap -A tekee, käytin sitä omaan localhostiini

    sudo nmap -A 10.0.2.15

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7502739a-8753-444e-8c4c-e549f2cb7f68)










