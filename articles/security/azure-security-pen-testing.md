<properties
   pageTitle="Penetrationstests | Microsoft Azure"
   description="Der Artikel bietet einen Überblick über den Penetrationstestprozess und dazu, wie Sie Penetrationstests an Ihren in der Azure-Infrastruktur ausgeführten Apps durchführen können."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="yurid"/>

# Penetrationstests

Einer der größten Vorteile von Microsoft Azure bei Test und Bereitstellung von Anwendungen ist, dass Sie keine lokale Infrastruktur zum Entwickeln, Testen und Bereitstellen der Anwendungen zusammenstellen müssen. Die Microsoft Azure-Plattformdienste stellen die gesamte Infrastruktur bereit. Sie müssen sich keine Gedanken über Anforderung, Erwerb und Aufbau eigener lokaler Hardware machen.

Das ist hervorragend – Sie müssen jedoch noch sicherstellen, dass Sie mit angemessener Sorgfalt für Sicherheit sorgen. Sie müssen u.a. einen Penetrationstest der Anwendungen durchführen, die Sie in Azure bereitstellen.

Möglicherweise wissen Sie bereits, dass Microsoft einen [Penetrationstests der Azure-Umgebung](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e) durchführt. Dies hilft uns, unsere Plattform zu verbessern, und ist wegweisend für unsere Aktionen im Hinblick auf die Verbesserung der Sicherheitskontrollen, die Einführung neuer Sicherheitskontrollen und die Verbesserung unserer Sicherheitsprozesse.

Wir führen keinen Penetrationstest Ihrer Anwendung durch, aber wir verstehen, dass Sie Penetrationstests Ihrer eigenen Anwendungen durchführen möchten und müssen. Das ist eine gute Sache, denn wenn Sie die Sicherheit Ihrer Anwendungen verbessern, machen Sie das gesamte Azure-Ökosystem sicherer.

Wenn Sie einen Penetrationstest Ihrer Anwendungen durchführen, kann dies von unserer Seite aus wie ein Angriff aussehen. Wir [überwachen kontinuierlich](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx), ob Angriffsmuster auftreten, und initiieren ggf. einen Sicherheitsvorfall-Reaktionsprozess. Es hilft weder Ihnen noch uns, wenn wir aufgrund Ihrer im Rahmen angemessener Sorgfalt durchgeführten Penetrationstests einen Sicherheitsvorfall-Reaktionsprozess initiieren.

Vorgehensweise

Wenn Sie bereit sind, einen Penetrationstest Ihrer auf Azure gehosteten Anwendungen durchzuführen, müssen Sie uns das mitteilen. Sobald wir wissen, dass Sie bestimmte Tests durchführen möchten, werden wir Sie nicht versehentlich herunterfahren (z.B. die IP-Adresse blockieren, über die Sie testen), solange Ihre Tests den Bedingungen von Azure für Penetrationstests entsprechen. Folgende Standardtests können Sie durchführen:

- Tests an Ihren Endpunkten, um die [wichtigsten 10 OWASP-Sicherheitsrisiken (Open Web Application Security Project)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) aufzudecken
- [Fuzzing](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) Ihrer Endpunkte
- [Portüberwachung](https://en.wikipedia.org/wiki/Port_scanner) Ihrer Endpunkte

Ein Testtyp, denn Sie nicht ausführen können, ist jegliche Form eines [Denial-of-Service](https://en.wikipedia.org/wiki/Denial-of-service_attack)-Angriffs (DoS). Dies schließt einen selbst initiierten DoS-Angriff bzw. das Durchführen zugehöriger Tests ein, die einen DoS-Angriff bestimmen, demonstrieren oder simulieren könnten.

Sind Sie bereit für Penetrationstests Ihrer in Microsoft Azure gehosteten Anwendungen? Wenn dies der Fall ist, wechseln Sie zur [Penetrationstest-Übersichtsseite](https://security-forms.azure.com/penetration-testing/terms) (und klicken Sie am unteren Rand der Seite auf die Schaltfläche „Create a Testing Request“ ). Außerdem finden Sie weitere Informationen zu den Bedingungen für Penetrationstests und nützliche Links dazu, wie Sie Sicherheitsprobleme im Zusammenhang mit Azure oder einem anderen Microsoft-Dienst berichten können.

<!---HONumber=AcomDC_0810_2016-->