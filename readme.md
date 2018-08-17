# WIZportaal meldingenkoppeling

De WIZportaal meldingenkoppeling maakt het mogelijk om op basis van een StUF zkn0310-bericht een melding aan te maken in WIZportaal.

## Webservice meldingen obv zkn0310

De interface om berichten aan te bieden aan WIZportaal is StUF zkn0310 (de webservice zkn0310_ontvangAsynchroon_mutatie.wsdl).
In deze repository is een geresolvede versie opgenomen. Hierin is alleen de benodigde wsdl opgenomen om een melding aan WIZportaal aan te bieden.
Gebruik hiervoor het bestand: `zkn0310_ontvangAsynchroon_mutatie_meldingen.wsdl`

## Documentatie velden

Op dit moment is de documentatie beperkt tot de commentaarregels die opgenomen zijn in de voorbeeldberichten.
Hierin is aangeven welk veld verplicht is, en met welk referentieveld in WIZportaal het veld een relatie moet hebben.

## Mapping xml-velden naar WIZportaal

|*WIZportaal veld frontend*|*Verplicht*|*Toelichting*|*Datatype*|*StUF Zaken*|
|soort melding| |Code soort omschrijving|Waarde uit referentielijst: melding type|//object/isVan/gerelateerde/code |
|binnenkomst melding| |Wijze van melden door burger|Waarde uit referentielijst: binnenkomst melding|//object/extraElementen/extraElement[@naam="binnenkomstMelding"]|
|datum melding|ja| |datum: yyyymmdd|//object/startdatum|
|beschrijving van de melding|ja| | |//object/heeftBetrekkingOp/omschrijving|
|soort melder|ja| |Waarde uit referentielijst: type melder|//object/heeftAlsInitiator/extraElementen/extraElement[@naam="soortMelder"]|
|naam melder|ja|als melder niet natuurlijk persoon is| |//object/heeftAlsInitiator/heeftAlsAanspreekpunt/gerelateerde/naam|
|naam melder|ja|als melder natuurlijk persoon is| |//object/heeftAlsInitiator/gerelateerde/natuurlijkPersoon/geslachtsnaam|
|anoniem| |Is de melding anoniem gedaan?|boolean |//object/heeftAlsInitiator/extraElementen/extraElement[@naam="indicatieAnoniem"]|
|organisatie| | | |//object/heeftAlsInitiator/gerelateerde/nietNatuurlijkPersoon/statutaireNaam|
|e-mailadres| | |string max 254|//object/heeftAlsInitiator/heeftAlsAanspreekpunt/gerelateerde/emailadres|
|telefoonnummer| | |string max 20|//object/heeftAlsInitiator/heeftAlsAanspreekpunt/gerelateerde/telefoonnummer|
|postcode| |postcode bij natuurlijk persoon|1234AB, string [1-9][0-9]{3}[A-Z]{0,2}|//object/heeftAlsInitiator/gerelateerde/natuurlijkPersoon/verblijfsadres/aoa.postcode|
|postcode| |postcode bij niet natuurlijk persoon|1234AB, string [1-9][0-9]{3}[A-Z]{0,2}|//object/heeftAlsInitiator/gerelateerde/natuurlijkPersoon/bezoekadres/aoa.postcode|
|type afhandeling|ja|Hoe wordt de melding afgehandeld|Waarde uit referentielijst: melding afhandelingstype|//object/resultaat/omschrijving|
|team| | |Waarde uit referentielijst: team|//object/heeftAlsVerantwoordelijke/gerelateerde/organisatorischeEenheid/naam|
|zaakstatus||Zaakstatus moet eigenschap _zaak is definitief_ hebben|Waarde uit referentielijst: zaakstatus|//object/heeft/gerelateerde/omschrijving|

## Stuurgegevens

De stuurgegevens moeten op de volgende wijze gevuld worden:
* berichtcode: Lk01
* zender.organisatie: Het OIN uit: configuratie - berichtenverkeer beheren - gemeente - oin
* zender.applicatie: De naam van de zendende applicatie
* zender.gebruiker: De gebruiker die het bericht inschiet
  Dit is voor logging doeleinden, in de meeste gevallen zal dit gevuld zijn met de webformulier of esb-user
* ontvanger.organisatie: configuratie - berichtenverkeer beheren - gemeente - oin
* ontvanger.applicatie: WIZportaal
* referentienummer: een geldig referentienummer dat voldoet aan de StUF-regels
* entiteittype: ZAK
