<!--s-->
## Ziel

Dezentrales Authentifizierungssystem für webbasierte Dienste.

* OAuth als Protokoll zur Authentifizierung für die Autorisierung von Zugriffsrechten zwischen zwei Applikationen
* OpenID Connect aufbauend auf OAuth erlaubt die Wiederverwendung desselben Logins für verschiedene Applikationen (Single-Sign-On) und den standardisierten Abruf von Profilinformationen

<!--v-->
## Geschichte

* 2005 OpenID Vorstellung des offenen Single-Sign-On Protokolls
* 2006 Entwicklung von OAuth 1.0 während der Integration von OpenID in Twitter (Delegation der Authentifizierung)
* 2007 Vortstellung von OAuth 1.0
* Juni 2007 Gründung der OpenID Foundation zur Sicherung von Markenrechten
* Dezember 2007 OpenID 2.0
* November 2008 Vorschlag an die IETF zur Standardisierung von OAuth
* 2012 OAuth 2 durch die IETF als RFC 6749 und RFC 6750 veröffentlicht.
* irgendwann OpenID Connect als Schicht oberhalb des OAuth Protokolls

<!--v-->
## Startup s0ftf1t

Fitnessstudio-Startup mit online Training-Tracker:
* gehe Trainieren wann immer du willst
* rufe deine Trainings-Historie ab wann immer du willst

<!--v-->
### Architektur

```puml
component "s0ft-fit.de"
```

### Typischer Flow der Authentifizierung
```puml
actor user
participant "webclient"
participant "server"
user -> webclient++ : login bei "s0ft-fit.de"
webclient -> server++ : login
server -> server : validiere
return ok
return willkommen
```
