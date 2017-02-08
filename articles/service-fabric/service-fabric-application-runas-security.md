---
title: Grundlegendes zur Service Fabric-Anwendung und zu Sicherheitsrichtlinien | Microsoft Docs
description: "Eine Übersicht über die Ausführung einer Service Fabric-Anwendung unter System- und lokalen Sicherheitskonten einschließlich dem SetupEntryPoint, an dem eine Anwendung verschiedene Aktionen mit bestimmten Berechtigungen ausführen muss, bevor sie gestartet wird."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/22/2016
ms.author: mfussell
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9484de017b9903cc22b27b26bba753b09b311749


---
# <a name="configure-security-policies-for-your-application"></a>Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung
Durch Verwenden von Azure Service Fabric können Sie Anwendungen sichern, die im Cluster unter verschiedenen Benutzerkonten ausgeführt werden. Mit Service Fabric werden auch die Ressourcen gesichert, die von Anwendungen zum Zeitpunkt der Bereitstellung in den Benutzerkonten genutzt werden, z.B. Dateien, Verzeichnisse und Zertifikate. So lässt sich erreichen, dass ausgeführte Anwendungen auch in einer gemeinsamen gehosteten Umgebung sicher voneinander abgegrenzt sind.

Standardmäßig werden Service Fabric-Anwendungen unter dem Konto ausgeführt, unter dem der Prozess „Fabric.exe“ ausgeführt wird. Darüber hinaus bietet Service Fabric die Möglichkeit zur Ausführung von Anwendungen in einem lokalen Benutzer- oder Systemkonto, das im Manifest der Anwendung angegeben wird. Unterstützte lokale Systemkontotypen sind **LocalUser**, **NetworkService**, **LocalService** und **LocalSystem**.

 Wenn Sie Service Fabric unter Windows Server in Ihrem Rechenzentrum mithilfe des eigenständigen Installers ausführen, können Sie Active Directory-Domänenkonten verwenden.

Sie können Benutzergruppen definieren und erstellen und dann jeder Gruppe Benutzer zur gemeinsamen Verwaltung hinzufügen. Dies ist nützlich, wenn es für verschiedene Diensteinstiegspunkte mehrere Benutzer gibt, die auf Gruppenebene bestimmte allgemeine Berechtigungen benötigen.

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Konfigurieren der Richtlinie für einen Setupeinstiegspunkt für Dienste
Wie im [Anwendungsmodell](service-fabric-application-model.md) beschrieben, ist der Setupeinstiegspunkt **SetupEntryPoint** ein privilegierter Einstiegspunkt, der mit den gleichen Anmeldeinformationen wie Service Fabric (meist mit dem Konto *NetworkService*) vor jedem anderen Einstiegspunkt ausgeführt wird. Die durch **EntryPoint** angegebene ausführbare Datei ist üblicherweise der Diensthost mit langer Ausführungsdauer. Mit einem separaten Setupeinstiegspunkt lässt sich also vermeiden, dass der die ausführbare Diensthostdatei über längere Zeiträume mit hohen Berechtigungen ausgeführt werden muss. Die von **EntryPoint** angegebene ausführbare Datei wird ausgeführt, nachdem **SetupEntryPoint** erfolgreich beendet wurde. Der resultierende Prozess wird überwacht und neu gestartet (er beginnt wieder mit **SetupEntryPoint**), sofern er beendet wird oder abstürzt.

Unten sehen Sie ein einfaches Beispiel für ein Dienstmanifest mit dem SetupEntryPoint und dem primären EntryPoint des Diensts.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-by-using-a-local-account"></a>Konfigurieren der Richtlinie mithilfe eines lokalen Kontos
Nach der Konfiguration des Diensts mit einem SetupEntryPoint können Sie im Anwendungsmanifest die Sicherheitsberechtigungen ändern, unter denen dieser ausgeführt wird. Das folgende Beispiel zeigt, wie Sie den Dienst so konfigurieren, dass dieser mit den Berechtigungen des Benutzeradministratorkontos ausgeführt wird.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Erstellen Sie zuerst den Abschnitt **Principals** mit einem Benutzernamen, z.B. „SetupAdminUser“. Dies bedeutet, dass der Benutzer Mitglied der Gruppe „Administratoren“ des Systems ist.

Konfigurieren Sie als Nächstes im Abschnitt **ServiceManifestImport** eine Richtlinie, gemäß der dieser Prinzipal auf **SetupEntryPoint** angewendet werden kann. Hierdurch wird Service Fabric informiert, dass die Datei **MySetup.bat** als `RunAs`-Vorgang mit Administratorrechten ausgeführt werden soll. Da Sie *keine* Richtlinie auf den primären Einstiegspunkt angewendet haben, wird der Code in **MyServiceHost.exe** unter dem Systemkonto **NetworkService** ausgeführt. Dies ist das Standardkonto, unter dem alle Diensteinstiegspunkte ausgeführt werden.

Nun fügen wir die Datei „MySetup.bat“ dem Visual Studio-Projekt hinzu, um die Administratorrechte zu testen. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Dienstprojekt, und fügen Sie eine neue Datei mit dem Namen „MySetup.bat“ hinzu.

Stellen Sie anschließend sicher, dass die Datei „MySetup.bat“ im Dienstpaket enthalten ist. Standardmäßig ist dies nicht der Fall. Wählen Sie die Datei aus, klicken Sie mit der rechten Maustaste, um das Kontextmenü zu öffnen, und wählen Sie **Eigenschaften**. Stellen Sie im Dialogfeld „Eigenschaften“ sicher, dass **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** festgelegt ist. Der folgende Screenshot zeigt dies.

![Visual Studio – Batchdatei für „CopyToOutput“ für „SetupEntryPoint“][image1]

Öffnen Sie nun die Datei „MySetup.bat“, und fügen Sie die folgenden Befehle hinzu:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Als Nächstes müssen Sie die Projektmappe erstellen und in einem lokalen Entwicklungscluster bereitstellen. Nachdem der Dienst gestartet wurde (wie im Service Fabric Explorer angezeigt), können Sie erkennen, dass die Datei „MySetup.bat“ auf zwei Arten erfolgreich war. Öffnen Sie eine PowerShell-Eingabeaufforderung, und geben Sie Folgendes ein:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Notieren Sie anschließend den Namen des Knotens, auf dem der Dienst bereitgestellt und im Service Fabric Explorer gestartet wurde, z.B. Knoten 2. Navigieren Sie als Nächstes zum Arbeitsordner der Anwendungsinstanz, um die Datei „out.txt“ zu ermitteln, in der der Wert von **TestVariable** angezeigt wird. Wenn dieser Dienst z.B. auf Knoten 2 bereitgestellt wurde, können Sie für **MyApplicationType** auf diesen Pfad zugreifen:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Konfigurieren der Richtlinie mithilfe eines lokalen Systemkontos
Häufig ist es von Vorteil, das Startskript mithilfe eines lokalen Systemkontos statt eines Administratorkontos auszuführen. Die Ausführung der RunAs-Richtlinie als Mitglied der Administratorgruppe funktioniert in der Regel nicht gut, da die Benutzerkontensteuerung (User Access Control, UAC) auf Computern standardmäßig aktiviert ist. In solchen Fällen **wird empfohlen, den SetupEntryPoint als LocalSystem auszuführen, anstatt als lokaler Benutzer, der der Administratorgruppe hinzugefügt wurde**. Das folgende Beispiel zeigt, wie SetupEntryPoint zur Ausführung als LocalSystem festgelegt wird:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>Starten von PowerShell-Befehlen über einen Setupeinstiegspunkt
Zum Ausführen von PowerShell über den Punkt **SetupEntryPoint** können Sie **PowerShell.exe** in einer Batchdatei ausführen, die auf eine PowerShell-Datei verweist. Fügen Sie zuerst dem Dienstprojekt eine PowerShell-Datei hinzu, z.B. **MySetup.ps1**. Denken Sie daran, die Eigenschaft *Kopieren, wenn neuer* so festzulegen, dass die Datei in das Dienstpaket einbezogen wird. Das folgende Beispiel zeigt eine Beispielbatchdatei zum Starten einer PowerShell-Datei namens „MySetup.ps1“, mit der die Systemumgebungsvariable **TestVariable** festgelegt wird.

„MySetup.bat“ zum Starten der PowerShell-Datei:

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Fügen Sie in der PowerShell-Datei Folgendes ein, um eine Systemumgebungsvariable festzulegen:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

> [!NOTE]
> Standardmäßig sucht die Batchdatei bei der Ausführung in dem Anwendungsordner **work** nach Dateien. In diesem Fall soll „MySetup.bat“ bei der Ausführung die Datei „MySetup.ps1“ im gleichen Ordner suchen, also im Anwendungsordner **CodePackage**. Um diesen Ordner zu ändern, legen Sie den Arbeitsordner fest:
> 
> 

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="use-console-redirection-for-local-debugging"></a>Verwenden der Konsolenumleitung für das lokale Debuggen
Gelegentlich ist es hilfreich, die Konsolenausgabe der Ausführung eines Skripts zum Debuggen auszuwerten. Zu diesem Zweck können Sie eine Richtlinie zur Konsolenumleitung festlegen, die die Ausgabe in eine Datei schreibt. Die Dateiausgabe wird in den Anwendungsordner **log** auf dem Knoten geschrieben, auf dem die Anwendung bereitgestellt und ausgeführt wird. (Siehe obiges Beispiel.)

> [!NOTE]
> Verwenden Sie die Richtlinie zur Konsolenumleitung nie in einer Anwendung, die in der Produktionsumgebung bereitgestellt wurde, da sich dies auf das Anwendungsfailover auswirken kann. Verwenden Sie diese *nur* für die lokale Entwicklung und das Debuggen.  
> 
> 

Das folgende Beispiel zeigt das Festlegen der Konsolenumleitung mit einem FileRetentionCount-Wert:

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Wenn Sie nun die Datei „MySetup.ps1“ so ändern, dass ein **Echo**-Befehl geschrieben wird, wird zu Debugzwecken in die Ausgabedatei geschrieben:

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Nachdem Sie Ihr Skript gedebuggt haben, entfernen Sie diese Richtlinie zur Konsolenumleitung sofort**.

## <a name="configure-a-policy-for-service-code-packages"></a>Konfigurieren einer Richtlinie für Dienstcodepakete
In den vorangegangenen Schritten wurde erläutert, wie eine RunAs-Richtlinie auf SetupEntryPoint angewendet wird. Nun schauen wir uns genauer an, wie verschiedene Prinzipale erstellt werden, die als Dienstrichtlinien angewendet werden können.

### <a name="create-local-user-groups"></a>Erstellen lokaler Benutzergruppen
Sie können Benutzergruppen definieren und erstellen werden, die es ermöglichen, dass der Gruppe ein oder mehrere Benutzer hinzugefügt werden. Dies ist besonders nützlich, wenn es für verschiedene Diensteinstiegspunkte mehrere Benutzer gibt, die auf Gruppenebene bestimmte allgemeine Berechtigungen benötigen. Das folgende Beispiel zeigt eine lokale Gruppe namens **LocalAdminGroup**, die über Administratorrechte verfügt. Zwei Benutzer, „Customer1“ und „Customer2“, wurden dieser lokalen Gruppe hinzugefügt.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Erstellen lokaler Benutzer
Sie können einen lokalen Benutzer erstellen, der zum Sichern eines Diensts in der Anwendung dienen kann. Wenn ein Konto vom Typ **LocalUser** im Abschnitt „Principals“ des Anwendungsmanifests angegeben wird, erstellt Service Fabric lokale Benutzerkonten auf Computern, auf denen die Anwendung bereitgestellt wird. Standardmäßig müssen diese Konten nicht die gleichen Namen wie im Anwendungsmanifest haben (z.B. „Customer3“ im folgenden Beispiel). Stattdessen werden sie dynamisch generiert und verfügen über zufällige Kennwörter.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Zuweisen von Richtlinien zu den Dienstcodepaketen
Der Abschnitt **RunAsPolicy** für ein **ServiceManifestImport**-Element gibt das Konto aus dem Abschnitt „Principals“ an, das zum Ausführen eines Codepakets verwendet werden soll. Außerdem werden damit Codepakete aus dem Dienstmanifest Benutzerkonten im Abschnitt „Principals“ zugeordnet. Sie können dies für die Setup-Einstiegspunkte oder primären Einstiegspunkte angeben oder `All` angeben, damit diese Einstellung für beide gilt. Das folgende Beispiel zeigt die Anwendung unterschiedlicher Richtlinien:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Wenn **EntryPointType** nicht angegeben ist, wird die Standardeinstellung auf „EntryPointType = Main“ festgelegt. Das Angeben von **SetupEntryPoint** ist besonders nützlich, wenn Sie bestimmte Setupvorgänge mit erhöhten Berechtigungen in einem Systemkonto ausführen möchten. Der tatsächliche Dienstcode kann unter einem Konto mit weniger hohen Berechtigungen ausgeführt werden.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Anwenden einer Standardrichtlinie auf alle Dienstcodepakete
Verwenden Sie den Abschnitt **DefaultRunAsPolicy**, um ein Standardbenutzerkonto für alle Codepakete anzugeben, für die keine bestimmte **RunAsPolicy** definiert ist. Wenn die meisten Codepakete, die im Dienstmanifest einer Anwendung angegeben sind, im gleichen Benutzerkonto ausgeführt werden müssen, kann für die Anwendung einfach eine RunAs-Standardrichtlinie mit diesem Benutzerkonto definiert werden. Im folgenden Beispiel wird beispielsweise Folgendes angegeben: Wenn für ein Codepaket keine **RunAsPolicy** angegeben ist, soll das Codepaket unter dem Standardkonto **MyDefaultAccount** ausgeführt werden, das im Abschnitt „Principals“ angegeben ist.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="use-an-active-directory-domain-group-or-user"></a>Verwenden einer Active Directory-Domänengruppe oder eines -Benutzers
Für eine Service Fabric-Instanz, die mit dem eigenständigen Installer unter Windows Server installiert wurde, können Sie den Dienst mit den Anmeldeinformationen für ein Active Directory-Benutzer- oder -Gruppenkonto ausführen. Hinweis: Dies bezieht sich auf eine lokale Active Directory-Instanz in Ihrer Domäne, nicht auf Azure Active Directory (Azure AD). Indem Sie einen Domänenbenutzer oder eine -gruppe verwenden, können Sie anschließend auf andere Ressourcen in der Domäne (z.B. Dateifreigaben) zugreifen, für die Berechtigungen gewährt wurden.

Im folgenden Beispiel wird ein Active Directory-Benutzer mit dem Namen *TestUser* verwendet, dessen Domänenkennwort mit dem Zertifikat *MyCert* verschlüsselt wird. Sie können den PowerShell-Befehl `Invoke-ServiceFabricEncryptText` verwenden, um den Verschlüsselungstext für den geheimen Schlüssel zu erstellen. Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in Service Fabric-Anwendungen](service-fabric-application-secret-management.md).

Sie müssen den privaten Schlüssel des Zertifikats zum Entschlüsseln des Kennworts mit einer Out-of-Band-Methode auf dem lokalen Computer bereitstellen (in Azure erfolgt dies über Azure Resource Manager). Wenn Service Fabric das Dienstpaket dann auf dem Computer bereitstellt, kann Service Fabric den geheimen Schlüssel entschlüsseln und (zusammen mit dem Benutzernamen) zum Authentifizieren bei Active Directory verwenden, sodass die Ausführung mit diesen Anmeldeinformationen erfolgt.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte
Wenn Sie eine RunAs-Richtlinie auf einen Dienst anwenden und im Dienstmanifest Endpunktressourcen mit dem HTTP-Protokoll deklariert sind, müssen Sie eine **SecurityAccessPolicy** angeben. Diese Richtlinie soll sicherstellen, dass Ports, die diesen Endpunkten zugeordnet sind, richtig auf der Zugriffssteuerungsliste für das RunAs-Benutzerkonto eingetragen sind, in dem der Dienst ausgeführt wird. Andernfalls hat **http.sys** keinen Zugriff auf den Dienst, sodass beim Aufrufen vom Client Fehler auftreten. Im folgenden Beispiel wird das Customer3-Konto auf den Endpunkt **ServiceEndpointName** angewendet, und es werden vollständige Zugriffsrechte gewährt.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Für den HTTPS-Endpunkt müssen Sie auch den Namen des Zertifikats zum Zurückgeben des Clients angeben. Hierfür können Sie **EndpointBindingPolicy**verwenden, indem Sie das Zertifikat im Zertifikatabschnitt des Anwendungsmanifests definieren.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Vollständiges Beispiel eines Anwendungsmanifests
Im folgenden Anwendungsmanifest sind viele unterschiedliche Einstellungen enthalten:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
* [Informationen zum Anwendungsmodell](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png



<!--HONumber=Dec16_HO2-->


