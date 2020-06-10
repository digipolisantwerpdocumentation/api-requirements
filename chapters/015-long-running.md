## Langdurende operaties

API operaties dienen steeds zo kort mogelijk worden gehouden. Indien bepaalde operaties toch langer zouden duren, zoals bijvoorbeeld update operaties, dan kunnen deze asynchroon worden afgehandeld indien functioneel mogelijk.

Vanuit technisch perspectief kan bijvoorbeeld volgende flow gevolgd worden :

1.  `De API consumer voert een request uit: POST /{resources}`
2.  `De API consumer ontvangt volgend antwoord: 202 Accepted, Location: /tasks/{id} Hierbij bevat de Location header de URI van een tijdelijke resource.`
3.  `De API consumer polt op de url /tasks/{id}. Indien de taak nog niet uitgevoerd is, is de response 200 OK met een body die de status van de taak beschrijft.`
4.  `Wanneer de taak uitgevoerd is, krijg je een response 303 See Other, Location: /{resources}/{resource-id}. De Location header bevat in dit geval de URI van de definitieve resource.  Wanneer er een fout is gebeurd bij het uitvoeren van de taak, krijg je een error response met de details van het probleem en de toepasselijke statuscode.`
5.  `De API consumer kan een request uitvoeren op de gegeven url : GET /resources/{resource-id} om de resource representatie op te halen.`
