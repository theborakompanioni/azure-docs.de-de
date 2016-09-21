<properties
	pageTitle="Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App"
	description="Es wird beschrieben, wie Sie Ihrer App in Azure App Service einen benutzerdefinierten Domänennamen (Vanity-Domäne) zuordnen."
	services="app-service"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor="jimbe"
	tags="top-support-issue"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/27/2016"
	ms.author="cephalin"/>

# Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen manuell der Web-App, dem Mobile App-Back-End oder der API-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) zuordnen.

Ihre App enthält bereits eine eindeutige Unterdomäne von „azurewebsites.net“. Wenn die App beispielsweise den Namen **contoso** hat, lautet der Domänenname **contoso.azurewebsites.net**. Sie können der App aber einen benutzerdefinierten Domänennamen zuordnen, damit die URL, z.B. `www.contoso.com`, Ihre Marke widerspiegelt.

>[AZURE.NOTE] Hilfe hierzu erhalten Sie von den Azure-Experten in den [Azure-Foren](https://azure.microsoft.com/support/forums/). Falls Sie mehr Support benötigen, können Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) besuchen und auf **Support erhalten** klicken.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## Erwerben einer neuen benutzerdefinierten Domäne im Azure-Portal

Falls Sie noch keinen benutzerdefinierten Domänennamen erworben haben, können Sie dies im [Azure-Portal](https://portal.azure.com) direkt in den Einstellungen der App nachholen und dort auch die Verwaltung durchführen. Mit dieser Option ist es einfach, der App eine benutzerdefinierte Domäne zuzuordnen. Dabei spielt es keine Rolle, ob Ihre App [Azure Traffic Manager](web-sites-traffic-manager-custom-domain-name.md) nutzt.

Eine Anleitung hierzu finden Sie unter [Erwerben eines benutzerdefinierten Domänennamens für App Service](custom-dns-web-site-buydomains-web-app.md).

## Externes Zuordnen einer erworbenen benutzerdefinierten Domäne

Wenn Sie bereits eine benutzerdefinierte Domäne über [Azure DNS](https://azure.microsoft.com/services/dns/) oder von einem Drittanbieter erworben haben, gibt es drei Hauptschritte zum Zuordnen der benutzerdefinierten Domäne zur App:

1. [*(Nur Ressourceneintrag)* Beschaffen Sie die IP-Adresse der App](#vip).
2. [Erstellen Sie die DNS-Einträge, mit denen die Domäne der App zugeordnet wird.](#createdns)
    - **Wo:** Eigenes Verwaltungstool der Domänenregistrierungsstelle (z.B. Azure DNS, GoDaddy usw.).
    - **Warum:** Damit die Domänenregistrierungsstelle weiß, wie die gewünschte benutzerdefinierte Domäne für Ihre Azure-App aufgelöst wird.
1. [Aktivieren Sie den benutzerdefinierten Domänennamen für Ihre Azure-App.](#enable)
    - **Wo:** [Azure-Portal](https://portal.azure.com).
    - **Warum:** Damit Ihre App weiß, wie sie auf Anforderungen reagieren soll, die an den benutzerdefinierten Domänennamen gestellt werden.
3. [Überprüfen Sie die DNS-Verteilung](#verify).

### Typen von Domänen, die Sie zuordnen können

Mit Azure App Service können Sie die folgenden Kategorien von benutzerdefinierten Domänen Ihrer App zuordnen.

- **Stammdomäne**: Der Domänenname, den Sie bei der Domänenregistrierungsstelle reserviert haben (normalerweise durch den Hosteintrag `@` dargestellt). Beispiel: **contoso.com**.
- **Unterdomäne**: Jede Domäne unterhalb Ihrer Stammdomäne. Beispiel: **www.contoso.com** (wird durch den Hosteintrag `www` dargestellt). Sie können verschiedene Unterdomänen derselben Stammdomäne unterschiedlichen Apps in Azure zuordnen.
- **Platzhalterdomäne**: [Jede Domäne, deren ganz links stehende DNS-Bezeichnung `*` lautet](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z.B. die Hosteinträge `*` und `*.blogs`). Beispiel: ***.contoso.com**.

### Typen von zulässigen DNS-Einträgen

Je nach Bedarf können Sie zwei unterschiedliche Arten von DNS-Standardeinträgen verwenden, um Ihre benutzerdefinierte Domäne zuzuordnen:

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A): Ordnet Ihren benutzerdefinierten Domänennamen direkt der virtuellen IP-Adresse der Azure-App zu.
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record): Ordnet Ihren benutzerdefinierten Domänennamen dem Azure-Domänennamen Ihrer App zu: **&lt;*App-Name*>.azurewebsites.net**.

Der Vorteil von CNAME besteht darin, dass er auch bei Änderungen der IP-Adresse beibehalten wird. Die virtuelle IP-Adresse Ihrer App kann sich ändern, wenn Sie die App löschen und neu erstellen oder eine Änderung von einem höheren Tarif zurück auf den Tarif **Shared** vornehmen. Bei einer Änderung dieser Art ist ein CNAME-Eintrag weiterhin gültig, während ein A-Eintrag aktualisiert werden muss.

Das Tutorial enthält die Schritte zur Verwendung des A-Eintrags und des CNAME-Eintrags.

>[AZURE.IMPORTANT] Erstellen Sie keinen CNAME-Eintrag für die Stammdomäne (d.h. den „Stammeintrag“). Weitere Informationen finden Sie unter [Why can't a CNAME record be used at the root domain](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain) (Warum kann ein CNAME-Eintrag nicht für die Stammdomäne verwendet werden?). Verwenden Sie stattdessen zum Zuordnen einer Stammdomäne zu Ihrer Azure-App einen A-Eintrag.

<a name="vip"></a>
## Schritt 1: *(Nur A-Eintrag)* Beschaffen der IP-Adresse der App
Sie benötigen die IP-Adresse Ihrer Azure-App, um einen benutzerdefinierten Domänennamen mit einem A-Eintrag zuzuordnen. Wenn Sie für die Zuordnung einen CNAME-Eintrag verwenden, können Sie diesen Schritt überspringen und mit dem nächsten Abschnitt fortfahren.

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2.	Klicken Sie im linken Menü auf **App Services**.

4.	Klicken Sie auf Ihre App und anschließend auf **Benutzerdefinierte Domänen**.

6.  Notieren Sie sich die IP-Adresse über dem Hostnamenabschnitt:

    ![Zuordnen eines benutzerdefinierten Domänennamens mit A-Eintrag: Abrufen der IP-Adresse für Ihre Azure App Service-App](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Lassen Sie dieses Blatt des Portals geöffnet. Sie kehren zu diesem Blatt zurück, wenn Sie die DNS-Einträge erstellen.

<a name="createdns"></a>
## Schritt 2: Erstellen von DNS-Einträgen

Melden Sie sich bei Ihrer Domänenregistrierungsstelle an, und verwenden Sie die verfügbaren Tools, um einen A-Eintrag oder einen CNAME-Eintrag zu erstellen. Da sich die Benutzeroberflächen der einzelnen Registrierungsstellen leicht unterscheiden, ist es ratsam, jeweils in der Dokumentation des Anbieters nachzulesen. Hier sind aber bereits einige allgemeine Richtlinien aufgeführt.

1.	Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**.
2.	Suchen Sie nach einem Link, mit dem Sie DNS-Einträge hinzufügen oder bearbeiten können. Dieser Link kann die Bezeichnung **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced** (Erweitert) haben.
3.  Erstellen Sie den Eintrag, und speichern Sie die Änderungen.
    - [Die Anleitung für einen A-Eintrag finden Sie hier](#a).
    - [Die Anleitung für einen CNAME-Eintrag finden Sie hier](#cname).

<a name="a"></a>
### Erstellen eines A-Eintrags

Wenn Sie der IP-Adresse Ihrer Azure-App einen A-Eintrag zuordnen möchten, müssen Sie sowohl einen A-Eintrag als auch einen TXT-Eintrag erstellen. Der A-Eintrag wird für die eigentliche DNS-Auflösung benötigt. Der TXT-Eintrag bestätigt gegenüber Azure, dass Sie der Besitzer des benutzerdefinierten Domänennamens sind.

Konfigurieren Sie Ihren A-Eintrag wie folgt (@ steht normalerweise für die Stammdomäne):
 
<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>Host</th>
    <th>Wert</th>
  </tr>
  <tr>
    <td>contoso.com (Stammdomäne)</td>
    <td>@</td>
    <td>IP-Adresse aus <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>www</td>
    <td>IP-Adresse aus <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>*.contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>IP-Adresse aus <a href="#vip">Schritt 1</a></td>
  </tr>
</table>

Für Ihren zusätzlichen TXT-Eintrag wird die Konvention der Zuordnung von „&lt;*Unterdomäne*>.&lt;*Stammdomäne*>“ zu „&lt;*App-Name*>.azurewebsites.net“ übernommen. Konfigurieren Sie Ihren TXT-Eintrag wie folgt:

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>TXT-Host</th>
    <th>TXT-Wert</th>
  </tr>
  <tr>
    <td>contoso.com (Stammdomäne)</td>
    <td>@</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>www</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>*.contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
### Erstellen eines CNAME-Eintrags

Wenn Sie einen CNAME-Eintrag für die Zuordnung zum Standarddomänennamen Ihrer Azure-App verwenden, benötigen Sie keinen zusätzlichen CNAME-Eintrag wie bei einem A-Eintrag.

>[AZURE.IMPORTANT] Erstellen Sie keinen CNAME-Eintrag für die Stammdomäne (d.h. den „Stammeintrag“). Weitere Informationen finden Sie unter [Why can't a CNAME record be used at the root domain](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain) (Warum kann ein CNAME-Eintrag nicht für die Stammdomäne verwendet werden?). Verwenden Sie stattdessen zum Zuordnen einer Stammdomäne zu Ihrer Azure-App einen [A-Eintrag](#a).

Konfigurieren Sie Ihren CNAME-Eintrag wie folgt (@ steht normalerweise für die Stammdomäne):

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>CNAME-Host</th>
    <th>CNAME-Wert</th>
  </tr>
  <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>www</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>*.contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
## Schritt 3: Aktivieren des benutzerdefinierten Domänennamens für Ihre App

Auf dem Blatt **Benutzerdefinierte Domänen** im Azure-Portal (siehe [Schritt 1](#vip)) müssen Sie der Liste den vollqualifizierten Domänennamen (Fully-Qualified Domain Name, FQDN) Ihrer benutzerdefinierten Domäne hinzufügen.

1.	Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, falls Sie dies noch nicht getan haben.

2.	Klicken Sie im Azure-Portal im linken Menü auf **App Services**.

3.	Klicken Sie auf Ihre App und anschließend auf **Benutzerdefinierte Domänen** > **Hostnamen hinzufügen**.

4.	Fügen Sie den FQDN der benutzerdefinierten Domäne der Liste hinzu (z.B. **www.contoso.com**).

    ![Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App: Hinzufügen zur Liste der Domänennamen](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure wird versucht, den Domänennamen zu überprüfen, den Sie hier verwenden. Stellen Sie sicher, dass es der gleiche Domänenname ist, für den Sie in [Schritt 2](#createdns) einen DNS-Eintrag erstellt haben.

5.  Klicken Sie auf **Überprüfen**.

6.  Nach dem Klicken auf **Überprüfen** startet Azure den Workflow zur Domänenüberprüfung. Dieser Workflow überprüft den Domänenbesitz sowie die Verfügbarkeit des Hostnamens und meldet die erfolgreiche Durchführung oder erstellt einen detaillierten Fehlerbericht mit Empfehlungen, wie der Fehler behoben werden kann.

7.  Nach erfolgreicher Überprüfung wird die Schaltfläche **Hostnamen hinzufügen** aktiv, und Sie können den Hostnamen zuweisen. Navigieren Sie in einem Browser zu Ihrer benutzerdefinierten Domäne. Ihre App sollte nun mit Ihrem benutzerdefinierten Domänennamen ausgeführt werden.

8.  Nachdem Azure Ihren neuen benutzerdefinierten Domänennamen konfiguriert hat, können Sie in einem Browser zum benutzerdefinierten Domänennamen navigieren. Der Browser sollte Ihre Azure-App öffnen. Das bedeutet, dass der benutzerdefinierte Domänenname richtig konfiguriert wurde.

> [AZURE.NOTE] Wenn der DNS-Eintrag bereits verwendet wird (wie beispielsweise in einem Szenario mit einer aktiven Domäne, die Datenverkehr bereitstellt) und Sie zur Domänenüberprüfung Ihre Web-App präemptiv an den Eintrag binden müssen, erstellen Sie einfach TXT-Einträge (siehe Bespiele in der folgenden Tabelle). Für Ihren zusätzlichen TXT-Eintrag wird die Konvention der Zuordnung von „&lt;*Unterdomäne*>.&lt;*Stammdomäne*>“ zu „&lt;*App-Name*>.azurewebsites.net“ übernommen.
> <table cellspacing="0" border="1">
>   <tr>
>     <th>FQDN-Beispiel</th>
>     <th>TXT-Host</th>
>     <th>TXT-Wert</th>
>   </tr>
>   <tr>
>     <td>contoso.com (Stammdomäne)</td>
>     <td>awverify.contoso.com</td>
>     <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
>   </tr>
>   <tr>
>     <td>www.contoso.com (Unterdomäne)</td>
>     <td>awverify.www.contoso.com</td>
>     <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
>   </tr>
>     <tr>
>     <td>*.contoso.com (Unterdomäne)</td>
>     <td>awverify.*.contoso.com</td>
>     <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
>   </tr>
> </table>
Wechseln Sie nach der Erstellung dieses DNS-Eintrags zurück zum Azure-Portal, und fügen Sie den benutzerdefinierten Domänennamen zu Ihrer Web-App hinzu.
 

<a name="verify"></a>
## Überprüfen der DNS-Verteilung

Es kann nach Abschluss der Konfigurationsschritte einige Zeit dauern, bis die Änderungen übernommen wurden – dies hängt von Ihrem DNS-Anbieter ab. Mit [http://digwebinterface.com/](http://digwebinterface.com/) können Sie überprüfen, ob die DNS-Verteilung erwartungsgemäß funktioniert. Geben Sie nach dem Navigieren zu der Website die Hostnamen in das Textfeld ein, und klicken Sie auf **Dig**. Überprüfen Sie anhand der Ergebnisse, ob die aktuellen Änderungen übernommen wurden.

![Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App: Überprüfen der DNS-Verteilung](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Die Verteilung von DNS-Einträgen kann bis zu 48 Stunden (manchmal mehr) dauern. Wenn alles richtig konfiguriert wurde, müssen Sie trotzdem noch warten, bis die Verteilung durchgeführt wurde.

## Nächste Schritte
Erfahren Sie, wie Sie Ihren benutzerdefinierten Domänennamen mit HTTPS sichern, indem Sie [ein SSL-Zertifikat in Azure erwerben](web-sites-purchase-ssl-web-site.md) oder [ein SSL-Zertifikat von einer anderen Stelle verwenden](web-sites-configure-ssl-certificate.md).

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

[Erste Schritte mit Azure DNS](../dns/dns-getstarted-create-dnszone.md) [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](../dns/dns-web-sites-custom-domain.md) [Delegieren von Domänen an Azure DNS](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png

<!---HONumber=AcomDC_0907_2016-->