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
	ms.date="07/26/2016"
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
1. [Aktivieren Sie den benutzerdefinierten Domänennamen für Ihre Azure-App](#enable).
    - **Wo**: [Azure-Portal](https://portal.azure.com).
    - **Warum**: Damit Ihre App weiß, wie sie auf Anforderungen reagieren soll, die an den benutzerdefinierten Domänennamen gestellt werden.
2. [Erstellen Sie die DNS-Einträge, mit denen die Domäne der App zugeordnet wird](#dns).
    - **Wo**: Eigenes Verwaltungstool der Domänenregistrierungsstelle (z.B. Azure DNS, www.godaddy.com usw.).
    - **Warum**: Damit die Domänenregistrierungsstelle weiß, wie die gewünschte benutzerdefinierte Domäne für Ihre Azure-App aufgelöst wird.
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

<a name="vip"></a>
## Schritt 1: *(Nur Ressourceneintrag)* Beschaffen der IP-Adresse der App
Sie benötigen die IP-Adresse Ihrer Azure-App, um einen benutzerdefinierten Domänennamen mit einem A-Eintrag zuzuordnen. Wenn Sie für die Zuordnung einen CNAME-Eintrag verwenden, können Sie diesen Schritt überspringen und mit dem nächsten Abschnitt fortfahren.

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.	Klicken Sie im linken Menü auf **App Services**.
4.	Klicken Sie auf Ihre App und dann auf **Einstellungen** > **Benutzerdefinierte Domänen und SSL** > **Externe Domänen einreichen**.
5.	Geben Sie unter **Domänennamen** Ihren benutzerdefinierten Domänennamen ein.
6.  Notieren Sie sich die IP-Adresse zur späteren Verwendung.
7.  Lassen Sie dieses Blatt des Portals geöffnet. Sie kehren zu diesem Blatt zurück, wenn Sie die DNS-Einträge erstellen.

<a name="dns"></a>
## Schritt 2: Erstellen von DNS-Einträgen

Melden Sie sich bei Ihrer Domänenregistrierungsstelle an, und verwenden Sie die verfügbaren Tools, um einen A-Eintrag oder einen CNAME-Eintrag zu erstellen. Da sich die Benutzeroberflächen der einzelnen Registrierungsstellen leicht unterscheiden, ist es ratsam, jeweils in der Dokumentation des Anbieters nachzulesen. Hier sind aber bereits einige allgemeine Richtlinien aufgeführt.

1.	Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link, der Sie zu Ihren eigenen Domänen führt.
2.	Suchen Sie nach einem Link, mit dem Sie DNS-Einträge hinzufügen oder bearbeiten können. Dieser Link kann die Bezeichnung **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced** (Erweitert) haben.
3.  Erstellen Sie den Eintrag, und speichern Sie die Änderungen.
    - [Die Anleitung für einen A-Eintrag finden Sie hier](#a).
    - [Die Anleitung für einen CNAME-Eintrag finden Sie hier](#cname).

<a name="a"></a>
### Erstellen eines A-Eintrags

Zum Verwenden eines A-Eintrags für die Zuordnung zur IP-Adresse Ihrer Azure-App müssen Sie sowohl einen A-Eintrag als auch einen CNAME-Eintrag erstellen. Der A-Eintrag gilt für die eigentliche DNS-Auflösung, und der CNAME-Eintrag gilt für Azure, um zu bestätigen, dass sich der benutzerdefinierte Domänenname in Ihrem Besitz befindet.

Der A-Eintrag sollte wie folgt konfiguriert werden (@ steht normalerweise für die Stammdomäne):
 
<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>Host/Name/Hostname</th>
    <th>Wert</th>
  </tr>
  <tr>
    <td>contoso.com (Stammdomäne)</td>
    <td>@</td>
    <td>IP-Adresse aus [Schritt 1]()</td>
  </tr>
  <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>www</td>
    <td>IP-Adresse aus [Schritt 1]()</td>
  </tr>
  <tr>
    <td>*.contoso.com (Platzhalter)</td>
    <td>*</td>
    <td>IP-Adresse aus [Schritt 1]()</td>
  </tr>
</table>

Für Ihren zusätzlichen CNAME-Eintrag wird die Konvention für die Zuordnung von „awverify.&lt;*Unterdomäne*>.&lt;*Stammdomäne*>“ zu „awverify.&lt;*Unterdomäne*>.azurewebsites.net“ übernommen. Unten sind hierzu Beispiele aufgeführt:

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>Host/Name/Hostname</th>
    <th>Wert</th>
  </tr>
  <tr>
    <td>contoso.com (Stammdomäne)</td>
    <td>awverify</td>
    <td>awverify.&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (Unterdomäne)</td>
    <td>awverify.www</td>
    <td>awverify.www.&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
  <tr>
    <td>*.contoso.com (Platzhalter)</td>
    <td>awverify</td>
    <td>awverify.&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
### Erstellen eines CNAME-Eintrags

Wenn Sie einen CNAME-Eintrag für die Zuordnung zum Standarddomänennamen Ihrer Azure-App verwenden, benötigen Sie keinen zusätzlichen CNAME-Eintrag wie bei einem A-Eintrag.

Der CNAME-Eintrag sollte wie folgt konfiguriert werden (@ steht normalerweise für die Stammdomäne):

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Beispiel</th>
    <th>Host/Name/Hostname</th>
    <th>Wert</th>
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


>[AZURE.NOTE] Die erforderlichen Domäneneinträge für Ihre Web-App können mit Azure DNS gehostet werden. Informationen zum Konfigurieren Ihrer benutzerdefinierten Domäne sowie zum Erstellen Ihrer Einträge in Azure DNS finden Sie unter [Erstellen von benutzerdefinierten DNS-Einträgen für eine Web-App](../dns/dns-web-sites-custom-domain.md).

<a name="enable"></a>
## Schritt 3: Aktivieren des benutzerdefinierten Domänennamens für Ihre App

Auf dem Blatt **Externe Domänen einreichen** im Azure-Portal (siehe [Schritt 1](#vip)) müssen Sie den vollqualifizierten Domänennamen (FQDN) Ihrer benutzerdefinierten Domäne der Liste hinzufügen.

1.	Navigieren Sie zurück zum Blatt **Externe Domänen einreichen** im Azure-Portal.

2.	Fügen Sie den FQDN der benutzerdefinierten Domäne der Liste hinzu (z.B. **www.contoso.com**).

    >[AZURE.NOTE] Azure versucht, den hier verwendeten Domänennamen zu bestätigen. Es muss sich also um denselben Domänennamen handeln, für den Sie in [Schritt 2](#dns) einen DNS-Eintrag erstellt haben. Wenn Sie sich sicher sind, dass

6.  Klicken Sie auf **Speichern**.

7.  Nachdem der neue benutzerdefinierte Domänenname erfolgreich konfiguriert wurde, können Sie in einem Browser zum benutzerdefinierten Domänennamen navigieren. Sie sollten nun die Ausführung Ihrer App verfolgen können, und

<a name="verify"></a>
## Überprüfen der DNS-Verteilung

Es kann nach Abschluss der Konfigurationsschritte einige Zeit dauern, bis die Änderungen übernommen wurden – dies hängt von Ihrem DNS-Anbieter ab. Mit [http://digwebinterface.com/](http://digwebinterface.com/) können Sie überprüfen, ob die DNS-Verteilung erwartungsgemäß funktioniert. Geben Sie nach dem Navigieren zu der Website die Hostnamen in das Textfeld ein, und klicken Sie auf **Dig**. Überprüfen Sie anhand der Ergebnisse, ob die aktuellen Änderungen übernommen wurden.

![](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Die Verteilung von DNS-Einträgen kann bis zu 48 Stunden (manchmal mehr) dauern. Wenn alles richtig konfiguriert wurde, müssen Sie trotzdem noch warten, bis die Verteilung durchgeführt wurde.

## Nächste Schritte

Weitere Informationen finden Sie unter [Erste Schritte mit Azure DNS](../dns/dns-getstarted-create-dnszone.md) sowie unter [Delegieren von Domänen an Azure DNS](../dns/dns-domain-delegation.md).

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.


<!-- Anchors. -->
[Overview]: #overview
[DNS record types]: #dns-record-types
[Find the virtual IP address]: #find-the-virtual-ip-address
[Create the DNS records]: #create-the-dns-records
[Enable the domain name on your web app]: #enable-the-domain-name-on-your-web-app

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png

<!---HONumber=AcomDC_0727_2016-->