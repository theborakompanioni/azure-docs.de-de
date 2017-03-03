---
title: "Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App"
description: "Es wird beschrieben, wie Sie Ihrer App in Azure App Service einen benutzerdefinierten Domänennamen (Vanity-Domäne) zuordnen."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 48644a39-107c-45fb-9cd3-c741974ff590
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 59565c22ecd42985e8a6b81c4983fc2e87637e36
ms.openlocfilehash: 589701270770494e4ec4d127a252712249da9f3a
ms.lasthandoff: 02/16/2017


---
# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen manuell der Web-App, dem Mobile App-Back-End oder der API-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md)zuordnen. 

> [!NOTE] 
> Sie können immer nur [einen benutzerdefinierten Domänennamen direkt über Azure erwerben](custom-dns-web-site-buydomains-web-app.md).
>
>

Es gibt drei grundlegende Schritte für die Zuordnung der benutzerdefinierten Domäne zu Ihrer App:

1. [*(Nur A-Datensatz)* Rufen Sie die IP-Adresse der App ab](#vip).
2. [Erstellen Sie die DNS-Einträge, mit denen die Domäne der App zugeordnet wird.](#createdns) 
   * **Wo:** Eigenes Verwaltungstool der Domänenregistrierungsstelle (z.B. Azure DNS, GoDaddy usw.)
   * **Warum:**Damit die Domänenregistrierungsstelle weiß, wie die gewünschte benutzerdefinierte Domäne für Ihre Azure-App aufgelöst wird.
3. [Aktivieren Sie den benutzerdefinierten Domänennamen für Ihre Azure-App.](#enable)
   * **Wo:**[Azure-Portal](https://portal.azure.com).
   * **Warum:**Damit Ihre App weiß, wie sie auf Anforderungen reagieren soll, die an den benutzerdefinierten Domänennamen gestellt werden.
4. [Überprüfen Sie die DNS-Verteilung](#verify).

## <a name="types-of-domains-you-can-map"></a>Typen von Domänen, die Sie zuordnen können
Mit Azure App Service können Sie die folgenden Kategorien von benutzerdefinierten Domänen Ihrer App zuordnen.

* **Stammdomäne:** Der Domänenname, den Sie bei der Domänenregistrierungsstelle reserviert haben (normalerweise durch den Hosteintrag `@` dargestellt). 
  Beispiel: **contoso.com**.
* **Unterdomäne** : Jede Domäne unterhalb Ihrer Stammdomäne. Beispiel: **www.contoso.com** (wird durch den Hosteintrag `www` dargestellt).  Sie können verschiedene Unterdomänen derselben Stammdomäne unterschiedlichen Apps in Azure zuordnen.
* **Platzhalterdomäne** - [Jede Domäne, deren ganz links stehende DNS-Bezeichnung `*`](https://en.wikipedia.org/wiki/Wildcard_DNS_record) lautet (z.B. die Hosteinträge `*` und `*.blogs`). Beispiel: **\*.contoso.com**.

## <a name="types-of-dns-records-you-can-use"></a>Typen von zulässigen DNS-Einträgen
Je nach Bedarf können Sie zwei unterschiedliche Arten von DNS-Standardeinträgen verwenden, um Ihre benutzerdefinierte Domäne zuzuordnen: 

* [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) : Ordnet Ihren benutzerdefinierten Domänennamen direkt der virtuellen IP-Adresse der Azure-App zu. 
* [CNAME:](https://en.wikipedia.org/wiki/CNAME_record) Ordnet Ihren benutzerdefinierten Domänennamen dem Azure-Domänennamen Ihrer App zu: **&lt;*App-Name*>.azurewebsites.net**. 

Der Vorteil von CNAME besteht darin, dass er auch bei Änderungen der IP-Adresse beibehalten wird. Die virtuelle IP-Adresse Ihrer App kann sich ändern, wenn Sie die App löschen und neu erstellen oder eine Änderung von einem höheren Tarif zurück auf den Tarif **Shared** vornehmen. Bei einer Änderung dieser Art ist ein CNAME-Eintrag weiterhin gültig, während ein A-Eintrag aktualisiert werden muss. 

Das Tutorial enthält die Schritte zur Verwendung des A-Eintrags und des CNAME-Eintrags.

> [!IMPORTANT]
> Erstellen Sie keinen CNAME-Eintrag für die Stammdomäne (d.h. den „Stammeintrag“). Weitere Informationen finden Sie unter [Why can't a CNAME record be used at the root domain](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain) (Warum kann ein CNAME-Eintrag nicht für die Stammdomäne verwendet werden?).
> Verwenden Sie stattdessen zum Zuordnen einer Stammdomäne zu Ihrer Azure-App einen A-Eintrag.
> 
> 

<a name="vip"></a>

## <a name="step-1-a-record-only-get-apps-ip-address"></a>Schritt 1: *(Nur A-Datensatz)* Rufen Sie die IP-Adresse der App ab.
Sie benötigen die IP-Adresse Ihrer Azure-App, um einen benutzerdefinierten Domänennamen mit einem A-Eintrag zuzuordnen. Wenn Sie für die Zuordnung einen CNAME-Eintrag verwenden, können Sie diesen Schritt überspringen und mit dem nächsten Abschnitt fortfahren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **App Services** .
3. Klicken Sie auf Ihre App und anschließend auf **Benutzerdefinierte Domänen**.
4. Notieren Sie sich die IP-Adresse über dem Hostnamenabschnitt:
   
   ![Zuordnen eines benutzerdefinierten Domänennamens mit A-Eintrag: Abrufen der IP-Adresse für Ihre Azure App Service-App](./media/web-sites-custom-domain-name/virtual-ip-address.png)
5. Lassen Sie dieses Blatt des Portals geöffnet. Sie kehren zu diesem Blatt zurück, wenn Sie die DNS-Einträge erstellen.

<a name="createdns"></a>

## <a name="step-2-create-the-dns-records"></a>Schritt 2: Erstellen von DNS-Einträgen
Melden Sie sich bei Ihrer Domänenregistrierungsstelle an, und verwenden Sie die verfügbaren Tools, um einen A-Eintrag oder einen CNAME-Eintrag zu erstellen. Da sich die Benutzeroberflächen der einzelnen Registrierungsstellen leicht unterscheiden, ist es ratsam, jeweils in der Dokumentation des Anbieters nachzulesen. Hier sind aber bereits einige allgemeine Richtlinien aufgeführt.

1. Suchen Sie die Seite für die Verwaltung von DSN-Einträgen. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**.
2. Suchen Sie nach einem Link, mit dem Sie DNS-Einträge hinzufügen oder bearbeiten können. Dieser Link kann die Bezeichnung **Zonendatei**, **DNS-Einträge** oder **Erweitert** haben.
3. Erstellen Sie den Eintrag, und speichern Sie die Änderungen.
   * [Die Anleitung für einen A-Eintrag finden Sie hier](#a).
   * [Die Anleitung für einen CNAME-Eintrag finden Sie hier](#cname).

<a name="a"></a>

### <a name="create-an-a-record"></a>Erstellen eines A-Eintrags
Wenn Sie der IP-Adresse Ihrer Azure-App einen A-Eintrag zuordnen möchten, müssen Sie sowohl einen A-Eintrag als auch einen TXT-Eintrag erstellen. Der A-Eintrag wird für die eigentliche DNS-Auflösung benötigt. Der TXT-Eintrag bestätigt gegenüber Azure, dass Sie der Besitzer des benutzerdefinierten Domänennamens sind. 

Konfigurieren Sie Ihren A-Datensatz wie folgt ((@ steht normalerweise für die Stammdomäne):

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
    <td>\*.contoso.com (Platzhalter)</td>
    <td>\*</td>
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
    <td>\*.contoso.com (Platzhalter)</td>
    <td>\*</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>

### <a name="create-a-cname-record"></a>Erstellen eines CNAME-Eintrags
Wenn Sie einen CNAME-Eintrag für die Zuordnung zum Standarddomänennamen Ihrer Azure-App verwenden, benötigen Sie keinen zusätzlichen TXT-Eintrag wie bei einem A-Eintrag. 

> [!IMPORTANT]
> Erstellen Sie keinen CNAME-Eintrag für die Stammdomäne (d.h. den „Stammeintrag“). Weitere Informationen finden Sie unter [Why can't a CNAME record be used at the root domain](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain) (Warum kann ein CNAME-Eintrag nicht für die Stammdomäne verwendet werden?).
> Verwenden Sie stattdessen zum Zuordnen einer Stammdomäne zu Ihrer Azure-App einen [A-Datensatz](#a).
> 
> 

Konfigurieren Sie Ihren CNAME-Eintrag wie folgt ((@ steht normalerweise für die Stammdomäne):

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
    <td>\*.contoso.com (Platzhalter)</td>
    <td>\*</td>
    <td>&lt;<i>App-Name</i>>.azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>

## <a name="step-3-enable-the-custom-domain-name-for-your-app"></a>Schritt 3: Aktivieren des benutzerdefinierten Domänennamens für Ihre App
Auf dem Blatt **Benutzerdefinierte Domänen** im Azure-Portal (siehe [Schritt 1](#vip)) müssen Sie der Liste den vollqualifizierten Domänennamen (Fully-Qualified Domain Name, FQDN) Ihrer benutzerdefinierten Domäne hinzufügen.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.
2. Klicken Sie im Azure-Portal im linken Menü auf **App Services** .
3. Klicken Sie auf Ihre App und anschließend auf **Benutzerdefinierte Domänen** > **Hostnamen hinzufügen**zuordnen.
4. Fügen Sie der Liste den FQDN der benutzerdefinierten Domäne hinzu (z.B. **www.contoso.com**).
   
   ![Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App: Hinzufügen zur Liste der Domänennamen](./media/web-sites-custom-domain-name/add-custom-domain.png)
   
   > [!NOTE]
   > Azure wird versucht, den Domänennamen zu überprüfen, den Sie hier verwenden. Stellen Sie sicher, dass es der gleiche Domänenname ist, für den Sie in [Schritt 2](#createdns)einen DNS-Eintrag erstellt haben. 
   > 
   > 
5. Klicken Sie auf **Überprüfen**.
6. Nach dem Klicken auf **Überprüfen** startet Azure den Workflow zur Domänenüberprüfung. Dieser Workflow überprüft den Domänenbesitz sowie die Verfügbarkeit des Hostnamens und meldet die erfolgreiche Durchführung oder erstellt einen detaillierten Fehlerbericht mit Empfehlungen, wie der Fehler behoben werden kann.    
7. Nach erfolgreicher Überprüfung wird die Schaltfläche **Hostnamen hinzufügen** aktiv, und Sie können den Hostnamen zuweisen. 
8. Nachdem Azure Ihren neuen benutzerdefinierten Domänennamen konfiguriert hat, können Sie in einem Browser zum benutzerdefinierten Domänennamen navigieren. Der Browser sollte Ihre Azure-App öffnen. Das bedeutet, dass der benutzerdefinierte Domänenname richtig konfiguriert wurde.

## <a name="migrate-an-active-domain-name"></a>Migrieren eines aktiven Domänennamens

Wenn der Domänenname, den Sie zuordnen möchten, bereits von einer vorhandenen Website verwendet wird und Sie Ausfallzeiten vermeiden möchten, lesen Sie unter [Migrieren einer aktiven benutzerdefinierten Domäne zu App Service](app-service-custom-domain-name-migrate.md) nach.

<a name="verify"></a>

## <a name="verify-dns-propagation"></a>Überprüfen der DNS-Verteilung
Es kann nach Abschluss der Konfigurationsschritte einige Zeit dauern, bis die Änderungen übernommen wurden – dies hängt von Ihrem DNS-Anbieter ab. Mit [http://digwebinterface.com/](http://digwebinterface.com/)können Sie überprüfen, ob die DNS-Verteilung erwartungsgemäß funktioniert. Geben Sie nach dem Navigieren zu der Website die Hostnamen in das Textfeld ein, und klicken Sie auf **Dig**. Überprüfen Sie anhand der Ergebnisse, ob die aktuellen Änderungen übernommen wurden.  

![Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App: Überprüfen der DNS-Verteilung](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [!NOTE]
> Die Verteilung von DNS-Einträgen kann bis zu 48 Stunden (manchmal mehr) dauern. Wenn alles richtig konfiguriert wurde, müssen Sie trotzdem noch warten, bis die Verteilung durchgeführt wurde.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Ihren benutzerdefinierten Domänennamen mit HTTPS sichern, indem Sie [ein SSL-Zertifikat in Azure erwerben](web-sites-purchase-ssl-web-site.md) oder [ein SSL-Zertifikat von einer anderen Stelle verwenden](web-sites-configure-ssl-certificate.md).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

[Erste Schritte mit Azure DNS](../dns/dns-getstarted-create-dnszone.md)  
[Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](../dns/dns-web-sites-custom-domain.md)  
[Delegieren von Domänen an Azure DNS](../dns/dns-domain-delegation.md)

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png

