# Endepunkt: Hent pasientens oppmøter

| Felt              | Verdi                                                          |
| ----------------- | -------------------------------------------------------------- |
| **Path**          | `/kuhr/oppslagpasientreiser/v1/process/hentpasientensoppmoter` |
| **HTTP Method**   | `POST`                                                         |
| **Scope**         | `nav:kuhr/oppslagpasientreiser`                                |
| **Request type**  | `application/json`                                             |
| **Response type** | `application/json`                                             |


API-et brukes av **Pasientreiser** for å slå opp hvilke oppmøter en pasient er registrert med i KUHR.
Det gjøres et oppslag basert på pasientens fødselsnummer og en dato.

---

## Request-beskrivelse

| Felt     | Type   | Beskrivelse                          |
| -------- | ------ | ------------------------------------ |
| **fnr**  | String | Pasientens fødselsnummer (11 sifre). |
| **dato** | Date   | Dato for oppslag (`YYYY-MM-DD`).     |

---

## Response-beskrivelse

| Felt                    | Type      | Beskrivelse                                             |
| ----------------------- |-----------|---------------------------------------------------------|
| **oppmoter**            | Array     | Liste over oppmøter registrert på pasienten.            |
| **orgnr**               | String    | Organisasjonsnummer for praksisen/helsetjenesten.       |
| **praksisnavn**         | String    | Navn på praksisen der oppmøtet er registrert.           |
| **oppmotetidspunkt**    | DateTime  | Tidspunkt for oppmøtet (`YYYY-MM-DD HH:mm:ss`).         |
| **samhandlerHPRid**     | String    | HPR-id for samhandleren som utførte behandlingen.       |
| **samhandlertype**      | String    | Type samhandler (kode, f.eks. `LE` for lege).           |
| **status**              | String    | Status for oppmøtet (f.eks. `mottatt`).                 |
| **henvisendeHPRid**     | String    | HPR-id for henvisende behandler (hvis tilgjengelig).     |
| **fysiskoppmote**       | String    | Angir om oppmøtet var fysisk (`ja`/`nei`).              |
| **fritakegenandel**     | String    | Kode for fritak egenandel (f.eks. `F`).                 |
| **kuhrenkeltregningid** | String    | Intern KUHR-id for enkeltregningen knyttet til oppmøtet. |

---

## Eksempel

**Request**

```http
POST /kuhr/oppslagpasientreiser/v1/process/hentpasientensoppmoter
```

```json
{
  "fnr": "16030621658",
  "dato": "2023-12-09"
}
```

**Response (200 OK)**

```json
{
  "oppmoter": [
    {
      "orgnr": "777777777",
      "praksisnavn": "Ørsta Helsesenter",
      "oppmotetidspunkt": "2023-12-09 09:53:04",
      "samhandlerHPRid": "222904176",
      "samhandlertype": "LE",
      "status": "mottatt",
      "henvisendeHPRid": null,
      "fysiskoppmote": "nei",
      "fritakegenandel": "F",
      "kuhrenkeltregningid": "100001810359169"
    }
  ]
}
```

