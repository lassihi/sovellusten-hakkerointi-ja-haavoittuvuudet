# Harjoitus 4: Kääntöpaikka
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h4-kaantopaikka-tero

## Suoritusympäristö:
Tietokone: Lenovo Legion Y540-15IRH kannettava kytkettynä langallisesti kotiverkkoon. \
-Intel Core i7-9750H \
-NVIDIA Geforce RTX 2060 6GB \
-16GB DDR4 2666MHz \
Käyttöjärjestelmä: Kali Linux (VMWare virtuaalikone)

## x) Lue/katso/kuuntele ja tiivistä.

Hammond 2022: Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat') (Video, noin 20 min)
- Ghidra asennettiin githubista (https://github.com/NationalSecurityAgency/ghidra), jonka jälkeen asennettiin oikea jdk-ympäristö.
- CTF-ohjelma avattiin Ghidralla ja ohjelmasta etsittiin arvo, johon käyttäjän syötettä verrataan.
- Löydetty heksadesimaaliarvo muutettiin desimaaliksi pythonilla.
- Desimaaliarvo syötettiin ohjelmalla, jolla lippu saatiin ratkaistua.

## a) Asenna Ghidra.

Kalilla Ghidra löytyy paketinhallinnasta.

    sudo apt-get update
    sudo apt-get install ghidra

Käynnistin Ghidran varmistaakseni, että asennus onnistui.

<img width="252" height="415" alt="image" src="https://github.com/user-attachments/assets/fb0247be-d13f-4507-bcc4-8b4ba184ef5d" />

Sovellus aukesi oikein.

## b) rever-C. Käänteismallinna packd-binääri C-kielelle Ghidralla. Etsi pääohjelma. Anna muuttujielle kuvaavat nimet. Selitä ohjelman toiminta. Ratkaise tehtävä binääristä, ilman alkuperäistä lähdekoodia. ezbin-challenges.zip'

Packd-binääri on viime viikon harjoituksesta tuttu. Binääri oli pakattu upx-packerilla ja kun pakkauksen sai purettua löytyi stringsin avulla salasana lippuun.

Ennen kuin binääriä lähtee staattisesti analysoimaan, tulee pakkaus purkaa, jotta saadaan esille oikea binääri.

Purin binäärin pakkauksen.

<img width="516" height="184" alt="image" src="https://github.com/user-attachments/assets/e5f6b2f0-265c-4b54-8798-d716d77794bb" />

Avasin ghidran komennolla `ghidra`. Loin uuden projektin oikeasta yläkulmasta File -> New Project.

<img width="477" height="242" alt="image" src="https://github.com/user-attachments/assets/99a8e8bb-3759-4e79-bd89-6ccffb50cbfb" />

Loin projektille uuden hakemiston.

    mkdir /home/kali/ghidra/packd

Ja tein projektin.

<img width="294" height="185" alt="image" src="https://github.com/user-attachments/assets/c2067483-d2a6-40ab-854c-a51aaecd0a34" />

Projektinäkymässä lisäsin projektiin uuden tiedoston klikkaamalla 'i' näppäintä ja navigoimalla packd binääriin.

Lisäsin binäärin oletusasetuksilla.

<img width="255" height="141" alt="image" src="https://github.com/user-attachments/assets/7fef9a9c-d0d6-488b-8c38-2bbde01b8d03" />

Tuplaklikkasin packd-tiedostoa sen analysoimiseksi ja vastasin "Yes".

<img width="264" height="70" alt="image" src="https://github.com/user-attachments/assets/aeb38912-9988-433a-9aa3-0e4df8f4d93f" />

Analyysin tein oletusasetuksilla.

Analyysin jälkeen Ghidra esitti suoraan main()-funktion eli pääohjelman ilman, oikealla puolella ikkunaa.

<img width="815" height="462" alt="image" src="https://github.com/user-attachments/assets/4acb93db-5ef4-4f38-ba1c-e671eda61971" />

Binäärissä näyttäisi olevan kaksi muuttujaa, int tyypin muuttuja iVar1 ja char tyypin muuttuja local_28. iVar1 muuttujalle nimesin cmp_result ja local_28 nimesin user_input, jotta ne kuvaisivat toimintaa oikein.

<img width="373" height="158" alt="image" src="https://github.com/user-attachments/assets/4191078e-809b-49e0-966f-39df171f0c1c" />

Ohjelman toiminta suoritusjärjestyksessä:

- `undefined8 main(void)`
  - Luo määrittämättömän tyyppisen pääohjelman, joka ei palauta arvoa.
- `int cmp_result;`
  - Alustaa kokonaisluku tyyppisen muuttujan "cmp_result".
- `char user_input [32]`
  - Alustaa 32 merkkiä pitkän merkkijonon "user_input".
- `puts("Wha\'s the password?");`
  - Kutsuu funktiota puts(), jolle annetaan parametriksi "Wha\'s the password?"
  - puts() tulostaa annetun merkkijonon
- `__isoc99_scanf(&DAT_0010201d, user_input)`
  - Kutsuu funktiota __isoc99_scanf(), jolle annetaan parametreiksi &DAT_0010201d, user_input.
  - __isoc99_scanf() on gcc kääntäjän versio scanf() funktiosta (luetaan käyttäjän antama syöte).
  - &DAT_0010201d on todennäköisesti pointer, joka määrittää mistä syöte luetaan.
  - user_input on muuttuja, johon syöte asetetaan.
- `cmp_result = strcmp(user_input, "piilos-AnAnAs")`
  - Syöttää cmp_result muuttujaan funktion strcmp(user_input, "piilos-AnAnAs") palautusarvon.
  - funktio strcmp() vertaa kahta merkkijonoa ja palauttaa arvoksi 0, jos ne vastaavat toisiaan.
- `if (cmp_result == 0)`
  - Ajaa if-ehdon alla olevan koodin, jos cmp_result == 0 saa boolean arvon True, muuten ajaa else-ehdon alla olevan koodin.
  - 
