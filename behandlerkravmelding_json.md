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

## Beskrivelse av felter

<table>
  <tbody>
    <tr>
      <th colspan="2">praksisId</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>praksisId</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">
Iden på praksisen det skal sendes inn krav for. Hentes fra Endepunkt: Hent registrerte praksiser
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">avdeling</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.avdeling</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandlerkrav.Avdeling</td>
    </tr>
    <tr>
      <td colspan="2">
Avdeling er obligatorisk for rehabiliteringsinstitusjon, ved egenandelsinnrapportering fra poliklinikker og for behandlingsreiser i utlandet.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">navnEPJ</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.navnEPJ</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandlerkrav.navnEPJ</td>
    </tr>
    <tr>
      <td colspan="2">
Angir hvilket EPJ-system som er benyttet.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">reshId</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.reshId</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandlerkrav.RESH</td>
    </tr>
    <tr>
      <td colspan="2">
Angir RESH-id til avdelingen. RESH er obligatorisk for rehabiliteringsinstitusjon, ved egenandelsinnrapportering fra poliklinikker og for behandlingsreiser i utlandet.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">guid</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.guid</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.GUID</td>
    </tr>
    <tr>
      <td colspan="2">
Unik identifikator. OID (object identifier – objektidentifikator)<br><br>GUID kan benyttes i tillegg til å oppgi RegningNr. Hvis GUID er benyttet, kan denne senere benyttes til å referere til opprinnelig regning hvis ny regning innebærer at den enten krediterer en tidligere innsendt regning eller ved at den korrigerer informasjon om BetaltEgenandel. Hvis GUID benyttes på denne måte, er det ikke nødvendig å ta med klassene Behandling og Pasient samt underliggende klasser da GUID refererer til den opprinnelige regningen hvor denne informasjonen allerede er oppgitt.<br>Eksempel: {4329c75a-e267-423e-8172-bd5ca622e4a8}
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">korrigeringBetaltEgenandel</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.korrigeringBetaltEgenandel</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.BetaltEgenandel.korrigering</td>
    </tr>
    <tr>
      <td colspan="2">
Brukes for å angi om det er en korrigering av om egenandel er betalt på en tidligere innsendt regning. På original regning skal korrigering alltid være satt til false.  <br><br>Om det angis at egenandel er betalt eller ikke så skal denne være med for å angi om det er original verdi eller en korrigering av tidligere innsendt regning. Angis med true eller false.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">betaltEgenandel</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.betaltEgenandel</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.BetaltEgenandel.erBetalt</td>
    </tr>
    <tr>
      <td colspan="2">
Brukes for å angi om egenandel er betalt. <br><br>Settes til true om egenandel er betalt og false om den ikke er betalt. Må angis hvis BetaltEgenandel er oppgitt. Det er kun mulig å endre erBetalt fra false til true. Regningen vil bli avvist hvis endringen går fra at egenandelen er betalt til ikke å være betalt.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kreditering</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.kreditering</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.Kreditering</td>
    </tr>
    <tr>
      <td colspan="2">
Hvis en tidligere innsendt og godkjent enkeltregning er feil, kan regningen krediteres. Etter kreditering kan korrigert regning sendes inn på ny.<br><br>Opprinnelig innsendt og godkjent regning kan krediteres ved å sende inn samme regning på ny med kreditering = true. Hvis GUID er benyttet på opprinnelig regning er det ikke nødvendig å ta med klassene Behandling og Pasient samt underliggende klasser da GUID refererer til den opprinnelige regningen hvor denne informasjonen allerede er oppgitt. Hvis GUID ikke benyttes skal krediteringen inneholde samme regningsinformasjon som på regningen som skal krediteres. Delkreditering av en regning er ikke mulig – det er kun hele regningen som kan krediteres. Regningen som krediteres må være mottatt før krediteringsregning. Hvis ikke vil krediteringsregningen avvises. Det er ikke mulig å kreditere en regning som er eldre enn 3 år. Hvis krediteringen er godkjent vil den opprinnelige regning få status som kreditert og refusjonsbeløpet på denne regningen vil komme til fradrag ved neste utbetaling. Etter at krediteringsregning er godkjent kan korrigert regning sendes inn. Beløp på en krediteringsregning bør angis som et minusbeløp og minusbeløpet bør også tas hensyn til i SumKravSamlet.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">regningsnummer</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.regningsnummer</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.RegningNr</td>
    </tr>
    <tr>
      <td colspan="2">
Unikt nummer per regning for hver behandler. Dette gjelder i forhold til alle regninger denne behandleren sender inn for refusjonskrav. Regningsnummer ønskes registrert som et numerisk løpenummer uten spesialtegn som oppgis i stigende rekkefølge og skal gjenspeile behandlingstidspunktet.<br><br>Dersom en tidligere innsendt regning er avvist, kan den korrigeres og sendes inn med samme regningsnummer. For øvrig skal regningsnummeret være et unikt nummer innenfor den enkelte virksomhet. 
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">tidspunkt</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.tidspunkt</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.DatoTid</td>
    </tr>
    <tr>
      <td colspan="2">
Dato og klokkeslett ved oppstart av konsultasjon/behandling. Angiis på formatet 2009-04-02T10:05:24
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">merknad</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.merknad</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Enkeltregning.Merknad</td>
    </tr>
    <tr>
      <td colspan="2">
Fri tekst, kommentar til enkeltregningen.<br><br>Fylles ut ved behov. Kan brukes til å begrunne hvorfor ikke fullt fnr/dnr er angitt, hvorfor en takst er repetert et høyt antall ganger etc. Ved bruk av Npe1-takst skal Norsk pasientskadeerstatnings saksnummer oppgis på formatet YYYY/12345 eks. 2025/11223.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">etternavn</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.etternavn</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.FamilyName</td>
    </tr>
    <tr>
      <td colspan="2">
For å angi personens etternavn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis, men må oppgis hvis kun pasientens fødselsdato (DateOfBirth) er angitt
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">fornavn</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.fornavn</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.GivenName</td>
    </tr>
    <tr>
      <td colspan="2">
For å angi personens fornavn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis, men må oppgis hvis kun pasientens fødselsdato (DateOfBirth) er angitt.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kjonn</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.kjonn</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.Sex</td>
    </tr>
    <tr>
      <td colspan="2">
Sosialt kjønn. Benyttes vanligvis ikke hvis fødselsnummer eller d-nummer oppgis.<br><br>Kodeverk: 3101 Kjønn<br>1 Mann<br>2 Kvinne<br>9 Ikke spesifisert
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">trygdenasjon</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.trygdenasjon</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.Nationality</td>
    </tr>
    <tr>
      <td colspan="2">
Personens medlemsstat knyttet til trygderettigheter. For eksempelvis en asylsøker som har trygderettigheter i Norge, skal det ikke oppgis landkode for pasientens hjemland. Skal kun brukes hvis pasientens nasjonalitet ikke er norsk (NO).<br><br>Landkode i henhold til ISO3166<br>Kommentar:<br>Konvensjonsland. <br>Kodeverk: 9043 Landkoder
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.identifikasjon.id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.Ident.Id</td>
    </tr>
    <tr>
      <td colspan="2">
Identifikasjon som personen er eller har vært kjent under. Vanligvis benyttes fødselsnummer (FNR) eller D-nummer (DNR) for personer. Hvis det ikke er mulig å oppgi Ident må DateOfBirth oppgis.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">type</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.identifikasjon.type</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.Ident.TypeId</td>
    </tr>
    <tr>
      <td colspan="2">
Primært er det FNR og DNR som skal benyttes. PNR benyttes for personer fra Australia. UID benyttes for utenlandske statsborgere uten FNR/DNR. HNR skal kun benyttes hvor dette er absolutt nødvendig eksempelvis for nyfødte personer som ikke har blitt tildelt fødselsnummer.<br><br>Kodeverk: ID-type for personer (subsett)<br>FNR	Fødselsnummer<br>DNR	D-nummer<br>HNR	H-nummer (nødnummer)<br>PNR    Passnummer <br>UID     Utenlandsk personID<br>DUF    DUF-nummer 
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">fodselsdato</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.fodselsdato</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Patient.DateOfBirth</td>
    </tr>
    <tr>
      <td colspan="2">
Angis på formatet ÅÅÅÅ-MM-DD (1982-03-30). 
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">Dok</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.Dok</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">CardId</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.CardId</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">Id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.Id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">TrygdekontorNavn</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.TrygdekontorNavn</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">TrygdekontorNr</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.TrygdekontorNr</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">eeaGyldighetFra</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.eeaGyldighetFra</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">eeaGyldighet</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.pasient.eea.eeaGyldighet</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td></td>
    </tr>
    <tr>
      <td colspan="2">

      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">arsakFriEgenandel</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.arsakFriEgenandel</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.KodeEgenandel</td>
    </tr>
    <tr>
      <td colspan="2">
Kode for årsak til fri egenandel.<br><br>Eksempler på kodeverdier:<br>A Smittefarlig sykdom <br>B Barn under 16 år<br>F Frikort<br>G HIV-infeksjon<br>H Pasientens tilstand til hinder for avkreving av egenandel<br><br>Gjeldende kodeverk 7461 finnes her:<br>https://finnkode.helsedirektoratet.no/adm/collections/7461?q=egenandels
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kodeverk</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.diagnoser.kodeverk</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Diagnose.S</td>
    </tr>
    <tr>
      <td colspan="2">
ICPC-2 eller ICPC-2b (beriket) skal benyttes for fysioterapeuter og kiropraktorer. Psykologer,  tannleger og fritt behandlingsvalg skal benytte ICD-10. Leger benytter kodeverk ICPC-2 eller ICPC-2b for allmennpraksis og ICD-10 for spesialistpraksis. For pasientreiser, rehabiliteringsinstitusjoner, behandlingsreiser i utlandet og kommunal fysioterapi skal diagnose ikke oppgis. Diagnosekode er ikke obligatorisk for tannleger. Dersom diagnose oppgis, kan ICD-10 kodeverket benyttes, eventuelt ICD-DA3 for dentale diagnoser.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.diagnoser.kode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Diagnose.V</td>
    </tr>
    <tr>
      <td colspan="2">
Diagnosen(e) behandleren setter. Hoveddiagnosen angis først. Det er ingen begrensning på hvor mange diagnoser som kan oppgis.<br>
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">sjeldenMedisinskTilstand</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.sjeldenMedisinskTilstand</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Sykdomslistepunkt</td>
    </tr>
    <tr>
      <td colspan="2">
Feltet benyttes av tannleger. Sykdomslisten ble fjernet fra og med 1. januar 2017 og sykdomslistepunkt skal ikke angis for fysioterapeuter på regninger etter dette tidspunkt.<br>For tannleger benyttes feltet for å angi hvilken sjelden medisinsk tilstand pasienten har. Feltet må benyttes av tannlege hvis kode 1 for sjelden medisinsk tilstand er angitt som behandlingsform (innslagspunkt). Listen over de sjelden medisinske tilstandene for tannlege finnes her:<br><br>https://www.helsedirektoratet.no/rundskriv/folketrygdloven-kap-5/folketrygdloven--5-6--5-6-a-og--5-25--undersokelse-og-behandling-hos-tannlege-og-tannpleier-for-sykdom-og-skade/smt-listen
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">dato</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.dato</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.HenvisningsDato</td>
    </tr>
    <tr>
      <td colspan="2">
Dato for når henvisningen er utstedt. Brukes av leger, psykologer, kjeveortopeder og ortoptister. Feltet kan også brukes av andre behandlere som mottar henvisninger.<br><br>For kjeveortopeder er det obligatorisk å oppgi henvisningsdato fra 01.01.2026. <br><br>Angis på formatet ÅÅÅÅ-MM-DD (1982-03-30). 
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.HenvisningsId</td>
    </tr>
    <tr>
      <td colspan="2">
Unik identifikasjon for hver henvisning som er brukt som grunnlag for behandlingen. Alle behandlinger knyttet til samme henvisning benytter samme henvisnings-ID. Brukes av leger, psykologer, ortoptister og kjeveortopeder. Feltet kan også brukes av andre behandlere som mottar henvisninger. Feltet er obligatorisk for ortoptist. <br><br>HenvisningsId'en bidrar til å identifisere behandlinger utført i samme behandlingsserie. Hvis HenvisningsDato, HenvisningsId, HenvisningDiagnose og RelatertBehandler oppgis er det ikke nødvendig å sende inn henvisningsblankett (05-04.21 og 05.08.05) til Helfo.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kodeverk</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.diagnoser.kodeverk</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.HenvisningsDiagnose.S</td>
    </tr>
    <tr>
      <td colspan="2">
Psykologer og ortoptister skal benytte ICD-10. Leger benytter kodeverk ICPC-2 eller ICPC-2b for allmennpraksis og ICD-10 for spesialistpraksis. Hvis HenvisningsDato, HenvisningsId, HenvisningDiagnose og RelatertBehandler oppgis er det ikke nødvendig å sende inn henvisningsblankett (05-04.21 og 05.08.05) til Helfo.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.diagnoser.kode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.HenvisningsDiagnose.V</td>
    </tr>
    <tr>
      <td colspan="2">
Diagnosen(e) som henvisende behandler oppgir. Hoveddiagnosen angis først. Brukes av leger, psykologer og ortoptister. Feltet kan også brukes av andre behandlere som mottar henvisninger.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.henvistFra.id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.RelatertBehandler.Id</td>
    </tr>
    <tr>
      <td colspan="2">
For lege, psykolog, ortoptist og andre behandlere som mottar henvisninger: Angir helsepersonellnummeret eller fødselsnummer/d-nummer til den behandler som har henvist pasienten. Når barnevernsadministrasjon har henvist til psykolog: Oppgi barnevernets/kommunens organisasjonsnummer.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">type</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.henvisning.henvistFra.type</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.RelatertBehandler.TypeId</td>
    </tr>
    <tr>
      <td colspan="2">
FNR, DNR eller HPR
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.utforendeBehandler.id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Behandlerident.Id</td>
    </tr>
    <tr>
      <td colspan="2">
Identifikasjonen til legen, fysioterapeuten, sykepleieren, jordmor eller tannlegen som har utført behandlingen. <br><br>Behandlerident kan oppgis for leger, kommunal fysioterapi, helsestasjon og tannlege. For lege må Behandlerident oppgis der praksistype er 2 eller 4 og kravet er sendt inn fra en kommune dvs. der behandlerkravmeldingen er signert med kommunens viksomhetssertifikat). Behandlerident for sykepleier skal oppgis der lege har delegert behandlingen/konsultasjonen til sykepleier. TypeId som godtas er kun FNR, DNR og HPR. For kommunal fysioterapi må Behandlerident oppgis på alle regninger. Det er ønskelig at HPR-nummer til lege eller jordmor oppgis på alle regninger fra helsestasjon. Der tannlegekrav sender inn på vegne av en institusjon (UIO, UIB etc.) og hvor behandlingen er utført av ulike tannleger kan Behandlerident brukes for å angi hvem som har utført behandlingen.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">type</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.utforendeBehandler.type</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Behandlerident.TypeId</td>
    </tr>
    <tr>
      <td colspan="2">
FNR, DNR eller HPR
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">id</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.relatertBehandler.id</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.RelatertBehandler.Id</td>
    </tr>
    <tr>
      <td colspan="2">
For tannlege: Angir helsepersonellnummeret til tannlege det er samarbeidet med ved implantatbehandling. Ved bruk av takster for kjeveortopedi (600-takstene) skal helsepersonellnummeret til behandleren som har henvist til behandlingen oppgis. 
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">type</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.relatertBehandler.type</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.RelatertBehandler.TypeId</td>
    </tr>
    <tr>
      <td colspan="2">
FNR, DNR eller HPR
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">moderasjonskode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.moderasjonskode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Moderasjonskode</td>
    </tr>
    <tr>
      <td colspan="2">
Benyttes kun av tannleger. Feltet brukes for å angi om pasienten er omfattet av søskenmoderasjon ved kjeveortopedisk behandling. For at refusjonen skal gis med høyeste prosentsats må S oppgis<br><br>Kodeverk: <br>S Søskenmoderasjon
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">stonadspunkt</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.stonadspunkt</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Behandlingsform</td>
    </tr>
    <tr>
      <td colspan="2">
Angir hvilket hovedstønadspunkt behandlingen gjelder. Brukes kun av tannlege og tannpleier. <br><br>For hver behandling må det angis hvilket stønadspunkt behandlingen dekkes etter. Det er hovedstønadspunkt som skal angis.<br><br>Se listen over gjeldende koder her:<br>https://www.helsedirektoratet.no/rundskriv/folketrygdloven-kap-5/rundskriv-til-folketrygdloven--5-6--5-6-a-og--5-25--undersokelse-og-behandling-hos-tannlege-og-tannpleier-for-sykdom-og-skade
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kodeverk</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.prosedyrekoder.kodeverk</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Prosedyrekode.Kodeverk</td>
    </tr>
    <tr>
      <td colspan="2">
Kodeverket som prosedyrekoden tilhører. Feltet benyttes av spesialistleger og psykologer. <br><br>Kodeverk: 8410 Medisinske kodeverk<br>K NCSP-N<br>M NCMP<br>R NCRP<br>Q NSCP/NCMP/NCRP Kode fra et av kodeverkene<br><br>Kodeverkene for NCMP og NCSP finnes her:<br>https://www.helsedirektoratet.no/digitalisering-og-e-helse/helsefaglige-kodeverk/nkpk
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.prosedyrekoder.kode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Prosedyrekode.Kode</td>
    </tr>
    <tr>
      <td colspan="2">
Selve kodeverdien. Feltet benyttes av spesialistleger og psykologer.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">belop</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.takster.belop</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Takst.Verdi</td>
    </tr>
    <tr>
      <td colspan="2">
Takstkodens /laboratoriekodens refusjonsverdi på behandlingstidspunktet.<br><br>Verdi behandleren krever refundert for første forekomst av taksten. Ved repetisjon av taksten skrives samme beløp, og antall bruk av taksten i "antall" feltet. Den faktiske verdien blir beregnet ut i fra verdi, antall og repetisjonsprosent som framgår av takstforskrift.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">kode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.takster.kode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Takst.Kode</td>
    </tr>
    <tr>
      <td colspan="2">
Takstkoder og laboratoriekoder som fastsatt i forskrift eller annet regelverk.Alle koder som gjelder samme behandling/undersøkelse skal føres opp på én og samme regning.<br><br>Taksten skrives slik den er oppgitt i forskriften, eks. A1a, B20, osv.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">antall</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.takster.antall</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Takst.Antall</td>
    </tr>
    <tr>
      <td colspan="2">
Antall ganger en takst eller laboratoriekode er brukt. Standardverdi = 1<br><br>Hvis for eksempel takst XXX er benyttet én gang skal det stå Antall=1.<br>Dersom takst XXX er benyttet én gang, og repetert 2 ganger utover den første, skal det stå Antall=3.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">tannkode</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.takster.tenner.tannkode</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Tann</td>
    </tr>
    <tr>
      <td colspan="2">
Angir hvilken region i munnen som har blitt behandlet eventuelt hvilken tann eller hvilke tenner som er behandlet med tilhørende angivelse av flate. Brukes kun av tannlege.<br><br><br>For hver behandling må enten region eller tann oppgis. Hvis region er oppgitt (eks. 01 for overkjeve) er det ikke nødvendig å angi flate. Hvis tann er oppgitt (17) skal det normalt også oppgis hvilken flate som er behandlet.  Hvis det er flere flater som er behandlet på en enkelt tann oppgis dette ved bruk av flere koder (eksempelvis 17O, 17M). Kode angis med to siffer og bokstav eventuelt kun med to siffer.<br>Eksempel på kode:<br>00 Hele munnen<br>01 Overkjeve (maxilær-området)<br>10 Øvre høyre kvadrant<br>11 Tann 11<br>11I Tann 11 incisal kant<br>11M Tann 11 mesial overflate<br>11D Tann 11 distal overflate <br>11G Tann 11 radiculær<br>11B Tann 11 buccal overflate<br>11P Tann 11 palatinal overflate<br>16O Tann 16 occlusal overflate<br>41L Tann 41 lingual overflate<br><br>Kodeverk:<br>ISO 3950 - 1995
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">belop</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.regninger.belop</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandling.Refusjon</td>
    </tr>
    <tr>
      <td colspan="2">
Det beløp som kreves refundert for enkeltregningen inkludert refusjon for fri egenandel. For tannleger og tannpleiere uten avtale om direkte oppgjør, men som plikter å rapportere inn egenandeler settes Refusjon til 0,- for alle regninger for at egenandelene skal bli godkjent.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">antallRegninger</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.antallRegninger</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandlerkrav.AntallRegninger</td>
    </tr>
    <tr>
      <td colspan="2">
Samlet antall innsendte regninger.
      </td>
    </tr>
  </tbody>
</table>

<table>
  <tbody>
    <tr>
      <th colspan="2">belop</th>
    </tr>
    <tr>
      <th>Full path</th>
      <td>behandlerkrav.belop</td>
    </tr>
    <tr>
      <th>Felt i BKM XML</th>
      <td>Behandlerkrav.SumKravSamlet</td>
    </tr>
    <tr>
      <td colspan="2">
Samlet krav for innsendte regninger.
      </td>
    </tr>
  </tbody>
</table>

