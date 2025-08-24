# Harjoitus 0: Korkeat standardit
Kurssi: Sovellusten hakkerointi ja haavoittuvuudet https://terokarvinen.com/sovellusten-hakkerointi/ \
Tehtävänanto: https://terokarvinen.com/sovellusten-hakkerointi/#h1-korkeat-standardit-lari

## a) Tutustu kurssin sanastoon, joka on määritelty SFS-EN ISO/IEC 27000:2020:en standardissa, kappaleessa 3. Terms and Definitions. Selvitä seuraavien kappaleiden määritteet ja selitä omin sanoin mitä ne tarkoittavat  3.2, 3.31, 3.56, 3.58, 3.77
### 3.2 Attack - Hyökkäys
Yritys luvattomasti tuhota, paljastaa, muuttaa, varastaa, käyttää hyväksi tai saada pääsy jonkun omaisuuteen.

### 3.31 Information security incident - Tietoturvatapoikkeama
Tapahtuma tai tapahtumaketju, joka aiheuttaa haittaa liiketoiminnalle ja tietoturvalle.

### 3.56 Requirement - Vaatimus
Pakosta, tarpeesta tai yleisestä käytänteestä peräisin oleva sääntö tai olettamus.

### 3.58 Review - Arviointi
Tutkimus siitä, kuinka tapahtuman käsittelyyn asetetut tavoitteet onnnistuivat.

### 3.77 Vulnerability - Haavoittuvuus
Heikkous järjestelmissä, prosesseissa tai ihmisissä, jota hyödyntämällä uhka voi aiheittaa vahinkoa.

(SFS-EN ISO/IEC 27000:2020:en)

## b) Tutustu standardiin ISO 27034-1 - 5. Selvitä mistä standardi kokonaisuudesta on kyse.
ISO27034-1 on ISO27034 standardin ensimmäinen osa, ja pyrkii ohjeistamaan organisaatioita integroimaan tietoturvaa olemassaoleviin ohjelmistokehityksen prosesseihin ohjelmistokehityksen jokaisessa vaiheessa. Se perustuu suurelta osin tavoiteltavan luetettavuustason (Targeted Level of Trust) määrittämiseen ja sovellusten rakentamiseen tämän saavuttamiseksi.

ISO27034-2 pureutuu Organization Normative Framework -käsitteeseen, joka sisältää sovelluksen tietoturvan rakennuspalaset.

ISO27034-3 kuvaa yleisellä tasolla sovellusten tietoturvan hallinnan prosessessia.

ISO27034-5 määrittelee sovelluksen tietoturvakontrollien ja elinkaaren viitekehyksien vähimmäisvaatimukset.

Kokonaisuudessaan ISO27043-1 — 5 siis pyrkii ohjeistamaan organisaatioille ja sovellusten kehityksestä vastaaville sidosryhmille, kuinka sovellusten tietoturvaa tulisi hallita. Standardin päätavoitteena on saavuttaa sovellusten riittävä turvallisuustaso.

(ISO/IEC 27034-1, https://www.iso27001security.com/html/27034.html)

## c) Kuuntele Podcast: Laatulöpinät 30: Tietoturvallisuus ohjelmistokehityksessä. Mieti mitä mieltä olet podcastin väittämistä?
### Mikään ohjelmisto ei ole täysin tietoturvallinen.
Mielestäni totta. Ohjelmistoja ei voi koskaan saada täysin tietoturvallisiksi, mutta niitä kannattaa aina vahvistaa kunhan se on ajallisesti ja resurssien puolesta järkevää. 
### Hallinnollinen tietoturva on teknisen tietoturvan onnistumisen edellytys.
Totta, etenkin kun projektin koko kasvaa. Mitä monimutkaisempi sovellus on, niin usein sitä enemmän haavoittuvuuksia se sisältää, jolloin on tärkeää, että on olemassa tietoturvasuunnitelmia, -käytänteitä ja -kontrolleja.
### Automaatiotestaus ohjelmiston tietoturvan kannalta on erittäin tärkeää.
Automaatiotestaus on mielestäni useassakin tapauksessa hyvin tärkeää, sillä jokaisen mahdollisen kokeen suorittaminen käsin voi olla mahdotonta, tai vähintäänkin aikaa vievää. Kaikkea ei automaatiotestauksella voi kuitenkaan suorittaa, jonka vuoksi pitäisi olla hyvin dokumentoituna mitä automaatiotestaus kattaa ja mitä ei.
### Ohjelmistoa suunniteltaessa voidaan tehdä paljonkin auttamaan käyttäjää toimiaan tietoturvallisesti, usein nämä toimenpiteet vaikuttavat kuitenkin negatiivisesti käytettävyyteen.
Väittämä pitää usein paikkansa, vaikka sen ei tarvitsisi. Käytettävyyttä suunniteltaessa tulisi ottaa huomioon myös tietoturva ja yrittää sulauttaa ne mahdollisimman hyvin yhteen.
### Ohjelmiston tietoturvallisuuden suunnitteluun vaikuttaa paljolti se, kuinka arkaluontoisia tietoja ohjelmistolla on tarkoitus käsitellä.
Arkaluontoiset tiedot asettavat paljon vaatimuksia, mutta tietoturvaan kuuluu muitakin ulottuuvuksia. Kuten podcastissa mainittiin, niin esimerkiski liiketoimintakriittiset ohjelmistot vaativat tietoturvaa, vaikka ne eivät käsittelisi ollenkaan arkaluontoisia tietotoja.
### Ohjelmistokehittäjät näkevät omat ohjelmistonsa aina merkittävästi riskialttiimpina, kuin muiden tekemät ohjelmistot.
Väittämä pitää mielestäni paikkansa ja johtuu todennäköisesti siitä, että ohjelmistokehittäjät tuntevat hyvin oman ohjelmistonsa toimintaperiaatteet ja heikkoudet, kun taas muiden ohjelmistot ovat tuntemattomampia. 

(https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/)
## d) Tee itsellesi riskienhallintasuunnitelma. Suunnitelmassa pitää olla kuvaus ympäristöstä. Miten varmistat, että ympäristöön ei päästä tunkeutumaan ulkopuolisten toimesta. Jos tutkit ympäristössä epäilyttäviä tiedostoja niin miten varmistat sen, että mahdolliset haittaohjelmat eivät pääse leviämään ympäristön ylkopuolelle.
Ympäristönä toimii Kali virtuaalikone vanhassa Debian 12 kannettavassa, jota käytän vain kyseiseen tarkoitukseen. Virtualisointiohjelmistona toimii VMWare. Kali ja Debian ovat lähes puhtaita asennuksia, eivätkä sisällä mitään henkilökohtaisia tietoja.

Pyrin estämään tunkeutumisen pitämällä ensinnäkin huolen fyysisestä turvallisuudesta. Konetta ei tulisi jättää valvomatta ulkopuolisten läsnäollessa ja siinä tulisi käyttää koko levyn salausta. Käyttöjärjestelmä ja ladatut ohjelmistot tulisi päivittää säännöllisesti uusimpaan versioon ja palomuuri konfiguroituna. Jos tarkoitus on pyrkiä estämään ulkopuolisten tunkeutuiminen, niin epäilyttävien tiestojen tai minkään muunkaan epäilyttävän tutkimista tulisi välttää.

Jos ympäristössä on kuitenkin tarkoitus tutkia epäilyttäviä tiedostoja, niin sulkisin pääsyn verkoon enkä käyttäisi jaettua muistia host- ja virtuaalikoneen välillä.

## Lähteet
Iso-Anttila, Karvinen: Sovellusten hakkerointi - 2025 alkusyksy: https://terokarvinen.com/sovellusten-hakkerointi/

SFS-EN ISO/IEC 27000:2020:en

ISO/IEC 27034-1

https://www.iso27001security.com/html/27034.html

Laatulöpinät 30: Tietoturvallisuus ohjelmistokehityksessä: https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/
