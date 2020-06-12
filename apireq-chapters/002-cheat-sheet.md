## Cheat sheet

Dit hoofdstuk geeft een beknopte versie van de requirements; meer
details en voorbeelden van elk item vind je verder in het document.

### Taal

Engels (URI, Payload keys)

### Formaat API documentatie op de API Engine

Swagger v2.0, JSON

### Payload

-   JSON (= application/json), tenzij geen andere mogelijkheid (formdata/file/…)
    -   keys
        -   dubbele quotes
        -   camelCase
        -   geen dots (.)
        -   niet starten met een cijfer
        -   attribuut is optioneel indien de value geen meerwaarde heeft EN waarde null heeft
    -   values
        -   null
        -   string : dubbele quotes
        -   datum en timestamp : "YYYY-MM-DDThh:mm:ss+01:00" (RFC3339)
        -   duration : "PYYYY-MM-DDThh:mm:ss" (ISO8601)   
            vb. "P0003-04-06T12:00:00" (3 jaar, 4 maanden, 6 dagen, 12 uur)
        -   geospatiale data : bijvoorbeeld lengte en breedte coördinaten (rfc7946)  
            vb. {"type": "Point", "coordinates": [100.0, 0.0]}
        -   Arrays worden altijd geëncapsuleerd in een object
    -   Hiërarchische structuur (objecten)

### Resource

-   URI Structuur
    -   https://{hostname}/{namespace}/{vx}/{resource-URI}
    -   opm. : op de API gateway zal de namespace bestaan uit organization/apiname
-   afspraken
    -   altijd in het meervoud (uitzondering : controllers (=commands), status resource, monitoring resource)
    -   lowercase (zowel URI als query parameters)
    -   geen underscores
    -   geen dots '.' (behalve in hostname)
    -   geen trailing slash /
    -   geen fragment \#
    -   geen media type file extensions, wel Content-Type en Accept headers
    -   locale : (Optioneel) : Content-language en Accept-language headers

### Request

-   filtering op id : steeds mee als node in URI (niet via query parameter)
- query parameters, enkel voor
  - filtering
    - op resource
      -   unieke query parameter per veld
      -   indien meerdere waarden voor een veld filter :

      **resource?parameter=waarde1,waarde2,waarde3**

      -   GEEN query parameter voor id's !
    - op resource representatie

         -   reserved : **fields**= (comma separated)
  -   sortering
      -   reserved : **sort**= (comma separated)
      -   default : ascending
      -   **-**\<parameter\> : descending
  -   paginatie
      -   reserved : **page**= (optional parameter, default value=1)
      -   reserved : **pagesize**=(optional parameter, default=bepaald door API)
      -	  reserved : **paging-strategy**=(optional parameter, default=withCount)
      
  - resource representation

    -   altijd via body, nooit via query parameters

### Paginatie

-   altijd gebruiken bij ophalen van collecties
-   query parameters : **page**= en **pagesize**= en **paging-strategy**= 
    -   optioneel mee te geven door clients (bij ontbreken worden de  default waardes gebruikt)
-   1-based (1e pagina is pagina 1)
-   Response volgens HAL specificatie
    -   media type : **application/hal+json**
    -   reserved keywords
        -   \_links
        -   \_embedded
        -   \_page
    -   voorbeelden
-   beide `paging-strategy` waardes (`noCount` en `withCount`) moeten ondersteund worden 

### Event resources

POST [/\<groepering>]*/\<event> waarbij \<event> eindigt op een voltooid deelwoord


### Error model

-   Exception shielding principe gebruiken
-   media type : **application/problem+json**
-   Error model (gebaseerd op RFC7807)
```
{
    "type": "string"                           (verplicht, URIformaat),
    "title": "string"                          (verplicht),
    "status": number                           (verplicht, HTTP response code),
    "identifier": "string"                     (verplicht, uniek),
    "code": "string"                           (verplicht, foutcode voor ontwikkelaars),
    "extraInfo": "custom"                      (optioneel, bv een array van validatiefouten)
}
```

### Versionering

-   semantic versioning (<http://semver.org>)
    -   Major : backwards incompatibele wijzigingen (breaking changes)
    -   Minor : nieuwigheden en wijzigingen die backwards compatible zijn
    -   Patch : bugfix
-   semantic version altijd opnemen in de swagger file.
-   enkel het major gedeelte wordt naar een consumer zichtbaar gemaakt (vb. ontsluiting op de API gateway) : https://{hostname}/{namespace}/{vx}/{resource-URI}
-   Indien de backend service een versie gebruikt in zijn URI, dient de versie altijd in het basepath gezet te worden en niet in de routes. Dit is om te vermijden dat er bij de ontsluiting van de API gateway 2 versies in de URL komen te staan.

### Langdurende operaties

-   asynchroon uitvoeren
-   HTTP response code 202 met in Location header een tijdelijke URI
-   consumer polt op tijdelijke URI naar de status
-   als taak is uitgevoerd, dan response code 303 met in Location header een definitieve URI van de resource.
-   De definitieve URI kan worden gebruikt om de resource op te vragen.

### Swagger

-   template : <https://drive.google.com/file/d/0B06myaSd5oKUOEduOHdHRl9RZU0/>
-   template met extra info (opm. geen geldige swagger wegens extrainfo) : <https://drive.google.com/file/d/0B06myaSd5oKUWU1samRrODcwUVk/>
-   steeds alle descriptions en summaries invullen (in het engels) : routes, parameters, ... : zie ook <https://github.com/digipolisantwerpdocumentation/api-requirements/blob/master/swagger-docs.md>
- generieke swagger definities voor :
  - [paging](../components/paging.yaml)
-   semantic version opnemen in swagger file

### HTTP verbs

-   RFC7231 compliant
-   PATCH verb : RFC7386 of RFC6902


Verb   | Usage                                                                                                        | Request body                              | Response body   
----   | -----                                                                                                        | ------------                              | -------------
GET    | opvragen van de representatie van een resource                                                               | leeg                                      | (gedeeltelijke) resource representatie
HEAD   | opvragen van de headers van een resource                                                                     | leeg                                      | leeg
PUT    | vervangen van  een bestaande resource (of creatie indien die nog niet bestaat, op basis van de opgegeven id) | representatie  van te vervangen resource  | optioneel       
POST   | creëren van een nieuwe resource                                                                              | representatie van te creëren resource    | **Location** header met URI,<br/>body optioneel
POST   | voor het uitvoeren(=creëren) van een controller(=command) (werkwoord, vb. search)                            | representatie van info voor controller   | optioneel   
PATCH  | vervangen van een gedeelte van een bestaande resource                                                        | te vervangen velden                     | optioneel       
DELETE | verwijderen van een resource                                                                                 | leeg                                      | optioneel       

### HTTP response codes

**Altijd moet de meest specifieke responsecode worden gebruikt; vb. 401 bij security ipv 400, 404 bij not found ipv 400.**

Code                         | GET | HEAD | PUT | POST                | PATCH | DELETE | Error object 
----                         | --- | ---  | --- | ----                | ----- | ------ | ------------ 
200 : OK (sync)              | X   | X    | X   |                     | X     | X      |              
201 : Created (sync)         |     |      |     | X                   |       |        |              
202 : Accepted (async)       |     |      | X   | X                   | X     | X      |              
204 : No content             |     |      | X   | X                   | X     | X      |              
303 : See other (async)      |     |      |     | X                   |       |        |              
400 : Bad request            | X   | X    | X   | X                   | X     | X      | Ja           
401 : Unauthorized           | X   | X    | X   | X                   | X     | X      | Optioneel    
403 : Forbidden              | X   | X    | X   | X                   | X     | X      | Optioneel    
404 : Not Found              | X   | X    | X   | X (parent resource) | X     | X      |              
405 : Method not allowed     | X   | X    | X   | X                   | X     | X      |              
415 : Unsupported media type | X   | X    | X   | X                   | X     | X      |              
429 : Too many requests      | X   | X    | X   | X                   | X     | X      | Optioneel    
500 : Internal server error  | X   | X    | X   | X                   | X     | X      | Ja           
