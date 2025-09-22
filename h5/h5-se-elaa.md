# Harjoitus 5: Se elää!
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/

Tehtävänanto: Tiedostot Lab0-5.zip tiedostot nostattavat dynaamisen analyysin tekoa asteittain ja alussa teillä on käytössä lähdekoodit, jotka helpottavat gdb:n käyttöä ja analyysiä mutta lopussa lähdekoodi jää pois ja teillä on käytössänne vain pelkkä koodi. Kirjoittakaa lyhyt raportti siitä mitä teette ja mitä saitte selville tehtävistä Lab0 - 2. Lab3 ja 4 ovat vapaaehtoisia.

(Tehtävät tehty moodlessa olevalla tehtävänannolla, joka hieman eroaa Teron sivuilla olevasta tehtävänannosta.) 

https://terokarvinen.com/sovellusten-hakkerointi/#h5-se-elaa-lari

## Suoritusympäristö:
Tietokone: Lenovo Legion Y540-15IRH kannettava kytkettynä langallisesti kotiverkkoon. \
-Intel Core i7-9750H \
-NVIDIA Geforce RTX 2060 6GB \
-16GB DDR4 2666MHz \
Käyttöjärjestelmä: Kali Linux (VMWare virtuaalikone)

## Lab 0

Latasin harjoitukset lab0 - lab5.zip ja purin ensimmäisen `unzip lab0.zip`

Ajoin hakemiston sisältävän binäärin `buggy_program`.

<img width="313" height="110" alt="image" src="https://github.com/user-attachments/assets/1a16ccc5-f56f-4acd-99e1-c96ce950821e" />

Avasin seuraavaksi binäärin GDB:llä, `gdb buggy_program`.

<img width="492" height="236" alt="image" src="https://github.com/user-attachments/assets/0ee27703-3bff-4e68-ab4a-6f45fe85dff0" />

Annoin komennon `layout split`, jolla sain lähdekoodin, konekoodin ja komentorivin näkyviin.

<img width="648" height="360" alt="image" src="https://github.com/user-attachments/assets/52c12755-e520-4e46-bc70-d555f147a135" />

Lähdekoodista ja sen kommenteista huomasin, että ohjelmassa on puskuriylivuoto ohjelmointivirheen vuoksi. Ohjelma käy numbers listan läpi useamman kerran, kuin sille annettujen arvojen lukumäärä, jonka vuoksi viimeinen tulostettu arvo on 0.

Ohjelman voisi korjata muuttamalla `i ≤ size` -> `i < size`.

## Lab 1

Purin harjoituksen lab1, `unzip lab1.zip`.

Ajoin sen sisältämän ohjelman `gdb_example1`.

<img width="312" height="116" alt="image" src="https://github.com/user-attachments/assets/2bfc5fbb-bd4a-41ca-be41-bf385ae8cf0e" />

Ohjelma tuotti merkkijonon "Khoor/#zruog1", jonka jälkeen se kaatui segmentation faultin vuoksi. Segmentation fault on virhe, joka syntyy muistin suojelemiseksi, kun ohjelma yrittää käyttää muistia tavalla joka ei ole sallittua (https://en.wikipedia.org/wiki/Segmentation_fault). 

Avasin ohjelman gdb:llä ja siirryin split layoutiin.

<img width="414" height="305" alt="image" src="https://github.com/user-attachments/assets/ebd1bc95-d974-446a-9dcb-a179e040d3b9" />

Lähdekoodista käy selville, että main ohjelma luo kaksi merkkijonoa, joista toinen saa arvon "Hello, world." ja toinen NULL. Kun muuttuja good_message syötetään parametrina funktioon print_scrambled, niin ohjelma tulostaa "Khoor/#zruog1", mutta kun bad_message syötetään print_scrambled funktioon niin ohjelma kaatuu. Tutkin tarkemmin komentoa, joka saa ohjelman kaatumaan. Lisäsin breakpointin print_scrambled funktioon `break print_scrambled` ja aloitin ohjelman ajon `r` (run).

<img width="521" height="434" alt="image" src="https://github.com/user-attachments/assets/25a831d8-37fe-42d3-96a4-b0067678e0fa" />

Gdb hyppäsi kohtaan, jossa print_scrambled ajetaan ensimmäisen kerran. Ensimmäisellä kerralla se saa parametrin good_message, joten jatkoin komennolla `c` (continue). Annoin vielä muutaman kerran komennon `ni` (next instruction), jonka jälkeen ohjelma antoi segfaultin. 

<img width="518" height="490" alt="image" src="https://github.com/user-attachments/assets/138ffa09-f274-43c8-8d7e-f3e29102c37a" />

Kaatuminen tapahtui `movzbl (%rax),%eax` kohdalla. En saanut ymmärrettyä movzbl logiikka, mutta kun vertailin $rax ja $eax rekisetereitä funktion kummankin ajon yhteydessä, huomasin että ensimmäisellä kerralla ne sisälsivät arvoja, kun taas toisella kerralla ne sisälsivät arvot 0.

<img width="399" height="166" alt="image" src="https://github.com/user-attachments/assets/eb99ee2a-9ed0-4a81-8e73-cdac06b45dca" />

En tiedä satavarmasti onko se syy ohjelman kaatumiselle, enkä löytänyt netistä vastausta, joten kysyin tekoälyltä. ChatGPT-5 antoi ymmärtää, että "The segfault happens because movzbl (%rax), %eax tries to read from memory at address 0x0 (since %rax = 0), and the null page is not accessible." (https://chatgpt.com/share/68d050c3-ef50-8005-83ed-bed622d7a953)

Pohjimmillaan kaatumisen syy on muuttujan bad_message NULL arvo. Jos ohjelmalla on tarkoitus käsitellä merkkijonoja, jotka voivat saada arvoksi NULL, niin ennen merkkijonon syöttämistä funktioon tulisi varmistaa, ettei sen arvo ole NULL.

## Lab 2

Lab 2 sisälsi hakemiston passtr, joka sisälsi kaksi binääriä passtr ja passtr2o. README.md käski etsimään lipun tehtävästä passtr2o. 

<img width="814" height="459" alt="image" src="https://github.com/user-attachments/assets/6dd5682e-7cc0-4d07-8db9-1a74f5de8c17" />

Testasin ohjelmaa ja se pyysii salasanaa, jolla lipun saa auki.

<img width="256" height="65" alt="image" src="https://github.com/user-attachments/assets/de96b350-7aac-4087-838c-679a64c20760" />

Avasin ohjelman passtr2o gdb:llä ja huomasin, että siinä ei ollut lähdekoodia tarjolla.

<img width="653" height="393" alt="image" src="https://github.com/user-attachments/assets/edcefed6-bbd8-4db5-a974-89133e0415be" />

Vaihdoin näkymäksi `layout asm`. 

Ennen tehtävien aloittamista olin katsonut videon (https://www.youtube.com/watch?v=Tmdnsre9z7s) gdb:stä kertaukseksi, jossa käytiin samantyylinen lipunryöstö läpi. Lipunryöstössä oli loputon sleep funktio, joka skipattiin hyppäämällä valitulle riville, josta ohjelman suoritusta jatkettiin ja lähdin yrittämään, jos samanlainen lähestymistapa toimisi. 

Lisäsin breakpointin mainiin `break main`, aloitin ohjelman ajamisen `r` ja ajoin ohjelmaa yksi konekäsky kerrallaan `ni`. Huomasin, että salasanaa pyydetään, kun __isoc_scanf kutsutaan.

<img width="548" height="311" alt="image" src="https://github.com/user-attachments/assets/fa7524e8-dd0d-4537-b95a-aab8776de49d" />

Kohdassa main+125 tehdään jne "Jump if Not Equal" vertailu, joka hyppää kohtaan main+153, jos salasana on väärä.

<img width="539" height="256" alt="image" src="https://github.com/user-attachments/assets/23a60ca0-8ddf-4077-8b55-8d9887ce0d5d" />

Jos tästä jatkaa, niin kohdassa main+155 tulostetaan "Sorry, no bonus." Oikea lippu tulostetaan siis todennäköisesti välillä main+137-153.

Käytin seuraavaksi gdb:n jump komentoa hyppäämään "jne" vertailun yli seuraavalle riville eli main+137. Tällöin vertailu skipataan, eli ohjelma käyttäytyy kuten olisin syöttänyt oikean salasanan.

Annoin komennon `jump *(main+137)` ja ohjelma tulosti lipun.

<img width="857" height="499" alt="image" src="https://github.com/user-attachments/assets/2631440e-781e-48db-93a0-ce9a011158cc" />

## Lab 3
Päivitys 22.9.2025

### Crackme02

Tarkastaa onko `%edi == $0x2` eli onko kaksi parametria. Jos ei, niin tulostaa virheen parametreista.

<img width="460" height="26" alt="image" src="https://github.com/user-attachments/assets/27d35bb2-482d-43c7-8af9-76eb2e7fc85a" />

Tarkastaa onko `%rax == %0x6`, eli ympäröivän koodin perusteella onko parametrin pituus 6. Tämän testasin oikeaksi eri pituisia parametreja käyttäen. Jos ei ole oikein, tulostaa virheen väärästä salasanasta. Jos on oikein ajaa funktion check_pw.

<img width="393" height="85" alt="image" src="https://github.com/user-attachments/assets/dfaf70b6-24bc-45db-9176-96cef7436cd1" />

Check_pw funktion toimintaa en saanut täysin ymmärrettyä, mutta oli kuitenkin pieni aavistus, että funktion palautusarvo `%eax = $0x1`, jos salasana on oikein ja `%eax = $0x0`, jos salasana on väärin.

<img width="451" height="194" alt="image" src="https://github.com/user-attachments/assets/ec8bf2aa-ce25-4a78-91b7-116c4df76dbc" />

Ennen syvempää analyysiä lähdin katsomaan ohjelman palautusarvoja eri tilanteissa, sillä tehtävän tarkoitus oli saada ohjelma palauttamaan arvon 0. 

Ilman parametria: `gdb crackme02`

<img width="471" height="470" alt="image" src="https://github.com/user-attachments/assets/b1eba584-dabf-4f7a-a0df-35efd035fafc" />

Väärä salasana: `gdb --args crackme02 123`

<img width="553" height="516" alt="image" src="https://github.com/user-attachments/assets/f538cf4e-d797-4e7a-a733-037fded71676" />

Väärä salasana, oikea pituus ja skipataan check_pw loppuun: `gdb --args crackme02 123456`

<img width="565" height="485" alt="image" src="https://github.com/user-attachments/assets/d966b97d-c529-4968-ab58-cd5fc5573df1" />

Näiden perusteella ohjelman palautusarvoksi (rax) saadaan 0, kun annetaan 6 merkkiä pitkä parametri ja skipataan check_pw loppuun riville check_pw+46.




## Lähteet
Wikipedia: Segmentation fault: https://en.wikipedia.org/wiki/Segmentation_fault

ChatGPT-5: Prompt: "Why is there a segfault when trying to "movzbl ($rax),%eax" where $rax and $eax = 0": https://chatgpt.com/share/68d050c3-ef50-8005-83ed-bed622d7a953

Hammond 2022: Intro to Debugging w/ GDB (PicoCTF 2022 #11 'gdb-test-drive'): https://www.youtube.com/watch?v=Tmdnsre9z7s

