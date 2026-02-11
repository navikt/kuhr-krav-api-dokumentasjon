# Eksempel: Normal arbeidsflyt

Eksempelet viser normal bruk av API-et for innsending av krav, oppfølging av kontrollresultater og henting av utbetalingsinformasjon.

I dette scenariet jobber en lege som fastlege i en kommune og skal sende inn refusjonskrav for behandling av pasienter i perioden **01.01.2026–07.01.2026**.


## Pålogging

For å kunne sende inn krav via API-et fra et EPJ-system må legen autentisere seg med **HelseID**. EPJ-systemet må i tillegg ha tildelt scopet **hdir:kuhr-krav-api/krav** i sin HelseID-klientprofil.

Dette gir EPJ-systemet et tilgangstoken som gjør det mulig å benytte Krav-API-et på vegne av legen. Alle endepunkter i API-et er tilgangsstyrt basert på legens identitet. Det betyr at det kun er mulig å sende inn og hente data som tilhører den legen som er autentisert via HelseID.



## Valg av praksis

Før en helseaktør kan sende inn krav, må det være registrert en praksis med tilhørende kontonummer i tjenesteportalen for helseaktører.

Det første steget i arbeidsflyten er derfor å hente ut tilgjengelige praksiser og velge hvilken praksis kravet skal sendes inn for.

Dette gjøres ved å kalle endepunktet [Hent registrerte praksiser](../endepunkt_hent_registrerte_praksiser.md). Endepunktet returnerer en liste over praksiser som er registrert på helseaktøren, inkludert blant annet praksistype og gyldighetsperioder. For korrekt behandling av kravet er det viktig å velge riktig praksis.

I dette eksempelet er det registrert to praksiser for legen:

* én for fastlegevirksomhet
* én for legevakt

Den aktuelle praksisen for dette kravet har praksistype **FALE** og er gyldig i perioden **01.01.2020–15.07.2028**.

En oversikt over gyldige praksistypekoder finnes i [Kodeverk](../kodeverk.md).



### Request

```http
GET /kuhr/krav/v1/data/praksis
```

### Response (200 OK)

```json
{
  "praksiser": [
    {
      "praksisId": "1004326178",
      "navn": "Kurbadet Helsesenter",
      "orgnr": "999999999",
      "type": "FALE",
      "status": "ok",
      "gyldigePerioder": [
        {
          "fraOgMedDato": "2020-01-01",
          "tilOgMedDato": "2028-07-15"
        }
      ]
    },
    {
      "praksisId": "1004326468",
      "navn": "Sentrum Legevakt",
      "orgnr": "999999998",
      "type": "LEVA",
      "status": "ok",
      "gyldigePerioder": [
        {
          "fraOgMedDato": "2024-01-01",
          "tilOgMedDato": "2028-07-15"
        }
      ]
    }
  ]
}
```


## Innsending av krav
I dette eksempelet er grunnlaget for kravet behandling av to pasienter den **06.01.2026**.

**Pasient STA TOMAT FNR=47908702367**

Fastlegen har telefonkontakt med pasienten kl. **13:03**, og det avtales at pasienten skal komme til legekontoret senere samme dag.

Følgende takst registreres:

* **1bd** – Enkel pasientkontakt skriftlig, per telefon eller ved elektronisk kommunikasjon.
  Taksten forutsetter at det gis råd eller veiledning, og gjelder også når kontakten resulterer i sykmelding, rekvisisjon eller henvisning.

Pasienten møter opp på legekontoret kl. **13:35**. Fastlegen fastslår at pasienten har brukket ankelen, med diagnosekode **L73**, og det skrives sykmelding.

Følgende takster registreres:

* **2ad** – Konsultasjon hos allmennlege
* **2cd** – Tillegg for tidsbruk ved konsultasjonsvarighet utover 20 minutter (per påbegynt 15 min)
* **L1** – Sykmelding


**Pasient MUNTER HORISONT - FNR=47908500667**

Pasienten ringer fastlegen kl. **15:00** og ber om resept i forbindelse med urinveisinfeksjon (diagnosekode **U71**).

Følgende takst registreres:

* **1i** – Skriving av elektronisk resept

Ved en feil registreres denne hendelsen på neste måned, **06.02.2026**.


Kl. **15:30** sender fastlegen inn et krav til **KUHR** gjennom sitt EPJ-system. 

Dette gjøres ved å kalle endepunktet [Send inn behandlerkrav](../endepunkt_send_inn_behandlerkravmelding.md). 

Først i meldingen oppgis hvilken praksisId kravet gjelder, i dette tilfellet Kurbadet Helsesenter. For hver registrerte behandling lages det en regning, med en unik guid, regningsnummer, tidspunkt, informasjon om pasienten, diagnose og takster for det som ble utført. Til slutt oppgis antall regninger og totalt beløp i kravet. 

Det er viktig at hver regning har en unik GUID og regningsnummer for helseaktøren. Disse kan senere brukes for oppretting, kreditering og korreksjon av betalt egenandel. 

Responsen er normalt mottatt status og en behandlerkravmeldingId som kan brukes for å følge opp den videre behandlingen. 


### Request

```http
POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding
```

```json
{
  "praksisId": "1004326178",
  "behandlerkrav": {
    "navnEPJ": "PasientJournalen v1.0",
    "reshId": null,
    "regninger": [
      {
        "guid": "f0bcd358-0676-49c7-94f8-c055c4b21332",
        "regningsnummer": "4",
        "tidspunkt": "2026-01-06T13:03:00+01:00",
        "pasient": {
          "identifikasjon": {
            "id": "47908702367",
            "type": "FNR"
          }
        },
        "diagnoser": [
          {
            "kodeverk": "2.16.578.1.12.4.1.1.7170",
            "kode": "L73"
          }
        ],
        "takster": [
          {
            "belop": 74.00,
            "kode": "1bd",
            "antall": 1
          }
        ],
        "belop": 74.00
      },
      {
        "guid": "b7184d58-0ed4-4277-a60c-720b1daf69f7",
        "regningsnummer": "5",
        "tidspunkt": "2026-01-06T13:35:00+01:00",
        "pasient": {
          "identifikasjon": {
            "id": "47908702367",
            "type": "FNR"
          }
        },
        "diagnoser": [
          {
            "kodeverk": "2.16.578.1.12.4.1.1.7170",
            "kode": "L73"
          }
        ],
        "takster": [
          {
            "belop": 15.00,
            "kode": "2ad",
            "antall": 1
          },
          {
            "belop": 255.00,
            "kode": "2cd",
            "antall": 1
          },
          {
            "belop": 26.00,
            "kode": "L1",
            "antall": 1
          }
        ],
        "belop": 296.00
      },
      {
        "guid": "32b24c0b-facd-4f5e-aff9-9849ae477db5",
        "regningsnummer": "6",
        "tidspunkt": "2026-02-06T15:00:00+01:00",
        "pasient": {
          "identifikasjon": {
            "id": "47908500667",
            "type": "FNR"
          }
        },
        "diagnoser": [
          {
            "kodeverk": "2.16.578.1.12.4.1.1.7170",
            "kode": "U71"
          }
        ],
        "takster": [
          {
            "belop": 64.00,
            "kode": "1i",
            "antall": 1
          }
        ],
        "belop": 64.00
      }
    ],
    "antallRegninger": 3,
    "belop": 434.00
  }
}

```

### Response (200 OK)

```json
{
  "behandlerkravmeldingId": "5138fbfe-3c9c-406f-b854-91177b00bb1a",
  "status": "mottatt"
}
```


## Hente kontrollresultat
Etter at kravet er sendt inn venter EPJ-systemet noen sekunder før det gjør et oppslag på **behandlerkravmeldingId** for å hente resultatet av kontrollen i KUHR.

Dette gjøres ved å kalle endepunktet [Hent status for innsendte krav](../endepunkt_hent_status_innsendte_behandlerkravmeldinger.md).

### Request

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/5138fbfe-3c9c-406f-b854-91177b00bb1a
```

### Response (200 OK)

```json
{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "5138fbfe-3c9c-406f-b854-91177b00bb1a",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "ikke_sendt_til_utbetaling",
      "innsendingId": "100001812531977",
      "vedtakId": null,
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-06T15:30:29+01:00",
      "sumKravbelop": 434,
      "sumUtbetaltbelop": null,
      "antallRegninger": 3,
      "innsending": {
        "regninger": [
          {
            "status": "avvist",
            "guid": "f0bcd358-0676-49c7-94f8-c055c4b21332",
            "regningsnummer": "4",
            "merknader": [
              {
                "nummer": 470,
                "tekst": "Takst 1bd/1be/1bk for enkel pasientkontakt, forespørsel, rådgivning ved papirbrev eller telefon står som ugyldig kombinasjon med alle takster. Her er takst 1bd/1bk skrevet på ny regning, og brukt 60 minutter før/etter konsultasjonstakst på regningsnummer 5. Refusjonskrav for takst 1bd/1bk er avslått under forutsetning av at taksten er brukt i forbindelse med konsultasjon."
              }
            ]
          },
          {
            "status": "godkjent",
            "guid": "b7184d58-0ed4-4277-a60c-720b1daf69f7",
            "regningsnummer": "5",
            "merknader": []
          },
          {
            "status": "avvist",
            "guid": "32b24c0b-facd-4f5e-aff9-9849ae477db5",
            "regningsnummer": "6",
            "merknader": [
              {
                "nummer": 766,
                "tekst": "Enkeltregning er datert frem i tid, oppgitt konsultasjonstid er 06.02.2026 15:00, innsending er mottatt 06.01.2026 15:30"
              }
            ]
          }
        ],
        "merknader": []
      },
      "vedtak": {
        "utbetaling": {
          "belop": null,
          "utbetaltdato": null,
          "kontonr": null,
          "kid": null
        },
        "merknader": []
      }
    }
  ]
}

```


## Rette feil og sende inn på nytt
Av de tre innsendte regningene er en godkjent og to avvist.

Den ene er avvist med merknadsnummer:
- 470 Takst 1bd/1be/1bk for enkel pasientkontakt, forespørsel, rådgivning ved papirbrev eller telefon står som ugyldig kombinasjon med alle takster. Her er takst 1bd/1bk skrevet på ny regning, og brukt 60 minutter før/etter konsultasjonstakst på regningsnummer 5. Refusjonskrav for takst 1bd/1bk er avslått under forutsetning av at taksten er brukt i forbindelse med konsultasjon.

Det betyr at det faktisk var feil bruk av taksten 1bd på regningen og det er ikke noe legen kan gjøre for å rette opp i dette.

Den andre avvisningen gjelder merknadsnummer:
- 766 Enkeltregning er datert frem i tid, oppgitt konsultasjonstid er 06.02.2026 15:00, innsending er mottatt 06.01.2026 15:30

Dette er en feil som legen kan rette opp ved å registrere riktig dato. For deretter å sende inn en ny kravmelding med bare denne regningen, med samme **guid** og **regningsnummer**. 

Dette gjøres ved å kalle endepunktet [Send inn behandlerkrav](../endepunkt_send_inn_behandlerkravmelding.md). 


### Request

```http
POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding
```

```json
{
  "praksisId": "1004326178",
  "behandlerkrav": {
    "navnEPJ": "PasientJournalen v1.0",
    "reshId": null,
    "regninger": [
      {
        "guid": "32b24c0b-facd-4f5e-aff9-9849ae477db5",
        "regningsnummer": "6",
        "tidspunkt": "2026-01-06T15:00:00+01:00",
        "pasient": {
          "identifikasjon": {
            "id": "47908500667",
            "type": "FNR"
          }
        },
        "diagnoser": [
          {
            "kodeverk": "2.16.578.1.12.4.1.1.7170",
            "kode": "U71"
          }
        ],
        "takster": [
          {
            "belop": 64,
            "kode": "1i",
            "antall": 1
          }
        ],
        "belop": 64
      }
    ],
    "antallRegninger": 1,
    "belop": 64
  }
}

```

### Response (200 OK)

```json
{
  "behandlerkravmeldingId": "f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71",
  "status": "mottatt"
}
```


## Hente nytt kontrollresultat
Etter at kravet er sendt inn venter EPJ-systemet noen sekunder før det gjør et oppslag på **behandlerkravmeldingId** for å hente resultatet av kontrollen i KUHR. Denne gangen er regningen godkjent.

### Request

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71
```

### Response (200 OK)

```json

{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "ikke_sendt_til_utbetaling",
      "innsendingId": "100001812532014",
      "vedtakId": null,
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-06T15:45:57+01:00",
      "sumKravbelop": 64,
      "sumUtbetaltbelop": null,
      "antallRegninger": 1,
      "innsending": {
        "regninger": [
          {
            "status": "godkjent",
            "guid": "32b24c0b-facd-4f5e-aff9-9849ae477db5",
            "regningsnummer": "6",
            "merknader": []
          }
        ],
        "merknader": []
      },
      "vedtak": {
        "utbetaling": {
          "belop": null,
          "utbetaltdato": null,
          "kontonr": null,
          "kid": null
        },
        "merknader": []
      }
    }
  ]
}
```

## Vise oversikt over innsendte krav
Etter at en eller flere meldinger er sendt inn til KUHR kan man følge opp den videre behandlingen frem til utbetaling ved å hente ut en oversikt over innsendte behandlerkravmeldinger. 

Dette gjøres ved å kalle endepunktet [Hent status for innsendte krav](../endepunkt_hent_status_innsendte_behandlerkravmeldinger.md).

Her oppgis query parameter for fra og med dato for å bare ta med meldinger som er sendt inn i 2026 i oversikten.

### Request

```http
GET /kuhr/krav/v1/data/behandlerkravmelding?fomMottattdato=2026-01-01
```

### Response (200 OK)

```json
{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "ikke_sendt_til_utbetaling",
      "innsendingId": "100001812532014",
      "vedtakId": null,
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-06T15:45:57+01:00",
      "sumKravbelop": 64,
      "sumUtbetaltbelop": null,
      "antallRegninger": 1
    },
    {
      "behandlerkravmeldingId": "5138fbfe-3c9c-406f-b854-91177b00bb1a",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "ikke_sendt_til_utbetaling",
      "innsendingId": "100001812531977",
      "vedtakId": null,
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-06T15:30:29+01:00",
      "sumKravbelop": 434,
      "sumUtbetaltbelop": null,
      "antallRegninger": 3
    }
  ]
}
```

## Sendt til utbetaling
Klokka 16 hver virkedag vil innsendte krav for en helseaktør samles opp og bli del av et vedtak og en utbetaling. Oppsamlingen i vedtak gjøres per praksisId. I behandlerkrav-oversikten vil man da se at det er samme vedtakId på begge. 

Dette gjøres ved å kalle endepunktet [Hent status for innsendte krav](../endepunkt_hent_status_innsendte_behandlerkravmeldinger.md).

Her oppgis query parameter for fra og med dato for å bare ta med meldinger som er sendt inn i 2026 i oversikten.

### Request

```http
GET /kuhr/krav/v1/data/behandlerkravmelding?fomMottattdato=2026-01-01
```

### Response (200 OK)

```json
{
  "behandlerkravmeldinger": [
    {
      "behandlerkravmeldingId": "f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "sendt_til_utbetaling",
      "innsendingId": "100001812532014",
      "vedtakId": "100001812531790",
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-06T15:45:57+01:00",
      "sumKravbelop": 64,
      "sumUtbetaltbelop": 64,
      "antallRegninger": 1
    },
    {
      "behandlerkravmeldingId": "5138fbfe-3c9c-406f-b854-91177b00bb1a",
      "meldingsstatus": "ferdig_behandlet",
      "kontrollstatus": "ferdig_kontrollert",
      "utbetalingsstatus": "sendt_til_utbetaling",
      "innsendingId": "100001812531977",
      "vedtakId": "100001812531790",
      "praksisId": "1004326178",
      "mottattidspunkt": "2026-01-0615:30:29+01:00",
      "sumKravbelop": 434,
      "sumUtbetaltbelop": 296,
      "antallRegninger": 3
    }
  ]
}
```


## Hente utbetalingsinformasjon
Etter 2-3 virkedager vil utbetalingen være gjennomført og man kan hente ut utbetalingsinformasjon, som tidspunkt, kontonr, kid og beløp. 

Dette gjøres ved å kalle endepunktet [Hent status for innsendte krav](../endepunkt_hent_status_innsendte_behandlerkravmeldinger.md).

I dette eksempelet er utbetalingsbeløpet på 360 kroner, mens bare 64 kroner var fra denne meldingen.

### Request

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71
```

### Response (200 OK)

```json
{
  "behandlerkravmeldingId": "f00ddf25-f4f1-49b7-acd7-88cf4cbf9c71",
  "meldingsstatus": "ferdig_behandlet",
  "kontrollstatus": "ferdig_kontrollert",
  "utbetalingsstatus": "utbetalt",
  "innsendingId": "100001812532014",
  "vedtakId": "100001812531790",
  "praksisId": "1004326178",
  "mottattidspunkt": "2026-01-06T15:45:57+01:00",
  "sumKravbelop": 64,
  "sumUtbetaltbelop": 64,
  "antallRegninger": 1,
  "innsending": {
    "regninger": [
      {
        "status": "godkjent",
        "guid": "32b24c0b-facd-4f5e-aff9-9849ae477db5",
        "regningsnummer": "6",
        "merknader": []
      }
    ],
    "merknader": []
  },
  "vedtak": {
    "utbetaling": {
      "belop": 360,
      "utbetaltdato": "2026-01-07T23:45:57+01:00",
      "kontonr": "12345678901",
      "kid": null
    },
    "merknader": []
  }
}
```

