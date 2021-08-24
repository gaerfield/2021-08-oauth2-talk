## OAuth Flows

<!--v-->
### Ist der Client auch der Resource Owner?

* Resource Owner ist ein Dienst ist der Zugriff benötigt (bspw. CronJobs)
* keine Nutzer-Authentifizierung notwendig
* M2M-Token
* Client-Id und Client-Secret zur Erlangung eines Access-Tokens

<!--v-->
### Client Credentials Flow

https://auth0.com/docs/flows/client-credentials-flow

<!--v-->
### auf dem Server ausgeführte web app

* Access Token wird direkt an den Webserver übermittelt auf dem der client betrieben wird
* kein Risiko des ungewollten Verlusts, weil keine Tokens in den Browser übertragen werden

<!--v-->
### Authorization Code Flow

https://auth0.com/docs/flows/authorization-code-flow

<!--v-->
### Is the Client a Single-Page App oder native App?

native apps:
* Client Secrets werden per Client (also per App) ausgestellt
* ist identisch für alle Nutzer und App-Instanzen
* eine native kann das Client Secret nicht sicher speichern
  * Dekompilierung
  * andere Apps können custom URL schemes (bspw. "MyApp://") registrieren und potentiell den redirect zu sich umleiten und den Authorization Code abfangen
* Single Page Apps können das Client Secret nicht sicher verwahren, weil deren gesamter Code im Browser liegt

<!--v-->
### Authorization Code Flow (PKCE)

* Erweiterung um:
  * ein durch den Client dynamisch erstelltes Secret, der `Code Verifier`
  * eine `Code Challenge` des `Code Verifier`
* Client sendet Code Challenge an den Authorization Server um `Authorization Code` zu erhalten
* `Code Verifier` verbleibt lokal
* mittels `Authorization Code` und `Code Verifier` kann ein Access Token geholt werden
* Attacken können nur den `Authorization Code` abfangen der ungenügend zur Ausstellung von Access Tokens ist

<!--v-->
### Authorization Code Flow (PKCE)

https://auth0.com/docs/flows/authorization-code-flow-with-proof-key-for-code-exchange-pkce

<!--v-->
### benötigt die WebApp nur ein id Token?

* "normaler" Login nach authorization code flow erfordert das backend ein secret zu verwalten
* die App führt keine Aufrufe an den Resource Server durch und benötigt nur ein Id Token?

<!--v-->
### Implicit Flow with Form Post

https://auth0.com/docs/flows/implicit-flow-with-form-post

<!--v-->
### Ist der Client voll vertrauenswürdig?

* Resource Owner Password Flow
  * User und Passwort werden in der Applikation eingetragen
  * evtl. an das Backend übermittelt
* ist **ausschließlich** dann zu verwenden werden, wenn redirect-basierte Flows (bspw. Authorization Code Flow) nicht funktionieren

<!--v-->
### Resource Owner Password Flow

https://auth0.com/docs/flows/resource-owner-password-flow

## Zusammenfassung

* Client Credentials Flow: für M2M-Szenarien
* Authorization Code Flow (PKCE): für Web und Native Apps
* Resource Owner Password Flow: um Geld bei M2M zu sparen
