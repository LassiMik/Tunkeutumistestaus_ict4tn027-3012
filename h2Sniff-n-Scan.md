# h2Sniff-n-Scan


## x) Lue/katso ja tiivistä

### Hoikkala "joohoi" 2020: Still Fuzzing Faster (U fool)

 - Kaikki nettisurffaus koostuu HTTP get pyynnöistä
 - Web fuzzer käyttää samoja get pyyntöjä, mutta muokkaa niitä hieman ja lähettää miljoonia pyyntöjä. Fuzzer yrittää näillä tunnistaa poikkeamia palvelinten vastauksissa.
 - Get pyyntöjen fuzzauksessa jokin osa pyynnöstä vaihdetaan "fuzz" termiksi ja fuzzer kokeilee millä kaikilla termeillä saa vastauksen palvelimelta. Tätä yleensä käytetään HTTP get pyyntöjen parametreissä tai headereissa. Myös post pyyntöjen dataa voi fuzzata. 
