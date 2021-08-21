## Modularisierung von s0ftf1t

<!--s-->
### Problemstellung

* skalierung von einem Fitnessstudio auf tausende
* Login und Historie sind unterschiedlich stark frequentiert und sollen unabhängig skalieren um Ressourcen zu sparen
* Wie autorisieren wir Nutzer zum Abruf seiner Daten?
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
database "session-management" as ses

user -> webclient++ : Klick auf Historie
webclient -> his++ : getFor(sessionId)
his -> ses++ : getUserForSession(sessionId)
return User
his -> his : getFor(User)
return history
return tabelle
```

<!--v-->
#### Abruf der Historie

* Sessions funktionieren gut für Warenkörbe (da session-Informationen verändert werden)
* ein Login ändert sich aber nicht, warum nicht direkt die Informationen teilen?

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
* JSON Web Token
* Aussprache: JOT
* Standard zum Austausch von **verifizierbaren** Informationen

<!--v-->
#### JWT

* Aufbau eines JWT-Token: "base64(Header).base64(Payload).signature"
  * Header: unter anderem den Signatur-Algorithmus
  * Payload: sog. "Claims", die verifizierbaren Entitäten
  * Signatur: hash des Klartexts aus header und payload entsprechend des im Header angegebenen Algorithmus
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
JWT.io


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
login -> login : signiere JWT mittels\nprivate login-private-Key
return --User-- **JWT**
return willkommen
...
user -> webclient++ : Klick auf Historie
webclient -> his++ : getFor(--User-- **JWT**)
his -> his : validiere Signatur mittels\nlogin-public-key
return history
return tabelle
```
