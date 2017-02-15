---
title: Aktivieren von Transparent Data Encryption in Azure Security Center | Microsoft Docs
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Aktivieren von Transparent Data Encryption“ umsetzen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6944454190d2744cd7cd9a25d9876c5c7427b5f8


---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Aktivieren von Transparent Data Encryption in Azure Security Center
Azure Security Center empfiehlt das Aktivieren von Transparent Data Encryption (TDE) für SQL-Datenbanken, sofern noch nicht erfolgt. TDE schützt Ihre Daten und hilft Ihnen beim Erfüllen von Complianceanforderungen, indem Ihre Datenbank, dazugehörige Sicherungen und ruhende Transaktionsprotokolldateien verschlüsselt werden, ohne das Änderungen an Ihrer Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096).

Diese Empfehlung gilt für nur den Azure SQL-Dienst und nicht für auf Ihren virtuellen Computern ausgeführtes SQL Server.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.
> 
> 

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Transparent Data Encryption aktivieren** aus.
   ![Aktivieren von Transparent Data Encryption][1]
2. Das Blatt **Transparent Data Encryption für SQL-Datenbanken aktivieren** wird geöffnet. Wählen Sie eine SQL-Datenbank aus, für die TDE aktiviert werden soll.
   ![Auswählen einer SQL-Datenbank, um TDE zu aktivieren][2]
3. Wählen Sie auf dem Blatt **Transparent Data Encryption** unter „Datenverschlüsselung“ **EIN** und dann auf dem oberen Menüband des Blatts **Speichern** aus.
   ![Aktivieren von TDE][3]
   
   Nach der Aktivierung von TDE für die ausgewählte SQL-Datenbank ändert sich **Verschlüsselungsstatus** in **Verschlüsselt**.    
   
   ![Verschlüsselungsstatus][4]

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Aktivieren von Transparent Data Encryption“ umsetzen. Weitere Informationen zu TDE für SQL finden Sie in folgenden Artikeln:

* [Transparent Data Encryption mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096)
* [Erste Schritte mit Transparent Data Encryption (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png



<!--HONumber=Nov16_HO3-->


