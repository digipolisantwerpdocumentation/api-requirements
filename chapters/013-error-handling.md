## Error handling

Naast een correcte weergave van fouten via response codes is het vaak nuttig om bijkomende informatie weer te geven in het response bericht.  
Denk hierbij aan functionele fouten die op een correcte en uniforme manier door de API consumer dienen te worden geïnterpreteerd.

### Exception shielding

Belangrijk om te benadrukken is het principe van exception shielding.

Bij exception shielding willen we vermijden dat interne technische foutboodschappen naar consumers toe worden blootgesteld door deze af te schermen. Interne implementatie details van een API dienen zoveel mogelijk te worden afgeschermd aangezien ze security gewijs een
bedreiging kunnen vormen.

Bovendien bieden ze geen meerwaarde naar consumers toe aangezien deze laatste niet de verantwoordelijkheid heeft deze technische fout af te handelen.

![alt\_text](images/APIReq03.png "")

Wat wel wenselijk is, is een algemeen technische fout type die we naar de consumer toe willen tonen, maar dewelke de technische details
afschermt. De API consumer kan gebruik maken van de identifier in dit fout type om bijkomende informatie te bekomen.

### Error model

Algemeen kan worden aangenomen dat een error response minimaal volgende informatie dient te bevatten:

-   Een unieke identifier waarmee de foutboodschap kan worden teruggevonden in logs en/of andere operationele systemen
-   Een korte omschrijving van de foutboodschap in een human-readable formaat
-   Een link dewelke wijst naar meer documentatie over de fout

De basis van het gebruikte error model bij digipolis is [Problem Details for HTTP APIs (RFC7807)](https://datatracker.ietf.org/doc/rfc7807/)

De RFC schrijft onderstaande specificaties voor.

Het response error object kan volgende velden bevatten:

-   **Type (string):** Een URI referentie (absoluut of relatief)[\[RFC3986\]](https://mnot.github.io/I-D/http-problem/#RFC3986) dewelke het probleem type identificeert. Indien verwezen wordt naar deze URI dient human-readable (HTML) documentatie voor het probleem te worden weergegeven.
-   **Title (string):** Een korte, human-readable omschrijving van het probleem. De title die hier wordt weergegeven mapt 1-op-1 met een omschrijving van het hierboven vernoemde Type.
-   **Status (number):** de HTTP status code. De reden van vermelding in het error object is omdat eventuele intermediaries zoals gateways de HTTP status code steeds kunnen wijzigen.
-   **Detail (string):** Meer specifieke human-readable detail informatie die specifiek is voor deze instantie van het probleem.
-   **Instance (string):** Een URI referentie (absoluut of relatief) dewelke de instantie van het probleem identificeert unieke identifier om de foutboodschap terug te vinden in log bestanden.
-   **Optionele extra info** die nuttig is voor de API consumer om het probleem te kunnen begrijpen.

Hierbij wordt gebruik gemaakt van het **`application/problem+json`** media type om foutboodschappen aan te duiden.

**Omdat deze RFC standaard te uitgebreid is voor onze noden, definiëren we het te gebruiken model als volgt:**

Verplicht te gebruiken velden in het error response zodat de API consumer het probleem op een logische manier kan begrijpen:

-   **Type (string)**: Een URI referentie (absoluut of relatief) [\[RFC3986\]](https://mnot.github.io/I-D/http-problem/#RFC3986) dewelke het probleem type identificeert. Indien verwezen wordt naar deze URI dient human-readable (HTML) documentatie voor het probleem te worden weergegeven.
-   **Title (string)**: Een korte, human-readable omschrijving van het probleem. De title die hier wordt weergegeven mapt 1-op-1 met een omschrijving van het hierboven vernoemde Type.

Daarnaast worden volgende velden ook in het antwoord verwacht:

-   **Status (number)**: de HTTP status code zoals deze door de backend wordt gegenereerd. De reden van vermelding in het error object is omdat eventuele intermediaries zoals gateways de HTTP status code steeds kunnen wijzigen.
-   **Identifier (string)**: Een unieke identifier om de foutboodschap terug te vinden in log bestanden.
-   **Code (string)**: Een foutcode die 1 op 1 mapt met het Type en waartegen ontwikkelaars kunnen programmeren.

Dit resulteert in volgend voorbeeld error response:  

```json
{
"type": "http://api-gateway/digipolis/payment/v1/payments/FE0032",
"title": "You do not have enough credit.",
"status": 400,
"identifier": "C5C68BE6-B5FF-11E5-B08F-D1D119563991",
"code": "FE0032"
}
```

Hierbij zijn identifier en code velden die we in elk error response verwachten. Deze worden gezien als optionele parameters van de RFC, maar zijn bij Digipolis verplicht.

Toegepast op bovenstaand error model kan onderstaande structuur gehanteerd worden voor technische fouten:  

```json
{
"type": "http://api-gateway/digipolis/payment/v1/payments/technical",
"title": "A technical error occured",
"status": 500,
"identifier": "C5C68BE6-B5FF-11E5-B08F-D1D119563991",
"code": "DA01245"
}
```

Aangezien het principe van exception shielding wordt gehanteerd wordt slechts 1 type technical error gedefinieerd, waarbij we de technische error details bovendien verbergen. De identificatie van de technische fout gebeurt steeds aan de hand van de meegegeven identifier. De API consumer dient deze identifier te gebruiken in alle communicatie rond de eigenlijke fout, zodat de operationele teams deze kunnen gebruiken om meer gedetailleerde informatie op te zoeken.

In sommige gevallen kan het nuttig zijn om **extra info** mee te geven zodat de consumer beter begrijpt wat het probleem is, zoals bvb bij validatiefouten :

```json
{
"type": "http://api-gateway/digipolis/payment/v1/payments/validation-error",
"title": "There are validation errors.",
"status": 400,
"identifier": "f4D27A4A-6bed-47D6-9487-1961DBB6C631",
"code": "VAL001",
"extraInfo": {
             "validationErrors":
                 [
                     {
                     "name": "account",
                     "reason": "The provided account does not exist."
                    },
                    {
                    "name": "amount", 
                    "reason": "The amount must be greater than 0."
                    }
                 ]
             }
}
```

### HTTP status codes en error model

Deze sectie beschrijft welke HTTP status codes vergezeld dienen te worden van een bijhorend error object

HTTP status code           | Betekenis                                                                                                                                   | Error object                  
----------------           | ---------                                                                                                                                   | ------------
200 OK                     | De request is succesvol en synchroon uitgevoerd. Van toepassing op GET en HEAD bij succesvol response, PUT en PATCH indien de update succesvol was en DELETE indien de resource succesvol werd verwijderd. | Neen                          
201 Created                | Indien een nieuwe resource succesvol is aangemaakt bij het uitvoeren van een PUT of POST call, of bij een succesvolle uitvoering van een POST van een controller.                                              | Neen                          
202 Accepted               | De request is succesvol geaccepteerd voor een PUT, POST, DELETE of PATCH en wordt verder asynchroon verwerkt.                               | Neen                          
303 See Other              | Wordt gebruikt voor het asynchroon afhandelen van langlopende operaties.                                                                    | Neen                          
400 Bad request            | De request kan niet worden verwerkt omdat de request body niet geparsed kan worden.                                                         | Ja                            
401 Unauthorized           | De request faalt omdat de gebruiker niet geauthenticeerd is.                                                                                | Optioneel                     
403 Forbidden              | De request faalt omdat de gebruiker niet geauthoriseerd is om de actie uit te voeren.                                                       | Optioneel                     
404 Not found              | De resource kon niet worden gevonden.                                                                                                       | Neen                          
405 Method not allowed     | De methode (HTTP verb) is niet toegelaten op deze resource.                                                                                 | Neen                          
415 Unsupported media type | De request faalt omdat de entiteit in de request in een formaat is die niet ondersteund wordt door de resource voor de bepaalde methode.    | Neen                          
429 Too many requests      | De API consumer heeft te veel requests gestuurd.                                                                                            | Optioneel                     
500 Internal Server Error  | Fout langs server kant.                                                                                                                     | Ja, technical indien mogelijk 
