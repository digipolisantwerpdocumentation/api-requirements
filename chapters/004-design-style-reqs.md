## API design & style requirements

Waarom dan de nood aan zulke API design & style requirements?

Het is belangrijk bij het ontwikkelen van APIs dat het ontwikkelteam consistent is in :
-   naamgevingen (resources altijd meervoud (uitzondering : controllers, status resource), lowercase...)
-   formatteringen van datums, geolocaties,...
-   het implementeren van paginatie/size limiet om de hoeveelheid data die in 1 keer opgevraagd wordt te beperken
-   het optioneel maken van velden die niet voor alle consumers even nuttig zijn
-   het antwoorden met een gestandaardiseerd error contract bij elke API met de bijbehorende juiste HTTP status codes
-   een correct gebruik van verbs, ...

Dit zorgt voor een verhoogde ease-of-use en kan de consuming developers heel wat frustraties en opzoekwerk besparen.
Voor interne API's betekent dit dat de consumers een kortere ontwikkeltijd nodig hebben om met jou API te integreren.
Voor publieke API's is dit des te meer belangrijk omdat je hierdoorzorgt voor een betere "developer experience" en je een hogere
adoptiegraad kan faciliteren.
