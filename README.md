# make-contact-form-automation

Make-automaation oppimisprojekti — tekoälyavusteinen ja automatisoitu asiakasyhteydenottojen käsittelyprosessi, joka hyödyntää verkkosivujen haravointia (web scraping) ja kalenterisynkronointia.

## Tarkoitus

Tämä on käytännön oppimisprojekti, jossa harjoittelin ensimmäistä kertaa Make.com-alustan käyttöä, integraatioita ja tekoälyagenttien (AI Agents) rakentamista. Projekti on toteutettu seuraamalla You Tube videon tutoriaalia ja tarkoituksena on ollut päästä itse "rakentamaan" automaatiota ja agentteja. 

Esimerkkitapauksena on yrityksen yhteydenotto-lomakkeen kautta tulevien pyyntöjen automaattinen käsittely automaation ja AI-agentin avulla.

Opin:
- Skenaarion luominen ilman mallipohjaa, eli "tyhjästä"
- Integraatiot: Data kulkee eri softien välillä (Google Sheets, kalenteri ja Gmail sekä Browse AI + Tally)
- Hyödyntämään automaatiossa "haaroittimia"? (Routers) ja suodattimia, jotta automaatio toimii oikein (URL-osoitteen tarkastus)
- Automaattista tiedonkeruuta, eli robotti käy lukemassa ja hakemassa tarvittavat tiedot kohdesivustolta puolestani (browse AI)
- AI Agentin rakentaminen: tekoälyagentti osaa itsenäisesti lukea viestejä, käydä katsomassa kalenterista vapaat ajat ja lähettää sähköpostilla valmiin vastaus- ja tapaamisen varauslinkin.
- Projekti havainnollisti, miten modernit automaatio- ja AI-työkalut mahdollistavat useiden eri järjestelmien yhdistämisen yhdeksi automatisoiduksi työnkuluksi. Samalla opin, että tuotantoympäristössä käyttöoikeuksien hallinta, tietoturva ja datan käsittely ovat keskeisiä suunnittelukohteita.

## Teknologiat

| Kerros / Rooli | Teknologia |
|---|---|
| Alusta / Integraatiot | Make |
| Lomake (Frontend) | Tally.com |
| Tietovarasto / Data | Google Sheets |
| Tiedonhaku / Scraping | Browse AI (HTML robotti) |
| Tekoäly / Älykkäät työkalut | Make AI Agent |
| Viestintä & Kalenteri | Gmail, Google Calendar |
| Suunnittelu | YouTube-ohjeet |

## Mitä se tekee

Tämä automaatio koostuu kahdesta eri Make-skenaariosta, jotka toimivat yhdessä:

1. **Lomakkeen vastaanotto ja datan validointi:** Asiakas täyttää yhteydenottolomakkeen (Tally). Make tarkistaa onko asiakkaan antama URL-osoite oikeassa muodossa ja päivittää tarvittaessa Google Sheets:iin osoitteen oikein. 
2. **Tiedonhaku (Asynkroninen Scraping):** Koska verkkosivun haravointi kestää hetken, automaatio ei jää odottamaan, vaan tallentaa Browse AI:n tehtävä-ID:n (Task ID) Google Sheetsiin. Toinen Make-skenaario poimii valmistuneen datan myöhemmin.
3. **Tekoälyagentin analyysi:** Automaatio syöttää kerätyt tiedot AI Agentille. Agentille on annettu "ohjeet" mm. minkälaisella tavalla ja tyylillä vastaus annetaan.  Agentti analysoi asiakkaan tarpeen ja laatii personoidun vastauksen suomeksi.
4. **Kalenterivaraus ja viestintä:** Automaatio tarkistaa **Google Calendarista** vapaat ajat, luo tapaamisen ja lähettää asiakkaalle sähköpostitse (**Gmail**) personoidun vastauksen sekä tapaamislinkin.

## Kuvakaappaukset

#### 1. Make-skenaariot
![Make Workflow](kuvat/make-workflow1.jpg)
![Make Workflow](kuvat/make-workflow2.jpg)

#### 2. AI Agentti
![AI Agent](kuvat/AI-Agent.jpg)

#### 3. Google Calendar
![Google Calendar](kuvat/Google-Calendar.jpg)

## Tietoturva ja riskienhallinta (Security & Risk Management)

> ⚠️ **Huomautus:** 
> Tämä projekti on toteutettu käyttäen kuvitteellista yritystä ja testisähköposteja. Projektissa käytetyt tiedot eivät ole aitoja, joten esitellyt riskit eivät ole tässä harjoitustyössä oleellisia. Alla olevat huomiot ovat kuitenkin kriittisiä asioita, jotka on ehdottomasti huomioitava ja ratkaistava, jos vastaava järjestelmä otetaan käyttöön oikeassa yrityksessä.

Tekoälyagentteihin ja automaatioketjuihin liittyy haavoittuvuuksia vastuullisuuden ja riskienhallinnan kannalta. Tässä projektissa tunnistettiin seuraavat kriittiset riskit ja niiden hallintakeinot:

### 1. Virheellisen tiedon siirtyminen
* **Riski:** Automaatio hakee tietoa Browse AI -robotilla asiakkaan antaman linkin perusteella. Jos ulkopuolisella sivustolla on vanhaa, virheellistä aineistoa, heikkolaatuinen data "saastuttaa" koko prosessin. Tekoälyagentti laatii vastauksen näiden väärien esitietojen pohjalta, mikä johtaa virheellisen sähköpostin lähettämiseen asiakkaalle.
* **Ratkaisu:** Prosessia voisi muuttaa niin, että tekoälyagentti ei lähetä sähköpostia suoraan, vaan tallentaa viestin luonnokseksi Gmail-tilille. Ihminen tarkistaa ja hyväksyy viestin sisällön aina ennen sen lähettämistä.

### 2. Sääntöjen kiertäminen ja hallinnan menetys
* **Riski:** Jos tekoälyagentin ohjeistusta ei ole rajattu riittävän tiukasti, agentti saattaa pyrkiä päätavoitteeseensa (kuten asiakkaan ystävälliseen palvelemiseen ja tapaamisen sopimiseen) keinoilla, jotka kiertävät organisaation sääntöjä tai vastuita. Agentti voi esimerkiksi luvata luvattomia alennuksia tai sopia tapaamisia väärille ajoille.
* **Ratkaisu:** Agentin toimintavaltuudet on määriteltävä tiukasti järjestelmäohjeissa (esim. *"Älä koskaan ota kantaa hinnoitteluun tai lupaa aikatauluja, joita ei ole annettu suorissa alkutiedoissa"*).

### 3. Laaja rajapintojen käyttö ja hyökkäyspinta (Expanded Attack Surface)
* **Riski:** Automaatiossa on laajasti käytössä useita eri rajapintoja ja ulkoisia palveluita (Tally, Make, Google Sheets, Browse AI, OpenAI, Gmail). Useiden rajapintojen yli kulkevaa dataliikennettä on vaikea monitoroida, mikä laajentaa järjestelmän hyökkäyspintaa ja lisää tietoturvaloukkausten riskiä.
* **Ratkaisu:** Integraatioiden hallinnassa on noudatettava **minimioikeuksien periaatetta**. Make-alustan liitoksille annetaan mahdollisimman tiukat ja rajatut käyttöoikeudet (esimerkiksi oikeus lukea ja kirjoittaa vain yhteen tiettyyn Google Sheets -taulukkoon koko Google Driven sijaan).

### 4. Henkilötietojen käsittely
* **Riski:** Koska yhteydenottolomakkeella kerätään henkilötietoja (nimi,puhelin,sähköposti), datan siirtäminen kolmannen osapuolen tekoälypalveluihin (kuten OpenAI) muodostaa tietosuojariskin.
* **Ratkaisu:** Tuotantoympäristössä on varmistettava, että käytössä on tekoälypalvelun kaupallinen API- tai yritystili (Enterprise), joka takaa sopimuksellisesti, ettei asiakasdataa käytetä tekoälymallien kouluttamiseen.

