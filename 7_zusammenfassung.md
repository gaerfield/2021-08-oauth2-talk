<!--s-->
## Zusammenfassung

* Java Web Tokens erlaubt in verteilten Systemen den Austausch verifizierbarer Informationen
* OAuth ermöglicht es Nutzern den Zugriff auf die eigenen Daten für Drittanwendungen zu steuern (ohne Weitergabe des Passworts)
* OpenId ermöglicht Single-Sign-On

<!--v-->
## Nachteile

* Logout nur eingeschränkt möglich
  * Refresh Tokens können invalidiert werden
  * Access Tokens behalten ihre Gültigkeit auch nach Logout

<!--v-->
## Fazit

* Authentifizierung ist eine klassische querschnittliche Aufgabe
* in verteilten Systemen durch eine eigene Komponente zu (wie bspw. auch Logging, Tracing und Monitoring)
* selbst dann sinnvoll, wenn die restliche Architektur monolithisch:
  * Zukunftssicherheit (Monolith leichter zu modularisieren)
  * vereinfacht die Architektur (nur permissions werden geprüft)
  * verlagert Sicherheitsrisiken zum Identity Provider
  * Standard-Konformität erlaubt einfachere Erweiterung auf neue Feature-Requests
  * kann getrennt voneinander betrieben werden (erhöht Wartbarkeit)
* sollte niemals selber implementiert werden
