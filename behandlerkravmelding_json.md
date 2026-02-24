# Behandlerkravmelding JSON

## Eksempel med alle felter

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
            "eeaGyldighet": "",
            "utstedelsesdato": ""
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
        "rekvirent": {
          "organisasjon": "9988991",
          "hprNr": "70008786"
        },
        "moderasjonskode": "S",
        "stonadspunkt": "4h",
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
        "prosedyrekoder": [
          {
            "kodeverk": "M",
            "kode": "ACFE10"
          },
          {
            "kodeverk": "M",
            "kode": "ACFE20"
          },
          {
            "kodeverk": "R",
            "kode": "TSV0LL///ZTX6FB",
            "belop": 25255.3
          },
          {
            "kodeverk": "G",
            "kode": "T56000/P11430/P20712/P33715/P33541/P33540/P33705/P33463/P32120/P33462/P33500/P33530",
            "belop": 42347.08
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

## Beskrivelse av felter

### praksisId

```json
{
  "praksisId": "1000005649"
}
```

Iden på praksisen det skal sendes inn krav for. Hentes fra Endepunkt: Hent registrerte praksiser

----

### behandlerkrav

```json
{
  "avdeling": "Akuttmottaket, Kongsvinger",
  "navnEPJ": "TestEPJ",
  "reshId": "123456",
  "antallRegninger": 1,
  "belop": 435.00,
  "regninger": []
}
```

----

#### avdeling

Avdeling er obligatorisk for rehabiliteringsinstitusjon, ved egenandelsinnrapportering fra poliklinikker og for
behandlingsreiser i utlandet.

----

#### navnEPJ

Angir hvilket EPJ-system som er benyttet.

----

#### reshId

Angir RESH-id til avdelingen. RESH er obligatorisk for rehabiliteringsinstitusjon, ved egenandelsinnrapportering fra
poliklinikker og for behandlingsreiser i utlandet.

----

#### antallRegninger

Samlet antall innsendte regninger.

----

#### belop

Samlet krav for innsendte regninger.

----

### regninger

Enkeltregning for en behandling. Et behandlerkrav kan inneholde mange enkeltregninger.

```json
{
  "guid": "0b8dc559-6c2d-419b-aeef-faaf537c6117",
  "korrigeringBetaltEgenandel": false,
  "betaltEgenandel": false,
  "kreditering": false,
  "regningsnummer": "12496",
  "tidspunkt": "2014-05-17T11:20:00+02:00",
  "merknad": "",
  "pasient": {},
  "arsakFriEgenandel": "F",
  "diagnoser": [],
  "sjeldenMedisinskTilstand": "FAB",
  "henvisning": {},
  "utforendeBehandler": {
    "id": "14057012345",
    "type": "FNR"
  },
  "relatertBehandler": {
    "id": "70008786",
    "type": "HPR"
  },
  "rekvirent": {
    "organisasjon": "9988991",
    "hprNr": "70008786"
  },
  "moderasjonskode": "S",
  "stonadspunkt": "4h",
  "takster": [],
  "prosedyrekoder": [],
  "norpatkoder": [],
  "ncrpkoder": [],
  "belop": 435.00
}
```

----

##### guid

Unik identifikator. OID (object identifier – objektidentifikator)

GUID kan benyttes i tillegg til å oppgi RegningNr. Hvis GUID er benyttet, kan denne senere benyttes til å referere til
opprinnelig regning hvis ny regning innebærer at den enten krediterer en tidligere innsendt regning eller ved at den
korrigerer informasjon om BetaltEgenandel. Hvis GUID benyttes på denne måte, er det ikke nødvendig å ta med klassene
Behandling og Pasient samt underliggende klasser da GUID refererer til den opprinnelige regningen hvor denne
informasjonen allerede er oppgitt.

----

##### korrigeringBetaltEgenandel

Brukes for å angi om det er en korrigering av om egenandel er betalt på en tidligere innsendt regning. På original
regning skal korrigering alltid være satt til false.

Om det angis at egenandel er betalt eller ikke så skal denne være med for å angi om det er original verdi eller en
korrigering av tidligere innsendt regning. Angis med true eller false.

----

##### betaltEgenandel

Brukes for å angi om egenandel er betalt.

Settes til true om egenandel er betalt og false om den ikke er betalt. Må angis hvis BetaltEgenandel er oppgitt. Det er
kun mulig å endre erBetalt fra false til true. Regningen vil bli avvist hvis endringen går fra at egenandelen er betalt
til ikke å være betalt.

----

##### kreditering

Hvis en tidligere innsendt og godkjent enkeltregning er feil, kan regningen krediteres. Etter kreditering kan korrigert
regning sendes inn på ny.

Opprinnelig innsendt og godkjent regning kan krediteres ved å sende inn samme regning på ny med kreditering = true. Hvis
GUID er benyttet på opprinnelig regning er det ikke nødvendig å ta med klassene Behandling og Pasient samt underliggende
klasser da GUID refererer til den opprinnelige regningen hvor denne informasjonen allerede er oppgitt. Hvis GUID ikke
benyttes skal krediteringen inneholde samme regningsinformasjon som på regningen som skal krediteres. Delkreditering av
en regning er ikke mulig – det er kun hele regningen som kan krediteres. Regningen som krediteres må være mottatt før
krediteringsregning. Hvis ikke vil krediteringsregningen avvises. Det er ikke mulig å kreditere en regning som er eldre
enn 3 år. Hvis krediteringen er godkjent vil den opprinnelige regning få status som kreditert og refusjonsbeløpet på
denne regningen vil komme til fradrag ved neste utbetaling. Etter at krediteringsregning er godkjent kan korrigert
regning sendes inn. Beløp på en krediteringsregning bør angis som et minusbeløp og minusbeløpet bør også tas hensyn til
i SumKravSamlet.

----

##### regningsnummer

Unikt nummer per regning for hver behandler. Dette gjelder i forhold til alle regninger denne behandleren sender inn for
refusjonskrav. Regningsnummer ønskes registrert som et numerisk løpenummer uten spesialtegn som oppgis i stigende
rekkefølge og skal gjenspeile behandlingstidspunktet.

Dersom en tidligere innsendt regning er avvist, kan den korrigeres og sendes inn med samme regningsnummer. For øvrig
skal regningsnummeret være et unikt nummer innenfor den enkelte virksomhet.

----

##### tidspunkt

Dato og klokkeslett ved oppstart av konsultasjon/behandling. Angis på formatet YYYY-MM-DDTHH:mm:ss±hh:mm

----

##### merknad

Fri tekst, kommentar til enkeltregningen.

Fylles ut ved behov. Kan brukes til å begrunne hvorfor ikke fullt fnr/dnr er angitt, hvorfor en takst er repetert et
høyt antall ganger etc. Ved bruk av Npe1-takst skal Norsk pasientskadeerstatnings saksnummer oppgis på formatet
YYYY/12345 eks. 2025/11223.

----

##### arsakFriEgenandel

Kode for årsak til fri egenandel.

Eksempler på kodeverdier:
A Smittefarlig sykdom
B Barn under 16 år
F Frikort
G HIV-infeksjon
H Pasientens tilstand til hinder for avkreving av egenandel

Gjeldende kodeverk 7461 finnes her:
https://finnkode.helsedirektoratet.no/adm/collections/7461?q=egenandels

----

##### sjeldenMedisinskTilstand

Feltet benyttes av tannleger. Sykdomslisten ble fjernet fra og med 1. januar 2017 og sykdomslistepunkt skal ikke angis
for fysioterapeuter på regninger etter dette tidspunkt.
For tannleger benyttes feltet for å angi hvilken sjelden medisinsk tilstand pasienten har. Feltet må benyttes av
tannlege hvis kode 1 for sjelden medisinsk tilstand er angitt som behandlingsform (innslagspunkt). Listen over de
sjelden medisinske tilstandene for tannlege finnes her:

https://www.helsedirektoratet.no/rundskriv/folketrygdloven-kap-5/folketrygdloven--5-6--5-6-a-og--5-25--undersokelse-og-behandling-hos-tannlege-og-tannpleier-for-sykdom-og-skade/smt-listen

----

##### utforendeBehandler

Identifikasjonen til legen, fysioterapeuten, sykepleieren, jordmor eller tannlegen som har utført behandlingen.

Behandlerident kan oppgis for leger, kommunal fysioterapi, helsestasjon og tannlege. For lege må Behandlerident oppgis
der praksistype er 2 eller 4 og kravet er sendt inn fra en kommune dvs. der behandlerkravmeldingen er signert med
kommunens viksomhetssertifikat). Behandlerident for sykepleier skal oppgis der lege har delegert
behandlingen/konsultasjonen til sykepleier. TypeId som godtas er kun FNR, DNR og HPR. For kommunal fysioterapi må
Behandlerident oppgis på alle regninger. Det er ønskelig at HPR-nummer til lege eller jordmor oppgis på alle regninger
fra helsestasjon. Der tannlegekrav sender inn på vegne av en institusjon (UIO, UIB etc.) og hvor behandlingen er utført
av ulike tannleger kan Behandlerident brukes for å angi hvem som har utført behandlingen.

----

##### relatertBehandler

For tannlege: Angir helsepersonellnummeret til tannlege det er samarbeidet med ved implantatbehandling. Ved bruk av
takster for kjeveortopedi (600-takstene) skal helsepersonellnummeret til behandleren som har henvist til behandlingen
oppgis.

----

##### rekvirent

----

##### moderasjonskode

Benyttes kun av tannleger. Feltet brukes for å angi om pasienten er omfattet av søskenmoderasjon ved kjeveortopedisk
behandling. For at refusjonen skal gis med høyeste prosentsats må S oppgis

Kodeverk:
S Søskenmoderasjon

----

##### stonadspunkt

Angir hvilket hovedstønadspunkt behandlingen gjelder. Brukes kun av tannlege og tannpleier.

For hver behandling må det angis hvilket stønadspunkt behandlingen dekkes etter. Det er hovedstønadspunkt som skal
angis.

Se listen over gjeldende koder her:
https://www.helsedirektoratet.no/rundskriv/folketrygdloven-kap-5/rundskriv-til-folketrygdloven--5-6--5-6-a-og--5-25--undersokelse-og-behandling-hos-tannlege-og-tannpleier-for-sykdom-og-skade

----

##### belop

Samlet krav for innsendte regninger.

----

##### pasient

```json
{
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
    "eeaGyldighet": "",
    "utstedelsesdato": ""
  }
}

```

----

###### etternavn

For å angi personens etternavn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis, men må oppgis hvis kun
pasientens fødselsdato (DateOfBirth) er angitt

----

###### fornavn

For å angi personens fornavn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis, men må oppgis hvis kun
pasientens fødselsdato (DateOfBirth) er angitt.

----

###### kjonn

Sosialt kjønn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis.

Kodeverk: 3101 Kjønn

- 1 Mann
- 2 Kvinne
- 9 Ikke spesifisert

----

###### trygdenasjon

Personens medlemsstat knyttet til trygderettigheter. For eksempelvis en asylsøker som har trygderettigheter i Norge,
skal det ikke oppgis landkode for pasientens hjemland. Skal kun brukes hvis pasientens nasjonalitet ikke er norsk (NO).

Landkode i henhold til ISO3166
Kommentar:
Konvensjonsland.
Kodeverk: 9043 Landkoder

----

###### identifikasjon

Identifikasjon som personen er eller har vært kjent under. Vanligvis benyttes fødselsnummer (FNR) eller D-nummer (DNR)
for personer. Hvis det ikke er mulig å oppgi Ident må DateOfBirth oppgis.

Primært er det FNR og DNR som skal benyttes. PNR benyttes for personer fra Australia. UID benyttes for utenlandske
statsborgere uten FNR/DNR. HNR skal kun benyttes hvor dette er absolutt nødvendig eksempelvis for nyfødte personer som
ikke har blitt tildelt fødselsnummer.

Kodeverk: ID-type for personer (subsett)

- FNR Fødselsnummer
- DNR D-nummer
- HNR H-nummer (nødnummer)
- PNR Passnummer
- UID Utenlandsk personID
- DUF DUF-nummer

----

###### fodselsdato

Angis på formatet ÅÅÅÅ-MM-DD (1982-03-30).

----

###### eea

```json
{
  "Dok": "",
  "CardId": "",
  "Id": "",
  "TrygdekontorNavn": "",
  "TrygdekontorNr": "",
  "eeaGyldighetFra": "",
  "eeaGyldighet": ""
}
```

----

##### diagnoser

```json
[
  {
    "kodeverk": "2.16.578.1.12.4.1.1.7170",
    "kode": "L09"
  }
] 
```

Diagnosen(e) behandleren setter. Hoveddiagnosen angis først. Det er ingen begrensning på hvor mange diagnoser som kan
oppgis.

ICPC-2 eller ICPC-2b (beriket) skal benyttes for fysioterapeuter og kiropraktorer. Psykologer, tannleger og fritt
behandlingsvalg skal benytte ICD-10. Leger benytter kodeverk ICPC-2 eller ICPC-2b for allmennpraksis og ICD-10 for
spesialistpraksis. For pasientreiser, rehabiliteringsinstitusjoner, behandlingsreiser i utlandet og kommunal fysioterapi
skal diagnose ikke oppgis. Diagnosekode er ikke obligatorisk for tannleger. Dersom diagnose oppgis, kan ICD-10
kodeverket benyttes, eventuelt ICD-DA3 for dentale diagnoser.

----

##### henvisning

```json
{
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
}
```

----

###### dato

Dato for når henvisningen er utstedt. Brukes av leger, psykologer, kjeveortopeder og ortoptister. Feltet kan også brukes
av andre behandlere som mottar henvisninger.

For kjeveortopeder er det obligatorisk å oppgi henvisningsdato fra 01.01.2026.

Angis på formatet ÅÅÅÅ-MM-DD (1982-03-30).

----

###### id

Unik identifikasjon for hver henvisning som er brukt som grunnlag for behandlingen. Alle behandlinger knyttet til samme
henvisning benytter samme henvisnings-ID. Brukes av leger, psykologer, ortoptister og kjeveortopeder. Feltet kan også
brukes av andre behandlere som mottar henvisninger. Feltet er obligatorisk for ortoptist.

HenvisningsId'en bidrar til å identifisere behandlinger utført i samme behandlingsserie. Hvis HenvisningsDato,
HenvisningsId, HenvisningDiagnose og RelatertBehandler oppgis er det ikke nødvendig å sende inn henvisningsblankett (
05-04.21 og 05.08.05) til Helfo.

----

###### diagnoser

Diagnosen(e) som henvisende behandler oppgir. Hoveddiagnosen angis først. Brukes av leger, psykologer og ortoptister.
Feltet kan også brukes av andre behandlere som mottar henvisninger.

Psykologer og ortoptister skal benytte ICD-10. Leger benytter kodeverk ICPC-2 eller ICPC-2b for allmennpraksis og ICD-10
for spesialistpraksis. Hvis HenvisningsDato, HenvisningsId, HenvisningDiagnose og RelatertBehandler oppgis er det ikke
nødvendig å sende inn henvisningsblankett (05-04.21 og 05.08.05) til Helfo.

----

###### henvistFra

For lege, psykolog, ortoptist og andre behandlere som mottar henvisninger: Angir helsepersonellnummeret eller
fødselsnummer/d-nummer til den behandler som har henvist pasienten. Når barnevernsadministrasjon har henvist til
psykolog: Oppgi barnevernets/kommunens organisasjonsnummer.

----

##### takster

```json
[
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
]
```

----

###### belop

Takstkodens /laboratoriekodens refusjonsverdi på behandlingstidspunktet.

Verdi behandleren krever refundert for første forekomst av taksten. Ved repetisjon av taksten skrives samme beløp, og
antall bruk av taksten i ""antall"" feltet. Den faktiske verdien blir beregnet ut i fra verdi, antall og
repetisjonsprosent som framgår av takstforskrift.

----

###### kode

Takstkoder og laboratoriekoder som fastsatt i forskrift eller annet regelverk.Alle koder som gjelder samme
behandling/undersøkelse skal føres opp på én og samme regning.

Taksten skrives slik den er oppgitt i forskriften, eks. A1a, B20, osv.

----

###### antall

Antall ganger en takst eller laboratoriekode er brukt. Standardverdi = 1

Hvis for eksempel takst XXX er benyttet én gang skal det stå Antall=1.
Dersom takst XXX er benyttet én gang, og repetert 2 ganger utover den første, skal det stå Antall=3.

----

###### tenner

Angir hvilken region i munnen som har blitt behandlet eventuelt hvilken tann eller hvilke tenner som er behandlet med
tilhørende angivelse av flate. Brukes kun av tannlege.

For hver behandling må enten region eller tann oppgis. Hvis region er oppgitt (eks. 01 for overkjeve) er det ikke
nødvendig å angi flate. Hvis tann er oppgitt (17) skal det normalt også oppgis hvilken flate som er behandlet. Hvis det
er flere flater som er behandlet på en enkelt tann oppgis dette ved bruk av flere koder (eksempelvis 17O, 17M). Kode
angis med to siffer og bokstav eventuelt kun med to siffer.
Eksempel på kode:

- 00 Hele munnen
- 01 Overkjeve (maxilær-området)
- 10 Øvre høyre kvadrant
- 11 Tann 11
- 11I Tann 11 incisal kant
- 11M Tann 11 mesial overflate
- 11D Tann 11 distal overflate
- 11G Tann 11 radiculær
- 11B Tann 11 buccal overflate
- 11P Tann 11 palatinal overflate
- 16O Tann 16 occlusal overflate
- 41L Tann 41 lingual overflate

Kodeverk:
ISO 3950 - 1995

----

##### prosedyrekoder

```json
[
  {
    "kodeverk": "M",
    "kode": "ACFE10"
  },
  {
    "kodeverk": "M",
    "kode": "ACFE20"
  },
  {
    "kodeverk": "R",
    "kode": "TSV0LL///ZTX6FB",
    "belop": 25255.3
  },
  {
    "kodeverk": "G",
    "kode": "T56000/P11430/P20712/P33715/P33541/P33540/P33705/P33463/P32120/P33462/P33500/P33530",
    "belop": 42347.08
  }
]
```

Feltet benyttes av spesialistleger og psykologer.

Kodeverk: 8410 Medisinske kodeverk

- K NCSP-N
- M NCMP
- R NCRP
- Q NSCP/NCMP/NCRP Kode fra et av kodeverkene

Kodeverkene for NCMP og NCSP finnes her:
https://www.helsedirektoratet.no/digitalisering-og-e-helse/helsefaglige-kodeverk/nkpk





