# Endepunkt: Hent status innsendte behandlerkravmeldinger

| Felt              | Verdi                                     |
|-------------------|-------------------------------------------|
| **Path**          | `/kuhr/krav/v1/data/behandlerkravmelding` |
| **HTTP Method**   | `GET`                                     |
| **Scope (en av)** | `nav:kuhr/krav` <br> `hdir:kuhr-krav-api/krav`                     |
| **Response type** | `application/json`                        |

Etter at et **behandlerkrav** er sendt inn til KUHR, kan dette API-et brukes for å hente status og informasjon om kontroll og utbetaling.

* Man kan hente en **oversikt over alle innsendte krav**.
* Man kan hente **detaljer for et enkelt krav**, inkludert kontrollresultater og utbetalingsinformasjon.

---

## Statuser i behandlingsprosessen
En krav som sendes til KUHR går igjennom tre steg frem til en endelig utbetaling og utsending av vedtaksbrev. Disse går i sekvens, er asynkrone både internt og i forhold til hverandre og kan variere i behandlingstid avhengig av størrelse på kravet, eksterne systemer og prosesser og eventuell manuellsaksbehandling.  

### **meldingsstatus**
Først mottas kravet som en melding og det gjøres en grunnleggende teknisk kontroll om kravet kan behandles, før det sendes videre til kontrollen. Dette steget tar normalt 1-2 sekunder, før meldingen er sendt til behandling.


| Verdi                   | Beskrivelse                                              |
|-------------------------|----------------------------------------------------------|
| `mottatt`               | Meldingen er mottatt og venter på videre behandling.     |
| `klar_til_behandling`   | Meldingen er klar til kontroll og eventuell utbetaling.  |
| `sendt_til_behandling`  | Meldingen er sendt til kontroll og eventuell utbetaling. |
| `feil_i_melding`        | Teknisk feil i meldingen, den kan ikke behandles videre. |

### **kontrollstatus**
Det neste steget er kontrollen av regningene i kravet. Hovedsakelig gjøres dette helt automatisk og resultatet vil normalt være klart i løpet 10-20 sekunder.

I enkelte tilfeller vil kravet måtte kontrolleres manuelt og det vil ta betydelig lenger tid før resultatet er klart. Status blir da satt til kontrolleres_manuelt.

Resultatet av kontrollen er ikke tilgjengelig før kontrollstatus er ferdig_kontrollert. Det vil si at status på regningene er behandles inntil da.

| Verdi                     | Beskrivelse                               |
|---------------------------|-------------------------------------------|
| `ikke_sendt_til_kontroll` | Kravet er ikke sendt til kontroll         |
| `kontrolleres`            | Kontroll pågår.                           |
| `kontrolleres_manuelt`    | Kontroll pågår manuelt.                   |
| `ferdig_kontrollert`      | Kontrollen er ferdig og resultatet klart. |

### **utbetalingsstatus**
Det siste steget er vedtak og utbetaling. Denne prosessen starter kl 16 hver ettermiddag. Da blir det laget en utbetaling som normalt tar 2-3 virkedager før er gjennomført.

I enkelte tilfeller skal det ikke gjøres en utbetaling og man får en ingen_utbetaling status.

Etter at utbetalingen er gjennomført blir det sendt ut et vedtaksbrev på e-post til helseaktøren. Den samme informasjonen er tilgjengelig i APIet.

Et vedtak og en utbetaling kan gjelde flere krav. Hvis det blir sendt inn flere krav samme dag så vil disse samles opp på samme utbetaling. Det betyr at de får samme vedtakId. 


| Verdi                       | Beskrivelse                       |
| --------------------------- |-----------------------------------|
| `ikke_sendt_til_utbetaling` | Ikke sendt videre til utbetaling. |
| `sendt_til_utbetaling`      | Kravet er sendt til utbetaling.   |
| `utbetalt`                  | Utbetalingen er gjennomført.      |
| `ingen_utbetaling`          | Ingen utbetaling skal foretas.    |


### **rebehandling**
Rebehandling av en melding skjer når det har oppstått en feil i kontrollen og det blir gjort en retting i KUHR og kontrollen kjøres på nytt. Det er da ikke nødvendig å sende inn kravet på nytt, men det vil være et nytt kontroll resultat på noen av regningene i kravet. Når det er gjort en rebehandling kommer dette som en egen rad i oversikten og detaljene. Denne vil ha feltet rebehandling=true. 

Dette er noe som skjer veldig sjelden og alltid være involvere manuelle prosesser og oppfølging. Det er likevel viktig at systemene har en grunnleggende håndtering når dette skjer. 

---


## Response – oversikt

Brukes når man henter **alle innsendte behandlerkravmeldinger** 

```http
GET /kuhr/krav/v1/data/behandlerkravmelding?fomMottattdato=<dato>&tomMottattdato=<dato>
```

Query parametere

| Felt                          | Beskrivelse                                                                     |                     
| ----------------------------- |---------------------------------------------------------------------------------|
| fomMottattdato | Optional fra og med mottattdato for filtrering av resultatet. Format yyyy-MM-dd |
| tomMottattdato | Optional til og med mottattdato for filtrering av resultatet. Format yyyy-MM-dd                  |


Response

| Felt                          | Type     | Beskrivelse                                                            |
|-------------------------------| -------- |------------------------------------------------------------------------|
| **behandlerkravmeldinger**    | Array    | Liste over innsendte behandlerkravmeldinger.                           |
| **behandlerkravmeldingId** | String   | Unik identifikator (GUID) for meldingen.                               |
| **meldingsstatus**         | String   | Teknisk status for meldingen.                                          |
| **kontrollstatus**         | String   | Status for kontroll                                                    |
| **utbetalingsstatus**      | String   | Status for utbetaling                                                  |
| **innsendingId**           | String   | ID for innsendingen                                                    |
| **rebehandling**           | boolean  | Flagg som angir om dette er en rebehandling av en melding              |
| **vedtakId**               | String   | ID for vedtaket. Et vedtak og en utbetaling kan gjelde flere krav.     |
| **praksisId**              | String   | ID til praksisen som har sendt kravet.                                 |
| **mottattidspunkt**        | DateTime | Tidspunkt for når meldingen ble mottatt (`YYYY-MM-DDTHH:mm:ss±hh:mm`). |
| **sumKravbelop**           | Number   | Samlet kravbeløp                                                       |
| **sumUtbetaltbelop**       | Number   | Samlet utbetalt beløp                                                  |
| **antallRegninger**        | Integer  | Antall regninger i kravet                                              |

---

## Response – detaljer for ett krav

Brukes når man henter **detaljer for et spesifikt krav**.

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/<behandlerkravmeldingId>
```

| Felt                       | Type     | Beskrivelse                                                            |
|----------------------------|----------|------------------------------------------------------------------------|
| **behandlerkravmeldinger**  | Array    | Liste over innsendte behandlerkravmeldinger.                           |
| **behandlerkravmeldingId** | String   | Unik identifikator (GUID) for meldingen.                               |
| **meldingsstatus**         | String   | Teknisk status for meldingen.                                          |
| **kontrollstatus**         | String   | Status for kontroll.                                                   |
| **utbetalingsstatus**      | String   | Status for utbetaling.                                                 |
| **innsendingId**           | String   | ID for innsendingen                                                    |
| **rebehandling**           | boolean  | Flagg som angir om dette er en rebehandling av en melding              |
| **vedtakId**               | String   | ID for vedtaket. Et vedtak og en utbetaling kan gjelde flere krav.     |
| **praksisId**              | String   | ID til praksisen som har sendt kravet.                                 |
| **mottattidspunkt**        | DateTime | Tidspunkt for når meldingen ble mottatt (`YYYY-MM-DDTHH:mm:ss±hh:mm`). |
| **sumKravbelop**           | Number   | Samlet kravbeløp                                                       |
| **sumUtbetaltbelop**       | Number   | Samlet utbetalt beløp                                                  |
| **antallRegninger**        | Integer  | Antall regninger i kravet                                              |
| **innsending**             | Object   | Informasjon om innsendingen.                                           |
| └─ **regninger**           | Array    | Liste over regninger i innsendingen.                                   |
|    └─ **status**           | String   | Status for regningen. Enten behandles, godkjent eller avvist           |
|    └─ **guid**             | String   | GUID for regningen.                                                    |
|    └─ **regningsnummer**   | String   | Regningsnummer.                                                        |
|    └─ **merknader**        | Array    | Eventuelle merknader på regningen.                                     |
|      └─ **nummer**         | String   | Merknadskode.                                                          |
|      └─ **tekst**          | String   | Beskrivelse av merknaden.                                              |
| └─ **merknader**           | Array    | Eventuelle merknader på innsendingen.                                  |
| **vedtak**                 | Object   | Informasjon om vedtaket og utbetalingen.                               |
| └─ **utbetaling**          | Object   | Utbetalingsinformasjon.                                                |
|    └─ **belop**            | Number   | Utbetalt beløp.                                                        |
|    └─ **utbetaltdato**     | Date     | Dato for utbetalingen.                                                 |
|    └─ **kontonr**          | String   | Kontonummer for utbetalingen.                                          |
|    └─ **kid**              | String   | KID-nummer for utbetalingen.                                           |
| └─ **merknader**           | Array    | Eventuelle merknader på vedtaket.                                      |
|    └─ **nummer**           | String   | Merknadskode.                                                          |
|    └─ **tekst**            | String   | Beskrivelse av merknaden.                                              |

---




## Eksempel: Hente alle innsendte krav

**Request**

```http
GET /kuhr/krav/v1/data/behandlerkravmelding
```

**Response (200 OK)**

```json
{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
      "meldingsstatus": "mottatt",
      "praksisId" : "1004326178",
      "mottattidspunkt" : "2025-07-17T11:20:00+02:00"
    },
    {
      "behandlerkravmeldingId": "ab1355d2-871b-4cc7-a143-f62125fab9ca",
      "meldingsstatus": "sendt_til_behandling",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "utbetalt",
      "innsendingId": "",
      "vedtakId": "",
      "praksisId" : "1004326178",
      "mottattidspunkt" : "2025-07-16T11:20:00+02:00",
      "sumKravbelop" : 500,
      "sumUtbetaltbelop" : 500,
      "antallRegninger" : 1
    }
  ]
}
```

---

## Eksempel: Hente detaljer for ett krav

**Request**

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/3f5cb512-d274-4c93-90af-437391c4294a
```

**Response (200 OK)**

```json
{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "3f5cb512-d274-4c93-90af-437391c4294a",
      "meldingsstatus": "sendt_til_behandling",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "ingen_utbetaling",
      "innsendingId": "100001811800023",
      "vedtakId": "100001811802344",
      "praksisId": "1004326178",
      "mottattidspunkt": "2025-07-16T11:20:00+02:00",
      "innsending": {
        "sumKravbelop": 396,
        "sumUtbetaltbelop": 198,
        "antallRegninger": 2,
        "regninger": [
          {
            "status": "avvist",
            "guid": "af49247c-09fc-4654-98e4-7d0e7affb4a6",
            "regningsnummer": "6897",
            "merknader": [
              {
                "nummer": "710",
                "tekst": "Samhandler har registrert at pasienten har frikort. Frikortvedtak er ikke registrert i frikortregisteret. Fra 1. april 2011 vil slike regninger bli automatisk avvist."
              }
            ]
          },
          {
            "status": "godkjent",
            "guid": "6f444575-5bdb-43ff-919b-c20cd0d4fa69",
            "regningsnummer": "6898",
            "merknader": []
          }
        ],
        "merknader": [
          {
            "nummer": "458",
            "tekst": "For behandling som kommer inn under frikort for helsetjenester, skal egenandelene rapporteres til Helfo minst hver 14. dag. Helfo anbefaler innsending via Helsenettet, ta kontakt med din EPJ-leverandør for oppkobling. Har du unntak fra å levere via Helsenettet, kan du bruke opplastningstjenesten i tjenesteportal for helseaktør https://internett-portal.helsedirektoratet.no"
          }
        ]
      },
      "vedtak": {
        "utbetaling": {
          "belop": 198,
          "utbetaltdato": "2025-07-20T08:30:00+02:00",
          "kontonr": "12345678901",
          "kid": "123456789"
        },
        "merknader": [
          {
            "nummer": "1478",
            "tekst": "1. januar 2021 ble de to frikort-ordningene for helsetjenester slått sammen til én ordning, med ett felles egenandelstak. Du må rapportere senest 14 dager etter behandlingen, for å unngå at frikortet til pasientene blir forsinket. Ved å se på din oversikt over takstbruk øverst på side 2 i dette vedtaket, vil du se snittalderen på dine regninger med egenandeler. Snittalderen bør være en god del lavere enn 14 dager, fordi du ved innsendingen hver 14. dag rapporterer inn egenandeler som er fra 0 til 14 dager gamle."
          }
        ]
      }
    }
  ]
}
```

