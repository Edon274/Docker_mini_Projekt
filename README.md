# Docker_mini_Projekt

Wir bauen einen kleinen Apache-Webserver in Docker. Die Webseite wird später über Port 8080 erreichbar sein, und die Webseite + Logs sollen ausserhalb vom Container gespeichert werden.

Verzeichnis für das Projekt erstellen
Damit wir alles sauber haben, machen wir zuerst einen neuen Ordner für unser Projekt und gehen gleich rein.

**mkdir webserver_mini_projekt**
**cd webserver_mini_projekt**

Dockerfile erstellen
Im Projektordner brauchen wir ein Dockerfile. Damit geben wir an, wie unser Apache-Webserver im Container laufen soll.

**touch Dockerfile**

Inhalt für das Dockerfile einfügen
Jetzt kommt der Inhalt rein. Er sagt Docker: "Nimm Apache, kopiere die Webseite rein, hör auf Port 80 und starte Apache richtig."

**FROM httpd:2.4
COPY ./public-html/ /usr/local/apache2/htdocs/
LABEL maintainer="edon thaqi"
EXPOSE 80
CMD ["httpd-foreground"]**

Image bauen
Jetzt sagen wir Docker: "Baue das Image genau nach dem Dockerfile."

**docker build -t my-mini-projekt .**

Container starten
Jetzt starten wir den Container. Wir geben ihm einen Namen, verbinden Port 8080 mit Port 80 im Container und binden zwei Verzeichnisse:

Eins für die Webseite (damit du die HTML-Dateien draussen hast)

Eins für die Logdateien (damit du sehen kannst, was Apache macht)

**docker run -d --name mein_webserver -p 8080:80 \
-v /home/vmadmin/webserver_mini_projekt/public-html:/usr/local/apache2/htdocs/ \
-v /home/vmadmin/webserver_mini_projekt/logs:/var/log/apache2/ \
my-mini-projekt**

index.html hinzufügen (falls sie noch nicht existiert)
Wenn du keine index.html hast, mach dir einfach schnell eine mit dem folgenden Befehl. Dann zeigt Apache auch wirklich was an.

**echo ""<h1>"Hallo, das ist meine Webseite"</h1>"" > /home/vmadmin/webserver_mini_projekt/public-html/index.html**
