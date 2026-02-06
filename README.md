# KUHR Krav API

**(Dette er dokumentasjon for et API som brukes i en pilot – det er ikke et ferdig produkt for allmenn bruk.)**

API-et brukes av EPJ-systemer til innsending av krav og rapportering av egenandeler til Helfo. Innsending skjer gjennom en behandlerkravmelding (BKM) via en HTTP POST-request. API-et gir også mulighet til å hente status og følge opp kontroll og utbetaling.

API-et kan benyttes av kommuner, privatpraktiserende behandlere og andre helseaktører, inkludert: leger, helsestasjoner, psykologer, fysioterapeuter, kiropraktorer, tannleger, tannpleiere, rehabiliteringsinstitusjoner, audiopedagoger, logopeder, ortoptister, jordmødre – samt Pasientreiser ANS.

API-et kan erstatte dagens innsending av behandlerkrav, som beskrevet her:
[Behandlerkrav (BKM)](https://www.helsedirektoratet.no/standarder/behandlerkravmelding-bkm)

Alle krav som er tidligere sendt inn gjennom andre kanaler vil også være tilgjengelig i APIet for oppfølging og historikk. Det betyr at APIet kan brukes til oversikt og rapportering på tvers av praksiser og systemer over tid for den enkelte helseaktøren.  

---

## Forutsetninger

For å kunne bruke API-et må helseaktøren være registrert hos Helfo med gyldig avtale og praksis for innsending av krav.

Se [helfo.no](https://www.helfo.no/) for mer informasjon om avtaler og praksisregistrering.

En helseaktør kan være registrert enten som virksomhet (org.nr.) eller som behandler (HPR-nummer eller fødselsnummer). Når Helfo har godkjent registreringen, gis det automatisk tilgang til API-et.

* **Virksomheter** autentiseres via **Maskinporten**
* **Behandlere** autentiseres via **HelseID**

Selve innsendingen skjer ved å sende en BKM kryptert med KUHRs offentlige nøkkel. Etter innsending kan krav følges opp ved å hente status og kontrollresultater.

---

## API-endepunkter

| Navn                                                                                        | Path                                                         | Scope (en av)                            | Type      | Beskrivelse                                                                                                           |
| ------------------------------------------------------------------------------------------- |--------------------------------------------------------------|------------------------------------------| --------- |-----------------------------------------------------------------------------------------------------------------------|
| [Hent registrerte praksiser](endepunkt_hent_registrerte_praksiser.md)                       | /kuhr/krav/v1/data/praksis                                   | nav:kuhr/krav<br>hdir:kuhr-krav-api/krav | GET/JSON  | Gir oversikt over praksiser registrert på helseaktøren. Brukes for å velge praksis-id ved innsending av krav.         |
| [Send inn behandlerkrav](endepunkt_send_inn_behandlerkravmelding.md)                        | /kuhr/krav/v1/process/sendinnbehandlerkravmelding            | nav:kuhr/krav<br>hdir:kuhr-krav-api/krav                            | POST/JWE  | Innsending av BKM.                                                                                                    |
| [Hent status for innsendte krav](endepunkt_hent_status_innsendte_behandlerkravmeldinger.md) | /kuhr/krav/v1/data/behandlerkravmelding                      | nav:kuhr/krav<br>hdir:kuhr-krav-api/krav                            | GET/JSON  | Henter oversikt over alle innsendte krav eller detaljer for et enkelt krav, inkludert kontrollresultat og utbetaling. |
| [Hent pasientens oppmøter](endepunkt_hent_pasientensoppmoter.md)                            | /kuhr/oppslagpasientreiser/v1/process/hentpasientensoppmoter | nav:kuhr/oppslagpasientreiser            | POST/JSON | Henter oversikt over oppmøter en pasient er registrert med i KUHR. (Kun tilgjengelig for Pasientreiser ANS.)          |
| KUHR Public Certificate Key - JWK                                                           | /kuhr/krav/v1/jwk                                            | nav:kuhr/krav<br>hdir:kuhr-krav-api/krav                             | GET/JSON  | Returnerer KUHRs offentlige nøkkel (JWK) for kryptering av POST-requests.                                             |

---

### Typer endepunkt

| Type          | Beskrivelse                                                                              |
| ------------- | ---------------------------------------------------------------------------------------- |
| **POST/JWE**  | HTTP POST der request-body er kryptert som JWE. Responsen returneres som ukryptert JSON. |
| **GET/JSON**  | HTTP GET som returnerer ukryptert JSON.                                                  |
| **POST/JSON** | HTTP POST med ukryptert JSON i request-body. Responsen returneres som ukryptert JSON.    |

*(Ukryptert betyr at dataene kun er sikret med HTTPS, ikke innholdskryptert.)*

---

## Behandlerkravmelding (BKM) JSON

En BKM i JSON-format er selve fagmeldingen som sendes inn. Den kan inneholde:

* Informasjon om behandler eller virksomhet
* Pasientopplysninger
* Takster for utført behandling
* Kravbeløp
* Diagnoser
* Henvisningsinformasjon

Detaljert dokumentasjon finnes her:
[Behandlerkravmelding JSON](behandlerkravmelding_json.md)

Her finnes også en mapping fra XML-basert BKM til JSON for å lette migrering.

---

## Kodeverk

API-et benytter flere kodeverk og kodetabeller. Disse er dokumentert her:
[Kodeverk](kodeverk.md)

---

## Autentisering og autorisasjon

### Maskinporten (virksomheter)

Virksomheter bruker **Maskinporten** for autentisering og autorisasjon.
Organisasjonen må være registrert både i Maskinporten og hos Helfo, med samme organisasjonsnummer.

Mer informasjon:
[Maskinporten – API-konsumentguide](https://docs.digdir.no/docs/Maskinporten/maskinporten_guide_apikonsument.html)

API-tilgangen kan delegeres videre til en leverandør gjennom Altinn.

**Maskinporten-scopes**

| Scope                         | Beskrivelse                                      |
| ----------------------------- | ------------------------------------------------ |
| nav:kuhr/krav                 | Tilgang til innsending og oppslag av krav.       |
| nav:kuhr/oppslagpasientreiser | Tilgang til pasientoppmøter (kun Pasientreiser). |

---

### HelseID (behandlere)

Behandlere som fremsetter krav personlig bruker **HelseID** til autentisering og autorisasjon. Det betyr at EPJen må tilby sluttbruker pålogging med HelseID.
Behandleren må være registrert hos Helfo med gyldig avtale og praksis.

Mer informasjon:
[HelseID – NHN utviklerportal](https://utviklerportal.nhn.no/informasjonstjenester/helseid/)

For APIet brukes HelseID scope **hdir:kuhr-krav-api/krav**

---

## Kryptering av helseopplysninger

Endepunkter av typen **POST/JWE** krever ende-til-ende-kryptering med JSON Web Encryption (JWE).
Dataene må krypteres med KUHRs offentlige nøkkel før innsending.

Dette er et ekstra sikkerhetstiltak på grunn av mengden sensitive helseopplysninger som behandles.

Mer informasjon:
[JWE-kryptering av POST-requests (POST/JWE)](jwe_ende_til_ende_kryptering.md)

---

## Miljøer

| Miljø | KUHR Krav API URL                                                            | Maskinporten                                                         | HelseID         |
| ----- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------- | --------------- |
| DEV   | [https://api-preprod.helserefusjon.no](https://api-preprod.helserefusjon.no) | [https://test.maskinporten.no/jwk](https://test.maskinporten.no/jwk) | https://helseid-sts.test.nhn.no/.well-known/openid-configuration/jwks |
| PROD  | (kommer senere)                                                              | (kommer senere)                                                      | (kommer senere) |


For mer informasjon om [Testdata](testdata.md) 

---

## Eksempler

| Navn                                                     | Beskrivelse                                            |
|----------------------------------------------------------|--------------------------------------------------------|
| [API-arbeidsflyt](eksempler/normal_arbeidsflyt_eksempel.md) | Eksempel på flyt fra innsending av krav til utbetaling |
| BKM-eksempel for lege                                    |                                                        |
| BKM-eksempel for tannlege                                |                                                        |
| BKM-eksempel for poliklinikk                             |                                                        |


---

## Endringslogg

Det føres en endringslogg for dokumentasjonen til API-et for å kunne følge oppdateringer:
[Endringslogg](endringslogg.md)


