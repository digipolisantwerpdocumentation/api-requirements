## Versionering

### Belang van een goede versioneringsstrategie

APIs wijzigen na verloop van tijd. Om zogenaamde breaking wijzigingen te kunnen opvangen zonder bestaande consumers te impacteren is het belangrijk de APIs te versioneren. De manier waarop en de gevallen waarin wordt geversioneerd worden beschreven in onderstaande
versioneringsstrategie. Deze is zo opgesteld dat versionering een goede indicatie geeft van functionele wijzigingen in de API.

### Algemene versioneringsstrategie

Algemeen dient API design op zo'n manier te gebeuren dat steeds naar een maximale backward compatibility wordt gestreefd. Breaking changes dienen ten allen tijden worden vermeden omdat dit steeds een wijziging langs consumer(s) kant met zich meebrengt.

Voorbeelden van backwards compatible (non breaking) changes voor REST APIs zijn:

-   Toevoegen van een nieuwe HTTP method
-   Toevoegen van een nieuwe response header
-   Toevoegen van een nieuwe resource aan het resource model
-   Toevoegen van nieuwe optionele parameters aan de request
-   Toevoegen van nieuwe informatie in de representatie van een bestaande resource

Voorbeelden van breaking changes zijn:

-   Verwijderen van resources
-   Hernoemen van resources
-   Wijzigen van een data type in een entiteit van de resource representatie
-   Semantische wijzigingen in een resource representatie
-   Verwijderen van ondersteuning van een bestaande HTTP methode
-   Wijzigen van HTTP status codes
-   Wijzigen of verwijderen van een response header

### API contract versionering
#### Gebruik `semver` versionering
##### R-VC-01
De algemene versionering strategie voor APIs is als volgt en is gebaseerd op semantic versioning (http://semver.org)

-   **Major**: backwards incompatibele wijzigingen
-   **Minor**: Nieuwe features of verwijdering van features die backwards compatibel zijn
-   **Patch**: bug fixes

Naar de API consumer toe wordt enkel de major versie zichtbaar gemaakt.  
Zoniet dient voor elke nieuwe feature een communicatie naar de consumers te gebeuren en dient langs consumer kant een wijziging te gebeuren.  
Door enkel de major versie te communiceren naar de consumer toe, betekent niet dat minor en patch informatie overbodig zijn.  
Integendeel, het is wenselijk dat deze opgezocht kunnen worden door de ontwikkelaar (**Discoverability** van functionele wijzigingen).

Dit versienummer dient steeds te worden opgenomen in de Swagger file en definieert dus de API contract versie.

### API technische versionering
#### Gebruik `major` versionering
##### R-VT-01
Versionering van de API zelf (major versie) doen we aan de hand van root namespace versionering.

In dit versioneringsmodel definieert een API versie een verzameling van resources die een welbepaalde set van functionaliteiten voorzien binnen een bepaald domein.

Een versie van een resource wordt impliciet voorgesteld door middel van zijn representatie en state. Een rechtstreeks gevolg hiervan is dat resources zelf nooit expliciet geversioneerd worden.

In dit versioning model versioneren we steeds elke API als volgt:
``` prettyprint
https://{hostname}/{namespace}/{vx}/{resource-URI}
```

Hierbij duidt x in **/vx** op de major versie van de API. Deze maakt dus integraal onderdeel uit van de URI. De **namespace** is een enkelvoudig zelfstandig naamwoord dat het onderwerp van de API bepaalt, gezien vanuit het standpunt van de API consumer.

Opm. : op de API gateway zal de namespace bestaan uit organization/apiname.

#### Voordelen
-   De consumer heeft een direct endpoint dat kan worden opgeroepen. De consumer ziet ook onmiddellijk zijn versie afhankelijkheid
-   Meest eenvoudige vorm van versionering, 75% van alle APIs wereldwijd gebruikt dit versioneringsmodel
-   Biedt een goede ondersteuning voor breaking changes aan resource model of representatie (API wijde veranderingen)
-   Eenvoudig te implementeren
-   Versionerings scope is API wide

#### Nadelen

-   Elke nieuwe API versie betekent ook een nieuw API endpoint voor de consumer
-   Resource specifieke wijzigingen zijn niet mogelijk zonder een versie update van de ganse API uit te voeren
-   Resource representatie specifieke wijzigingen zijn vaak niet mogelijk zonder een versie upgrade uit te voeren
