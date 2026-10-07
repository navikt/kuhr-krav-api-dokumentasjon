# Testdata

## Registrering av praksis for behandler

For å kunne teste APIet i testmiljøet må det registreres en praksis for behandleren det skal testes med. Det er lagt opp
sånn at EPJ leverandørene selv kan gjøre den nødvendige registreringen.

Finn en test-helseaktør med HPRnr og FNR i SyntPop.
https://www.nhn.no/tjenester/testunivers/syntpop

Logg på Tjenesteportalen for helseaktører og registrer praksisinformasjon.
https://praksisinformasjon.test.helsedirektoratet.no/

Organisasjonen som registreres på en praksis må være registrert i testdatasettet til Tenor eller det 'ekte'
Enhetsregisteret. Det finnes organisasjoner som ligger i Adresseregisteret som ikke ligger i Enhetsregisteret, og disse
kan ikke brukes i testmiljøet.

## Registrering av praksis for virksomhet

## Pasienter for bruk på regninger

## Autentisering i testmiljøet

For å kunne teste APIet i testmiljøet må det dere bruke HelseId eller Maskinporten sine respektive testmiljøer for
autentisering.

Hvis dere bruker
test-token-tjenesten ( https://utviklerportal.nhn.no/informasjonstjenester/helseid/helseid-selvbetjening/test-token-tjenesten/docs/test-token-tjenesten_no_nnmd )
til HelseId, er det enkelte claims dere må passe på at dere får med i tokenet, som alltid vil være med når dere bruker
den ekte flowen til HelseId.

- `aud` må være `hdir:kuhr-krav-api`
- `pid` må være med i tokenet, dette er helsaktøren som dere har registrert praksis for, og som dere skal teste med
- `helseid://claims/identity/security_level` må være med i tokenet, og må ha verdien `4`
- `helseid://claims/client/claims/orgnr_parent` må være med i tokenet, og må ha registrert organisasjonsnummer for
  virksomheten som dere har registrert praksis for, og som dere skal teste med
- `scope` må inneholde `hdir:kuhr-krav-api/krav`

Eksempel på full request for test token tjenesten:
```json
{
  "audience": "hdir:kuhr-krav-api",
  "signJwtWithInvalidSigningKey": false,
  "setInvalidIssuer": false,
  "withoutDefaultClientClaims": true,
  "withoutDefaultUserClaims": true,
  "clientClaimsParameters": {
    "scope": [
      "openid",
      "profile",
      "read",
      "hdir:kuhr-krav-api/krav"
    ],
    "jti": "F4F832F0C68E24F0011F773B71CC6739",
    "client_id": "eeb808a2-6e6f-42ae-849a-505432cf128f",
    "clientTenancy": true,
    "clientAuthenticationMethodsReferences": "private_key_jwt",
    "orgnrParent": "883974832"
  },
  "userClaimsParameters": {
    "pid": "15876498518",
    "securityLevel": "4"
  },
  "getPersonFromPersontjenesten": true,
  "onlySetNameForPerson": true,
  "getHprNumberFromHprregisteret": true,
  "setSubject": true,
  "createDPoPTokenWithDPoPProof": true,
  "dPoPProofParameters": {
    "htmClaimValue": "GET",
    "htuClaimValue": "https://api-preprod.helserefusjon.no/kuhr/krav/v1/data/praksis"
  }
}
```


