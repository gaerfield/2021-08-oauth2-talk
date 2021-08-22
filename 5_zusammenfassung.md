<!--s-->
## Zusammenfassung

* Java Web Tokens erlauben in verteilten Systemen den Austausch verifizierbarer Informationen
* OAuth ermöglicht es Nutzern den Zugriff auf die eigenen Daten für Drittanwendungen zu steuern (ohne das Passwort weitergeben zu müssen)
* OpenId ermöglicht es Logins wiederzuverwenden

<!--v-->
## Fazit

* die Implementation einer Authentifizierung ist nach meiner Erfahrung schon immer kompliziert gewesen, selbst in Monolithen:
  * User Datenbank muss gepflegt werden
  * Rollen und Gruppen müssen verwaltet werden
  * etc.
* selbst wenn keine Anforderung für Social Logins oder Zugriffserlaubnis für Drittanwendungen besteht, ist der Einsatz eines Identity-Providers zu bevorzugen (sei es on-premise oder service):
  * Implementationskosten verlagern sich auf Konfigurationskosten
  * Verringerung der Anwendungskomplexität:
    * das Backend muss nichts über Authentifikation wissen
    * alle relevanten Informationen sind im Token enthalten
    * oder können beim Identity Provider abgefragt werden
  * Betriebskosten sind identisch oder geringer
  * geringere Sicherheitsrisiken
  * erzwingt sinvollere Architektur durch Trennung von Verantwortlichkeiten über standardisierte Schnittstellen

<!--v-->
## Fazit

Bloß nicht selber implementieren!

###  Links

* https://openidconnect.net/

dieses mal mit openid
