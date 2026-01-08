## 2026-01-08
behandlerkravmelding_json.md
- Fjernet mapping til felt Behandling.RelatertBehandler.TypeId i BKM XML

jwe_ende_til_ende_kryptering.md
- Fjernet ekstra /jwk på URL

## 2025-12-01
README.md
- Tydeligjort at det kreves sluttbrukerpålogging med HelseID

behandlerkravmelding_json
- Endret format på behandlerkrav.regninger.tidspunkt til YYYY-MM-DDTHH:mm:ss±hh:mm



## 2025-11-25
Lagt til testdata.md

## 2025-11-24
endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Fjernet innsendingId fra innsendingen i JSON eksempel
- Lagt til vedtakId og innsendinId i JSON eksempel

README.md
- Lagt til HelseID miljø for DEV

## 2025-11-13
endepunkt_send_inn_behandlerkravmelding.md
- Lagt til at det gis HTTP 400 på feil i melding

endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Ny meldingsstatus klar_til_behandling


## 2025-10-31
endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Fjernet listen over innsendinger på vedtak
- Fjernet felt utbetalingId


## 2025-10-29
README.md
- Endret scope for JWK endepunktet fra nav:kuhr/jwk til nav:kuhr/krav


## 2025-10-28
endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Fjernet felt oppdaterttidspunkt
- Oppdatert responsen for detaljer om et krav til å ha feltene fra innsending på samme sted som i oversikten
- Lagt til HelseID scope hdir:kuhr-krav-api/krav

README.md
- Lagt til HelseID scope hdir:kuhr-krav-api/krav i oversikten

endepunkt_hent_registrerte_praksiser.md
- Lagt til HelseID scope hdir:kuhr-krav-api/krav 

endepunkt_send_inn_behandlerkravmelding.md
- Lagt til HelseID scope hdir:kuhr-krav-api/krav


## 2025-10-23
endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Endret fra HTTP 200 til 400 Bad Request på feilmeldingrespons

## 2025-10-21 
endepunkt_hent_status_innsendte_behandlerkravmeldinger.md
- Endret path /kuhr/krav/v1/data/behandlerkrav til /kuhr/krav/v1/data/behandlerkravmelding

endepunkt_send_inn_behandlerkravmelding.md
- Endret feilkode fra MANGLER_BEHANDLERKRAVMELDING til MANGLER_BEHANDLERKRAV
- Oppdatert eksempel på feil_i_melding

README.md
- Endret path /kuhr/krav/v1/data/behandlerkrav til /kuhr/krav/v1/data/behandlerkravmelding
- Endret path /kuhr/jwk/jwk til /kuhr/jwk  
- Lagt til scope for HelseID hdir:kuhr-krav-api/krav