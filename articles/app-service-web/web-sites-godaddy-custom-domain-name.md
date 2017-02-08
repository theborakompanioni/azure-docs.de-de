---
title: "Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service (GoDaddy)"
description: "Erfahren Sie, wie Sie einen Domänennamen von GoDaddy mit Azure-Web-Apps verwenden"
services: app-service
documentationcenter: 
author: erikre
manager: wpickett
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: fe8b4b097f7ff157d624c09b8dbf26f94401395c


---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Konfigurieren eines benutzerdefinierten (direkt von GoDaddy erworbenen) Domänennamens in Azure App Service
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Wenn Sie eine Domäne über Azure App Service-Web-Apps erworben haben, lesen Sie den letzten Schritt zum [Erwerben einer Domäne für Web-Apps](custom-dns-web-site-buydomains-web-app.md).

Dieser Artikel enthält Anweisungen zur Verwendung eines benutzerdefinierten Domänennamens, der mit [App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714) direkt bei [GoDaddy](https://godaddy.com) erworben wurde.

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Interpretation von DNS-Datensätzen
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Hinzufügen eines DNS-Datensatzes zu Ihrer benutzerdefinierten Domäne
Um Ihre benutzerdefinierte Domäne mit einer Web-App im App Service zu verknüpfen, müssen Sie einen neuen Eintrag in die DNS-Tabelle für Ihre benutzerdefinierte Domäne einfügen. Verwenden Sie hierzu die von GoDaddy bereitgestellten Tools. Mit den folgenden Schritten können Sie die DNS-Tools für GoDaddy.com abrufen.

1. Melden Sie sich bei Ihrem Konto auf „GoDaddy.com“ an, und wählen Sie **My Account** (Mein Konto) und dann **Manage my domains** (Meine Domänen verwalten) aus. Wählen Sie abschließend das Dropdownmenü für den Domänennamen, den Sie mit Ihrer Azure-Web-App verwenden möchten, und anschließend **DNS verwalten**aus.
   
    ![Benutzerdefinierte Domänenseite für GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. Rufen Sie auf der Seite **Domain details** (Domänendetails) die Registerkarte **DNS Zone File** (DNS-Zonendatei) auf. Dieser Abschnitt wird verwendet, um die DNS-Datensätze für die Domäne hinzuzufügen und zu ändern.
   
    ![Registerkarte der DNS-Zonendatei](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Wählen Sie **Add Record** aus, um einen vorhandenen Datensatz hinzuzufügen.
   
    Um einen vorhandenen Datensatz zu **bearbeiten**, wählen Sie das Stift- und Papiersymbol neben dem Datensatz aus.
   
   > [!NOTE]
   > Beachten Sie vor dem Hinzufügen von neuen Datensätzen, dass GoDaddy bereits DNS-Einträge für gängige Stammdomänen (im Editor als **Host** bezeichnet) erstellt hat, z.B. **email**, **files**, **mail** und andere. Wenn der Name, den Sie verwenden möchten, bereits vorhanden ist, bearbeiten Sie den vorhandenen Datensatz, statt einen neuen zu erstellen.
   > 
   > 
3. Beim Hinzufügen eines Datensatzes müssen Sie zuerst den Datensatztyp auswählen.
   
    ![Datensatztyp auswählen](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Als Nächstes müssen Sie den **Host** (die benutzerdefinierte Domäne oder Unterdomäne) bereitstellen und angeben, worauf dieser verweist (unter **Points to** (Verweist auf)).
   
    ![Zonendatensatz hinzufügen](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * Wenn Sie einen Eintrag vom Typ **A (Host)** hinzufügen, müssen Sie das Feld **Host** entweder auf **@** (Stammdomänenname wie **contoso.com**), auf * (Platzhalter für mehrere Unterdomänen) oder auf die gewünschte Unterdomäne (etwa **www.**) festlegen. Das Feld **Points to** (Verweist auf) muss auf die IP-Adresse Ihrer Azure-Web-App festgelegt werden.
   * Wenn Sie einen **CNAME (Alias)**-Eintrag hinzufügen, müssen Sie im Feld **Host** die gewünschte Unterdomäne festlegen. Beispiel: **www**. Sie müssen das Feld **Points to** (Verweist auf) auf den **.azurewebsites.net**-Domänennamen Ihrer Azure-Web-App festlegen. Beispiel: **contoso.azurewebsites.net**.
4. Klicken Sie auf **Weiteren hinzufügen**.
5. Wählen Sie als Eintragstyp die Option **TXT** aus, und geben Sie als Hostwert**** das Zeichen **@** und unter **Points to** (Verweist auf) den Wert **&lt;Name Ihrer Web-App&gt;.azurewebsites.net** ein.
   
   > [!NOTE]
   > Anhand dieses TXT-Eintrags prüft Azure, ob Sie der Besitzer der Domäne sind, die von dem A-Eintrag oder vom ersten TXT-Eintrag beschrieben wird. Nachdem die Domäne der Web-App im Azure-Portal zugeordnet wurde, kann der TXT-Eintrag entfernt werden.
   > 
   > 
6. Nachdem Sie die gewünschten Datensätze hinzugefügt oder bearbeitet haben, klicken Sie auf **Finish** , um die Änderungen zu speichern.

<a name="enabledomain"></a>

## <a name="enable-the-domain-name-on-your-web-app"></a>Aktivieren des Domänennamens auf Ihrer Web-App
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)




<!--HONumber=Jan17_HO3-->


