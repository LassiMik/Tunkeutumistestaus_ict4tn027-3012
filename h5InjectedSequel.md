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
"SELECT * FROM Users WHERE UserId = " + käyttäjän_syöte
```

Ohjelmassa on valmiina lainausmerkkien sisällä oleva osio ja tietokantaan syötetään suoraan käyttäjän syöte
Jos käyttäjän syöte on esimerkiksi ```1 OR 1=1```


## Lähteet 

 - https://terokarvinen.com/2016/03/05/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/

 - https://owasp.org/www-project-top-ten/2017/A1_2017-Injection

 - https://portswigger.net/web-security/sql-injection

 - https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/























