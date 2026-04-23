# Testdata

## Registrering av praksis for behandler
For å kunne teste APIet i testmiljøet må det registreres en praksis for behandleren det skal testes med. Det er lagt opp sånn at EPJ leverandørene selv kan gjøre den nødvendige registreringen. 

Finn en test-helseaktør med HPRnr og FNR i SyntPop.
https://www.nhn.no/tjenester/syntpop


Logg på Tjenesteportalen for helseaktører og registrer praksisinformasjon.
https://praksisinformasjon.test.helsedirektoratet.no/


## Registrering av praksis for virksomhet



## Pasienter for bruk på regninger

## Autentisering i testmiljøet
For å kunne teste APIet i testmiljøet må det dere bruke HelseId eller Maskinporten sine respektive testmiljøer for autentisering.

Hvis dere bruker test-token-tjenesten ( https://utviklerportal.nhn.no/informasjonstjenester/helseid/helseid-selvbetjening/test-token-tjenesten/docs/test-token-tjenesten_no_nnmd ) til HelseId, er det enkelte claims dere må passe på at dere får med i tokenet, som alltid vil være med når dere bruker den ekte flowen til HelseId.
- `aud` må være `hdir:kuhr-krav-api`
- `pid` må være med i tokenet, dette er helsaktøren som dere har registrert praksis for, og som dere skal teste med
- `helseid://claims/identity/security_level` må være med i tokenet, og må ha verdien `4`
- `helseid://claims/client/claims/orgnr_parent` må være med i tokenet, og må ha registrert organisasjonsnummer for virksomheten som dere har registrert praksis for, og som dere skal teste med
- `scope` må inneholde `hdir:kuhr-krav-api/krav`



