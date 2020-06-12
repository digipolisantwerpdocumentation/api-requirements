## Event resources
### Event resource afspraken voor het URI formaat
#### R-ER-001
##### Gebruik POST methode en een voltooid deelwoord dat het event beschrijft
Event resources worden als volgt aangegeven : 

POST [/\<groepering>]*/\<event>

parameter     | omschrijving
---------     | ------------
\<groepering> | mag 0 of meerdere keren voor komen om het event te verduidelijken
\<event>      | eindigt op voltooid deelwoord

### Voorbeelden

POST https://api-gateway/digipolis/business-party/v1/events/businesspartycreated

POST https://api-gateway/digipolis/business-party/v1/events/business-party-created

POST https://api-gateway/digipolis/business-party/v1/events/digipolis/business-party-created 

POST https://api-gateway/digipolis/business-party/v1/businesspartycreated

POST https://api-gateway/digipolis/business-party/v1/business-party-created

POST https://api-gateway/digipolis/business-party/v1/digipolis/business-party-created 
