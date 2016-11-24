

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
> [!NOTE]
> Dieser Artikel beschreibt das Navigieren sowie das Auswählen von Images virtueller Computer mit einer aktuellen Installation der Azure-Befehlszeilenschnittstelle oder von Azure PowerShell. Als Voraussetzung müssen Sie in den Ressourcen-Manager-Modus wechseln. Wechseln Sie über die Azure-Befehlszeilenschnittstelle durch Eingabe von `azure config mode arm` in diesen Modus. 
> 
> 

Der schnellste und einfachste Weg ein Datenträgerabbild zu suchen, um es entweder mit `azure vm quick-create` zu verwenden oder um eine Vorlagendatei einer Ressourcengruppe zu erstellen, ist den Befehl `azure vm image list` aufzurufen und den Speicherort, den Herausgebernamen (keine Unterscheidung zwischen Groß- und Kleinschreibung) und ein Angebot - wenn Sie es kennen - zu übergeben. Zum Beispiel ist die folgende Liste nur ein kurzes Beispiel (einige Listen sind sehr lang), wenn Sie wissen, dass „Canonical“ ein Herausgeber für das „UbuntuServer“-Angebot ist.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

Die **URN**-Spalte ist das Formular, das Sie an `azure vm quick-create` weiterleiten.

Oftmals ist jedoch noch unbekannt, was verfügbar ist. In diesem Fall können Sie durch Images navigieren, indem Sie mithilfe von `azure vm image list-publishers` zuerst Herausgeber ermitteln und auf die Speicherort-Eingabeaufforderung mit dem Datencenter-Speicherort antworten, den Sie voraussichtlich für Ihre Ressourcengruppe verwenden werden. Beispielsweise werden im Folgenden alle Herausgeber von Datenträgerabbildern am Speicherort „West US“ aufgelistet (übergeben Sie das Speicherort-Argument mit Kleinbuchstaben und Entfernen von Leerzeichen von Standardspeicherorten).

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Diese Listen können sehr umfangreich sein, daher zeigt die oben dargestellte Beispielliste lediglich einen Ausschnitt. Angenommen, Sie würden bemerken, dass Canonical tatsächlich ein Herausgeber am Speicherort „West US“ ist, dann könnten Sie nun seine Angebote finden, indem Sie `azure vm image list-offers` aufrufen und den Speicherort und den Herausgeber über die Eingabeaufforderung übergeben. Dies ist im folgenden Beispiel dargestellt:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Jetzt wissen wir, dass in der Region „West US“ Canonical das **UbuntuServer** -Angebot in Azure herausgibt. Aber welche SKUs? Rufen Sie `azure vm image list-skus` ab, und antworten Sie auf die Eingabeaufforderung mit dem Standort, Herausgeber und dem von Ihnen entdeckten Angebot, um diese zu erhalten.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

Mit diesen Informationen können Sie nun genau das von Ihnen gewünschte Image finden, indem Sie den ursprünglichen Aufruf oben aufrufen.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

Jetzt können Sie genau das Datenträgerabbild auswählen, das Sie verwenden möchten. Um schnell mithilfe der von Ihnen soeben gefundenen URN-Informationen einen virtuellen Computer zu erstellen oder eine Vorlage mit diesen URN-Informationen zu verwenden, lesen Sie die Informationen unter [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure-Ressourcen-Manager](../articles/xplat-cli-azure-resource-manager.md).

## <a name="powershell"></a>PowerShell
> [!NOTE]
> Installieren und konfigurieren Sie die [neueste Version von Azure PowerShell](../articles/powershell-install-configure.md). Wenn Sie Azure PowerShell-Module vor Version 1.0 nutzen, verwenden Sie die folgenden Befehle, zunächst müssen Sie jedoch `Switch-AzureMode AzureResourceManager`ausführen. 
> 
> 

Wenn Sie mit dem Azure-Ressourcen-Manager einen neuen virtuellen Computer erstellen, kann es sein, dass Sie in einigen Fällen ein Datenträgerabbild mit den folgenden Datenträgerabbild-Eigenschaften angeben müssen:

* Herausgeber
* Angebot
* SKU

Diese Werte werden beispielsweise für das PowerShell-Cmdlet `Set-AzureRMVMSourceImage` oder mit einer Vorlagendatei einer Ressourcengruppe benötigt, in der Sie den Typ des zu erstellenden virtuellen Computers angeben müssen.

Wenn Sie diese Werte angeben müssen, können Sie durch die Images navigieren, um die Werte folgendermaßen zu bestimmen:

1. Auflistung der Herausgeber von Images
2. Auflistung der Angebote eines bestimmten Anbieters
3. Auflistung der SKUs eines bestimmten Angebots

Listen Sie zunächst die Herausgeber mit den folgenden Befehlen auf:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Tragen Sie den von Ihnen gewählten Herausgebernamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Tragen Sie den von Ihnen gewählten Angebotsnamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Sie können der Anzeige des Befehls `Get-AzureRMVMImageSku` alle Informationen entnehmen, die Sie brauchen, um das Image für einen neuen virtuellen Computer anzugeben.

Im Folgenden lernen Sie ein vollständiges Beispiel kennen:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Für den Herausgeber von „MicrosoftWindowsServer“:

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Für das Angebot „WindowsServer“:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Wenn Sie den gewählten SKU-Namen aus dieser Liste kopieren, besitzen Sie alle Informationen für das PowerShell-Cmdlet `Set-AzureRMVMSourceImage` oder eine Ressourcengruppenvorlage.

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/

<!--HONumber=Nov16_HO3-->


