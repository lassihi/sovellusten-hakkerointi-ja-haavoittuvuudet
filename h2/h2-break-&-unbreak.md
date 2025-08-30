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

Aloitin asentamalla ohjeen vaatimat paketit.

    sudo apt-get -y install wget unzip micro

Jonka jälkeen latasin tehtävät

<img width="835" height="249" alt="image" src="https://github.com/user-attachments/assets/ac45049f-0c04-4457-9577-b97712bf8583" />

ja purin ne, `unzip teros-challenges.zip`.

Yritin käynnistää harjoituksen Python flask sovelluksen, joka ei onnistunut, sillä flask puuttui. 

<img width="828" height="125" alt="image" src="https://github.com/user-attachments/assets/9c48ad61-1948-4d06-9430-e1f608bcabb2" />

Huomasin samalla ohjeesta, että lisäksi täytyy asentaa paketti python3-flask-sqlalchemy, joten asensin ne ja käynnistin sovelluksen.

    sudo apt-get install python3-flask python3-flask-sqlalchemy

<img width="1082" height="149" alt="image" src="https://github.com/user-attachments/assets/8671148f-27b5-42b7-b4f7-a0a4f76c725b" />

Kun sovellus oli käynnissä, suuntasin sen tarjoamalle verkkosivulle selaimella.

<img width="912" height="630" alt="image" src="https://github.com/user-attachments/assets/b4d5a6a6-51e6-4068-828f-a5844eaabd0a" />

"Game advice"-osiosta huomasin, että sovellus antoi vihjeenä tietokantakyselyn, jonka vuoksi päätin alkuun testata yleisimpiä SQL-injektioita. 

<img width="912" height="630" alt="image" src="https://github.com/user-attachments/assets/a7d585c1-826f-4110-a913-6b6f6c3efa93" />

Tämä ei toiminut, sillä syöttökenttä hyväksyi vain numeroita. Ilmoitus oli kuitenkin selaimen luoma, eikä sovelluksen, joten epäilin oliko suodatus tehty selaintasolla, joka vahvistui kun tutkin tarkemmin syötteen koodia selaimen inspectorilla.

<img width="956" height="739" alt="image" src="https://github.com/user-attachments/assets/cab14d35-fe79-480c-998c-764ddf8fa9d3" />

Muokkasin syötekentän tyyppiä merkkijonoksi (`type="string"`), tallensin muutokset ja yritin injektiota uudestaan. Tällä kertaa sain virheilmoituksen.

<img width="1008" height="247" alt="image" src="https://github.com/user-attachments/assets/eb849d39-b382-41b8-9379-427cf0931858" />

Virheilmoitus varmisti, että olin oikeilla jäljilla ja hieman sql-lausetta tutkiessani huomasin, että olin unohtanut yhden hipsun toisen ehdon alusta, jolloin kysely oli virheellinen.

Virheellinen kysely: `SELECT password FROM pins WHERE pin='1' OR 1=1'`

Sijoitin toisen ehdon eteen hipsun (`1 OR '1=1`) ja ajoin kyselyn.

<img width="752" height="588" alt="image" src="https://github.com/user-attachments/assets/6e8e6625-eda9-40d4-886b-10409cae9111" />

Kysely meni läpi ja salasanaksi paljastui "foo". En saanut kuitenkaan selville oliko kyseessä käyttäjän admin salasana. Yritin tässä kohtaa kerrata SQL:n syntaksia ja mahdollisesti saada dumpattua koko pins-taulun, mutta vaikutti siltä, että kyselyllä saisi vain yhden salasanan kerrallaan, joka oli aina "foo".

Lopulta sain idean hakea rivi kerrallaan jokaisen password-tietueen, jossa onnistuin alla olevalla kyselyllä.
ORDER BY määrittää minkä sarakkeen mukaan tulokset järjestetään (oletuksena laskevassa järjestyksessä), LIMIT määrittä kuinka monta tietuetta näytetään ja OFFSET määrittää monesko tietue esitetään. 
https://www.geeksforgeeks.org/sql/sql-limit-clause/

    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 0 --';
    
Jokaisen kyselyn jälkeen OFFSET nousee yhdellä, jotta saan seuraavan rivin.

    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 1 --';
    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 2 --';
    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 3 --';
    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 4 --';
    SELECT password FROM pins WHERE pin='1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 5 --';

<img width="777" height="568" alt="image" src="https://github.com/user-attachments/assets/5f52ef0a-a7bc-47e5-a1a4-d9f0b5f7bf0c" />
<img width="777" height="601" alt="image" src="https://github.com/user-attachments/assets/895b439f-9503-4d20-86a2-8e581e0b94b2" />
<img width="777" height="568" alt="image" src="https://github.com/user-attachments/assets/87685c60-3d7f-4bf2-8b44-70e5527e92d2" />
<img width="777" height="601" alt="image" src="https://github.com/user-attachments/assets/c3ccaeb1-4744-425d-96b8-c65bdbc4f3a5" />

Viides sekä kuudes rivi antoivat salasanaksi "(not found)", joten oletin rivejä olevan neljä. Salasanojen sisällön perustellaa vaikuttaisi siltä, että admin salasana olisi "SUPERADMIN%%rootALL-FLAG{Tero-e45f8764675e4463db969473b6d0fcdd}". 

## b) Korjaa 010-staff-only haavoittuvuus lähdekoodista. Osoita testillä, että ratkaisusi toimii.
Korjataan tässä vain SQL-injektio.

Avasin lähdekoodin, `micro staff-only.py`.

Haavoittuvuus on funktiossa `hello()`.

<img width="762" height="389" alt="image" src="https://github.com/user-attachments/assets/fe3cb2b1-9f64-4503-90c2-1c72af2dba81" />

Ohjelma asettaa käyttäjän syötteeen merkkijonoon (`pin = str(request.form['pin']`) ja ajaa merkkijonon osana SQL-kyselyä.

    sql = "SELECT password FROM pins WHERE pin='"+pin+"';"
    row = ""
    with app.app_context():
		    res=db.session.execute(text(sql))
		    db.session.commit()
		    row = res.fetchone()

Korjattu funktio:

<img width="862" height="451" alt="image" src="https://github.com/user-attachments/assets/47d20e9f-b625-4ae8-8f28-a45d38c706ff" />

Ohjelma ottaa pin-koodin kokonaislukuna tai palauttaa sen arvoksi "0", jos tämä ei ole mahdollista.

	if "pin" in request.form:
		try:
		    pin = int(request.form['pin'])
		except:
		    pin = "0"
	else:
		pin = "0"

Luodaan kysely, jossa muuttuja "pin" annetaan parametrina SQL injektioilta suojaamiseksi. https://stackoverflow.com/questions/17972020/how-to-execute-raw-sql-in-flask-sqlalchemy-app

    res=db.session.execute('SELECT password FROM pins WHERE pin=:pin', {'pin':str(pin)})

Käynnistin ohjelman ja testasin eri arvoilla.

Kun syötin alkuperäisen injektion, niin muutti sovellus pin-koodiksi 0.

<img width="954" height="884" alt="image" src="https://github.com/user-attachments/assets/e1e306b6-2d7d-4695-9c37-028c7f7a4ebc" />

<img width="755" height="558" alt="image" src="https://github.com/user-attachments/assets/8069346c-39fa-4116-98ad-63ca6d25fc02" />

Myöskään injektio `1' OR 1=1 ORDER BY password LIMIT 1 OFFSET 0 --` ei toiminut enää.

Salasanat saa veilä esille, jos tietää oikean pin-koodin.

<img width="755" height="558" alt="image" src="https://github.com/user-attachments/assets/2d81dd5c-ff56-4597-8e32-8851482a7358" />

## c) Ratkaise dirfuzt-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf. Tämä auttaa 020-your-eyes-only ratkaisemisessa.

Asensin ffuf:n ja seclists github repositoryn, josta löytyy suosituimmat sanalistat.

		sudo apt-get install ffuf
  		git clone --depth 1 https://github.com/danielmiessler/SecLists.git

Sekä harjoituksen dirfuzt-1.

		wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
Annoin itselleni ajo-oikeudet harjoitukseen ja käynnistin sen.

<img width="416" height="132" alt="image" src="https://github.com/user-attachments/assets/f094f7fb-15a7-4de1-8d36-f3ecfb73f431" />

Siirryin selaimella harjoitussivulle tarkistaakseni, että se toimii.

<img width="793" height="147" alt="image" src="https://github.com/user-attachments/assets/5334a596-3b37-49ed-8269-725c66429372" />

Näyttää toimivan, joten aloin työstämään hakemistoja ffufilla. Otin ensiksi testiin sanalistan [common.txt
](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt)

<img width="921" height="532" alt="image" src="https://github.com/user-attachments/assets/c2d65fd1-e26d-43b5-a2dd-f4993dd4ed4d" />

Pyynnöt rajoitettiin jostain syystä lähes välittömästi vain noin muutamaan sekunnissa. Ffufista en löytänyt tähän ratkaisua, joten syynä oli mahdollisesti sovelluksen rate limit. Avasin taas selaimella sovelluksen, tein testihaun "127.0.0.2:8000/testi" ja kopioin haun pyynnön selaimen kehittäjätyökalujen Network-välilehdeltä curl syntaksissa. 

<img width="1007" height="822" alt="image" src="https://github.com/user-attachments/assets/ef5f3c50-1561-4d2a-b4b1-5460e4a331fb" />

Ffuf komennon rakenne on hyvin lähellä curlia, joten pienillä muokkauksilla sain selainta imitoivan ffuf komennon luotua.

<img width="1915" height="109" alt="image" src="https://github.com/user-attachments/assets/0105c409-37d6-438c-94e1-c8f799617b06" />

Tällä kertaa ffuf sai kierrettyä rajoitukset.

<img width="824" height="766" alt="image" src="https://github.com/user-attachments/assets/3fa16a05-4b94-49a1-a4e5-619ce29158ee" />

Komento kuitenkin tuotti niin paljon rivejä, että filtteröin pois tulokset, joiden koko oli 154 tavua, `-fs 154`.

Ajoin uuden komennon.

<img width="1920" height="930" alt="image" src="https://github.com/user-attachments/assets/ef118218-f46b-4d9a-9d02-c5dc3e56f06a" />

Tuloksissa olevasta wp-admin sivusta löytyi tehtävän lippu.

<img width="999" height="230" alt="image" src="https://github.com/user-attachments/assets/7dbad097-2964-478a-8e3c-fc957c241cf3" />


## d) Murtaudu 020-your-eyes-only. Ks. Karvinen 2024: Hack'n Fix

## e) Korjaa 020-your-eyes-only haavoittuvuus. Osoita testillä, että ratkaisusi toimii.
