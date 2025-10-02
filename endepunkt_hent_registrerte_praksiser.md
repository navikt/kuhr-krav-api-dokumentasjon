# Endepunkt: Hent registrerte praksiser

| Felt              | Verdi                        |
| ----------------- | ---------------------------- |
| **Path**          | `/kuhr/krav/v1/data/praksis` |
| **HTTP Method**   | `GET`                        |
| **Scope**         | `nav:kuhr/krav`     |
| **Response type** | `application/json`           |


Endepunktet gir en oversikt over praksiser som er registrert på helseaktøren.
Dette brukes for å velge riktig **praksisId** ved innsending av krav.

For å sende inn en behandlerkrav til KUHR må det finnes en registrert praksis.
**PraksisId** må oppgis når meldingen sendes inn.

---

##  Response

| Felt                | Type   | Beskrivelse                                                                     |
| ------------------- | ------ |---------------------------------------------------------------------------------|
| **praksiser**       | Array  | Liste over registrerte praksiser knyttet til helseaktøren.                      |
| **praksisId**       | String | Unik identifikator for praksisen (må oppgis ved innsending av krav).            |
| **navn**            | String | Navn på praksisen.                                                              |
| **orgnr**           | String | Organisasjonsnummer til praksisen.                                              |
| **type**            | String | [Praksistyper](kodeverk.md#Praksistyper)                                        |
| **status**          | String | Status på praksisen, `ok` eller `ikke_ok`. Angir om praksisen er klar til bruk. |
| **gyldigePerioder** | Array  | Liste over perioder hvor praksisen er gyldig.                                   |
| └─ **fraOgMedDato** | Date   | Startdato for gyldighetsperioden (`YYYY-MM-DD`).                                |
| └─ **tilOgMedDato** | Date   | Sluttdato for gyldighetsperioden (`YYYY-MM-DD`). Kan være null for åpen slutt.  |

---

## Eksempel

**Request**

```http
GET /kuhr/krav/v1/data/praksis
```

**Response (200 OK)**

```json
{
  "praksiser": [
    {
      "praksisId": "1004326178",
      "navn": "Test Praksis",
      "orgnr": "999999999",
      "type": "FALE",
      "status": "ok",
      "gyldigePerioder": [
        {
          "fraOgMedDato": "2020-01-01",
          "tilOgMedDato": "2025-07-15"
        }
      ]
    }
  ]
}
```

