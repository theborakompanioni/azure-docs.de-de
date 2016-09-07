<properties
   pageTitle="Erstellen eines Dienstprinzipals im Portal | Microsoft Azure"
   description="Beschreibt das Erstellen einer neuen Active Directory-Anwendung und eines Dienstprinzipals, der mit der rollenbasierten Zugriffskontrolle in Azure-Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="tomfitz"/>

# Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Bei Verwendung einer Anwendung, die auf Ressourcen zugreifen oder diese ändern muss, müssen Sie eine Active Directory-Anwendung (AD) einrichten und ihr die erforderlichen Berechtigungen zuweisen. In diesem Thema erfahren Sie, wie diese Schritte über das Portal ausgeführt werden. Derzeit müssen Sie das klassische Portal verwenden, um eine neue Active Directory-Anwendung zu erstellen, und dann zum Azure-Portal wechseln, um der Anwendung eine Rolle zuzuweisen.

> [AZURE.NOTE] Unter Umständen ist es für Sie einfacher, die AD-Anwendung und den Dienstprinzipal über die [PowerShell](resource-group-authenticate-service-principal.md) oder die [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md) einzurichten. Dies gilt besonders, wenn Sie für die Authentifizierung ein Zertifikat verwenden möchten. Dieses Thema beschreibt nicht die Verwendung eines Zertifikats.

Eine Erläuterung der Active Directory-Konzepte finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

## Erstellen einer Active Directory-Anwendung

1. Melden Sie sich über das [klassische Portal](https://manage.windowsazure.com/) bei Ihrem Azure-Konto an.

2. Stellen Sie sicher, dass Sie das Active Directory-Standardverzeichnis für Ihr Abonnement kennen. Sie können nur Zugriff für Anwendungen gewähren, die sich im selben Verzeichnis wie Ihr Abonnement befinden. Wählen Sie **Einstellungen** aus, und suchen Sie den Namen des mit Ihrem Abonnement verknüpften Verzeichnisses. Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Standardverzeichnis suchen](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Wählen Sie im linken Bereich **Active Directory** aus.

     ![Active Directory auswählen](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Wählen Sie das Active Directory aus, das Sie zum Erstellen der Anwendung nutzen möchten. Wenn Sie über mehr als eine Active Directory-Instanz verfügen, erstellen Sie die Anwendung im Standardverzeichnis für Ihr Abonnement.

     ![Verzeichnis wählen](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Wählen Sie **Anwendungen** aus, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.

     ![Anwendungen anzeigen](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Wenn Sie in diesem Verzeichnis bisher noch keine Anwendung erstellt haben, sollten Sie eine Anzeige ähnlich der folgenden sehen. Wählen Sie **EINE ANWENDUNG HINZUFÜGEN** aus.

     ![Anwendung hinzufügen](./media/resource-group-create-service-principal-portal/create-application.png)

     Oder klicken Sie im unteren Bereich auf **Hinzufügen**.

     ![Hinzufügen](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Wählen Sie den Anwendungstypen aus, den Sie erstellen möchten. Wählen Sie für dieses Tutorial **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen** aus.

     ![neue Anwendung](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Geben Sie einen Namen für die Anwendung ein, und wählen Sie den Typ der Anwendung aus, die Sie erstellen möchten. Erstellen Sie für dieses Tutorial eine **WEBANWENDUNG UND/ODER WEB-API**, und klicken Sie auf die Schaltfläche „Weiter“. Wenn Sie **NATIVE CLIENTANWENDUNG** auswählen, entsprechen die restlichen Schritte in diesem Artikel nicht der tatsächlichen Vorgehensweise.

     ![Anwendung benennen](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Tragen Sie die Eigenschaften Ihrer Anwendung ein. Geben Sie für die **ANMELDE-URL** den URI einer Website an, die Ihre Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. Geben Sie für die **APP-ID URI ** die URI an, die Ihre Anwendung identifiziert.

     ![Anwendungseigenschaften](./media/resource-group-create-service-principal-portal/app-properties.png)

Sie haben Ihre Anwendung erstellt.

## Abrufen der Client-ID und des Authentifizierungsschlüssels

Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung. Wenn die Anwendung mit ihren eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie außerdem einen Authentifizierungsschlüssel.

1. Wählen Sie die Registrierkarte **Konfigurieren** aus, um das Kennwort für Ihre Anwendung zu konfigurieren.

     ![Anwendung konfigurieren](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopieren Sie die **CLIENT-ID**.
  
     ![Client-ID](./media/resource-group-create-service-principal-portal/client-id.png)

3. Wenn die Anwendung mit ihren eigenen Anmeldeinformationen ausgeführt wird, navigieren Sie nach unten zum Abschnitt **Schlüssel**, und wählen Sie den gewünschten Gültigkeitszeitraum für Ihr Kennwort aus.

     ![keys](./media/resource-group-create-service-principal-portal/create-key.png)

4. Wählen Sie **Speichern** aus, um Ihren Schlüssel zu erstellen.

     ![speichern](./media/resource-group-create-service-principal-portal/save-icon.png)

     Der gespeicherte Schlüssel wird angezeigt, sodass Sie ihn kopieren können. Kopieren Sie den Schlüssel jetzt, da Sie ihn später nicht mehr abrufen können.

     ![gespeicherter Schlüssel](./media/resource-group-create-service-principal-portal/save-key.png)

## Abrufen der Mandanten-ID

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben. Bei Web-Apps und Web-API-Apps können Sie die Mandanten-ID abrufen, indem Sie unten auf der Seite **Endpunkte anzeigen** auswählen und die ID wie im der folgenden Abbildung gezeigt abrufen.

   ![Mandanten-ID](./media/resource-group-create-service-principal-portal/save-tenant.png)

Sie können die Mandanten-ID auch über PowerShell abrufen:

    Get-AzureRmSubscription

Oder mit der Azure-Befehlszeilenschnittstelle:

    azure account show --json

## Festlegen der delegierten Berechtigungen

Wenn Ihre Anwendung im Auftrag eines angemeldeten Benutzers auf Ressourcen zugreift, müssen Sie der Anwendung die delegierten Berechtigungen für den Zugriff auf andere Anwendungen gewähren. Sie gewähren diesen Zugriff auf der Registerkarte **Konfigurieren** im Abschnitt **Berechtigungen für andere Anwendungen**. Eine delegierte Berechtigung ist standardmäßig bereits für die Azure Active Directory-Instanz aktiviert. Lassen Sie diese delegierte Berechtigung unverändert.

1. Wählen Sie **Anwendung hinzufügen** aus.

2. Wählen Sie in der Liste die **Azure-Dienstverwaltungs-API** aus. Klicken Sie dann auf das Symbol zum Fertigstellen.

      ![App auswählen](./media/resource-group-create-service-principal-portal/select-app.png)

3. Aktivieren Sie in der Dropdownliste für delegierte Berechtigungen das Kontrollkästchen **Als Organisation auf Azure-Dienstverwaltung zugreifen**.

      ![Berechtigung auswählen](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Speichern Sie die Änderungen.

## Zuweisen einer Anwendung zur Rolle

Wenn Ihre Anwendung mit ihren eigenen Anmeldeinformationen ausgeführt wird, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](./active-directory/role-based-access-built-in-roles.md).

Um einer Anwendung eine Rolle zuzuweisen, müssen Sie über die richtigen Berechtigungen verfügen. Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt. Die Rolle „Mitwirkender“ verfügt nicht über die richtigen Zugriffsrechte.

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Zum Zuweisen der Anwendung an eine Rolle wechseln Sie vom klassischen Portal zum [Azure-Portal](https://portal.azure.com).

1. Überprüfen Sie Ihre Berechtigungen, um sicherzustellen, dass Sie den Dienstprinzipal einer Rolle zuweisen können. Wählen Sie **Meine Berechtigungen** für Ihr Konto.

    ![Meine Berechtigungen auswählen](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Zeigen Sie die zugewiesenen Berechtigungen für Ihr Konto an. Wie bereits erwähnt, müssen Sie zur Rolle „Besitzer“ oder zur Rolle „Benutzerzugriffsadministrator“ gehören oder über eine benutzerdefinierte Rolle verfügen, die Schreibzugriff für „Microsoft.Authorization“ gewährt. Die folgende Abbildung zeigt ein Konto, das der Rolle „Mitwirkender“ für das Abonnement zugewiesen wurde – dies sind keine adäquaten Berechtigungen, um eine Anwendung zu einer Rolle zuzuweisen.

    ![Meine Berechtigungen anzeigen](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Wenn Sie nicht über die erforderlichen Berechtigungen zum Gewähren des Zugriffs auf eine Anwendung verfügen, muss Ihr Abonnementadministrator Sie zur Rolle „Benutzerzugriffsadministrator“ hinzufügen, oder es muss ein Administrator den Zugriff auf die Anwendung gewähren.

1. Navigieren Sie zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Für dieses Thema können Sie zu einer Ressourcengruppe navigieren und auf dem Blatt der Ressourcengruppe die Option **Zugriffssteuerung** auswählen.

     ![Benutzer auswählen](./media/resource-group-create-service-principal-portal/select-users.png)

2. Wählen Sie **Hinzufügen**.

     ![„Hinzufügen“ wählen](./media/resource-group-create-service-principal-portal/select-add.png)

3. Wählen Sie die **Leser**-Rolle (bzw. die Rolle, der die Anwendung zugewiesen werden soll).

     ![Rolle wählen](./media/resource-group-create-service-principal-portal/select-role.png)

4. Beim ersten Anzeigen der Liste mit Benutzern, die Sie der Rolle hinzufügen können, werden keine Anwendungen angezeigt. Nur Gruppen und Benutzer werden angezeigt.

     ![Benutzer anzeigen](./media/resource-group-create-service-principal-portal/show-users.png)

5. Sie müssen nach Ihrer Anwendung suchen. Geben Sie den Anfang des Namens Ihrer Anwendung ein, und die Liste der verfügbaren Optionen ändert sich. Wählen Sie Ihre Anwendung aus, wenn Sie in der Liste angezeigt wird.

     ![An Rolle zuweisen](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Wählen Sie **OK**, um das Zuweisen der Rolle abzuschließen. Jetzt sollten Sie Ihre Anwendung in der Liste der Benutzer sehen, die einer Rolle für die Ressourcengruppe zugewiesen sind.


Weitere Informationen zum Zuweisen von Benutzern und Anwendungen zu Rollen über das Portal finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## Beispielanwendungen

Die folgenden Beispielanwendungen veranschaulichen die Anmeldung als Dienstprinzipal:

**.NET**

- [Deploy an SSH Enabled VM with a Template with .NET (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Manage Azure resources and resource groups with .NET (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Getting Started with Resources - Deploy Using ARM Template - in Java (Erste Schritte mit Ressourcen – Bereitstellen mithilfe einer Azure Resource Manager-Vorlage – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Getting Started with Resources - Manage Resource Group - in Java (Erste Schritte mit Ressourcen – Verwalten von Ressourcengruppen – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Deploy an SSH Enabled VM with a Template in Python (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Manage Azure resources and resource groups with Python (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Deploy an SSH Enabled VM with a Template in Node.js (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Manage Azure resources and resource groups with Node.js (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Deploy an SSH Enabled VM with a Template in Ruby (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Manage Azure resources and resource groups with Ruby (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## Nächste Schritte

- Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](./active-directory/role-based-access-control-configure.md).
- Eine Videodemo dieser Schritte finden Sie unter [Aktivieren der programmgesteuerten Verwaltung einer Azure-Ressource mit Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

<!---HONumber=AcomDC_0824_2016-->