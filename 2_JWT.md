<!--s-->
## Modularisierung von s0ftf1t

<!--v-->
### Problemstellung

* wir haben expandiert, auf tausende Fitnessstudios
* Login und Historie sind unterschiedlich stark frequentiert und sollen unabhängig skalieren um Ressourcen zu sparen
* Wie überprüfen wir eine erfolgreiche Authentifizierung in einem verteilten System?
  * Klasssicher Ansatz: sessionIds

<!--v-->
#### Login

```puml
actor user
participant "webclient"
participant "login"
database "session-management" as ses

user -> webclient++ : login bei "s0ftf1t.de"
webclient -> login++ : login
login -> login : validiere
login -> ses++ : loggedIn(User)
ses -> ses : lege session an und speichere User
return sessionId
return sessionId
return willkommen
```

<!--v-->
#### Abruf der Historie

```puml
actor user
participant "webclient"
participant "bankdruecken-historie" as his
participant "login"
database "session-management" as ses

user -> webclient++ : Klick auf Historie
webclient -> his++ : getFor(sessionId)
his -> login++ : getUserForSession(sessionId)
login -> ses++ : sessionId
return User
return User
his -> his : getFor(User)
return history
return tabelle
```

<!--v-->
#### Abruf der Historie

* Sessions funktionieren gut für Warenkörbe (da session-Informationen verändert werden)
* ein Login ändert sich aber nicht
* Kann der Login nicht direkt geliefert werden statt einer Id die auf den User verweist?

<!--v-->
### Token

```puml
actor user
participant "webclient"
participant "login"
participant "bankdruecken-historie" as his

user -> webclient++ : login bei "s0ftf1t.de"
webclient -> login++ : login
login -> login : validiere
return User=Achim
return willkommen
...
user -> webclient++ : Klick auf Historie
webclient -> his++ : getFor(user = Achim)
return history
return tabelle
```

<!--v-->
#### JWT

* Wie sicherstellen, dass das Token nicht verändert wurde?
  * JSON Web Token (Aussprache: JOT)
  * JWT ist Standard zum Austausch von **verifizierbaren** Informationen

<!--v-->
#### JWT

* Aufbau eines JWT-Token:
  * "base64(Header).base64(Payload).signature"
  * Header: unter anderem den Signatur-Algorithmus
  * Payload: sog. "Claims", die verifizierbaren Entitäten
  * Signatur: hash des Klartexts aus header und payload entsprechend des im Header angegebenen Algorithmus
  * Beispiel: JWT.io

<!--v-->
### JWT

```puml
actor user
participant "webclient"
participant "login"
participant "bankdruecken-historie" as his

user -> webclient++ : login bei "s0ftf1t.de"
webclient -> login++ : login
login -> login : validiere
login -> login : erzeuge JWT
login -> login : signiere JWT mittels\nprivate login-key
return --User-- **JWT**
return willkommen
...
user -> webclient++ : Klick auf Historie
webclient -> his++ : getFor(--User-- **JWT**)
his -> his : validiere Signatur mittels\nlogin-public-key
return history
return tabelle
```

<!--v-->
### Fazit

In einem verteilten System ist eine zentrale Authentifikations-Instanz welche JWT-Tokens ausstellt hinreichend.

```puml
left to right direction

cloud "s0ft-fit" {
  component Login
  note right of [Login]
    erstellt und signiert
    Auth-Token (JWT)
  end note
  component "bankdruecken-historie" as his
  note right of [his]
    prüft Signatur
    des Auth-Token
  end note
  component webclient

  webclient --> Login: einloggen
  webclient --> his : Historie abrufen\nmit JWT im Header
  webclient <-- Login : liefert Auth-Token (JWT)

  his -> Login : ruft public key ab
}



```
