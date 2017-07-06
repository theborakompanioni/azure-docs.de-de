---
title: "Leitfaden für das Erstellen eines Datendiensts für den Marketplace | Microsoft Docs"
description: "Detaillierte Anweisungen zum Erstellen, Zertifizieren und Bereitstellen eines Datendiensts für den Erwerb im Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 148f8638-ee80-4100-8d63-5afa4167ca1b
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f479602647d67e7048371df76b128e33b27c1014
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016


---
# <a name="examples-of-mapping-an-existing-web-service-to-odata-through-csdls"></a>Beispiele für das Zuordnen eines vorhandenen Webdiensts zu OData über CSDLs
> [!IMPORTANT]
> **Derzeit integrieren wir keine neuen Herausgeber von Datendiensten mehr. Für neue Datendienste wird keine Auflistung genehmigt.** Wenn Sie eine SaaS-Geschäftsanwendung haben, die Sie auf AppSource veröffentlichen möchten, finden Sie [hier](https://appsource.microsoft.com/partners) weitere Informationen. Wenn Sie eine IaaS-Anwendung oder einen Dienst für Entwickler auf Azure Marketplace veröffentlichen möchten, finden Sie [hier](https://azure.microsoft.com/marketplace/programs/certified/) weitere Informationen.
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a>Beispiel: FunctionImport für "rohe" Daten wurde mithilfe von "POST" zurückgegeben
Verwenden Sie POST-Rohdaten zum Erstellen einer neuen Unterordnung und zum Zurückgeben seiner serverdefinierten URL (Speicherort) oder zum Aktualisieren eines Teils der Unterordnung in der serverdefinierten URL.  Dabei ist die Unterordnung ein Datenstrom, d.h. unstrukturiert, wie beispielsweise eine Textdatei.  Beachten Sie, dass POST ohne Speicherort nicht idempotent ist.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="AddUsageEvent" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Add usage event (data acquisition)</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-delete"></a>Beispiel: FunctionImport verwendet "DELETE"
Verwenden Sie DELETE, um einen angegebenen URI zu entfernen.

        <EntitySet Name="DeleteUsageFileEntitySet" EntityType="MyOffer.DeleteUsageFileEntity" />
        <FunctionImport Name="DeleteUsageFile" EntitySet="DeleteUsageFileEntitySet" ReturnType="Collection(MyOffer.DeleteUsageFileEntity)"  d:AllowedHttpMethods="DELETE" d:EncodeParameterValues="true” d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643" >
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Delete usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="DeleteUsageFileEntity" d:Map="//boolean">
        <Property Name="boolean" Type="String" Nullable="true" d:Map="./boolean" />
        </EntityType>

## <a name="example-functionimport-using-post"></a>Beispiel: FunctionImport verwendet "POST"
Verwenden Sie POST-Rohdaten zum Erstellen einer neuen Unterordnung und zum Zurückgeben seiner serverdefinierten URL (Speicherort) oder zum Aktualisieren eines Teils der Unterordnung in der serverdefinierten URL.  Dabei ist die Unterordnung eine Struktur. Beachten Sie, dass POST ohne Speicherort nicht idempotent ist.

        <EntitySet Name="CreateANewModelEntitySet2" EntityType=" MyOffer.CreateANewModelEntity2" />
        <FunctionImport Name="CreateModel" EntitySet="CreateANewModelEntitySet2" ReturnType="Collection(MyOffer.CreateANewModelEntity2)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Create A New Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-put"></a>Beispiel: FunctionImport verwendet "PUT"
Verwenden Sie PUT zum Erstellen einer neuen Unterordnung oder zum Aktualisieren der gesamten Unterordnung einer serverdefinierten URL.  Wenn die Unterordnung eine Struktur ist, ist PUT idempotent, weshalb mehrere Vorkommnisse zum selben Zustand führen, d.h. x=5.  PUT sollte mit dem gesamten Inhalt der angegebenen Ressource verwendet werden.

        <EntitySet Name="UpdateAnExistingModelEntitySet" EntityType="MyOffer.UpdateAnExistingModelEntity" />
        <FunctionImport Name="UpdateModel" EntitySet="UpdateAnExistingModelEntitySet" ReturnType="Collection(MyOffer.UpdateAnExistingModelEntity)" d:EncodeParameterValues="true" d:AllowedHttpMethods="PUT" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Update an Existing Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="UpdateAnExistingModelEntity" d:Map="//string">
        <Property Name="string"     Type="String" Nullable="true" d:Map="./string" />
        </EntityType>


## <a name="example-functionimport-for-raw-data-returned-using-put"></a>Beispiel: FunctionImport für "rohe" Daten wurde mithilfe von "PUT" zurückgegeben
Verwenden Sie PUT-Rohdaten zum Erstellen einer neuen Unterordnung oder zum Aktualisieren der gesamten Unterordnung einer serverdefinierten URL.  Dabei ist die Unterordnung ein Datenstrom, d.h. unstrukturiert, wie beispielsweise eine Textdatei.  PUT ist idempotent, weshalb mehrere Vorkommnisse zum selben Zustand führen, d.h. x=5.  PUT sollte mit dem gesamten Inhalt der angegebenen Ressource verwendet werden.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="CancelBuild” ReturnType="Raw(text/plain)" d:AllowedHttpMethods="PUT" d:EncodeParameterValues="true" d:BaseUri=” http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Cancel Build</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>


## <a name="example-functionimport-for-raw-data-returned-using-get"></a>Beispiel: FunctionImport für "rohe" Daten wurde mithilfe von "GET" zurückgegeben
Verwenden Sie GET-Rohdaten, um eine Unterordnung zurückzugeben, die unstrukturiert ist, d. h. Text.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="GetModelUsageFile" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="GET" d:BaseUri="https://cmla.cloudapp.net/api2/model/builder/build?buildId={buildId}&amp;apiVersion={apiVersion}">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Download A Models Usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Accept" d:Value="application/xml,application/xhtml+xml,text/html;" />
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-for-paging-through-returned-data"></a>Beispiel: FunctionImport für 'Paging' durch zurückgegebene Daten".
Verwenden Sie mit GET „Implementieren von RESTful-Paging“ für die Daten.  Das Standardpaging ist auf 100 Zeilen pro Seite an Daten festgelegt.

        <EntitySet Name=”CropEntitySet" EntityType="MyOffer.CropEntity" />
        <FunctionImport    Name="GetCropReport" EntitySet="CropEntitySet” ReturnType="Collection(MyOffer.CropEntity)" d:EmitSelfLink="false" d:EncodeParameterValues="true" d:Paging="SkipTake" d:MaxPageSize="100" d:BaseUri="http://api.mydata.org/Crop? report={report}&amp;series={series}&amp;start={$skip}&amp;size=100">
        <Parameter Name="report" Type="Int32" Mode="In" Nullable="false" d:SampleValues="4"  d:enum="1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19"  />
        <Parameter Name="series"    Type="String"    Mode="In" Nullable="false" d:SampleValues="FARM" />
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="text/xml;charset=UTF-8" />
        </d:Headers>
        <d:Namespaces>
        <d:Namespace d:Prefix="diffgr" d:Uri="urn:schemas-microsoft-com:xml-diffgram-v1" />
        </d:Namespaces>
        </FunctionImport>

## <a name="see-also"></a>Weitere Informationen
* Wenn Sie sich über den gesamten OData-Zuordnungsprozess und seinen Zweck informieren möchten, lesen Sie den Artikel [OData-Zuordnung im Datendienst](marketplace-publishing-data-service-creation-odata-mapping.md) , um Definitionen, Strukturen und Anweisungen kennen zu lernen.
* Wenn Sie mehr über bestimmte Knoten und ihre Parameter erfahren möchten, lesen Sie den Artikel [OData-Zuordnungsknoten für Datendienste](marketplace-publishing-data-service-creation-odata-mapping-nodes.md). Dort finden Sie Definitionen und Erläuterungen, Beispiele und Kontext für Anwendungsfälle.
* Um zum vorgeschriebenen Pfad für die Veröffentlichung eines Datendiensts im Azure Marketplace zurückzukehren, lesen Sie den Artikel [Leitfaden zum Veröffentlichen von Datendiensten](marketplace-publishing-data-service-creation.md).


