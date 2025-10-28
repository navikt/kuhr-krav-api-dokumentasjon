# Endepunkt: Hent status innsendte behandlerkravmeldinger

| Felt              | Verdi                                     |
|-------------------|-------------------------------------------|
| **Path**          | `/kuhr/krav/v1/data/behandlerkravmelding` |
| **HTTP Method**   | `GET`                                     |
| **Scope (en av)** | `nav:kuhr/krav` <br> `hdir:kuhr-krav-api/krav`                     |
| **Response type** | `application/json`                        |

Etter at en **behandlerkrav** er sendt inn til KUHR, kan dette API-et brukes for å hente status og informasjon om kontroll og utbetaling.

* Man kan hente en **oversikt over alle innsendte krav**.
* Man kan hente **detaljer for et enkelt krav**, inkludert kontrollresultater og utbetalingsinformasjon.

---

## Statuser i behandlingsprosessen

### **meldingsstatus**

| Verdi                  | Beskrivelse                                              |
| ---------------------- | -------------------------------------------------------- |
| `mottatt`              | Meldingen er mottatt og venter på videre behandling.     |
| `sendt_til_behandling` | Meldingen er sendt til kontroll og eventuell utbetaling. |
| `feil_i_melding`       | Teknisk feil i meldingen, den kan ikke behandles videre. |

### **kontrollstatus**

| Verdi                | Beskrivelse                               |
| -------------------- |-------------------------------------------|
| `ikke_sendt_til_kontroll`| Kravet er ikke sendt til kontroll         |
| `kontrolleres`       | Kontroll pågår.                           |
| `ferdig_kontrollert` | Kontrollen er ferdig og resultatet klart. |

### **utbetalingsstatus**

| Verdi                       | Beskrivelse                        |
| --------------------------- | ---------------------------------- |
| `ikke_sendt_til_utbetaling` | Ikke sendt videre til utbetaling.  |
| `sendt_til_utbetaling`      | Meldingen er sendt til utbetaling. |
| `utbetalt`                  | Utbetalingen er gjennomført.       |
| `ingen_utbetaling`          | Ingen utbetaling skal foretas.     |

---


## Response – oversikt

Brukes når man henter **alle innsendte behandlerkravmeldinger** 

```http
GET /kuhr/krav/v1/data/behandlerkravmelding
```

| Felt                          | Type     | Beskrivelse                                                                         |
| ----------------------------- | -------- | ----------------------------------------------------------------------------------- |
| **behandlerkravmeldinger**    | Array    | Liste over innsendte behandlerkravmeldinger.                                        |
| └─ **behandlerkravmeldingId** | String   | Unik identifikator (GUID) for kravet.                                               |
| └─ **meldingsstatus**         | String   | Teknisk status for meldingen (`mottatt`, `sendt_til_behandling`, `feil_i_melding`). |
| └─ **kontrollstatus**         | String   | Status for kontroll (f.eks. `kontrolleres`, `ferdig_kontrollert`)     |
| └─ **utbetalingsstatus**      | String   | Status for utbetaling (`utbetalt`, `ingen_utbetaling` osv.)          |
| └─ **innsendingId**           | String   | ID for innsendingen                                            |
| └─ **vedtakId**               | String   | ID for vedtaket                                                |
| └─ **praksisId**              | String   | ID til praksisen som har sendt kravet.                                              |
| └─ **mottattidspunkt**        | DateTime | Tidspunkt for når meldingen ble mottatt (`YYYY-MM-DDTHH:mm:ss±hh:mm`).              |
| └─ **sumKravbelop**           | Number   | Samlet kravbeløp                                                        |
| └─ **sumUtbetaltbelop**       | Number   | Samlet utbetalt beløp                                                   |
| └─ **antallRegninger**        | Integer  | Antall regninger i kravet                                               |

---

## Response – detaljer for ett krav

Brukes når man henter **detaljer for et spesifikt krav**.

```http
GET /kuhr/krav/v1/data/behandlerkravmelding/<behandlerkravmeldingId>
```

| Felt                       | Type     | Beskrivelse                                    |
| -------------------------- | -------- | ---------------------------------------------- |
| **behandlerkravmeldingId** | String   | Unik identifikator (GUID) for kravet.          |
| **meldingsstatus**         | String   | Teknisk status for meldingen.                  |
| **kontrollstatus**         | String   | Status for kontroll.                           |
| **utbetalingsstatus**      | String   | Status for utbetaling.                         |
| **innsendingId**           | String   | ID for innsendingen                                            |
| **vedtakId**               | String   | ID for vedtaket                                                |
| **praksisId**              | String   | ID til praksisen som har sendt kravet.                                              |
| **mottattidspunkt**        | DateTime | Tidspunkt for når meldingen ble mottatt (`YYYY-MM-DDTHH:mm:ss±hh:mm`).              |
| **sumKravbelop**           | Number   | Samlet kravbeløp                                                        |
| **sumUtbetaltbelop**       | Number   | Samlet utbetalt beløp                                                   |
| **antallRegninger**        | Integer  | Antall regninger i kravet                                               |
| **innsending**             | Object   | Informasjon om innsendingen.                   |
| └─ **regninger**           | Array    | Liste over regninger i innsendingen.           |
|    └─ **status**           | String   | Status for regningen.                          |
|    └─ **guid**             | String   | GUID for regningen.                            |
|    └─ **regningsnummer**   | String   | Regningsnummer.                                |
|    └─ **merknader**        | Array    | Eventuelle merknader på regningen.             |
|      └─ **nummer**         | String   | Merknadskode.                                  |
|      └─ **tekst**          | String   | Beskrivelse av merknaden.                      |
| └─ **merknader**           | Array    | Eventuelle merknader på innsendingen.          |
| **vedtak**                 | Object   | Informasjon om vedtaket.                       |
| └─ **innsendinger**        | Array    | Liste over innsendinger som inngår i vedtaket. |
|    └─ **innsendingId**     | String   | ID for innsending.                             |
| └─ **utbetaling**          | Object   | Utbetalingsinformasjon.                        |
|    └─ **utbetalingId**     | String   | ID for utbetalingen.                           |
|    └─ **belop**            | Number   | Utbetalt beløp.                                |
|    └─ **utbetaltdato**     | Date     | Dato for utbetalingen.                         |
|    └─ **kontonr**          | String   | Kontonummer for utbetalingen.                  |
|    └─ **kid**              | String   | KID-nummer for utbetalingen.                   |
| └─ **merknader**           | Array    | Eventuelle merknader på vedtaket.              |
|    └─ **nummer**           | String   | Merknadskode.                                  |
|    └─ **tekst**            | String   | Beskrivelse av merknaden.                      |

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
      "sumKravbelop" : 500.0,
      "sumUtbetaltbelop" : 500.0,
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
  "behandlerkravmeldingId": "3f5cb512-d274-4c93-90af-437391c4294a",
  "meldingsstatus": "sendt_til_behandling",
  "kontrollstatus": "ferdig_kontrollert",
  "utbetalingsstatus": "ingen_utbetaling",  
  "praksisId": "1004326178",
  "mottattidspunkt": "2025-07-16T11:20:00+02:00",
  "innsending": {
    "innsendingId": "",
    "sumKravbelop": 0.0,
    "sumUtbetaltbelop": 0.0,
    "antallRegninger": 1,
    "regninger": [
      {
        "status" : "",
        "guid": "",
        "regningsnummer": "",
        "merknader": [
          {
            "nummer": "",
            "tekst": ""
          }
        ]
      }
    ],
    "merknader": [
      {
        "nummer": "",
        "tekst": ""
      }
    ]
  },
  "vedtak": {
    "vedtakId": "",
    "innsendinger": [
      { "innsendingId": "" }
    ],
    "utbetaling": {
      "utbetalingId": "",
      "belop": 0.0,
      "utbetaltdato": "",
      "kontonr": "",
      "kid": ""
    },
    "merknader": [
      {
        "nummer": "",
        "tekst": ""
      }
    ]
  }
}
```

