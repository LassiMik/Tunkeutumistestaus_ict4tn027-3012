# h5 Injected Sequel

## x) Lue/katso/kuuntele ja tiivistä.

### [Karvinen 2016: PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu](https://terokarvinen.com/2016/03/05/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/)

Karvinen aloittaa lataamalla ja käynnistämällä postgresql komennoilla 

```
$ sudo apt-get -y install postgresql
$ sudo systemctl start postgresql # needed in 2023
$ sudo -u postgres createdb $(whoami)
$ sudo -u postgres createuser $(whoami)
```

Tällä tavalla, kun postgre tietokanta sekä käyttäjä on sama, kuin käyttäjänimi, autentikaatio on automaattista.
Tietokantaan siirrytään komennolla 

```
psql
```

tietokannan sisällä voi syöttää sql lausekkeita. 
postgressä yhdenllä numerolla kasvavan pääavaimen voi luoda syntaksilla "SERIAL PRIMARY KEY"
Muuten syntaksi on samanlaista, kuin muissa isoissa sql tietokannan hallintajärjestelmissä

### [OWASP 2017: A1:2017-Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection)

 - OWASP:in artikkeli A1:2017-injection kertoo injektioheikkoudesta, missä ei tunnistettu käyttäjä voi syötää haitallista tekstiä
mihin vain tekstikenttiin tai selaimen hakukenttään. 

 - Heikkoudelle on oleellista, että palvelin ei tarkasta tai filtteröi käyttäjän tekstiä sekä suorittaa komennot käyttäjän syötteellä.

 - Uudemmissa versioissa tietokannan hallintajärjestelmistä injektioheikkouksia on vähemmän. 

 - Injektiohaavoittuvuuksia löytyy usein SQL-, LDAP-, XPath- tai NoSQL-kyselyistä, käyttöjärjestelmäkomennoista, XML-parsereita, SMTP-headereista, expression kielistä ja ORM-kyselyistä.

- Injektiohaavoittuvuus voi johtaa tietojen katoamiseen, -korruptoitumiseen tai -paljastamiseen luvattomille osapuolille, vastuun menettämiseen tai pääsyn epäämiseen. Injektio voi joskus johtaa täydelliseen isäntäkoneen haltuunottoon.

### [PortSwigger Academy: SQL injection](https://portswigger.net/web-security/sql-injection)

 - SQL-injektio on eräänlainen kyberhyökkäys, jossa hyökkääjä lisää haitallista SQL-koodia (Structured Query Language) verkkosovelluksen syöttökenttiin.
 - Tämä voi manipuloida sovelluksen tietokantakyselyitä ja mahdollistaa luvattoman pääsyn arkaluonteisiin tietoihin, tietojen muokkaamisen tai jopa tietojen poistamisen.
 - Haavoittuvuus syntyy, kun verkkosovellus ei tarkista tai puhdista käyttäjien syötteitä oikein, jolloin hyökkääjät voivat syöttää ja suorittaa omia SQL-komentojaan.
 - SQL-injektion estäminen edellyttää parametroitujen kyselyjen, syötteiden validoinnin ja muiden suojaustoimenpiteiden käyttöä sen varmistamiseksi, että käyttäjien syötteitä käsitellään turvallisesti eivätkä ne aiheuta riskiä sovelluksen tietokannalle.


## a) CRUD

### Tee uusi PostgreSQL-tietokanta

Ajoin komennot postgresql lataamiseksi ja käynnistämiseksi
```
sudo apt-get -y install postgresql
sudo systemctl start postgresql # needed in 2023
sudo -u postgres createdb $(whoami)
sudo -u postgres createuser $(whoami)
psql
```

### demonstroi sillä create, read, update, delete (CRUD). Taulujen nimet monikossa, kenttien nimet yksikössä, molemmat englanniksi.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/d53c3281-73fd-4048-8c32-fbba816ef23f)

Kohtasin ongelman create lausekkeessa. "permission denied for schema public". Etsin netistä [tietoa](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/) 

Korjasin ongelman vaihtamalla postgre käyttäjäksi, jonka postgresql loi automaattisesti

```
sudo -i -u postgres
```
CREATE

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/4a957b1c-593b-45d5-9db1-391ba3dd6941)

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/a928ba80-cbd8-4919-9d3e-b1000327406c)

READ

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/702f4e29-cfeb-4b04-b160-5f4e2835c58a)

UPDATE 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1680e2c2-faef-4652-a930-5666561ecc9e)

DELETE 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f6696b94-36b2-4aa6-b5c8-f38b39bd1f24)


## b) SQLi me. Kuvaile yksinkertainen SQL-injektio, ja demonstroi se omaan tietokantaasi psql-komennolla. Selitä, mikä osa on käyttäjän syötettä ja mikä valmiina ohjelmassa.

Nettisivusto, jolla haetaan käyttäjän id:llä tiettyä käyttäjää

```
"SELECT * FROM names WHERE UserId = " + käyttäjän_syöte
```

Ohjelmassa on valmiina lainausmerkkien sisällä oleva osio ja tietokantaan syötetään suoraan käyttäjän syöte
Jos käyttäjän syöte on esimerkiksi ```1 OR 1=1``` tietokanta palauttaa kaikki käyttäjät, koska ```1=1``` on aina tosi

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/a3fd433e-6428-4c21-8afc-6ab7b1941c87)

## [PortSwigger Labs](https://portswigger.net/)

### d) SQL injection vulnerability allowing login bypass

Kirjautuessa nettisivu lähettää post pyynnön, jonka bodyssä on username ja password

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f7fea92d-6105-4287-8f99-7086e5db65c4)

Editoin post pyynnön usernamen ```administrator'--```

```'--``` kommentoi pois kohdan, jossa tarkistetaan salasana ja päästään pelkästään käyttäjänimellä kirjautumaan.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c26b427c-6ca9-453c-a3b2-46f3e4a202b6)


![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7775c3a6-23a3-4ffc-a53f-4df7fdb36f72)

### e) SQL injection attack, querying the database type and version on Oracle

Tehtävässä käytetään "Union select" sql lausetta. En ole aikaisemmin käyttänyt lauseketta. 

Nappasin kategoriavalikon pyynnön zapilla. Vaihdoin GET pyynnön "category" filtterin. ```'+UNION+SELECT+BANNER,+NULL+FROM+v$version--```

SQL ```UNION``` antaa meidän tehdä uuden sql SELECT komennon valmiin selectin sisällä. Normaalisti UNION komentoa käytetään useamman select lauseen liittämiseen toisiinsa.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/10b24ed8-2cda-412b-ac67-366161a05b8e)

Uusi filtteri saa palvelimen vastaamaan sen versionumerolla.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/e203f565-f8bc-48b4-b141-82afa699b9d4)


### f) SQL injection attack, querying the database type and version on MySQL and Microsoft

Tässä hyökkäyksessä käytetään samaa ```UNION SELECT``` lauseketta, kuin aikaisemmassa tehtävässä

Tähän lisätään haku, jolla nähdään tietokannan versio. 

Löysin [täältä](https://portswigger.net/web-security/sql-injection/cheat-sheet) , millä saadaan tietokannan versio selvitettyä.

```'+UNION+SELECT+@@version,+NULL#```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/90c7d119-23e3-407b-bd22-3982d5a47cc2)

Joka palauttaa tietokannan version 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/831a49a7-2bf7-484e-9ce1-aa456836f375)

### g) SQL injection attack, listing the database contents on non-Oracle databases

Sama kategorian hakupyyntö napataan zapilla. 

Miten selvitän kaiken tietokannan tiedon? Katsoin apua tehtävän solution kohdasta. 

```+UNION+SELECT+table_name,NULL+FROM+all_tables--``` on oikea skripti, kokeilen sitä.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1b862c34-c945-419f-8243-ebd3e4baad19)

Haku palauttaa kaikki SQL pöytien nimet. Pöytiä on paljon, mutta joukosta löytyi yksi kiinnostava, missä voi olla nimen perusteella käyttäjien tiedot

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/d2b23b96-8db0-437f-b557-59f3c49cd9c8)

Kehitetään uusi injektio, jossa käytetään tietokantataulun nimeä

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c44e31fe-ed3a-481a-936c-3f9f565b5a85)

Injektio palauttaa kaksi taulua. Toisessa käyttäjänimet ja toisessa salasanat

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1e9eb72c-b3d1-4e57-b4e0-098dde07473f)

Viimeisessä injektiossa käytetään kaikki oppimamme tieto kohteesta. ```'+UNION+SELECT+USERNAME_AULGXF,+PASSWORD_RKNXQL+FROM+USERS_IGMESX--```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/0476baae-e0bb-4095-9a4b-cb57375bbd80)

Saimme käyttäjien tiedot.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/14c1e2eb-ccec-4555-a76a-c8700dff594b)

Kirjaudutaan vielä administratorin tilille.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/f25b6d30-2754-4cdf-bd2d-f2ef8088eafe)

### h) SQL injection UNION attack, determining the number of columns returned by the query

Tehtävässä päätellään UNION injektiohyökkäyksellä, montako kolumnia haku palauttaa.

```'+UNION+SELECT+NULL--``` Tehtävässä lisätään null arvoja, kunnes palvelin palauttaa null arvoja

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/b5b74e50-73de-45f9-a3bd-4c630510d585)

Kolmen nullin kohdalla palvelin palautti normaalit tiedot 

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/a4cc7265-711b-4519-981a-027c235b7feb)

Neljällä nullilla tehtävä onnistui, mutta en saanut null arvoja palvelimelta

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/7b5b24e1-eb95-41f1-8fda-1f9838582f3e)

### i) SQL injection UNION attack, retrieving data from other tables

Testataan aluksi montako kolumnia tietokanta palauttaa

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c70e4ef2-0ad6-4a9d-8448-eeab19fb1223)

kaksi.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/65e87a33-f404-47c6-9d85-200e6cb765c4)

Käytetään samaa ```UNION SELECT``` lauseketta.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/06b88b7e-aa67-44e6-9e72-829bae0cdf55)

Palvelin palauttaa käyttäjien tiedot.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/1f06c179-55ec-4054-93ab-942d23b1a090)


### j) SQL injection UNION attack, retrieving multiple values in a single column

Sama tarkastus alkuun. ```'+UNION+SELECT+NULL,'abc'--```

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/552358bc-1a31-49a8-8450-74083586dbb2)

Tälläkertaa tietokanta palauttaa vain yhden kolumnin tietoa.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/c0904e5c-70f5-43cf-a5af-7a2a682f02e1)

Joudutaan hakemaan tietoa eritavalla, kun tietokanta palauttaa vain yhden kolumnin tietoa, eikä kaksi eli username ja password. Taulut pitää liittää yhteen. "||" eli pipet yhdistävät kahden haun tiedot. Lisäsin pipejen väliin välilyönnin, jotta tiedot olisivat helpommin luettavissa.

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/ba505157-2114-468d-9fe7-3fb06cfcab48)

Haku palauttaa käyttäjien tiedot

![image](https://github.com/LassiMik/Tunkeutumistestaus_ict4tn027-3012/assets/112076377/b9ad75f3-b4eb-497a-8ca5-3d790e7e56b6)


## Lähteet 

 - Tehtävät https://terokarvinen.com/2023/eettinen-hakkerointi-2023/

 - https://terokarvinen.com/2016/03/05/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/

 - https://owasp.org/www-project-top-ten/2017/A1_2017-Injection

 - https://portswigger.net/web-security/sql-injection

 - https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/

 - https://portswigger.net/web-security/sql-injection/cheat-sheet

 - https://www.ibm.com/docs/en/informix-servers/14.10?topic=expression-concatenation-operator























