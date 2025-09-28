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

## Lab2

Valitsin Android-FOSS listalta (https://github.com/offa/android-foss) sovellukseksi "Hacker's keyboard" (https://github.com/klausw/hackerskeyboard). APK:n (Android Package) latasin F-Droidin sivuilta, `wget https://f-droid.org/packages/org.pocketworkstation.pckeyboard/`.

<img width="623" height="90" alt="image" src="https://github.com/user-attachments/assets/d58142da-4ff7-49c9-9455-7ae8043b0776" />

File tunnisti tiedoston olevan apk ja binwalk tiedoston sisältävän zip-dataa.

<img width="1892" height="740" alt="image" src="https://github.com/user-attachments/assets/af21c715-5bda-4d62-8b49-76ebbb264b25" />

`strings -n 10 org.pocketworkstation.pckeyboard_1041001.apk` löysi hyvin paljon kovakoodattua tekstiä tiedostosta, pääasiassa tiedostopolkuja ja käyttöliittymän tekstejä. Osa merkkijonoista oli suomeksi.

<img width="898" height="841" alt="image" src="https://github.com/user-attachments/assets/4ed0cb80-0f98-490c-86d4-ada8e888120c" />

Purin apk:n binwalkilla, `binwalk -eM org.pocketworkstation.pckeyboard_1041001.apk`. Apk:n sisältä läytyi tiedostot AndroidManifest.XML, classes.dex, resources.arcs ja hakemistot lib, META-INF ja res.

<img width="1374" height="99" alt="image" src="https://github.com/user-attachments/assets/a8cebc3d-8f4d-4cc4-81a2-4550bf99b580" />

Heti mielenkiintoni herätti hakemiston META-INF sisältö, joka sisälsi .RSA-päättyisen tiedoston. 

<img width="1501" height="251" alt="image" src="https://github.com/user-attachments/assets/72f954f1-e5b0-41a9-8b73-2f0df2e2cbdd" />

Nopealla Googlauksellä kävi ilmi, että .RSA-tiedosto sisältää varmenteen (kehittäjän julkinen avain ja sertifikaatti) ja allekirjoituksen (kehittäjän yksityisellä avaimella allekijoitettu apk) (https://stackoverflow.com/questions/39305775/what-are-the-purposes-of-files-in-meta-inf-folder-of-an-apk-file#39305776).Hakemiston .SF ja .MF päättyiset tiedostot sisältävät tiivisteitä apk:n sisältämistä tiedostoista. Näiden avulla voidaan varmistaa apk:n eheys.

.RSA tiedoston sisällön saa auki keytool työkalulla.

<img width="1499" height="427" alt="image" src="https://github.com/user-attachments/assets/fc5cec53-ceb3-4b31-bee8-648a590c57b6" />


Toinen mielenkiintoinen apk:n hakemisto oli lib, josta löytyy ohjelman käyttämiä kirjastoja eri arkkitehtuureille.

