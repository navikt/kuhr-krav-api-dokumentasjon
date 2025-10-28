# Endepunkt: Send inn behandlerkrav

| Felt              | Verdi                                               |
| ----------------- |-----------------------------------------------------|
| **Path**          | `/kuhr/krav/v1/process/sendinnbehandlerkravmelding` |
| **HTTP Method**   | `POST`                                              |
| **Scope (en av)** | `nav:kuhr/krav` <br> `hdir:kuhr-krav-api/krav`                     |
| **Request type**  | JWE                                                 |
| **Response type** | `application/json`                                  |

Endepunktet mottar en **behandlerkrav**, utfører teknisk kontroll av at meldingen oppfyller kravene, og lagrer den dersom kontrollen er godkjent.

Requesten må sendes kryptert, se [JWE-kryptering av POST-requests (POST/JWE)](jwe_ende_til_ende_kryptering.md)

Detaljert dokumentasjon av BKM finnes her:
[Behandlerkravmelding JSON](behandlerkravmelding_json.md)

Ved suksess returneres en GUID som kan brukes til videre oppfølging av kravet.
Dersom meldingen ikke oppfyller kravene, returneres en GUID, koder og tilbakemelding på hva som er galt.

---

## Response

| Felt                       | Type   | Beskrivelse                                                    |
| -------------------------- | ------ | -------------------------------------------------------------- |
| **behandlerkravmeldingId** | String | Unik identifikator (GUID) for meldingen.                       |
| **status**                 | String | Status for meldingen, f.eks. `mottatt` eller `feil_i_melding`. |
| **feilmeldinger**         | Array  | (Kun ved feil) Liste med feilmeldinger.                        |
| └─ **kode**          | String | Kontrollkode som identifiserer hvilken validering som feilet.  |
| └─ **melding**             | String | Beskrivende tekst, med spesifikk detaljer om feilen.                         |

---

## Feilmeldingskoder

Ved innsending gjøres følgende kontroller:

* **Virus- og trusselscanning** – meldingen inneholder ikke skadelig innhold.
* **JSON-format** – riktig struktur, påkrevde felter og gyldige datatyper.
* **PraksisId** – må eksistere og være autorisert for innlogget samhandler.
* **Duplikatkontroll** – meldingen er ikke allerede sendt inn og lagret.

Ved feil gis det en status = `feil_i_melding` med en eller flere feilmeldinger.

| Kode                                   | Beskrivelse                                                              |
| -------------------------------------- |--------------------------------------------------------------------------|
| UKJENT_FORMAT                      | Behandlerkravmelding har ikke JSON-format                                |
| UKJENT_FELT                        | Ukjent felt i JSON                                                       |
| MANGLER_PRAKSISID                  | Mangler påkrevd felt `praksisId`                                         |
| MANGLER_BEHANDLERKRAV  | Mangler påkrevd felt `behandlerkrav`                                     |
| MANGLER_REGNINGER             | Mangler påkrevd felt `regninger`                                         |
| MANGLER_GUID                  | Mangler påkrevd felt `guid`                                              |
| UKJENT_PRAKSISID    | Oppgitt `praksisId` finnes ikke for helseaktøren                         |
| DUPLIKAT_KRAV           | Meldingen er identisk med en tidligere melding som allerede er behandlet |

---

## Eksempler

### Eksempel 1 – innsending med egenandeler

**Request**

```http
POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding
```

```json
{
  "praksisId": "1000005649",
  "behandlerkrav": {
    "regninger": [
      {
        "guid": "0b8dc559-6c2d-419b-aeef-faaf537c6117",
        "regningsnummer": "12496",
        "tidspunkt": "2014-05-17T11:20:00+02:00",
        "pasient": {
          "identifikasjon": {
            "id": "14057012345",
            "type": "FNR"
          }
        },
        "arsakFriEgenandel": "F",
        "belop": 435.00
      }
    ],
    "antallRegninger": 1,
    "belop": 435.00
  }
}
```

**Response (200 OK)**

```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "mottatt"
}
```

---

### Eksempel 2 – manglende behandlerkrav

**Request**

```http
POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding
```

```json
{
  "praksisId": "1000005649"
}
```

**Response (400 Bad Request)**

```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "feil_i_melding",
  "feilmeldinger": [
    {
      "kode": "MANGLER_BEHANDLERKRAV",
      "melding": "Mangler påkrevd felt behandlerkrav"
    }
  ]
}
```

---

### Eksempel 3 – innsending med alle felter

**Request**

```http
POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding
```

```json
{
  "praksisId": "1000005649",
  "behandlerkrav": {
    "avdeling": "Akuttmottaket, Kongsvinger",
    "navnEPJ": "TestEPJ",
    "reshId": "123456",
    "regninger": [
      {
        "guid": "0b8dc559-6c2d-419b-aeef-faaf537c6117",
        "korrigeringBetaltEgenandel": false,
        "betaltEgenandel": false,
        "kreditering": false,
        "regningsnummer": "12496",
        "tidspunkt": "2014-05-17T11:20:00+02:00",
        "merknad": "",
        "pasient": {
          "etternavn": "Etternavn",
          "fornavn": "Fornavn",
          "kjonn": "2",
          "trygdenasjon": "NO",
          "identifikasjon": {
            "id": "14057012345",
            "type": "FNR"
          },
          "fodselsdato": "2014-03-11",
          "eea": {
            "Dok": "",
            "CardId": "",
            "Id": "",
            "TrygdekontorNavn": "",
            "TrygdekontorNr": "",
            "eeaGyldighetFra": "",
            "eeaGyldighet": ""
          }
        },
        "arsakFriEgenandel": "F",
        "diagnoser": [
          {
            "kodeverk": "2.16.578.1.12.4.1.1.7170",
            "kode": "L09"
          }
        ],
        "sjeldenMedisinskTilstand": "FAB",
        "henvisning": {
          "dato": "2014-03-11",
          "id": "1",
          "diagnoser": [
            {
              "kodeverk": "2.16.578.1.12.4.1.1.7170",
              "kode": "L09"
            }
          ],
          "henvistFra": {
            "id": "70008786",
            "type": "HPR"
          }
        },
        "utforendeBehandler": {
          "id": "14057012345",
          "type": "FNR"
        },
        "relatertBehandler": {
          "id": "70008786",
          "type": "HPR"
        },
        "moderasjonskode": "S",
        "stonadspunkt": "4h",
        "prosedyrekoder": [
          {
            "kodeverk": "M",
            "kode": "ACFE10"
          },
          {
            "kodeverk": "M",
            "kode": "ACFE20"
          }
        ],
        "takster": [
          {
            "belop": 435.00,
            "kode": "3ad",
            "antall": 1,
            "tenner": [
              { "tannkode": "25I" },
              { "tannkode": "34I" }
            ]
          }
        ],
        "belop": 435.00
      }
    ],
    "antallRegninger": 1,
    "belop": 435.00
  }
}
```

**Response (200 OK)**

```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "mottatt"
}
```
