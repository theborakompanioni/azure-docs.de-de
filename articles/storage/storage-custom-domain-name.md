---
title: "Konfigurieren eines Domänennamens für Ihren Blobspeicher-Endpunkt | Microsoft Docs"
description: "Erfahren Sie, wie Sie im klassischen Azure-Portal eine benutzerdefinierte Benutzerdomäne dem Blobspeicher-Endpunkt für ein Azure Speicherkonto zuordnen."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: fe4b9c356e5f7d56cb7e1fa62344095353d0b699
ms.openlocfilehash: 65ecd654cf6f1558a8f9ad6d6718ae5396fb52d0


---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt
## <a name="overview"></a>Übersicht
Sie können eine benutzerdefinierte Domäne für den Zugriff auf Blob-Daten in Ihrem Azure-Speicherkonto konfigurieren. Der Standardendpunkt für den Blobspeicher ist `<storage-account-name>.blob.core.windows.net`. Wenn Sie dem Blobendpunkt für Ihr Speicherkonto eine benutzerdefinierte Domäne und Unterdomäne wie z.B. **www.contoso.com** zuordnen, können Benutzer auch unter Verwendung dieser Domäne auf Blobdaten in Ihrem Speicherkonto zugreifen.

> [!IMPORTANT]
> Azure Storage unterstützt noch nicht HTTPS für benutzerdefinierte Domänen. Uns ist bekannt, dass Kunden an diesem Feature interessiert sind. Es wird daher in einer zukünftigen Version verfügbar sein.
>
>

Es gibt zwei Methoden, um Ihre benutzerdefinierte Domäne auf den Blob-Endpunkt für Ihr Speicherkonto zu verweisen. Die einfachste Methode besteht darin, einen CNAME-Datensatz zu erstellen, der Ihre benutzerdefinierte Domäne und Unterdomäne dem Blob-Endpunkt zuordnet. Ein CNAME-Datensatz ist eine DNS-Funktion, die eine Quelldomäne einer Zieldomäne zuordnet. In diesem Fall ist die Quelldomäne Ihre benutzerdefinierte Domäne und Unterdomäne (beachten Sie, dass die Unterdomäne immer erforderlich ist). Die Zieldomäne ist der Blob-Dienstendpunkt.

Die Zuordnung der benutzerdefinierten Domäne zum Blob-Endpunkt kann jedoch zu einer kurzzeitigen Downtime für die Domäne führen, während Sie die Domäne im [klassischen Azure-Portal](https://manage.windowsazure.com)registrieren. Wenn Ihre benutzerdefinierte Domäne derzeit eine Anwendung mit einer Vereinbarung zum Servicelevel (SLA) unterstützt, die keine Downtime zulässt, können Sie die Azure-Unterdomäne **asverify** verwenden, um einen Registrierungs-Zwischenschritt bereitzustellen, sodass Benutzer auf die Domäne zugreifen können, während die DNS-Zuordnung stattfindet.

Die folgende Tabelle enthält Beispiel-URLs für den Zugriff auf Blob-Daten in einem Speicherkonto namens **mystorageaccount**. Die für das Speicherkonto registrierte benutzerdefinierte Domäne ist **www.contoso.com**:

| Ressourcentyp | URL-Formate |
| --- | --- |
| Speicherkonto |**Standard-URL:** http://mystorageaccount.blob.core.windows.net<p>**URL der benutzerdefinierten Domäne:** http://www.contoso.com</td> |
| Blob |**Standard-URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**URL der benutzerdefinierten Domäne:** http://www.contoso.com/mycontainer/myblob |
| Stammcontainer |**Standard-URL:** http://mystorageaccount.blob.core.windows.net/myblob oder http://mystorageaccount.blob.core.windows.net/$root/myblob<p>**URL der benutzerdefinierten Domäne:** http://www.contoso.com/myblob or http://www.contoso.com/$root/myblob |

## <a name="register-a-custom-domain-for-your-storage-account"></a>Registrieren einer benutzerdefinierten Domäne für ein Speicherkonto
Gehen Sie nach diesem Verfahren vor, um Ihre benutzerdefinierte Domäne zu registrieren, wenn es unerheblich ist, ob die Domäne Benutzern kurzzeitig nicht zur Verfügung steht, oder wenn die benutzerdefinierte Domäne derzeit keine Anwendung hostet.

Wenn Ihre benutzerdefinierte Domäne derzeit eine Anwendung unterstützt, die keine Ausfallzeiten zulässt, gehen Sie nach dem unter <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">Registrieren einer benutzerdefinierten Domäne für ein Speicherkonto unter Verwendung der Zwischenunterdomäne "asverify"</a> beschriebenen Verfahren vor.

Zum Konfigurieren eines benutzerdefinierten Domänennamens müssen Sie einen neuen CNAME-Datensatz bei der Domänenregistrierung erstellen. Der CNAME-Datensatz gibt einen Alias für einen Domänennamen an. In diesem Fall ordnet er die Adresse der benutzerdefinierten Domäne dem Blob Storage-Endpunkt für Ihr Speicherkonto zu.

Die Methoden zum Festlegen eines CNAME-Datensatzes der verschiedenen Registrierungen sind zwar ähnlich, jedoch unterscheiden sie sich auch in einigen Punkten. Das Konzept ist allerdings gleich. Beachten Sie, dass viele Basispakete für die Domänenregistrierung keine DNS-Konfiguration anbieten, daher müssen Sie möglicherweise Ihr Domänenregistrierungspaket aktualisieren, bevor Sie den CNAME-Datensatz erstellen können.

1. Navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)zur Registerkarte **Speicher** .
2. Klicken Sie auf der Registerkarte **Speicher** auf den Namen des Speicherkontos, für das Sie die benutzerdefinierte Domäne zuordnen möchten.
3. Klicken Sie auf die Registerkarte **Configure** .
4. Klicken Sie am unteren Rand des Bildschirms auf **Domäne verwalten**, um das Dialogfeld **Benutzerdefinierte Domäne verwalten** anzuzeigen. Oben im Dialogfeld wird ein Text mit Anweisungen zum Erstellen des CNAME-Datensatzes angezeigt. Ignorieren Sie für dieses Verfahren den Text, der sich auf die Unterdomäne **asverify** bezieht.
5. Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle an, und öffnen Sie die Seite für die DNS-Verwaltung. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
6. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Möglicherweise müssen Sie eine Seite mit erweiterten Einstellungen aufrufen und nach den Wörtern **CNAME**, **Alias** oder **Unterdomänen** suchen.
7. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Unterdomänenalias wie etwa **www** oder **photos** an. Geben Sie dann einen Hostnamen (Ihr Blob-Dienstendpunkt) im Format **mystorageaccount.blob.core.windows.net** an (hierbei ist **mystorageaccount** der Name Ihres Speicherkontos). Der zu verwendende Hostname wird im Text des Dialogfelds **Benutzerdefinierte Domäne verwalten** bereitgestellt.
8. Nachdem Sie den CNAME-Eintrag erstellt haben, kehren Sie zum Dialogfeld **Benutzerdefinierte Domäne verwalten** zurück, und geben Sie den Namen Ihrer benutzerdefinierten Domäne einschließlich Unterdomäne in das Feld **Benutzerdefinierter Domänenname** ein. Beispiel: Wenn die Domäne **contoso.com** und die Unterdomäne **www** lautet, geben Sie **www.contoso.com** ein. Wenn die Unterdomäne **photos** lautet, geben Sie **photos.contoso.com** ein. Beachten Sie, dass die Unterdomäne erforderlich ist.
9. Klicken Sie auf die Schaltfläche **Registrieren** , um die benutzerdefinierte Domäne zu registrieren.

Bei erfolgreicher Registrierung wird die Meldung **Ihre benutzerdefinierte Domäne ist aktiv**angezeigt. Benutzer können jetzt Blob-Daten in Ihrer benutzerdefinierten Domäne anzeigen, sofern sie über die entsprechenden Berechtigungen verfügen.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Registrieren einer benutzerdefinierten Domäne für ein Speicherkonto unter Verwendung der Zwischenunterdomäne "asverify"
Gehen Sie nach diesem Verfahren vor, um Ihre benutzerdefinierte Domäne zu registrieren, wenn diese derzeit eine Anwendung mit einer SLA unterstützt, die keine Downtime zulässt. Indem Sie einen CNAME-Eintrag erstellen, der von „asverify.&lt;Unterdomäne&gt;.&lt;benutzerdefinierteDomäne&gt; auf „asverify.&lt;Speicherkonto&gt;.blob.core.windows.net“ verweist, können Sie Ihre Domäne bei Azure vorab registrieren. Anschließend können Sie einen zweiten CNAME-Eintrag erstellen, der von „&lt;Unterdomäne&gt;.&lt;benutzerdefinierteDomäne&gt;“ auf „&lt;Speicherkonto&gt;.blob.core.windows.net“ verweist. An diesem Punkt wird Datenverkehr zu Ihrer benutzerdefinierten Domäne an Ihren Blobendpunkt geleitet.

Die Unterdomäne "asverify" ist eine spezielle Unterdomäne, die von Azure erkannt wird. Indem Sie Ihrer eigenen Unterdomäne **asverify** voranstellen, erlauben Sie Azure, Ihre benutzerdefinierte Domäne zu erkennen, ohne den DNS-Datensatz für die Domäne zu ändern. Sobald Sie den DNS-Datensatz für die Domäne ändern, wird diese dem Blob-Endpunkt ohne Downtime zugeordnet.

1. Navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)zur Registerkarte **Speicher** .
2. Klicken Sie auf der Registerkarte **Speicher** auf den Namen des Speicherkontos, für das Sie die benutzerdefinierte Domäne zuordnen möchten.
3. Klicken Sie auf die Registerkarte **Configure** .
4. Klicken Sie am unteren Rand des Bildschirms auf **Domäne verwalten**, um das Dialogfeld **Benutzerdefinierte Domäne verwalten** anzuzeigen. Oben im Dialogfeld wird ein Text mit Anweisungen zum Erstellen des CNAME-Datensatzes mit der Unterdomäne **asverify** angezeigt.
5. Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle an, und öffnen Sie die Seite für die DNS-Verwaltung. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
6. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Möglicherweise müssen Sie eine Seite mit erweiterten Einstellungen aufrufen und nach den Wörtern **CNAME**, **Alias** oder **Unterdomänen** suchen.
7. Erstellen Sie einen neuen CNAME-Datensatz, und geben Sie einen Unterdomänenalias an, der die Unterdomäne "asverify" enthält. Die angegebene Unterdomäne kann beispielsweise das Format **asverify.www** oder **asverify.photos** haben. Geben Sie dann einen Hostnamen (Ihr Blob-Dienstendpunkt) im Format **asverify.mystorageaccount.blob.core.windows.net** an (hierbei ist **mystorageaccount** der Name Ihres Speicherkontos). Der zu verwendende Hostname wird im Text des Dialogfelds **Benutzerdefinierte Domäne verwalten** bereitgestellt.
8. Nachdem Sie den CNAME-Eintrag erstellt haben, kehren Sie zum Dialogfeld **Benutzerdefinierte Domäne verwalten** zurück, und geben Sie den Namen Ihrer benutzerdefinierten Domäne in das Feld **Benutzerdefinierter Domänenname** ein. Beispiel: Wenn die Domäne **contoso.com** und die Unterdomäne **www** lautet, geben Sie **www.contoso.com** ein. Wenn die Unterdomäne **photos** lautet, geben Sie **photos.contoso.com** ein. Beachten Sie, dass die Unterdomäne erforderlich ist.
9. Klicken Sie auf das Kontrollkästchen **Erweitert: Verwenden Sie die Unterdomäne 'asverify', um meine benutzerdefinierte Domäne zu registrieren**.
10. Klicken Sie auf die Schaltfläche **Registrieren** , um Ihre benutzerdefinierte Domäne vorzuregistrieren.

    Bei erfolgreicher Vorregistrierung wird die Meldung **Ihre benutzerdefinierte Domäne ist aktiv**angezeigt.
11. An diesem Punkt wurde Ihre benutzerdefinierte Domäne von Azure überprüft, allerdings wird der Datenverkehr zu Ihrer Domäne noch nicht zu Ihrem Speicherkonto geleitet. Um den Vorgang anzuschließen, kehren Sie zur Website der DNS-Registrierungsstelle zurück und erstellen einen weiteren CNAME-Eintrag, um Ihre Unterdomäne zum Endpunkt Ihres Blob-Diensts zuzuordnen. Geben Sie etwa die Unterdomäne **www** oder **photos** und den Hostnamen **mystorageaccount.blob.core.windows.net** an (wobei **mystorageaccount** der Name Ihres Speicherkontos ist). Mit diesem Schritt ist die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen.
12. Abschließend können Sie den CNAME-Datensatz löschen, den Sie mit **asverify**erstellt haben, da er nur als Zwischenschritt benötigt wurde.

Benutzer können jetzt Blob-Daten in Ihrer benutzerdefinierten Domäne anzeigen, sofern sie über die entsprechenden Berechtigungen verfügen.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Überprüfen, ob die benutzerdefinierte Domäne auf den Blob-Dienstendpunkt verweist
Um zu überprüfen, ob Ihre benutzerdefinierte Domäne tatsächlich Ihrem Blob-Dienstendpunkt zugeordnet ist, erstellen Sie einen Blob in einem öffentlichen Container in Ihrem Speicherkonto. Greifen Sie anschließend in einem Webbrowser mit einem URI im folgenden Format auf den Blob zu:

    http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Sie können beispielsweise den folgenden URI verwenden, um über die benutzerdefinierte Unterdomäne **photos.contoso.com**, die einem Blob im Container **myforms** zugeordnet ist, auf ein Webformular zuzugreifen:

    http://photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Aufheben der Registrierung einer benutzerdefinierten Domäne in Ihrem Speicherkonto
Um die Registrierung einer benutzerdefinierten Domäne aufzuheben, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie im Navigationsbereich auf **Speicher**.
3. Klicken Sie auf der Seite **Speicher** auf den Namen des Speicherkontos, um das Dashboard anzuzeigen.
4. Klicken Sie im Menüband auf **Domäne verwalten**.
5. Klicken Sie im Dialogfeld **Benutzerdefinierte Domäne verwalten** auf **Registrierung aufheben**.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Zuordnen einer benutzerdefinierten Domäne zu einem CDN-Endpunkt (Content Delivery Network, Netzwerk für die Inhaltsübermittlung)](../cdn/cdn-map-content-to-custom-domain.md)




<!--HONumber=Nov16_HO4-->


