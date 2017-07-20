---
title: Verwalten von Service Fabric-Anwendungen mithilfe von Azure CLI 2.0
description: Beschreibt den Prozess der Bereitstellung und Entfernung von Anwendungen aus einem Service Fabric-Cluster mithilfe von Azure CLI 2.0
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: a99819f6a1c0ef31e14c95b6bd47138feb05053f
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
# <a name="manage-service-fabric-application-using-azure-cli-20"></a>Verwalten der Service Fabric-Anwendung mithilfe von Azure CLI 2.0

Befolgen Sie diese Dokumentation zum Erstellen und Löschen von Anwendungen, die in einem Service Fabric-Cluster ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Achten Sie darauf, dass Sie Azure CLI 2.0 installieren und Ihren Service Fabric-Cluster auswählen. Weitere Informationen finden Sie unter [Erste Schritte mit der Azure CLI 2.0-Dokumentation](service-fabric-azure-cli-2-0.md).

Ihnen müsste auch ein Service Fabric-Anwendungspaket vorliegen, das nun bereitgestellt werden kann. Weitere Informationen zum Erstellen und Packen einer Anwendung finden Sie unter [Anwendungsmodelldokumentation](service-fabric-application-model.md).

## <a name="overview"></a>Übersicht

Das Bereitstellen einer neuen Anwendung besteht aus vier Schritten:

1. Laden Sie ein Anwendungspaket in den Service Fabric-Imagespeicher hoch
1. Bereitstellen eines Anwendungstyps
1. Festlegen und erstellen einer Anwendung
1. Festlegen und Erstellen von Diensten

Zum Entfernen einer vorhandenen Anwendung sind drei Schritte erforderlich:

1. Löschen der Anwendung
1. Aufheben der Bereitstellung des verknüpften Anwendungstyps
1. Löschen von Imagespeicherinhalt

## <a name="deploy-a-new-application"></a>Hinzufügen einer neuen Anwendung

Um eine neue Anwendung bereitzustellen, gehen Sie folgendermaßen vor

### <a name="upload-a-new-application-package-to-the-image-store"></a>Laden Sie das Anwendungspaket in den Imagespeicher hoch

Vor dem Erstellen einer Anwendung, muss das Anwendungspaket in den Service Fabric-Imagespeicher hochgeladen werden.
Angenommen, dass Ihr Anwendungspaket im `app_package_dir`-Verzeichnis vorhanden ist. Verwenden Sie die folgenden Befehle, um das Verzeichnis hochzuladen:

```azurecli
az sf application upload --path ~/app_package_dir
```

Für große Anwendungspakete können Sie die `--show-progress`-Option festlegen, um den Status des Uploads angezeigt zu bekommen.

### <a name="provision-application-type"></a>Bereitstellen eines Anwendungstyps

Nachdem der Upload abgeschlossen ist, muss die Anwendung bereitgestellt werden. Verwenden Sie den folgenden Befehl zum Bereitstellen der Anwendung

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

Die `application-type-build-path` ist identisch mit dem Namen des Verzeichnisses, das das Anwendungspaket enthält, das zuvor hochgeladen wurde

### <a name="create-application-from-application-type"></a>Erstellen von Anwendungen über Anwendungstyp

Nachdem die Anwendung bereitgestellt wurde, können Sie die Anwendung mithilfe des folgenden Befehls benennen und erstellen:

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

Hier `app-name` ist der Name, den Sie der Instanz der Anwendung geben möchten. Die anderen Parameter können auf dem Anwendungsmanifest gefunden werden, das zuvor bereitgestellt wurde.

Der Anwendungsname muss mit dem `fabric:/`-Präfix beginnen.

### <a name="create-services-for-the-new-application"></a>Dienste für die neue Anwendung erstellen

Nachdem eine Anwendung erstellt wurde, können Sie Dienste aus der Anwendung erstellen. In diesem Beispiel erstellen wir einen neuen zustandslosen Dienst aus unserer Anwendung. Die Dienste, die Sie aus einer Anwendung erstellen, können in einem Dienstmanifest in dem zuvor bereitgestellten Anwendungspaket definiert werden.

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-creation-and-health"></a>Überprüfen Sie die Erstellung der Anwendung und der Integrität

Um sicherzustellen, dass eine Anwendung und der Dienst erfolgreich bereitgestellt wurden, können Sie überprüfen, ob die Anwendung und der Dienst aufgeführt sind, um die folgenden Befehlen zu verwenden:

```azurecli
az sf application list
az sf service list --application-list TestApp
```

Um sicherzustellen, dass der Dienst fehlerfrei ist, verwenden Sie ähnliche Befehle zum Abrufen der Integrität des Dienstes und der Anwendung

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

Fehlerfreie Dienste und Anwendungen müssen einen `HealthState`-Wert von `Ok` aufweisen.

## <a name="remove-an-existing-application"></a>Entfernen einer vorhandenen Anwendung

Um eine Anwendung zu entfernen, gehen Sie folgendermaßen vor

### <a name="delete-the-application"></a>Löschen der Anwendung

Führen Sie den folgenden Befehl aus, um die Anwendung zu löschen

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Aufheben der Bereitstellung des Anwendungstyps

Sobald die Anwendung gelöscht wurde, kann die Bereitstellung des Anwendungstyps aufgehoben werden, wenn Sie diese nicht mehr benötigen. Verwenden Sie den folgenden Befehl zum Aufheben der Bereitstellung des Anwendungstyps

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

Hier müssen der Typname und die Typversion mit dem Namen und der Version im Anwendungsmanifest übereinstimmen, das vorher schon bereitgestellt wurde

### <a name="delete-application-package"></a>Löschen eines Anwendungspakets

Nach der Aufhebung der Bereitstellung des Anwendungstyps, kann das Anwendungspaket aus dem Imagespeicher gelöscht werden, wenn Sie dieses nicht mehr benötigen. Das Löschen von Anwendungspaketen gibt Speicherplatz frei. Verwenden Sie den folgenden Befehl, um das Anwendungspaket aus dem Imagespeicher zu löschen:

```azurecli
az sf application package-delete --content-path app_package_dir
```

Hier muss `content-path` den gleichen Namen wie das Verzeichnis haben, das anfänglich beim Erstellen der Anwendung hochgeladen wurde

## <a name="related-articles"></a>Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

