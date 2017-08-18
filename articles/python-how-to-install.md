---
title: "Installieren von Python und SDK – Azure"
description: "Erfahren Sie mehr über die Installation von Python und des SDK zur Verwendung mit Azure."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 3e6bcf301a9257a60e6b921934bb6a04b4dd5d53
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="installing-python-and-the-sdk"></a>Installieren von Python und SDK
Die Installation von Python unter Windows ist einfach. Unter Mac, Linux und [Bash für Windows](https://msdn.microsoft.com/commandline/wsl/about) ist Python bereits vorinstalliert. Dieser Leitfaden führt Sie durch die Installation und bereitet Ihren Computer für den Einsatz mit Azure vor.

## <a name="whats-in-the-python-azure-sdk"></a>Was umfasst das Python Azure SDK?
Das Azure SDK für Python enthält Komponenten, mit deren Hilfe Sie Python-Anwendungen für Azure entwickeln, bereitstellen und verwalten können. Insbesondere enthält das Azure SDK für Python Folgendes:

* **Verwaltungsbibliotheken**. Diese Klassenbibliotheken bieten eine Schnittstelle für die Verwaltung von Azure-Ressourcen wie Speicherkonten und virtuellen Computern.
* **Laufzeitbibliotheken**. Diese Basisklassenbibliotheken bieten eine Schnittstelle für den Zugriff auf Azure-Features wie Speicher und Service Bus.

## <a name="which-python-and-which-version-to-use"></a>Welche Python und welche Version sollte verwendet werden?
Verschiedene Ausführungen von Python-Interpretern sind verfügbar. Einige Beispiele:

* CPython – der standardmäßige und am häufigsten verwendete Python-Interpreter
* PyPy – schnelle, kompatible alternative Implementierung zu CPython
* IronPython – Python-Interpreter, der unter .Net/CLR ausgeführt wird
* Jython – Python-Interpreter, der in JVM (Java Virtual Machine) ausgeführt wird

**CPython** , Version 2.7 oder 3.3 oder höher und PyPy 5.4.0 wurden getestet und unterstützen das Python Azure SDK.

## <a name="where-to-get-python"></a>Wo erhalte ich Python?
Es gibt mehrere Möglichkeiten, CPython zu beziehen:

* Direkt unter [www.python.org][www.python.org]
* Von angesehenen Distributoren wie [www.continuum.io][www.continuum.io], [www.enthought.com][www.enthought.com] oder [www.activestate.com][www.activestate.com]
* Aus der Source erzeugen!

Sofern Sie keine spezifischen Anforderungen haben, empfehlen wir Ihnen die ersten beiden Optionen.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Installation des SDK unter Windows, Linux und MacOS (nur Clientbibliotheken)
Wenn Sie Python bereits installiert haben, können Sie "pip" zum Installieren eines Pakets aller Clientbibliotheken in Ihrer vorhandenen Python 2.7- oder Python 3.3+-Umgebung verwenden. Auf diese Weise werden die Pakete aus dem [Python Package Index][Python Package Index] (PyPI) heruntergeladen.

Möglicherweise benötigen Sie Administratorrechte:

* Linux und MacOS: Verwenden Sie den `sudo`-Befehl `sudo pip install azure-mgmt-compute`.
* Windows: Öffnen Sie eine PowerShell/Eingabeaufforderung als Administrator.

Sie können jede Bibliothek einzeln für jeden Azure-Dienst installieren:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Vorschaupakete können mit dem `--pre` -Flag installiert werden:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Sie können auch eine Gruppe von Azure-Bibliotheken mithilfe des Metapakets `azure` in einer einzelnen Zeile installieren. Da noch nicht alle Pakete in diesem veröffentlichten Metapaket stabil sind, ist das Metapaket `azure` weiter in der Vorschauphase.
Die Kernpakete gelten jedoch hinsichtlich Qualität und Vollständigkeit des Codes derzeit als stabil.

* Sie werden sobald wie möglich in Übereinstimmung mit anderen Sprachen offiziell so bezeichnet werden.
  Bis dahin planen wir keine weiteren größeren Änderungen.

Da es sich um eine Vorabversion handelt, müssen Sie das `--pre` -Flag verwenden:

```console
   $ pip install --pre azure
```

oder direkt

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Beziehen weiterer Pakete
Der [Python Package Index][Python Package Index] (PyPI) verfügt über eine umfassende Auswahl von Python-Bibliotheken.  Wenn Sie eine Distribution installieren, verfügen Sie bereits über die meisten der interessanten Bits für eine Vielzahl von Szenarien – von der Webentwicklung bis hin zum technischem Computing.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[Python-Tools für Visual Studio][Python-Tools für Visual Studio] (PTVS) ist ein kostenloses/Open-Source-Software-Plug-In von Microsoft, das VS in eine vollwertige Python IDE verwandelt:

![how-to-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Die Verwendung von PTVS ist optional, aber empfehlenswert, da Sie so bei Python- und Webprojekten/-lösungen von Support, Debugging, Profilerstellung, interaktiven Fenstern, Vorlagenbearbeitung und IntelliSense profitieren.

PTVS erleichtert auch die Bereitstellung unter Microsoft Azure, mit Unterstützung für die Bereitstellung in [Clouddiensten](cloud-services/cloud-services-python-ptvs.md) und auf [Websites](app-service-web/web-sites-python-ptvs-django-mysql.md).

PTVS funktioniert mit Ihren vorhandenen Installationen von Visual Studio 2013, 2015 oder 2017.  Dokumentation, Downloads und Diskussionen finden Sie unter [Python-Tools für Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure-Szenarien für Linux und MacOS
Für Linux oder Mac OS werden die folgenden wichtigen Azure-Szenarien unterstützt:

1. Nutzung von Azure Services über die Clientbibliotheken für Python
2. Ausführen Ihrer App in einem virtuellen Linux-Computer
3. Entwickeln und Veröffentlichen auf Azure-Websites mit Git

Beim ersten Szenario können Sie vielseitige Web-Apps erstellen, die von den Vorteilen der Azure PaaS-Funktionen profitieren, wie beispielsweise [Blobspeicher](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [Warteschlangenspeicher](storage/storage-python-how-to-use-queue-storage.md), [Tabellenspeicher](storage/storage-python-how-to-use-table-storage.md) usw. über Python-Wrapper für die Azure-REST-APIs. Diese funktionieren unter Windows, Mac und Linux identisch.  Sie können diese Clientbibliotheken auch von Ihrem lokalen Entwicklungscomputer oder einem virtuellen Linux-Computer verwenden, der auf Azure ausgeführt wird.

Bei dem VM-Szenario starten Sie einen virtuellen Linux-Computer Ihrer Wahl (Ubuntu, CentOS, Suse) und führen eine beliebige Anwendung aus bzw. verwalten diese.  Beispielsweise können Sie [IPython][IPython]-REPL/Notebook auf Ihrem Windows-/Mac-/Linux-Computer ausführen und Ihren Browser auf einen virtuellen Linux- oder Windows-Computer mit Multi-Proc verweisen, der die IPython-Engine in Azure ausführt.

Informationen zur Einrichtung eines virtuellen Linux-Computers finden Sie im Tutorial [Erstellen eines virtuellen Linux-Computers](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) .

Mit Git-Bereitstellung können Sie eine Python-Webanwendung entwickeln und sie auf einer Azure-Website von jedem beliebigen Betriebssystem aus veröffentlichen.  Wenn Sie das Repository in Azure übertragen, erstellt es automatisch eine virtuelle Umgebung und installiert mit pip die erforderlichen Pakete.

Weitere Informationen zum Entwickeln und Veröffentlichen von Azure-Websites finden Sie in den Tutorials zum [Erstellen von Websites mit Django](app-service-web/web-sites-python-create-deploy-django-app.md), [Erstellen von Websites mit Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md) und [Erstellen von Websites mit Flask](app-service-web/web-sites-python-create-deploy-flask-app.md). Weitere allgemeine Informationen zur Verwendung aller kompatiblen WSGI-Frameworks finden Sie unter [Konfigurieren von Python mit Azure-Websites](app-service-web/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Weitere Software und Ressourcen:
* [Azure SDK für Python: ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK für Python: GitHub](https://github.com/Azure/azure-sdk-for-python)
* [Offizielle Azure-Beispiele für Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continuum Analytics Python-Distribution][Continuum Analytics Python Distribution]
* [Enthought Python-Distribution][Enthought Python Distribution]
* [ActiveState Python-Distribution][ActiveState Python Distribution]
* [SciPy – Suite wissenschaftlicher Python-Bibliotheken][SciPy - A suite of Scientific Python libraries]
* [NumPy – numerische Bibliothek für Python][NumPy - A numerics library for Python]
* [Django-Projekt – ein ausgereiftes Web-Framework/CMS][Django Project - A mature web framework/CMS]
* [IPython – ein erweitertes REPL/Notebook für Python][IPython - an advanced REPL/Notebook for Python]
* [Python-Tools für Visual Studio auf GitHub][Python Tools for Visual Studio on GitHub]
* [Python Developer Center](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython Notebook on Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python-Tools für Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]: storage-python-how-to-use-blob-storage.md

