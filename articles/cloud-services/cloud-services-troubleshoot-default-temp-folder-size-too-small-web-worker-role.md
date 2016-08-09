<properties
   pageTitle="Standardmäßige Größe des TEMP-Ordners ist für eine Rolle zu gering | Microsoft Azure"
   description="Eine Clouddienstrolle verfügt über begrenzten Speicherplatz für den Ordner TEMP. Dieser Artikel enthält einige Vorschläge dazu, wie Sie vermeiden, dass kein Speicherplatz mehr vorhanden ist."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="07/19/2016"
   ms.author="v-six" />

# Standardmäßige Größe des TEMP-Ordners ist für eine Clouddienst-Web-/Workerrolle zu gering

Das standardmäßige temporäre Verzeichnis einer Clouddienst-Web- oder -Workerrolle hat eine maximale Größe von 100 MB. Es kann vorkommen, dass dieser Platz auch belegt wird. In diesem Artikel wird beschrieben, wie Sie verhindern, dass der Speicherplatz des temporären Verzeichnisses erschöpft ist.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Wie kann es passieren, dass kein Speicherplatz mehr übrig ist?

Die standardmäßigen Windows-Umgebungsvariablen TEMP und TMP stehen für Code zu Verfügung, der in Ihrer Anwendung ausgeführt wird. Sowohl TEMP als auch TMP zeigen auf ein einzelnes Verzeichnis mit einer maximalen Größe von 100 MB. Alle Daten, die in diesem Verzeichnis gespeichert werden, werden nicht über den Lebenszyklus des Clouddiensts beibehalten. Wenn die Rolleninstanzen in einem Clouddienst recycelt werden, wird das Verzeichnis bereinigt.

## Vorschläge zum Beheben des Problems

Implementieren Sie eine der folgenden Alternativen:

- Konfigurieren Sie eine lokale Speicherressource, und greifen Sie direkt darauf zu, anstatt TEMP oder TMP zu verwenden. Rufen Sie die [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx)-Methode auf, um aus Code, der in Ihrer Anwendung ausgeführt wird, auf eine lokale Speicherressource zuzugreifen.

- Konfigurieren Sie eine lokale Speicherressource, und lassen Sie die Verzeichnisse TEMP und TMP auf den Pfad der lokalen Speicherressource verweisen. Führen Sie diese Änderung in der [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)-Methode durch.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie die Zielverzeichnisse für TEMP und TMP über die OnStart-Methode ändern:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## Nächste Schritte

Lesen Sie einen Blog, der beschreibt, [wie Sie die Größe des temporären Ordners für die Azure-Webrolle ASP.NET erhöhen](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Sehen Sie sich weitere [Artikel zur Problembehandlung](/?tag=top-support-issue&product=cloud-services) für Clouddienste an.

Erfahren Sie in der [Blogreihe von Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx), wie Sie Probleme bei Clouddienstrollen mithilfe der Computerdiagnosedaten von Azure-PaaS beheben.

<!---HONumber=AcomDC_0727_2016-->