<!--s-->
## OAUTH

<!--v-->
### neue Andforderung

* s0ft-fit ist riesig geworden
* die Tracker-App FitX möchte unsere Daten in ihre App für statistische Auswertungen integrieren

<!--v-->
### Problemstellung

* Wie geben wir der Firma Zugriff auf unsere Api?
* Wie beschränken wir den Zugriff nur auf die Nutzer, die tatsächlich diese Daten weiterreichen möchten?

<!--v-->
### OAuth

OAuth erlaubt es Nutzern einer Anwendung eine andere Anwendung in Ihrem Namen aufzurufen, ohne dass geheime Details der Zugangsberechtigung geteilt werden müsen.

<!--v-->
#### Beispiel Github

* [Gitub - creating an oauth app](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app)

<!--v-->
#### OAuth Grundbegriffe

* Resource Owner: Benutzer (der Zugriff auf seine Daten gewährt)
* Resource Server: Dienst auf dem die geschützten Resourcen liegen
* Client: Drittanwendung, die Zugriff auf die Daten möchte
* Authorization Server: der Server, der den _Resource Owner_ authentifiziert und _Access Token_ und _Refresh Token_  ausstellt
* Access Token: kurzlebiges geheimes Token, mittels dessen Resourcen beim _Resource Server_ im Namen des _Resource Owner_ abgerufen werden können
* Refresh Token: "langlebiges" geheimes Token mittels dem neue _Access Token_ beim _Authorization Server_ abgerufen werden können ohne sich erneut authentifizieren zu müssen
* Scope: feingranulare Zugriffsrechte die durch den _Client_ angefragt werden
* Consent: die explizite Zustimmung des _Resource Owner_ ob sie dem _Client_ die angefragten Scopes gewähren

<!--v-->
#### der Oauth-Flow am Beispiel Github
```puml

Actor "Ich\n(Resource Owner)" as reso
participant "Postman\n(Client)" as client
participant "Github\n(Resource Server)" as ress
participant "Github Auth\n(Authorization Server)" as auth

reso -> client++ : get repositories
group wenn kein access token vorhanden
client -> auth++ : redirect
auth -> reso++ : bitte einloggen
return username+password
auth -> auth : authentifiziere
auth -> reso++ : Zugriff gewähren für scope "repo"?
return OK
return github-token
end
client -> ress++ : GET /users/gaerfield/repos\nRequest-Header: github-token
return repositories
return repositories
```

<!--v-->
#### Beispiel Github II

* andere scopes ausprobieren

<!--v-->
### Also OAuth selbst implementieren?

* tendenziell eher nein
* stattdessen Identity Server nutzen:
  * [Liste div. Server](https://openid.net/developers/certified/)
  * z.B. Keycloak
* oder extern einkaufen:
  * (Okta)[https://www.okta.com/de/]
  * (Auth0)[https://auth0.com/de]
