# Digipolis API design & style requirements v6.0.5

geldig vanaf 01 mei 2019

## Inhoudstabel
<!-- PC : generated with md-toc (https://github.com/digipolisantwerp/markdown-toc_app_go) -->

<!-- dgp-toc-start -->
* [Document historiek](apireq-chapters/001-document-history.md#document-historiek)
* [Cheat sheet](apireq-chapters/002-cheat-sheet.md#cheat-sheet)
  * [Taal](apireq-chapters/002-cheat-sheet.md#taal)
  * [Formaat API documentatie op de API Engine](apireq-chapters/002-cheat-sheet.md#formaat-api-documentatie-op-de-api-engine)
  * [Payload](apireq-chapters/002-cheat-sheet.md#payload)
  * [Resource](apireq-chapters/002-cheat-sheet.md#resource)
  * [Request](apireq-chapters/002-cheat-sheet.md#request)
  * [Paginatie](apireq-chapters/002-cheat-sheet.md#paginatie)
  * [Event resources](apireq-chapters/002-cheat-sheet.md#event-resources)
  * [Error model](apireq-chapters/002-cheat-sheet.md#error-model)
  * [Versionering](apireq-chapters/002-cheat-sheet.md#versionering)
  * [Langdurende operaties](apireq-chapters/002-cheat-sheet.md#langdurende-operaties)
  * [Swagger](apireq-chapters/002-cheat-sheet.md#swagger)
  * [HTTP verbs](apireq-chapters/002-cheat-sheet.md#http-verbs)
  * [HTTP response codes](apireq-chapters/002-cheat-sheet.md#http-response-codes)
* [API's](apireq-chapters/003-api.md#apis)
* [API design & style requirements](apireq-chapters/004-design-style-reqs.md#api-design--style-requirements)
* [REST introductie](apireq-chapters/005-rest.md#rest-introductie)
* [Resource representatie](apireq-chapters/006-resource-representation.md#resource-representatie)
  * [Taal](apireq-chapters/006-resource-representation.md#taal)
    * [R-TL-001](apireq-chapters/006-resource-representation.md#r-tl-001)
      * [API's in het Engels](apireq-chapters/006-resource-representation.md#apis-in-het-engels)
  * [JSON conventies](apireq-chapters/006-resource-representation.md#json-conventies)
    * [R-JS-001](apireq-chapters/006-resource-representation.md#r-js-001)
      * [Gebruik steeds dubbele quotes bij keys](apireq-chapters/006-resource-representation.md#gebruik-steeds-dubbele-quotes-bij-keys)
    * [R-JS-002](apireq-chapters/006-resource-representation.md#r-js-002)
      * [Gebruik steeds dubbele quotes bij string values](apireq-chapters/006-resource-representation.md#gebruik-steeds-dubbele-quotes-bij-string-values)
    * [R-JS-003](apireq-chapters/006-resource-representation.md#r-js-003)
      * [Gebruik steeds camelCase om keys weer te geven](apireq-chapters/006-resource-representation.md#gebruik-steeds-camelcase-om-keys-weer-te-geven)
    * [R-JS-004](apireq-chapters/006-resource-representation.md#r-js-004)
      * [Gebruik geen dots "." in keys](apireq-chapters/006-resource-representation.md#gebruik-geen-dots--in-keys)
    * [R-JS-005](apireq-chapters/006-resource-representation.md#r-js-005)
      * [Keys mogen niet starten met cijfers](apireq-chapters/006-resource-representation.md#keys-mogen-niet-starten-met-cijfers)
    * [R-JS-006](apireq-chapters/006-resource-representation.md#r-js-006)
      * [Verwijder null waardes uit de resource representatie indien deze geen betekenis hebben](apireq-chapters/006-resource-representation.md#verwijder-null-waardes-uit-de-resource-representatie-indien-deze-geen-betekenis-hebben)
    * [R-JS-007](apireq-chapters/006-resource-representation.md#r-js-007)
      * [Toon lege waardes in de resource representatie](apireq-chapters/006-resource-representation.md#toon-lege-waardes-in-de-resource-representatie)
    * [R-JS-008](apireq-chapters/006-resource-representation.md#r-js-008)
      * [Encapsuleer arrays steeds in een object](apireq-chapters/006-resource-representation.md#encapsuleer-arrays-steeds-in-een-object)
  * [Datums en timestamps](apireq-chapters/006-resource-representation.md#datums-en-timestamps)
    * [R-DT-001](apireq-chapters/006-resource-representation.md#r-dt-001)
      * [Formatteer datums en timestamps volgens RFC339](apireq-chapters/006-resource-representation.md#formatteer-datums-en-timestamps-volgens-rfc339)
  * [Durations](apireq-chapters/006-resource-representation.md#durations)
    * [R-DU-001](apireq-chapters/006-resource-representation.md#r-du-001)
      * [Formatteer durations volgens ISO8601](apireq-chapters/006-resource-representation.md#formatteer-durations-volgens-iso8601)
  * [Geospatiale data](apireq-chapters/006-resource-representation.md#geospatiale-data)
    * [R-GS-001](apireq-chapters/006-resource-representation.md#r-gs-001)
      * [Formatteer geospatiale data volgens RFC7946](apireq-chapters/006-resource-representation.md#formatteer-geospatiale-data-volgens-rfc7946)
  * [Hiërarchie](apireq-chapters/006-resource-representation.md#hi%c3%abrarchie)
    * [R-HA-001](apireq-chapters/006-resource-representation.md#r-ha-001)
      * [Structureer resource hiërarchisch ipv vlak](apireq-chapters/006-resource-representation.md#structureer-resource-hi%c3%abrarchisch-ipv-vlak)
* [Resources](apireq-chapters/007-resources.md#resources)
  * [URI structuur](apireq-chapters/007-resources.md#uri-structuur)
    * [R-US-001](apireq-chapters/007-resources.md#r-us-001)
      * [Benader resource volgens vastgelegde structuur](apireq-chapters/007-resources.md#benader-resource-volgens-vastgelegde-structuur)
    * [R-US-002](apireq-chapters/007-resources.md#r-us-002)
      * [Versioneer API URI](apireq-chapters/007-resources.md#versioneer-api-uri)
  * [Naming conventions](apireq-chapters/007-resources.md#naming-conventions)
    * [R-NC-001](apireq-chapters/007-resources.md#r-nc-001)
      * [Gebruik zelfstandige naamwoorden voor resources, behalve voor controllers](apireq-chapters/007-resources.md#gebruik-zelfstandige-naamwoorden-voor-resources-behalve-voor-controllers)
    * [R-NC-002](apireq-chapters/007-resources.md#r-nc-002)
      * [Gebruik geen werkwoorden in resource namen, behalve bij controllers](apireq-chapters/007-resources.md#gebruik-geen-werkwoorden-in-resource-namen-behalve-bij-controllers)
    * [R-NC-003](apireq-chapters/007-resources.md#r-nc-003)
      * [Definieer resources in het meervoud](apireq-chapters/007-resources.md#definieer-resources-in-het-meervoud)
    * [R-NC-004](apireq-chapters/007-resources.md#r-nc-004)
      * [Gebruik steeds lowercase voor URI en query parameters](apireq-chapters/007-resources.md#gebruik-steeds-lowercase-voor-uri-en-query-parameters)
    * [R-NC-005](apireq-chapters/007-resources.md#r-nc-005)
      * [Gebruik geen underscores "\_" of dots "." in de URI](apireq-chapters/007-resources.md#gebruik-geen-underscores-%5c_-of-dots--in-de-uri)
    * [R-NC-006](apireq-chapters/007-resources.md#r-nc-006)
      * [Gebruik hyphenation om woorden van elkaar te scheiden](apireq-chapters/007-resources.md#gebruik-hyphenation-om-woorden-van-elkaar-te-scheiden)
    * [R-NC-007](apireq-chapters/007-resources.md#r-nc-007)
      * [Gebruik geen trailing slash in de URI](apireq-chapters/007-resources.md#gebruik-geen-trailing-slash-in-de-uri)
    * [R-NC-008](apireq-chapters/007-resources.md#r-nc-008)
      * [Gebruik geen fragments in de URI](apireq-chapters/007-resources.md#gebruik-geen-fragments-in-de-uri)
  * [Media types en content negotiation](apireq-chapters/007-resources.md#media-types-en-content-negotiation)
    * [R-MC-001](apireq-chapters/007-resources.md#r-mc-001)
      * [Gebruik steeds media types en content negotiation](apireq-chapters/007-resources.md#gebruik-steeds-media-types-en-content-negotiation)
    * [R-MC-002](apireq-chapters/007-resources.md#r-mc-002)
      * [Gebruik nooit file extenties in de URI om media types te communiceren](apireq-chapters/007-resources.md#gebruik-nooit-file-extenties-in-de-uri-om-media-types-te-communiceren)
    * [R-MC-003](apireq-chapters/007-resources.md#r-mc-003)
      * [Gebruik standaard het application/json media type](apireq-chapters/007-resources.md#gebruik-standaard-het-application%2fjson-media-type)
    * [R-MC-004](apireq-chapters/007-resources.md#r-mc-004)
      * [Definieer optioneel een locale](apireq-chapters/007-resources.md#definieer-optioneel-een-locale)
    * [R-MC-005](apireq-chapters/007-resources.md#r-mc-005)
      * [Gebruik specifieke HTTP headers voor media-types en content negotiation](apireq-chapters/007-resources.md#gebruik-specifieke-http-headers-voor-media-types-en-content-negotiation)
* [Request](apireq-chapters/008-request.md#request)
  * [Request body & query parameters](apireq-chapters/008-request.md#request-body--query-parameters)
    * [R-RQ-001](apireq-chapters/008-request.md#r-rq-001)
      * [Verstuur resource representaties via de request body](apireq-chapters/008-request.md#verstuur-resource-representaties-via-de-request-body)
  * [HTTP verbs](apireq-chapters/008-request.md#http-verbs)
    * [R-HV-001](apireq-chapters/008-request.md#r-hv-001)
      * [Gebruik HTTP verbs correct en volgens RFC7231](apireq-chapters/008-request.md#gebruik-http-verbs-correct-en-volgens-rfc7231)
    * [R-HV-002](apireq-chapters/008-request.md#r-hv-002)
      * [Gebruik HTTP verb PATCH volgens RFC7386 of RFC6902](apireq-chapters/008-request.md#gebruik-http-verb-patch-volgens-rfc7386-of-rfc6902)
  * [Voorbeelden](apireq-chapters/008-request.md#voorbeelden)
    * [GET](apireq-chapters/008-request.md#get)
    * [HEAD](apireq-chapters/008-request.md#head)
    * [PUT](apireq-chapters/008-request.md#put)
    * [POST](apireq-chapters/008-request.md#post)
    * [PATCH](apireq-chapters/008-request.md#patch)
    * [DELETE](apireq-chapters/008-request.md#delete)
* [Status codes & response](apireq-chapters/009-statuscodes-response.md#status-codes--response)
  * [Vastgelegde HTTP Status codes en response informatie](apireq-chapters/009-statuscodes-response.md#vastgelegde-http-status-codes-en-response-informatie)
    * [R-SC-001](apireq-chapters/009-statuscodes-response.md#r-sc-001)
      * [Gebruik de correcte status code bij elke response en de benodigde reponse informatie](apireq-chapters/009-statuscodes-response.md#gebruik-de-correcte-status-code-bij-elke-response-en-de-benodigde-reponse-informatie)
* [Filteren en sorteren](apireq-chapters/010-filter-sort.md#filteren-en-sorteren)
  * [Filteren op resources](apireq-chapters/010-filter-sort.md#filteren-op-resources)
    * [R-FR-001](apireq-chapters/010-filter-sort.md#r-fr-001)
      * [Filter op een resource via id](apireq-chapters/010-filter-sort.md#filter-op-een-resource-via-id)
    * [R-FR-002](apireq-chapters/010-filter-sort.md#r-fr-002)
      * [Filter op een resource via andere unieke sleutel](apireq-chapters/010-filter-sort.md#filter-op-een-resource-via-andere-unieke-sleutel)
    * [R-FR-003](apireq-chapters/010-filter-sort.md#r-fr-003)
      * [Gebruik geen gecombineerde sleutels voor een unieke sleutel](apireq-chapters/010-filter-sort.md#gebruik-geen-gecombineerde-sleutels-voor-een-unieke-sleutel)
    * [R-FR-004](apireq-chapters/010-filter-sort.md#r-fr-004)
      * [Filter op een resource via query parameters](apireq-chapters/010-filter-sort.md#filter-op-een-resource-via-query-parameters)
  * [Filteren op resource representatie](apireq-chapters/010-filter-sort.md#filteren-op-resource-representatie)
    * [R-FP-001](apireq-chapters/010-filter-sort.md#r-fp-001)
      * [Filter op een resource representatie via de fields query parameter](apireq-chapters/010-filter-sort.md#filter-op-een-resource-representatie-via-de-fields-query-parameter)
  * [Sorteren](apireq-chapters/010-filter-sort.md#sorteren)
    * [R-SR-001](apireq-chapters/010-filter-sort.md#r-sr-001)
      * [Sorteer response door middel van de sort query parameter](apireq-chapters/010-filter-sort.md#sorteer-response-door-middel-van-de-sort-query-parameter)
* [Paginatie](apireq-chapters/011-paging.md#paginatie)
  * [Pagineren van collecties](apireq-chapters/011-paging.md#pagineren-van-collecties)
    * [R-PC-001](apireq-chapters/011-paging.md#r-pc-001)
      * [Gebruik altijd paginering bij het ophalen van collecties](apireq-chapters/011-paging.md#gebruik-altijd-paginering-bij-het-ophalen-van-collecties)
    * [R-PC-002](apireq-chapters/011-paging.md#r-pc-002)
      * [Gebruik altijd het vastgelegde pagineringsmodel bij het ophalen van collecties](apireq-chapters/011-paging.md#gebruik-altijd-het-vastgelegde-pagineringsmodel-bij-het-ophalen-van-collecties)
    * [R-PC-003](apireq-chapters/011-paging.md#r-pc-003)
      * [Ondersteun beide paging-strategy methodes](apireq-chapters/011-paging.md#ondersteun-beide-paging-strategy-methodes)
  * [Paginatie query parameters](apireq-chapters/011-paging.md#paginatie-query-parameters)
    * [R-PQ-001](apireq-chapters/011-paging.md#r-pq-001)
      * [Gebruik de vastgelegde query parameters page, pagesize en paging-strategy voor paginering](apireq-chapters/011-paging.md#gebruik-de-vastgelegde-query-parameters-page-pagesize-en-paging-strategy-voor-paginering)
    * [R-PQ-002](apireq-chapters/011-paging.md#r-pq-002)
      * [Paginering is 1 based](apireq-chapters/011-paging.md#paginering-is-1-based)
    * [R-PQ-003](apireq-chapters/011-paging.md#r-pq-003)
      * [Gebruik withCount of noCount waarden voor de paging strategie](apireq-chapters/011-paging.md#gebruik-withcount-of-nocount-waarden-voor-de-paging-strategie)
  * [Paginatie response bericht](apireq-chapters/011-paging.md#paginatie-response-bericht)
    * [R-PR-001](apireq-chapters/011-paging.md#r-pr-001)
      * [Gebruik de HAL specificatie voor gepagineerde responses](apireq-chapters/011-paging.md#gebruik-de-hal-specificatie-voor-gepagineerde-responses)
    * [R-PR-002](apireq-chapters/011-paging.md#r-pr-002)
      * [Gebruik media type application/hal+json voor gepagineerde responses](apireq-chapters/011-paging.md#gebruik-media-type-application%2fhal%2bjson-voor-gepagineerde-responses)
    * [Voorbeelden](apireq-chapters/011-paging.md#voorbeelden)
* [Event resources](apireq-chapters/012-event-resources.md#event-resources)
  * [Event resource afspraken voor het URI formaat](apireq-chapters/012-event-resources.md#event-resource-afspraken-voor-het-uri-formaat)
    * [R-ER-001](apireq-chapters/012-event-resources.md#r-er-001)
      * [Gebruik POST methode en een voltooid deelwoord dat het event beschrijft](apireq-chapters/012-event-resources.md#gebruik-post-methode-en-een-voltooid-deelwoord-dat-het-event-beschrijft)
  * [Voorbeelden](apireq-chapters/012-event-resources.md#voorbeelden)
* [Error handling](apireq-chapters/013-error-handling.md#error-handling)
  * [Exception shielding](apireq-chapters/013-error-handling.md#exception-shielding)
    * [R-ES-001](apireq-chapters/013-error-handling.md#r-es-001)
      * [Gebruik Exception Shielding](apireq-chapters/013-error-handling.md#gebruik-exception-shielding)
  * [Error model](apireq-chapters/013-error-handling.md#error-model)
    * [R-EM-001](apireq-chapters/013-error-handling.md#r-em-001)
      * [Gebruik het door Digipolis vastgelegde error model](apireq-chapters/013-error-handling.md#gebruik-het-door-digipolis-vastgelegde-error-model)
  * [HTTP status codes en error model](apireq-chapters/013-error-handling.md#http-status-codes-en-error-model)
    * [R-EC-001](apireq-chapters/013-error-handling.md#r-ec-001)
      * [Gebruik de correcte status codes en error responses](apireq-chapters/013-error-handling.md#gebruik-de-correcte-status-codes-en-error-responses)
* [Versionering](apireq-chapters/014-versioning.md#versionering)
  * [Belang van een goede versioneringsstrategie](apireq-chapters/014-versioning.md#belang-van-een-goede-versioneringsstrategie)
  * [Algemene versioneringsstrategie](apireq-chapters/014-versioning.md#algemene-versioneringsstrategie)
  * [API contract versionering](apireq-chapters/014-versioning.md#api-contract-versionering)
    * [R-VC-001](apireq-chapters/014-versioning.md#r-vc-001)
      * [Gebruik semver versionering](apireq-chapters/014-versioning.md#gebruik-semver-versionering)
  * [API technische versionering](apireq-chapters/014-versioning.md#api-technische-versionering)
    * [R-VT-001](apireq-chapters/014-versioning.md#r-vt-001)
      * [Gebruik major versionering](apireq-chapters/014-versioning.md#gebruik-major-versionering)
    * [Voordelen](apireq-chapters/014-versioning.md#voordelen)
    * [Nadelen](apireq-chapters/014-versioning.md#nadelen)
* [Langdurende operaties](apireq-chapters/015-long-running.md#langdurende-operaties)
  * [Asynchrone verwerking](apireq-chapters/015-long-running.md#asynchrone-verwerking)
    * [R-AV-001](apireq-chapters/015-long-running.md#r-av-001)
      * [Gebruik Asynchrone verwerking voor langdurende operaties](apireq-chapters/015-long-running.md#gebruik-asynchrone-verwerking-voor-langdurende-operaties)
    * [R-AV-002](apireq-chapters/015-long-running.md#r-av-002)
      * [Gebruik minimaal HTTP status code 202 Accepted](apireq-chapters/015-long-running.md#gebruik-minimaal-http-status-code-202-accepted)
    * [R-AV-003](apireq-chapters/015-long-running.md#r-av-003)
      * [Implementeer eventueel een polling mechanisme voor statusopvolging](apireq-chapters/015-long-running.md#implementeer-eventueel-een-polling-mechanisme-voor-statusopvolging)
* [Postman tips](postman-tips.md#postman-tips)
  * [Wat doe je gewoonlijk](postman-tips.md#wat-doe-je-gewoonlijk)
  * [Het Probleem](postman-tips.md#het-probleem)
  * [De Oplossing](postman-tips.md#de-oplossing)
    * [1. variabelen in scheme, host en basePath](postman-tips.md#1-variabelen-in-scheme-host-en-basepath)
    * [2. variabelen in het path](postman-tips.md#2-variabelen-in-het-path)
    * [3. query variable](postman-tips.md#3-query-variable)
    * [4. header variable](postman-tips.md#4-header-variable)
  * [Standard apikey & tenant](postman-tips.md#standard-apikey--tenant)
  * [Postman environment files](postman-tips.md#postman-environment-files)
    * [globale environment variabelen](postman-tips.md#globale-environment-variabelen)
    * [environment variabelen](postman-tips.md#environment-variabelen)
* [Goede documentatie in Swagger files](swagger-docs.md#goede-documentatie-in-swagger-files)
  * [Tone of voice](swagger-docs.md#tone-of-voice)
  * [Concepten](swagger-docs.md#concepten)
  * [Swagger file](swagger-docs.md#swagger-file)
    * [Info object](swagger-docs.md#info-object)
    * [Werken met Tags](swagger-docs.md#werken-met-tags)
    * [De Operations](swagger-docs.md#de-operations)
      * [Tags](swagger-docs.md#tags)
      * [Summary & description](swagger-docs.md#summary--description)
      * [Deprecated](swagger-docs.md#deprecated)
    * [Parameters](swagger-docs.md#parameters)
    * [Responses](swagger-docs.md#responses)
<!-- dgp-toc-end -->