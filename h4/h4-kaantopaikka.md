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

Hammond [2022: Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat')](https://www.youtube.com/watch?v=oTD_ki86c9I&t=885s)(Video, noin 20 min)
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

Analyysin jälkeen Ghidra esitti suoraan main-funktion eli pääohjelman oikealla puolella ikkunaa.

<img width="815" height="462" alt="image" src="https://github.com/user-attachments/assets/4acb93db-5ef4-4f38-ba1c-e671eda61971" />

Binäärissä näyttäisi olevan kaksi muuttujaa, int tyypin muuttuja iVar1 ja char tyypin muuttuja local_28. iVar1 muuttujalle nimesin cmp_result ja local_28 nimesin user_input, jotta ne kuvaisivat toimintaa oikein.

<img width="373" height="158" alt="image" src="https://github.com/user-attachments/assets/4191078e-809b-49e0-966f-39df171f0c1c" />

Ohjelman toiminta suoritusjärjestyksessä:

- `undefined8 main(void)`
  - Luo määrittämättömän tyyppisen pääohjelman, joka ei ota parametria.
- `int cmp_result;`
  - Alustaa kokonaisluku tyyppisen muuttujan "cmp_result".
- `char user_input [32]`
  - Alustaa 32 merkkiä pitkän merkkijonon "user_input".
- `puts("Wha\'s the password?");`
  - Kutsuu funktiota puts(), jolle annetaan parametriksi "What\'s the password?"
  - puts() tulostaa annetun merkkijonon
- `__isoc99_scanf(&DAT_0010201d, user_input)`
  - Kutsuu funktiota __isoc99_scanf(), jolle annetaan parametreiksi &DAT_0010201d, user_input.
  - __isoc99_scanf() on gcc kääntäjän versio scanf() funktiosta (luetaan käyttäjän antama syöte). (https://stackoverflow.com/questions/16376341/isoc99-scanf-and-scanf)
  - &DAT_0010201d on todennäköisesti pointer yms., joka määrittää mistä kohdasta syöte luetaan.
  - user_input on muuttuja, johon syöte asetetaan.
- `cmp_result = strcmp(user_input, "piilos-AnAnAs")`
  - Syöttää cmp_result muuttujaan funktion strcmp(user_input, "piilos-AnAnAs") palautusarvon.
  - funktio strcmp() vertaa kahta merkkijonoa ja palauttaa arvoksi 0, jos ne vastaavat toisiaan.
- `if (cmp_result == 0) {puts("Yes! That\'s the password. FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}");`
- `else {puts("Sorry, no bonus.");}`
  - Ajaa if-ehdon alla olevan koodin, jos cmp_result == 0 saa arvon True, muuten ajaa else-ehdon alla olevan koodin.
-  `return 0`
  - Ohjelma palauttaa arvon 0, jos se on ajettu onnistuneesti.

## c) Jos väärinpäin. Muokkaa passtr-ohjelman binääriä (ilman alkuperäistä lähdekoodia) niin, että se hyväksyy kaikki salasanat paitsi oikean. Osoita testein, että ohjelma toimii. ezbin-challenges.zip
Suljin Ghidran ja avasin sen uudestaan. Loin uuden projektin "passtr", hakemistoon ~/ghidra/passtr/. Lisäsin alkuperäisen passtr binäärin projektiin ja analysoin sen automaattisesti Ghidralla.

<img width="816" height="449" alt="image" src="https://github.com/user-attachments/assets/c6d7b074-4087-494a-8c22-1f64d2fa4e7a" />

Passtr on hyvin saman tyylinen kuin packd. Vain salasana ja lippu eroavat toisistaan.

Tunnilla käytiin samantyylinen esimerkki läpi, jossa if lauseen logiikka voitiin kääntää, kun jotain verrattiin nollaan. Tämä tehtiin vaihtamalla x86 assembly ohje JNZ (jump in not zero) ohjeeseen JZ (jump if zero).

Tuplaklikkaamalla decompilerin if lausetta, antoi Ghidra oikean kohdan assembly koodissa.

<img width="432" height="178" alt="image" src="https://github.com/user-attachments/assets/56992b0a-fcca-4504-b4ff-7b220cee30be" />

"JNZ" riviä klikkaamalla nähdään, että jos arvo ei ole nolla (eli jos annettu salasana ei täsmää oikeaa salasanaa), niin ohjelma hyppää tulostamaan "Sorry, no bonus."

<img width="311" height="122" alt="image" src="https://github.com/user-attachments/assets/fae5c3ad-72c9-4d1e-b136-b8b1a64ccef6" />

Huomasin myös, että Ghidra osaa visualisoida tämän kun klikkasi "Display Function Graph" työkaluriviltä.

<img width="306" height="350" alt="image" src="https://github.com/user-attachments/assets/4355f33b-0ef5-4bc9-b7b4-1180fc46c43f" />

Käänsin if-else lauseen logiikan toisin päin vaihtamalla Patch Instruction ominaisuuden avulla "JNZ" tilalle "JZ".

Muutoksen jälkeen uusi logiikka näkyi myös decompilerin c-koodissa.

<img width="693" height="226" alt="image" src="https://github.com/user-attachments/assets/3dbad489-4153-4686-9d2c-700d87bd04d4" />

Loin uuden hakemiston muokatuille binääreille, `mkdir ~/ghidra/patched/`. Tallensin muokatun binäärin sinne, Export Program -> Format: Original file.

Lopuksi kävin tarkastamassa, että tiedosto tallentui oikein, annoin siihen ajo-oikeudet ja varmistin, että ohjelma tulostaa lipun, kun sille antaa väärän salasanan.

<img width="535" height="111" alt="image" src="https://github.com/user-attachments/assets/21749cc0-302b-4dce-bfb7-5af14d1ae513" />

Annoin vielä oikean salasanan, jolloin tulostui ilmoitus väärästä salasanasta, niin kuin oli tarkoitus.

<img width="150" height="47" alt="image" src="https://github.com/user-attachments/assets/254bdd9f-d831-4791-8c45-6e040333d575" />

## d) Nora CrackMe: Käännä binääreiksi Tindall 2023: NoraCodes / crackmes. Lue README.md: älä katso lähdekoodeja, ellet tarvitse niitä apupyöriksi. Näissä tehtävissä binäärejä käänteismallinnetaan. Binäärejä ei muokata, koska muutenhan jokaisen tehtävän ratkaisu olisi vaihtaa palautusarvoksi "return 0".

Kopion repositoryn githubista `git clone https://github.com/NoraCodes/crackmes.git `ja käänsin harjoitukset binääriksi `make`.

<img width="425" height="414" alt="image" src="https://github.com/user-attachments/assets/0d62b72b-8dcd-4990-97ef-3d2f0ff28c0d" />

README.md:n perusteella tarkoitus on saada harjoitukset palauttamaan status koodin 0, käänteismallinnetun koodin perusteella.

## e) Nora crackme01. Ratkaise binääri.

Loin Ghidrassa uuden projektin, jonne lisäsin ensimmäisen binäärin "crackme01.64" ja suoritin analysoinnin.

<img width="845" height="434" alt="image" src="https://github.com/user-attachments/assets/3c10cdeb-cb4f-4785-94ba-97e6808a8b78" />

Näyttäisi heti siltä, että tekstistä salasana "password1" suoraan selkokielisenä. Jos salasana on oikein ohjelma palauttaa arvon 0.

<img width="217" height="42" alt="image" src="https://github.com/user-attachments/assets/24959f48-c8a3-462f-9913-decf98faafd9" />

Salasana oli oikein ja jos olisi tajunnut heti käyttää stringsiä olisi tehtävän saanut nopeammin ratkaistua.

## e) Nora crackme01e. Ratkaise binääri.
Tutkin hieman ohjelmaa ja se näyttäisi toimivan samalla periaatteella, kuin edellinen.

<img width="223" height="124" alt="image" src="https://github.com/user-attachments/assets/e8c26908-2949-4d2a-b200-41e3b13c9565" />

Edellisestä kohdasta viisaampana tutkin heti aluksi binäärin stringsillä.

<img width="273" height="250" alt="image" src="https://github.com/user-attachments/assets/3da25027-b12a-4900-bd0a-d245b27f5720" />

Tuloksista löytyi kiinnostava merkkijono "slm!paas.k". Zsh ei pitänyt huutomerkistä salasanan keskellä, mutta kun sen sai lisättyä merkkijonoon, niin tehtävä ratkesi.

<img width="287" height="126" alt="image" src="https://github.com/user-attachments/assets/5093396d-be97-4ab4-865c-ea7900cc0fc1" />

Varmistin vielä Ghidralla, että ohjelma palauttaa arvon 0, kun syöttää oikean salasanan.

<img width="656" height="414" alt="image" src="https://github.com/user-attachments/assets/86341645-533e-443c-a348-87b5c4e0b8d2" />

Näin oli.

## f) Nora crackme02. Nimeä pääohjelman muuttujat käänteismallinnetusta binääristä ja selitä ohjelman toiminta. Ratkaise binääri.

Aloitin avaamalla ohjelman Ghidralla.

<img width="658" height="382" alt="image" src="https://github.com/user-attachments/assets/ee319900-f87c-4b5a-83b1-dc07cf5278ae" />

Nimesin muuttujat seuraavalla tavalla:
* param_1 = cmd_params_num
* param_2 = input_param
* cVar1 = input_character
* cVar2 = password_character
* uVar3 = return_value
* lVar4 = counter

<img width="301" height="271" alt="image" src="https://github.com/user-attachments/assets/9d96c9c1-e68f-4496-8fa8-8280e69cb9fa" >

Ohjelman selitys suoritusjärjestyksessä:
-     undefined8 main(int cmd_params_num,long input_param)

    - Määritellään pääohjelma main, joka saa kaksi parametria. Ensimmäinen on komentoriviparametrien lukumäärä (esim. `./crackme02 salasana` = 2) ja toinen käyttäjän syöte.

-     char password_character;
      undefined8 return_value;
      long counter;
      char input_character;

    - Alustetaan neljä muuttujaa.

-     if (cmd_params_num == 2) {
      ....
      }
      else {
          puts("Need exactly one argument.");
          return_value = 0xffffffff;
      }

    - Tarkistetaan montako komentoriviparametria annettiin.
    - Jos 2, jatketaan if lauseen sisälle.
    - jos ei 2, niin suoritetaan else lauseen koodi.

-     password_character = 'p';
      counter = 0;

    - Asetetaan password_character muuttujaan arvo 'p'.
    - Asetetaan counter muuttujaan arvo 0.
 
- Do-while loopissa tarkastetaan onko while True.
    - Jos True suoritetaan do osio, kunnes while = False.

- WHILE:
    -     } while (password_character != '\0');
          printf("Yes, %s is correct!\n");
          return_value = 0;
        - Tarkastetaan onko password_character tyhjä merkki.
            - Jos tyhjä, niin kerrotaan vastauksen olevan oikein ja palautetaan 0.
            - Jos ei tyhjä, jatketaan do lauseeseen. 

- DO:
    -     input_character = *(char *)(*(long *)(input_param + 8) + counter);
        - En tarkalleen tiedä mitä jokainen asia tässä tekee, mutta päättelemällä asetetaan input_charecter muuttujan arvoksi input_param[counter]
    -     if (input_character == '\0') break;
        - Jos ollaan käyty koko input_param läpi, poistutaan loopista.
    -     if (password_character + -1 != (int)input_character) {
              printf("No, %s is not correct.\n");
              return 1;
          }
        - Jos password_character esitettynä kokonaislukuna -1, ei ole sama kuin input_character esitettynä kokonaislukuna, niin tulosta merkkijono ja palauta arvo 1.
    -     password_character = "password1"[counter + 1];
        - Muutetaan password_character arvoksi merkkijonon "password1" seuraava kirjain.
    -     counter = counter + 1;
        - Nostetaan loopin kierrosten laskuria yhdellä.
    - Tarkistetaan while-arvo ja mahdollisesti suoritetaan do uudestaan.
 
Selitys on hieman monimutkainen, mutta käytännössä ohjelma ratkeaa, kun syötetään merkkijono, jonka jokainen merkki esitettynä kokonaislukuna on salasanan "password1" vastaavan merkin kokonaislukuesitystä yhden pienempi.

Esim. 
- p = 112, joten 112-1 = 111 = 'o'
- a = 97, joten 97-1 = 96 = '`'
- s = 115, joten 115-1 = 114 = 'r'
- jne...

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/5151a7b6-510d-4cf3-877e-03fbc12155b3" />

Kuva: https://www.geeksforgeeks.org/dsa/ascii-table/

Näin saadaan "password1" vastaava merkkijono "o´rrvnqc0". Tämä kun syötetään ohjelmalle, saadaan binääri ratkaistua.

<img width="208" height="42" alt="image" src="https://github.com/user-attachments/assets/1c4a7884-7a3e-4c5e-9516-17b22c06b28e" />

## g) Vapaaehtoinen: Ja sen yli. Crackme01 on useampia ratkaisuja. Montako löydät? Miksi?
Kun alunperin tutkin tehtävää crackme01 huomasin, että annettua salasanaa verrataan oikean salasanaan funktiolla strncmp(string, "password01", 9). Strncmp:n kolmas parametri kertoo kuinka monta kirjainta merkkijonojen alusta verrataan toisiinsa (https://www.tutorialspoint.com/c_standard_library/c_function_strncmp.htm). Lopuilla kirjaimilla ei ole enää vertauksen kannalta väliä, joten kunhan vastaus alkaa "password1", niin sen perään voi lisätä minkä tahansa merkin tai merkkijonon ja saada oikean vastauksen. Ratkaisuja löytyy siis enemmän kuin mitä osaan itse laskea.

<img width="263" height="125" alt="image" src="https://github.com/user-attachments/assets/c8455cff-1f3b-4885-8e44-64d29832d629" />

## h) Vapaaehtoinen: Pyytämättäkin. Crackme02 on kaksi ratkaisua. Löydätkö molemmat?
Niin pitkään ehdin crackme02 käänteismallinnettua koodia tutkia, että löysin myös muutaman muun ratkaisun.

Ohjelma hyväksyy kaikki seuraavat merkkijonot ratkaisuina: "", "o", "o`", "o'r", ..., "o'rrvnqc", "o'rrvnqc0". Yhteensä ratkaisuja on 10.

Tämä johtuu siitä, että ohjelma lopettaa vastauksen tarkastamisen heti kun se kohtaa siinä tyhjän merkin.

<img width="320" height="269" alt="image" src="https://github.com/user-attachments/assets/d3bf9515-ee0e-44a8-8c48-d9245ca17566" />

<img width="294" height="164" alt="image" src="https://github.com/user-attachments/assets/f7941cb0-f299-42ce-a127-6b6377af060f" />

## Lähteet

Iso-Anttila, Karvinen: Sovellusten hakkerointi - 2025 alkusyksy: https://terokarvinen.com/sovellusten-hakkerointi/

Hammond 2022: Ghidra for Reverse Engineering: https://www.youtube.com/watch?v=oTD_ki86c9I&t=885s

Karvinen: Ezbin-challenges: https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip

https://stackoverflow.com/questions/16376341/isoc99-scanf-and-scanf

Tindall 2023: crackmes: https://github.com/NoraCodes/crackmes

GeeksForGeeks 2025: ASCII Values Alphabets ( A-Z, a-z & Special Character Table ): https://www.geeksforgeeks.org/dsa/ascii-table/

Tutorials Point: C Library - strncmp() function: https://www.tutorialspoint.com/c_standard_library/c_function_strncmp.htm
