# Harjoitus 7: Uhagre2
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h7-uhagre2-tero

## Suoritusympäristö:
Tietokone: Lenovo Legion Y540-15IRH kannettava kytkettynä langallisesti kotiverkkoon. \
-Intel Core i7-9750H \
-NVIDIA Geforce RTX 2060 6GB \
-16GB DDR4 2666MHz \
Käyttöjärjestelmä: Debian 12, Kali Linux (VMWare virtuaalikone)

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
