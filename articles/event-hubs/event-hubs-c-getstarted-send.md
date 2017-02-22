---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von C | Microsoft-Dokumentation
description: Senden von Ereignissen an Azure Event Hubs mithilfe von C
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 01/30/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: fe331199333d492dbc42c9125c9da96a44066ee1
ms.openlocfilehash: a6d6db0e0e6b6fe8d8ccbd6cbd3858ae0df5cb2d

---

# <a name="send-events-to-azure-event-hubs-using-c"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von C

## <a name="introduction"></a>Einführung
Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

In diesem Tutorial erfahren Sie, wie Sie mithilfe einer Konsolenanwendung in C Ereignisse an einen Event Hub senden. Klicken Sie zum Empfangen von Ereignissen links im Inhaltsverzeichnis auf die entsprechende Empfangssprache.

Für dieses Tutorial benötigen Sie Folgendes:

* Eine C-Entwicklungsumgebung. In diesem Lernprogramm wird vom GCC-Stack auf einem virtuellen Linux-Computer mit Azure mit Ubuntu 14.04 ausgegangen.
* Microsoft Visual Studio Express für Windows
* Ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis-Hubs
In diesem Abschnitt schreiben wir eine C-App, um Ereignisse an den Event Hub zu senden. Wir verwenden die Proton AMQP-Bibliothek aus dem [Apache Qpid-Projekt](http://qpid.apache.org/). Dies entspricht der Verwendung von Service Bus-Warteschlangen und -Themen mit AMQP aus C, wie [hier](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504)beschrieben. Weitere Informationen finden Sie in der [Qpid Proton-Dokumentation](http://qpid.apache.org/proton/index.html).

1. Klicken Sie auf der Seite [Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html) auf den Link **Installing Qpid Proton** (Installieren von Qpid Proton), und befolgen Sie die Anweisungen für Ihre Umgebung.
2. Um die Proton-Bibliothek zu kompilieren, installieren Sie die folgenden Pakete:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Laden Sie die [Qpid Proton-Bibliothek](http://qpid.apache.org/proton/index.html) herunter, und extrahieren Sie sie, z.B.:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Erstellen Sie einen Build-Verzeichnis, kompilieren und installieren Sie:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Datei namens **sender.c** mit folgendem Inhalt. Vergessen Sie nicht, den Wert für den Namen des Event Hubs und den Namespacenamen (letzterer lautet i. d. R. `{event hub name}-ns`) zu ersetzen. Sie müssen auch eine URL-codierte Version des Schlüssels für das zuvor erstellte **SendRule**-Element eingeben. Die URL-Codierung können Sie [hier](http://www.w3schools.com/tags/ref_urlencode.asp) vornehmen.
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     
      {                                                                          
        if(pn_messenger_errno(messenger))                                        
        {                                                                        
          printf("check\n");                                                     
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); 
        }                                                                        
      }  
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Kompilieren Sie die Datei, wobei **gcc**vorausgesetzt wird:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > In diesem Code verwenden wir ein Ausgabefenster von 1, um eine sofortige Ausgabe der Meldungen zu erzwingen. Im Allgemeinen sollten Anwendungen Nachrichten stapelweise ausgeben, um den Durchsatz zu erhöhen. Weitere Informationen zur Verwendung der Qpid Proton-Bibliothek in dieser und anderen Umgebungen und auf Plattformen, für die Bindungen bereitgestellt werden (derzeit Perl, PHP, Python und Ruby), finden Sie auf der Seite [Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html) (in englischer Sprache).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3



<!--HONumber=Feb17_HO1-->


