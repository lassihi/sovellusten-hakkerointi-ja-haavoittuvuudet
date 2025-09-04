# Harjoitus 3: No strings attached
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h3-no-strings-attached-tero

## a) Strings. Lataa ezbin-challenges.zip Aja 'passtr'. Selvitä oikea salasana 'strings' avulla. Selvitä myös lippu.
Asensin harjoitukset ja purin zip tiedoston.

<img width="181" height="100" alt="image" src="https://github.com/user-attachments/assets/283f6f3a-3ec5-4d6c-9dee-aeadbe974198" />

Vaihdoin hakemistoa ja ajoin ohjelman.

    cd challenges/passtr

<img width="206" height="92" alt="image" src="https://github.com/user-attachments/assets/b19b077b-1f60-4c7d-bf83-521badb1534d" />

Tutkin seuraavaksi passtr ohjelman binääriä strings ohjelmalla.

<img width="265" height="467" alt="image" src="https://github.com/user-attachments/assets/2e42f87a-edd4-4d92-9316-5e4331a78cfb" />

Strings tulosti seuraavat mielenkiintoiset rivit binääristä.

    What's the password?
    %19s
    sala-hakkeri-321
    Yes! That's the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}

Rivit näyttäisivät sisältävän salasanan, sekä lipun. Kokeilin antaa salasanan passtr ohjelmalle.

<img width="360" height="58" alt="image" src="https://github.com/user-attachments/assets/5f414c8e-453a-4bc5-aca7-b2ca11895d95" />

Salasana oli oikein, kuten myös lippu.

## b) Tee passtr.c -ohjelmasta uusi versio, jossa salasana ei näy suoraan sellaisenaan binääristä. Osoita testillä, että salasana ei näy. (Obfuskointi riittää.)

Avasin lähdekoodin microlla.

    micro passtr.c

<img width="487" height="190" alt="image" src="https://github.com/user-attachments/assets/5af2842a-1879-4d7a-a86b-5d5f4d048479" />

C on itselleni täysin tuntematon, joten käytin artikkelin https://yurisk.info/2017/06/25/binary-obfuscation-string-obfuscating-in-C/index.html ohjeita salasanan obfuskoimiseksi.
Lopullinen ohjelma näytti seuraavanlaiselta.

<img width="526" height="337" alt="image" src="https://github.com/user-attachments/assets/ba7d76ca-aca4-4d21-a7b8-f520a238b323" />

HIDE_LETTER lisää jokaiseen salasanan merkkiin täysluvun, jolloin merkeistä tulee binäärisotkua kun ohjelma käännetään. Kun ohjelma ajetaan ja salasanaa halutaan käyttää, 
UNHIDE_STRING tekee käänteisen operaation jokaiselle merkille. HIDE_STRING taas obfuskoi salasanan, kun sitä ei enää tarvita mustissa.

(https://yurisk.info/2017/06/25/binary-obfuscation-string-obfuscating-in-C/index.html)

Tarkastin löytääkö strings salasanan:

<img width="269" height="462" alt="image" src="https://github.com/user-attachments/assets/5eb34d87-d540-4a78-a5bf-8b3134d3f688" />

Salasanaa, eikä mitään sen merkkejä näe enää suoraan binääristä.

## c) Packd. Aja 'packd' paketista ezbin-challenges.zip. Mikä on salasana? Mikä on lippu? (Tämä tehtävä on hieman haastavampi. Kirjaa ylös kokeilemasi lähestymistavat ja keksimäsi hypoteesit. Toivottavasti pääset itse maaliin, mutta jos et, läpikävely paljastuu tunnilla...)

Ulkoa päin packd on hyvin samanlainen, kuin passtr.

<img width="327" height="211" alt="image" src="https://github.com/user-attachments/assets/7367deb0-fa95-424f-ab98-54bd4c5f3199" />

Tutkin jälleen binäärin strings:n avulla.

    strings packd

Alla mielenkiintoisimpia rivejä tulosteesta.

<img width="480" height="264" alt="image" src="https://github.com/user-attachments/assets/a5912f16-76a9-4b72-9e52-13e342f46217" />

Näistä huomion herätti erityisesti mahdollinen lippu/osa lipusta, kommentit UPX packerista ja jokin hakemisto /proc/self/exe. Päätin tutkia UPX packeria tarkemmin. DuckDuckGo antoi hakusanalla "UPX packer" ehdotukseksi videon "[Unpacking UPX malware](https://www.youtube.com/watch?v=8scn4PKry-o)", joka kuulosti lupaavalta.
Nopealla vilkaisulla huomasin, että videosta ei juurikaan muuten ole hyötyä kuin vain todistamaan, että olen oikeilla jäljillä.

Löysin artikkelin https://tech-zealots.com/reverse-engineering/dissecting-manual-unpacking-of-a-upx-packed-file/, joka ehdotti UPX pakkauksen purkamista optiolla -d. Huomasin samalle, että UPX on valmiksi kaliin ladattu työkalu.

Tein kopion packd tiedostosta ja purin alkuperäisen packd tiedoston UPX:n avulla.

<img width="494" height="223" alt="image" src="https://github.com/user-attachments/assets/45be93ad-512b-4201-a485-24e31fe5e518" />

Tutkin puretun tiedoston strings:n avulla.

<img width="373" height="473" alt="image" src="https://github.com/user-attachments/assets/3d0ed83c-969d-4a86-935e-75852e2367f1" />

Tulosteessa näkyi tällä kertaa lippu kokonaisena, sekä mahdollinen salasana "piilos-AnAnAs".

Kokeilin salasanaa alkuperäisen packd tiedoston kopiossa.

<img width="436" height="68" alt="image" src="https://github.com/user-attachments/assets/eff1b881-d176-412b-9ebd-0695b6a54e52" />

Salasana oli oikein.

## d) Vapaaehtoinen bonus: Cryptopals. Crypto Challenge Set 1. Tätä voi tehdä useamman viikon bonuksena. Jos saat ratkaistua kohdat 1 .. "4. Detect single-character XOR", olet jo astunut salakirjoituksen maailmaan.

Tein seuraavat tehtävät eri koneella, sillä virtuaalikoneella ohjelmoiminen on aiemmin tuottanut tuskaa.

### Challenge 1:

<img width="493" height="208" alt="image" src="https://github.com/user-attachments/assets/e00e1000-77e6-4903-8db8-137bdb758281" />

Ilmeisesti haasteessa pitää kirjoittaa ohjelma, joka vastaa tehtävänantoon. Tein seuraavan koodin.

<img width="236" height="48" alt="image" src="https://github.com/user-attachments/assets/5e7e075d-7cb6-4ee8-a8e5-6ef31f1e33f2" />

Ajoin ohjelman.

<img width="622" height="36" alt="image" src="https://github.com/user-attachments/assets/31a8e792-7cec-45f8-aedf-ebf49882c878" />

Tulos ei ollut sama kuin olisi pitänyt, joka johtui siitä, että se ei ollut base64. Muutin koodia hieman.

<img width="260" height="110" alt="image" src="https://github.com/user-attachments/assets/68ce45d4-6214-4e36-826d-4b6081bffd5d" />

Ajoin ohjelman uudestaan.

<img width="622" height="50" alt="image" src="https://github.com/user-attachments/assets/8d742da8-8016-4359-8636-2331b63c0ae1" />

Sain tulostettua oikean tekstin.

(Jälkeenpäin katsottuna koodissa oli muutama muukin virhe, mutta se kuitenkin tulosti oikean tekstin :) )

## Challenge 2:

<img width="501" height="190" alt="image" src="https://github.com/user-attachments/assets/1fcdcdbe-aeb3-439b-b502-e5ebade3cdf0" />

Sain seuraavan koodin luotua.

<img width="464" height="111" alt="image" src="https://github.com/user-attachments/assets/13766463-25c2-40fb-adef-8027bc8db0a7" />

Muuttujille bytes1 ja string2 ei kuitenkaan voinut suorittaa XOR operaatiota, sillä ne olivat eri tyyppiä (bytes ja str). Olin myös antanut saman merkkijonon kahdesti.
Challenge 1 mainitsi, että kaikki operaatiot tulee tehdä tehtävissä tavuille, joten muutin toisenkin muuttujan tavuksi ja stackoverflown https://stackoverflow.com/questions/29408173/byte-operations-xor-in-python avulla sain suoritettua XOR operaation niille. Lopuksi muutin tuloksen takaisin heksadesimaaliksi.

<img width="465" height="170" alt="image" src="https://github.com/user-attachments/assets/904ccb50-b841-4a2f-b75e-c47d896b8efc" />

Ohjelma tuotti oikean tuloksen:

<img width="332" height="26" alt="image" src="https://github.com/user-attachments/assets/1ce0dc33-1abd-4db8-9ca0-47270d33cd6d" />
