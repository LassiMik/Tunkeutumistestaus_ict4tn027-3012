# h5 Injected Sequel

## x) Lue/katso/kuuntele ja tiivistä.

### Karvinen 2016: PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu

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

### OWASP 2017: A1:2017-Injection

 - OWASP:in artikkeli A1:2017-injection kertoo injektioheikkoudesta, missä ei tunnistettu käyttäjä voi syötää haitallista tekstiä
mihin vain tekstikenttiin tai selaimen hakukenttään. 

 - Heikkoudelle on oleellista, että palvelin ei tarkasta tai filtteröi käyttäjän tekstiä sekä suorittaa komennot käyttäjän syötteellä.

 - Uudemmissa versioissa tietokannan hallintajärjestelmistä injektioheikkouksia on vähemmän. 

 - Injektiohaavoittuvuuksia löytyy usein SQL-, LDAP-, XPath- tai NoSQL-kyselyistä, käyttöjärjestelmäkomennoista, XML-parsereita, SMTP-headereista, expression kielistä ja ORM-kyselyistä.

- Injektiohaavoittuvuus voi johtaa tietojen katoamiseen, -korruptoitumiseen tai -paljastamiseen luvattomille osapuolille, vastuun menettämiseen tai pääsyn epäämiseen. Injektio voi joskus johtaa täydelliseen isäntäkoneen haltuunottoon.

### PortSwigger Academy: SQL injection


































