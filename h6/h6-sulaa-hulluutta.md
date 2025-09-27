# Harjoitus 6: Sulaa hulluutta
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/

Tehtävänanto:
- Lab0: Tutki tiedostoa h1.jpg, jo opituilla työkaluilla mitä saat selville?
- Lab1: Tutki tiedostoa h1.jpg binwalk (Versio 2.x tai 3.x huom toimivat eri tavalla), mitä tietoja löydät nyt tiedostosta. Mitä työkalua käyttäisit tiedostojen erottamiseen?
- Lab2: FOSS (Free Android OpenSource) Tutustu listaan eri android applikaatioita. https://github.com/offa/android-foss. Valitse listalla itsellesi mielenkiintoisin applikaatio ja mene sen GitHubiin. Lataa ohjelman APK itsellesi ja käytä seuraavia työkaluja tutustuaksesi miten APK:n voi avata. ZIP, JADX ( https://github.com/skylot/jadx ), Bytecode-viewer ( https://github.com/Konloch/bytecode-viewer/ )

## Suoritusympäristö:
Tietokone: Lenovo Legion Y540-15IRH kannettava kytkettynä langallisesti kotiverkkoon. \
-Intel Core i7-9750H \
-NVIDIA Geforce RTX 2060 6GB \
-16GB DDR4 2666MHz \
Käyttöjärjestelmä: Kali Linux (VMWare virtuaalikone)

## Lab0
Ajoin aluksi komennon `file h1.jpg` tutkiakseni minkä tyyppinen tiedosto on kyseessä.

<img width="948" height="56" alt="image" src="https://github.com/user-attachments/assets/92a5b116-06c8-4ec6-aca4-bffa3aa07f35" />

File tunnistaa tiedoston olevan normaali JPEG.

`open h1.jpg` avaa kuvan normaalisti.

<img width="565" height="263" alt="image" src="https://github.com/user-attachments/assets/cf7616eb-3b2c-4377-835d-b32de0fa5fde" />

Tähän asti kurssilla opituista työkaluista (strings, ghidra, gdb) hyödyllisin on strings. Tiedosto ei ole ajettava ohjelma, joten Ghidrasta tai dgb:stä ei ole hyötyä.

`strings h1.jpg` tulosti hyvin pitkän listan sekalaisia merkkejä. Jos otetaan mukaan vain yli kymmenen merkkiä pitkät merkkijonot `-n 10`, saadaan näkyviin useampi mahdollinen .xml-päätteinen tiedosto. XML (extensible markup language) on datan esittämiseen, tallentamiseen ja siirtämiseen käytetty merkintäkieli.

<img width="960" height="476" alt="image" src="https://github.com/user-attachments/assets/6b1258cf-7117-4ec5-a88b-acf523ce123f" />

## Lab1
Kalissa oli valmiiksi binwalk v2.4.3 asennettuna.

Ajoin h1.jpg tiedoston binwalkilla.

<img width="941" height="275" alt="image" src="https://github.com/user-attachments/assets/4bfdde54-e90f-4015-9c37-01b2d1de87ab" />

Binwalkin mukaan kuva sisältää jpg:n lisäksi tiff ja zip dataa. Purin kuvatiedoston sisältämät datat `binwalk -eM h1.jpg`. 
* `-e` = Automatically extract known file types
* `-M` = Recursively scan extracted files

<img width="935" height="306" alt="image" src="https://github.com/user-attachments/assets/5fae0536-99e3-492f-8f98-da8f32ac39ad" />

Binwalk loi hakemiston _h1.jpg.extracted, joka sisälsi tiedoston 494F5.zip.

Yritin purkaa sitä, mutta unzip ei löytänyt zip-tiedoston loppua.

<img width="466" height="120" alt="image" src="https://github.com/user-attachments/assets/0d9bb858-b6b4-491c-8ac2-877f58c16a50" />

Ajoin tiedoston filella, joka tunnisti sen Microsoft Word tiedostoksi.

<img width="323" height="46" alt="image" src="https://github.com/user-attachments/assets/9c912cf9-bc42-4240-b5a3-abd470e53bc4" />

Asensin libreoffice-writerin word dokumentin avaamiseksi, `sudo apt-get install libreoffice-writer`

Samalla kuin libreoffice writer asentui, sain purettua tiedoston 7z:n avulla komennolla `7z x 494F5.zip`. Purettuna tiedosto näytti sisältävän word dokumentin tietoja.

<img width="367" height="272" alt="image" src="https://github.com/user-attachments/assets/703c5efc-b972-495b-ab84-eb69bfaa2ee6" />

Libreoffice oli asentunut, joten avasin .zip tiedoston sillä, `libreoffice 494F5.zip`.

<img width="271" height="128" alt="image" src="https://github.com/user-attachments/assets/b33f0ea9-0770-4831-bfd5-0cd192a1fe3a" />

Valitsin "Yes" ja dokumentti avautui. Tiedoston sisältämä dokumentti sisälsi tulevaisuuden ennustuksia eri aloilta.

<img width="488" height="468" alt="image" src="https://github.com/user-attachments/assets/20335020-fbdc-408d-b24e-700029a9b7c0" />

<img width="479" height="316" alt="image" src="https://github.com/user-attachments/assets/43aebac6-0710-4613-be8e-6f59fcee8989" />
