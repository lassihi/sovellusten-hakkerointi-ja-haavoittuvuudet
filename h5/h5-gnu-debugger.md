# Harjoitus 5: GNU Debugger
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/

## Suoritusympäristö:
Tietokone: Lenovo Legion Y540-15IRH kannettava kytkettynä langallisesti kotiverkkoon. \
-Intel Core i7-9750H \
-NVIDIA Geforce RTX 2060 6GB \
-16GB DDR4 2666MHz \
Käyttöjärjestelmä: Kali Linux (VMWare virtuaalikone)

## Tiedostot Lab0-5.zip tiedostot nostattavat dynaamisen analyysin tekoa asteittain ja alussa teillä on käytössä lähdekoodit, jotka helpottavat gdb:n käyttöä ja analyysiä mutta lopussa lähdekoodi jää pois ja teillä on käytössänne vain pelkkä koodi. Kirjoittakaa lyhyt raportti siitä mitä teette ja mitä saitte selville tehtävistä Lab0 - 2. Lab3 ja 4 ovat vapaaehtoisia.

### Lab 0

Latasin harjoitukset lab0 - lab5.zip ja purin ensimmäisen `unzip lab0.zip`... 

Ajoin hakemiston sisältävän binäärin `buggy_program`.

<img width="313" height="110" alt="image" src="https://github.com/user-attachments/assets/1a16ccc5-f56f-4acd-99e1-c96ce950821e" />

Avasin seuraavaksi binäärin GDB:llä, `gdb buggy_program`.

<img width="492" height="236" alt="image" src="https://github.com/user-attachments/assets/0ee27703-3bff-4e68-ab4a-6f45fe85dff0" />

Annoin komennon `layout split`, jolla sain lähdekoodin, konekoodin ja komentorivin näkyviin.

<img width="648" height="360" alt="image" src="https://github.com/user-attachments/assets/52c12755-e520-4e46-bc70-d555f147a135" />

Lähdekoodista ja sen kommenteista huomasin, että ohjelmassa on puskuriylivuoto ohjelmointivirheen vuoksi. Ohjelma käy numbers listan läpi useamman kerran, kuin sille annettujen arvojen lukumäärä, jonka vuoksi viimeinen tulostettu arvo on 0.

Ohjelman voisi korjata muuttamalla `i ≤ size` -> `i < size`.

### Lab 1

Purin harjoituksen lab1, `unzip lab1.zip`.

Ajoin sen sisältämän ohjelman `gdb_example1`.

<img width="312" height="116" alt="image" src="https://github.com/user-attachments/assets/2bfc5fbb-bd4a-41ca-be41-bf385ae8cf0e" />

Ohjelma tuotti merkkijonon "Khoor/#zruog1", jonka jälkeen se kaatui segmentation faultin vuoksi. Segmentation fault on virhe, joka syntyy muistin suojelemiseksi, kun ohjelma yrittää käyttää muistia tavalla joka ei ole sallittua (https://en.wikipedia.org/wiki/Segmentation_fault). 

Avasin ohjelman gdb:llä ja siirryin split layoutiin.
