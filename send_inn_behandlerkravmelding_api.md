## API: Send inn behandlerkravmelding
**(APIet krever bruk av Maskinporten med scope nav:kuhr/krav og JWE ende-til-ende kryptering)**

URLer:
Dev: https://api-preprod.helserefusjon.no/kuhr/krav/v1/process/sendinnbehandlerkravmelding
Prod: 

Mottar en behandlerkravmelding, gjøre en teknisk kontroll av at meldingen oppfyller kravene for videre behandling og lagrer den hvis kravene er oppfylt. Det sendes et svar med en GUID som kan brukes for videre oppfølging av behandlingen av kravet.

Hvis behandlerkravmeldingen ikke oppfyller kravene for videre behandling gis det beskjed tilbake med en GUID, en feilstatus og tilbakemelding på hva problemet er.

Innholdet i meldinger som ikke oppfyller kravene blir ikke lagret. Det blir kun lagret en GUID som referanse og hva problemet var. 

Kravene som kontrolleres er:
- Virus- og trusselscanning av meldingen - det er ingenting i meldingen som kan være teknisk farlig
- Meldingen har riktig JSON format, altså at det er en behandlerkravmelding, med alle påkrevede felt og riktige datatyper og verdier
- Den oppgitte praksis-iden finnes og tilhører den autentiserte innsenderen - er autorisert til å sende inn krav på denne praksisen
- Teknisk duplikat - er meldingen allerede sendt inn og lagret for behandling


## Eksempel POST med felter for rapportering av egenandeler

POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding

Request

```json
{
  "praksisId": "1000005649",
  "behandlerkravmelding": {
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

Response - 200 OK
```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "mottatt"
}
```



## Eksempel POST med alle felter

POST /kuhr/krav/v1/process/sendinnbehandlerkravmelding

Request

```json
{
  "praksisId": "1000005649",
  "behandlerkravmelding": {
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
              {
                "tannkode": "25I"
              },
              {
                "tannkode": "34I"
              }
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

Response - 200 OK
```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "mottatt"
}
```

Response - 400 Bad Request
```json
{
  "behandlerkravmeldingId": "9101aba1-d5a2-410f-8ab8-22da30dca5db",
  "status": "feil_i_melding",
  "tilbakemelding": [
    {
      "kontrollNr": "MK1",
      "melding": "Virus eller annen malware funnet i meldingen"
    },
    {
      "kontrollNr": "MK2",
      "melding": "Meldingen oppfyller ikke kravene til data format"
    }
  ]
}
```

## BKM felt dokumentasjon
| Path                                                              | Description                            | 
|-------------------------------------------------------------------|----------------------------------------|
| praksisId                                                        | Practice ID                            |
| behandlerkravmelding.avdeling                                     | Department name                        |
| behandlerkravmelding.navnEPJ                                      | EPJ system name                        |
| behandlerkravmelding.reshId                                       | RESH ID                                |
| behandlerkravmelding.antallRegninger                              | Number of invoices                     |
| behandlerkravmelding.belop                                        | Total amount                           |
| behandlerkravmelding.regninger                                    | List of invoices                       |
| behandlerkravmelding.regninger[].guid                             | Invoice GUID                           |
| behandlerkravmelding.regninger[].korrigeringBetaltEgenandel       | Correction paid copayment              |
| behandlerkravmelding.regninger[].betaltEgenandel                  | Copayment paid                         |
| behandlerkravmelding.regninger[].kreditering                      | Credit flag                            |
| behandlerkravmelding.regninger[].regningsnummer                   | Invoice number                         |
| behandlerkravmelding.regninger[].tidspunkt                        | Timestamp                              |
| behandlerkravmelding.regninger[].merknad                          | Remarks                                |
| behandlerkravmelding.regninger[].pasient.etternavn                | Patient last name                      |
| behandlerkravmelding.regninger[].pasient.fornavn                  | Patient first name                     |
| behandlerkravmelding.regninger[].pasient.kjonn                    | Patient gender                         |
| behandlerkravmelding.regninger[].pasient.trygdenasjon             | Patient insurance nation               |
| behandlerkravmelding.regninger[].pasient.identifikasjon.id        | Patient ID                             |
| behandlerkravmelding.regninger[].pasient.identifikasjon.type      | Patient ID type                        |
| behandlerkravmelding.regninger[].pasient.fodselsdato              | Patient birth date                     |
| behandlerkravmelding.regninger[].pasient.eea.Dok                  | EEA document                           |
| behandlerkravmelding.regninger[].pasient.eea.CardId               | EEA card ID                            |
| behandlerkravmelding.regninger[].pasient.eea.Id                   | EEA ID                                 |
| behandlerkravmelding.regninger[].pasient.eea.TrygdekontorNavn     | EEA insurance office name              |
| behandlerkravmelding.regninger[].pasient.eea.TrygdekontorNr       | EEA insurance office number            |
| behandlerkravmelding.regninger[].pasient.eea.eeaGyldighetFra      | EEA validity from                      |
| behandlerkravmelding.regninger[].pasient.eea.eeaGyldighet         | EEA validity to                        |
| behandlerkravmelding.regninger[].arsakFriEgenandel                | Reason for no copayment                |
| behandlerkravmelding.regninger[].diagnoser                        | List of diagnoses                      |
| behandlerkravmelding.regninger[].diagnoser[].kodeverk             | Diagnosis code system                  |
| behandlerkravmelding.regninger[].diagnoser[].kode                 | Diagnosis code                         |
| behandlerkravmelding.regninger[].sjeldenMedisinskTilstand         | Rare medical condition                 |
| behandlerkravmelding.regninger[].henvisning.dato                  | Referral date                          |
| behandlerkravmelding.regninger[].henvisning.id                    | Referral ID                            |
| behandlerkravmelding.regninger[].henvisning.diagnoser             | Referral diagnoses                     |
| behandlerkravmelding.regninger[].henvisning.diagnoser[].kodeverk  | Referral diagnosis code system         |
| behandlerkravmelding.regninger[].henvisning.diagnoser[].kode      | Referral diagnosis code                |
| behandlerkravmelding.regninger[].henvisning.henvistFra.id         | Referred from ID                       |
| behandlerkravmelding.regninger[].henvisning.henvistFra.type       | Referred from type                     |
| behandlerkravmelding.regninger[].utforendeBehandler.id            | Performing provider ID                 |
| behandlerkravmelding.regninger[].utforendeBehandler.type          | Performing provider ID type            |
| behandlerkravmelding.regninger[].samarbeidendeBehandler.id        | Collaborating provider ID              |
| behandlerkravmelding.regninger[].samarbeidendeBehandler.type      | Collaborating provider ID type         |
| behandlerkravmelding.regninger[].moderasjonskode                  | Moderation code                        |
| behandlerkravmelding.regninger[].stonadspunkt                     | Subsidy point                          |
| behandlerkravmelding.regninger[].prosedyrekoder                   | List of procedure codes                |
| behandlerkravmelding.regninger[].prosedyrekoder[].kodeverk        | Procedure code system                  |
| behandlerkravmelding.regninger[].prosedyrekoder[].kode            | Procedure code                         |
| behandlerkravmelding.regninger[].takster                          | List of tariffs                        |
| behandlerkravmelding.regninger[].takster[].belop                  | Tariff amount                          |
| behandlerkravmelding.regninger[].takster[].kode                   | Tariff code                            |
| behandlerkravmelding.regninger[].takster[].antall                 | Tariff count                           |
| behandlerkravmelding.regninger[].takster[].tenner                 | List of teeth                          |
| behandlerkravmelding.regninger[].takster[].tenner[].tannkode      | Tooth code                             |
| behandlerkravmelding.regninger[].belop                            | Invoice amount                         |

