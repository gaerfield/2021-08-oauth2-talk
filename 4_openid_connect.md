<!--s-->
## OpenID

<!--v-->
### neue Anforderung

User sollen sich mit ihren bestehenden Google-Konto oder Github-Konto anmelden können (Single Sign On).

### Lösung 1

Wir integrieren alle provider für jede Komponente im Backend:

```puml
left to right direction
cloud google
cloud github
cloud s0ft-fit {
  component login
  component "bankdruecken-history" as his
  component client

  client --> login : login falls s0ft-fit-account
  client --> github : login falls github-account
  client --> google : login falls google

  client ---> his : GET /history\nHeader enthält\ngoogle-token || github-token || s0ftfit-token
  login <-- his : get public key
  google <-- his : get public key
  github <-- his : get public key

  his -> his : validiere token je nach typ
}
```

### Lösung 2 - OpenID Connect

OpenId Connect (OIDC) der aufbauend auf OAuth weitere "claims" definiert und Standard-Endpoints für automatische Discovery:

* Auth0: https://s0ft-fit.eu.auth0.com/.well-known/openid-configuration
* Google: https://accounts.google.com/.well-known/openid-configuration

#### grobe Architektur

Im Grunde: wir registrieren s0ft-fit als Drittanwendung in google bzw. github für den Abruf von Profile-Informationen.

```puml
left to right direction
cloud google
cloud github
cloud s0ft-fit {
  component "Identity Provider\n(Auth0)" as login
  component "bankdruecken-history" as his
  component client

  client --> login : redirect to login
  google --> login  : rufe idToken ab
  github <-- login  : rufe idToken ab
  login --> login : validiere Token
  client <-- login : liefere s0ft-fit\nAccess Token


  login <-- his : rufe public key ab
  client --> his : rufe history mit\ns0ft-fit Token ab
  his -> his : validiere token je nach typ
}
```

#### grobe Architektur

```puml

Actor "Ich\n(Resource Owner)" as reso
participant "Postman\n(Client)" as client
participant "Github\n(Resource Server)" as ress

participant "login\n(Authorization Server)" as auth

reso -> client++ : get repositories
group wenn kein access token vorhanden
client -> auth++ : redirect
auth -> reso++ : Zugriff gewähren für scope "repo"?
return OK
return github-token
end
client -> ress++ : GET /users/gaerfield/repos\nRequest-Header: github-token
return repositories
return repositories
```

###  später
* https://openidconnect.net/

dieses mal mit openid
