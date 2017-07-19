---
title: Erstellen eines Azure IoT Edge-Moduls mit C# | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie ein BLE-Datenkonvertierungsmodul mit den neuesten Azure IoT Edge-NuGet-Paketen, Visual Studio Code und C# schreiben.
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: azure, iot, tutorial, modul, nuget, vscode, csharp, edge
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 91ff3c96e4d7928131eba83c0e8c1951782447be
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Erstellen eines Azure IoT Edge-Moduls mit C&#x23;

In diesem Tutorial wird veranschaulicht, wie Sie ein Modul für `Azure IoT Edge` mit `Visual Studio Code` und `C#` erstellen.

In diesem Tutorial zeigen wir Ihnen, wie Sie eine Umgebung einrichten und ein [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)-Datenkonvertierungsmodul mit den neuesten `Azure IoT Edge NuGet`-Paketen schreiben. 

>[!NOTE]
Es wird das `.NET Core SDK` verwendet, bei dem die plattformübergreifende Kompatibilität unterstützt wird. Das folgende Tutorial wurde mit dem Betriebssystem `Windows 10` geschrieben. Einige der Befehle in diesem Tutorial können je nach `development environment` variieren. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Abschnitt richten wir Ihre Umgebung für die Entwicklung mit dem `Azure IoT Edge`-Modul ein. Dies gilt sowohl für Betriebssysteme vom Typ **Windows 64 Bit** als auch **Linux 64 Bit (Ubuntu/Debian 8)**.

Die folgende Software ist erforderlich:

- [Git-Client](https://git-scm.com/downloads)
- [.NET Core SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Es ist nicht erforderlich, das Repository für dieses Beispiel zu klonen. Der gesamte Beispielcode dieses Tutorials ist aber im folgenden Repository enthalten:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Erste Schritte

1. Installieren Sie `.NET Core SDK`.
2. Installieren Sie `Visual Studio Code` und die `C# extension` über den Visual Studio Code Marketplace.

Sehen Sie sich dieses [kurze Videotutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) zu den ersten Schritten mit `Visual Studio Code` und dem `.NET Core SDK` an.

## <a name="creating-the-azure-iot-edge-converter-module"></a>Erstellen des Azure IoT Edge-Konvertierungsmoduls

1. Initialisieren Sie ein neues C#-Projekt mit `.NET Core`-Klassenbibliothek:
    - Öffnen Sie eine Eingabeaufforderung (`Windows + R` -> `cmd` -> `enter`).
    - Navigieren Sie zu dem Ordner, in dem Sie das `C#`-Projekt erstellen möchten.
    - Geben Sie **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3** ein. 
    - Dieser Befehl erstellt eine leere Klasse mit dem Namen `Class1.cs` in Ihrem Projektverzeichnis.
2. Navigieren Sie zu dem Ordner, in dem wir gerade das Klassenbibliothekprojekt erstellt haben, indem Sie **cd IoTEdgeConverterModule** eingeben.
3. Öffnen Sie das Projekt in `Visual Studio Code`, indem Sie **code .** eingeben.
4. Klicken Sie nach dem Öffnen des Projekts in `Visual Studio Code` auf die Datei **IoTEdgeConverterModule.csproj**, um die Datei wie in der folgenden Abbildung zu öffnen:

    ![Visual Studio Code-Bearbeitungsfenster](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Fügen Sie das `XML`-Blob aus dem folgenden Codeausschnitt zwischen dem schließenden `PropertyGroup`-Tag und dem schließenden `Project`-Tag ein (in der obigen Abbildung Zeile 6), und speichern Sie die Datei, indem Sie `Ctrl` + `S` drücken.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Nachdem Sie die Datei `.csproj` gespeichert haben, sollten Sie von `Visual Studio Code` über das Dialogfeld `unresolved dependencies` eine Aufforderung erhalten. Dies ist in der folgenden Abbildung dargestellt: 

    ![Visual Studio Code-Dialogfeld zum Wiederherstellen von Abhängigkeiten](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    a) Klicken Sie auf `Restore`, um alle Verweise in der Projektdatei `.csproj` wiederherzustellen, einschließlich des hinzugefügten `PackageReferences`-Elements. 

    b) `Visual Studio Code` erstellt automatisch die Datei `project.assets.json` in Ihrem Ordner `obj`. Diese Datei enthält Informationen zu den Abhängigkeiten Ihres Projekts, um nachfolgende Wiederherstellungen zu beschleunigen.
 
    >[!NOTE]
    `.NET Core Tools` sind jetzt MSBuild-basiert. Dies bedeutet, dass anstelle der Datei `project.json` die Projektdatei `.csproj` erstellt wird.

    - Wenn Sie in `Visual Studio Code` keine Aufforderung erhalten, ist das kein Problem, da wir den Vorgang manuell durchführen können. Öffnen Sie das integrierte Terminalfenster von `Visual Studio Code`, indem Sie `Ctrl` + `backtick` drücken oder die Menüs `View` -> `Integrated Terminal` verwenden.
    - Geben Sie im `Integrated Terminal`-Fenster den Text **dotnet restore** ein.
    
7. Benennen Sie die Datei `Class1.cs` in `BleConverterModule.cs` um. 

    a) Klicken Sie zum Umbenennen der Datei zuerst auf die Datei, und drücken Sie anschließend die Taste `F2`.
    
    b) Geben Sie den neuen Namen **BleConverterModule** wie in der folgenden Abbildung ein:

    ![Visual Studio Code – Umbenennen einer Klasse](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Ersetzen Sie den vorhandenen Code in der Datei `BleConverterModule.cs`, indem Sie den folgenden Codeausschnitt in die Datei `BleConverterModule.cs` einfügen.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Speichern Sie die Datei, indem Sie `Ctrl` + `S` drücken.

10. Erstellen Sie eine neue Datei mit dem Namen `Untitled-1`, indem Sie wie in der folgenden Abbildung die Taste `Ctrl` + `N` drücken:

    ![Visual Studio Code - – Neue Datei](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. Zum Deserialisieren des `JSON`-Objekts, das wir vom simulierten `BLE`-Gerät erhalten, kopieren Sie den folgenden Code in das Fenster `Untitled-1` des Dateicode-Editors. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Speichern Sie die Datei unter dem Namen `BleData.cs`, indem Sie `Ctrl` + `Shift` + `S` drücken.
    - Wählen Sie im Dialogfeld „Speichern unter“ im Dropdownmenü `Save as Type` die Option `C# (*.cs;*.csx)`. Dies ist in der folgenden Abbildung dargestellt:

    ![Visual Studio Code – Dialogfeld „Speichern unter“](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Erstellen Sie eine neue Datei mit dem Namen `Untitled-1`, indem Sie `Ctrl` + `N` drücken.

14. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei `Untitled-1` ein. Diese Klasse ist ein `Azure IoT Edge`-Modul, das wir verwenden, um die vom `BleConverterModule` empfangenen Daten auszugeben.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Speichern Sie die Datei unter dem Namen `DotNetPrinterModule.cs`, indem Sie `Ctrl` + `Shift` + `S` drücken.
    - Wählen Sie im Dialogfeld „Speichern unter“ im Dropdownmenü `Save as Type` die Option `C# (*.cs;*.csx)`.

16. Erstellen Sie eine neue Datei mit dem Namen `Untitled-1`, indem Sie `Ctrl` + `N` drücken.

17. Fügen Sie zum Deserialisieren des `JSON`-Objekts, das wir vom `BleConverterModule` erhalten, den folgenden Codeausschnitt in die Datei `Untitled-1` ein. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Speichern Sie die Datei unter dem Namen `BleConverterData.cs`, indem Sie `Ctrl` + `Shift` + `S` drücken.
    - Wählen Sie im Dialogfeld „Speichern unter“ im Dropdownmenü `Save as Type` die Option `C# (*.cs;*.csx)`.

19. Erstellen Sie eine neue Datei mit dem Namen `Untitled-1`, indem Sie `Ctrl` + `N` drücken.

20. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei `Untitled-1` ein.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Speichern Sie die Datei unter dem Namen `gw-config.json`, indem Sie `Ctrl` + `Shift` + `S` drücken.
    - Wählen Sie im Dialogfeld „Speichern unter“ im Dropdownmenü `Save as Type` die Option `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. Aktualisieren Sie die Datei `IoTEdgeConverterModule.csproj` mit dem folgenden XML-Blob, um das Kopieren der Konfigurationsdatei in das Ausgabeverzeichnis zu ermöglichen:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - Die aktualisierte Datei `IoTEdgeConverterModule.csproj` sollte wie in der folgenden Abbildung aussehen:

    ![Visual Studio Code – Aktualisierte CSPROJ-Datei](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Erstellen Sie eine neue Datei mit dem Namen `Untitled-1`, indem Sie `Ctrl` + `N` drücken.

24. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei `Untitled-1` ein.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Speichern Sie die Datei unter dem Namen `binplace.ps1`, indem Sie `Ctrl` + `Shift` + `S` drücken.
    - Wählen Sie im Dialogfeld „Speichern unter“ im Dropdownmenü `Save as Type` die Option `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Erstellen Sie das Projekt, indem Sie `Ctrl` + `Shift` + `B` drücken. Wenn Sie das Projekt zum ersten Mal erstellen, erhalten Sie von `Visual Studio Code` im Dialogfeld `No build task defined.` eine Aufforderung. Dies ist in der folgenden Abbildung dargestellt:

    ![Visual Studio Code – Dialogfeld zur Buildaufgabe](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    a) Klicken Sie auf die Schaltfläche `Configure Build Task`.

    b) Im Dropdownmenü `Select a Task Runner` des Dialogfelds: Wählen Sie `.NET Core` wie in der folgenden Abbildung: 

    ![Visual Studio Code – Dialogfeld „Aufgabe auswählen“](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    c) Wenn Sie auf das `.NET Core`-Element klicken, wird im Verzeichnis `.vscode` die Datei `tasks.json` erstellt und im `code editor`-Fenster geöffnet. Es ist nicht erforderlich, diese Datei zu ändern. Schließen Sie die Registerkarte.

27.  Öffnen Sie das integrierte Terminalfenster von `Visual Studio Code`, indem Sie `Ctrl` + `backtick` drücken, oder verwenden Sie die Menüs `View` -> `Integrated Terminal`, und geben Sie **.\binplace.ps1** an der `PowerShell`-Eingabeaufforderung ein. Mit diesem Befehl werden alle Abhängigkeiten in das Ausgabeverzeichnis kopiert.

28. Navigieren Sie im `Integrated Terminal`-Fenster zum Projektausgabeverzeichnis, indem Sie **cd .\bin\Debug\netstandard1.3** eingeben.

29. Führen Sie das Beispielprojekt aus, indem Sie an der `Integrated Terminal`-Eingabeaufforderung den Befehl **.\gw.exe gw-config.json** eingeben. 
    - Wenn Sie die Schritte dieses Tutorials befolgt haben, sollte jetzt das Beispielprojekt `Azure IoT Edge BLE Data Converter Module` wie in der folgenden Abbildung ausgeführt werden:
    
        ![Ausführen des Beispiels für ein simuliertes Gerät in Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Wenn Sie die Anwendung beenden möchten, drücken Sie die Taste `<Enter>`.

>[!IMPORTANT]
Es ist nicht ratsam, `Ctrl` + `C` zum Beenden der `IoT Edge`-Gatewayanwendung zu verwenden (also **gw.exe**). Es kann nämlich sein, dass der Prozess dadurch unplanmäßig beendet wird.


