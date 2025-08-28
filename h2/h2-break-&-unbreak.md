# Harjoitus 2: Break & unbreak
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h1-korkeat-standardit-lari

## x) Lue/katso/kuuntele ja tiivistä.
OWASP: OWASP Top 10: [A01 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
  - Rikkinäinen pääsynhallinta oli vuoden 2021 yleisin haavoittuvuus verkkosovelluksissa.
  - Pääsynhallinnan tehtävänä on varmistaa, että käyttäjät eivät voi toimia asetettujen oikeuksien ulkopuolella. Rikkinäisessä pääsynhallinnassa tämä rajoitus on mahdollista ohittaa joko vahingossa tai tahalleen.
  - Rikkinäinen pääsynhallinta voi johtaa esimerkiksi luottamuksellisen tai arkaluontoisen tiedon paljastumiseen, sovellusrajapintoihin pääsyyn, tietojen manipulintoiin tai sovelluksen muokkaamiseen.

Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
  - Verkkopalvelimet sisältävät lähes aina piilotettuja hakemistoja, joiden etsimisessä voidaan käyttää fuzzausta.
  - ffuf on fuzzaukseen kehitetty työkalu.
  - Asenna ffuf ja hae sopiva sanalista, jonka jälkeen aloita fuzzaus:

        ffuf -u https://127.0.0.1/FUZZ -w common.txt

PortSwigger: [Access control vulnerabilities and privilege escalation](https://portswigger.net/web-security/access-control)

Karvinen 2006: [Raportin kirjoittaminen](https://terokarvinen.com/2006/raportin-kirjoittaminen-4/)

## a) Murtaudu 010-staff-only. Ks. Karvinen 2024: Hack'n Fix

## b) Korjaa 010-staff-only haavoittuvuus lähdekoodista. Osoita testillä, että ratkaisusi toimii.

## c) Ratkaise dirfuzt-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf. Tämä auttaa 020-your-eyes-only ratkaisemisessa.

## d) Murtaudu 020-your-eyes-only. Ks. Karvinen 2024: Hack'n Fix

## e) Korjaa 020-your-eyes-only haavoittuvuus. Osoita testillä, että ratkaisusi toimii.
