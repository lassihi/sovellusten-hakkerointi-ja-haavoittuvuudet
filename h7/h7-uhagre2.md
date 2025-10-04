# Harjoitus 7: Uhagre2
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h7-uhagre2-tero

## Suoritusympäristö:
Tietokone: MacBook Air M2 16GB \
Käyttöjärjestelmä: MacOS Tahoe 26.0.1 \
Python 3.13.7

## x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
* Schneier 2015: Applied Cryptography, 20ed: Chapter 1: Foundations:
    * 1.1 Terminology ("Historical Terms" loppuun)
      * Viestien salaamista kutsutaan **kryptografiaksi**. Salaamisen lisäksi kryptografiaa hyödynnetään usein myös todennuksessa, sekä eheyden ja kiistämättömyyden varmistamisessa. **Kryptoanalyysi** on salakirjoituksen purkamiseen keskittynyt tieteenala. **Kryptologia** on matematiikan haara, joka käsittää kryptografian ja kryptologian.
      * Kryptografiset algoritmit (salaukset/salausalgoritmit) ovat matemaattisia funktioita, joita käytetää salaamiseen ja salauksen purkamiseen.
      * Toimintaperiaatteiltaan julkisten salausalgoritmien turvallisuus perustuu täysin avaimeen (yhteen tai useampaan). Symmetrisessä algoritmissa sama avain salaa ja purkaa viestin, kun taas epäsymmetrisessä algoritmissa viesti salataan eri avaimella (julkinen avain), kuin se puretaan (yksityinen avain). 
    * 1.4 Simple XOR
        * Exclusive OR on looginen operaatio. XOR saa arvon, jos vain toinen sille annetuista arvoista on tosi, muuten saadaan arvoksi epätosi.
        * XOR operaatiota on käytetty monissa kaupallisissa ohjelmissa salaukseen.
        * Tosiasiassa yksinkertaisen XOR salauksen purku on triviaalia.
    * 1.7 Large Numbers
        * Suurien lukujen havainnollistamiseksi annettu vertauskuvia.
            * Todennäiköisyys kuolla autokolarissa (Yhdysvalloissa elinaikana) 2^7.
            * Aikaa seuraavaan jääkauteen 2^14 vuotta.
            * Todennäköisyys kuolla salamaniskuun päivittäin 2^33.
            * Atomien määrä planeetassa 2^170.
            * Universumin tilavuus 2^280
* Karvinen 2024: Python Basics for Hackers
   * Pythonissa ydintoiminta ja pienet muutokset kannattaa testata interaktiivisella tulkilla.
   * Micro-editorilla F5 mahdollistaa ohjelman ajamisen.
   * `ord()`, `hex()` ja `bin()` -funktiot auttavat muunnoksissa.
   * Hyödyllisiä kirjastoja ovat requests, binascii ja base64.

## Ratkaise CryptoPals Set 1 -haasteet. Tehtävät saa ratkaista millä vain ohjelmointikielellä ja käyttää mitä tahansa tekstieditoria tai IDE:ä. Tehtäviä ei kannata ratkaista tekoälyllä, koska se vain kopioi malliratkaisun suoraan koulutusmateriaalistaan.

### a) 1. Convert hex to base64.

Tehty [h3 lisätehtävänä](https://github.com/lassihi/sovellusten-hakkerointi-ja-haavoittuvuudet/blob/main/h3/h3-no-strings-attached.md#d-vapaaehtoinen-bonus-cryptopals-crypto-challenge-set-1-tätä-voi-tehdä-useamman-viikon-bonuksena-jos-saat-ratkaistua-kohdat-1--4-detect-single-character-xor-olet-jo-astunut-salakirjoituksen-maailmaan).

### b) 2. Fixed XOR.

Tehty [h3 lisätehtävänä](https://github.com/lassihi/sovellusten-hakkerointi-ja-haavoittuvuudet/blob/main/h3/h3-no-strings-attached.md#d-vapaaehtoinen-bonus-cryptopals-crypto-challenge-set-1-tätä-voi-tehdä-useamman-viikon-bonuksena-jos-saat-ratkaistua-kohdat-1--4-detect-single-character-xor-olet-jo-astunut-salakirjoituksen-maailmaan).

### c) 3. Single-byte XOR cipher.

Tehty [h3 lisätehtävänä](https://github.com/lassihi/sovellusten-hakkerointi-ja-haavoittuvuudet/blob/main/h3/h3-no-strings-attached.md#d-vapaaehtoinen-bonus-cryptopals-crypto-challenge-set-1-tätä-voi-tehdä-useamman-viikon-bonuksena-jos-saat-ratkaistua-kohdat-1--4-detect-single-character-xor-olet-jo-astunut-salakirjoituksen-maailmaan).

### d) 4. Detect single-character XOR.

<img width="904" height="323" alt="image" src="https://github.com/user-attachments/assets/4ff0185c-d0de-476f-a3f6-ab63c4b7819d" />

Tehtävänä harjoituksessa on löytää liitteenä olevasta tiedostosta 60 merkkiä pitkä teksti, joka vasten on tehty yhtä merkkiä käyttäen XOR operaatio.

Päätin luoda 3. cryptopals harjoituksen koodia käyttäen uuden ohjelman, joka tekee tiedoston jokaiseen riviin XOR operaation jokaista merkkiä kohden ja palauttaa niistä parhaiten englannin kielen kirjaintiheyttä vastaavan rivin.

Tallensin tehtävän tiedoston samaan hakemistoon, kuin mihin loin ohjelman.

<img width="937" height="92" alt="image" src="https://github.com/user-attachments/assets/0f28231c-a7aa-4617-bac1-899bbd8062d8" />

Cryptopals challenge 3:n muokattu koodi. Muokkauksiin kuuluu pääasiassa koko utf-8 merkkiavaruuden käyttö avaimena, vain parhaan tuloksen palauttaminen ja "ei-perinteisistä" merkeistä rankaiseminen tuloksessa.
<img width="1017" height="847" alt="image" src="https://github.com/user-attachments/assets/3b783a94-afc1-40ff-8b05-c6cb059b7d4e" />

Uusi challenge 4 ohjelma, joka ottaa sisään tiedoston sisällön, syöttää sen jokaisen rivin challenge 3 ohjelmalle ja palauttaa vastaukset pistejärjestyksessä.
<img width="1213" height="407" alt="image" src="https://github.com/user-attachments/assets/12aa807e-1a6c-498b-bb40-caaf9773b149" />

Challenge 4 ohjelman antamat top 10 tulokset.
<img width="1017" height="606" alt="image" src="https://github.com/user-attachments/assets/2050f5c8-113b-4f11-aaa0-a420f086fa17" />

Tuloksista huomataan, että rivi 7b5a4215415d544115415d5015455447414c155c46155f4058455c5b523f sisältää viestin "Now that the party is jumping\n". Viestille on suoritettu XOR operaatio merkin "5" kanssa.

Tulos ei ollut ensimmäisenä, sillä on hyvin mahdollista, että satunnaisesti luotu merkkijono voi frekvenssianalyysin jälkeen täsmätä paremmin normaalia englannin kielen frekvenssiä kuin oikea teksti, etenkin kun analyysissä huomioidaan vain aakkoset (kuten olen itse tehnyt).

### e) Vapaaehtoinen, suositeltava: 5. Implement repeating-key XOR.

<img width="1213" height="593" alt="image" src="https://github.com/user-attachments/assets/c6bd6c23-4fab-4e96-a9cb-edaa0175ddba" />

Eli tekstin jokaiselle tavulle tehdään XOR operaatio avaimen vastaavan tavun kanssa. Teksti on pitempi kuin avain, joten kun avaimen jokainen tavu on käyty läpi, niin aloitetaan taas avaimen alusta.

Ohjelmasta tuli yllättävän yksinkertainen.
<img width="1470" height="474" alt="image" src="https://github.com/user-attachments/assets/de23c7a7-c9d1-44d4-ac20-72187605b6a8" />

<img width="1470" height="88" alt="image" src="https://github.com/user-attachments/assets/9b9ef804-f958-44dc-82fb-6936363491f5" />

### g) Vapaaehtoinen: 6. Break repeating-key XOR.

<img width="734" height="812" alt="image" src="https://github.com/user-attachments/assets/14456b98-acbe-4148-9451-ff93a0a9c054" />

Latasin tiedoston.

<img width="1083" height="91" alt="image" src="https://github.com/user-attachments/assets/4547546f-abb6-44a5-b40d-df92dca0a7f2" />

