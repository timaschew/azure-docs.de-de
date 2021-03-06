---
title: Grundlegendes zu iDNS in Azure Stack | Microsoft-Dokumentation
description: Grundlegendes zu iDNS-Features und -Funktionen in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-09/28/2018started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: scottnap
ms.openlocfilehash: 783262a5b55bd645ae3b85c1f00434648d7ee35f
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47584966"
---
# <a name="introducing-idns-for-azure-stack"></a>Einführung in iDNS für Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

iDNS ist ein Netzwerkfeature von Azure Stack, mit dem Sie externe DNS-Namen (z.B. http://www.bing.com.) auflösen können. Darüber hinaus ermöglicht das Feature die Registrierung interner virtueller Netzwerknamen. Auf diese Weise können Sie virtuelle Computer im selben virtuellen Netzwerk nach dem Namen statt der IP-Adresse auflösen. Bei diesem Ansatz müssen keine benutzerdefinierten DNS-Servereinträge angegeben werden. Weitere Informationen zu DNS finden Sie unter [Azure DNS – Übersicht](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>Wozu dient iDNS?

Mit iDNS in Azure Stack erhalten Sie die folgenden Funktionen, ohne benutzerdefinierte DNS-Servereinträge angeben zu müssen:

- Gemeinsam genutzte DNS-Namensauflösungsdienste für Mandantenworkloads.
- Autorisierender DNS-Dienst für Namensauflösung und DNS-Registrierung innerhalb des virtuellen Netzwerks des Mandanten.
- Rekursiver DNS-Dienst für die Auflösung von Internetnamen aus den Mandanten-VMs. Mandanten müssen nicht mehr benutzerdefinierte DNS-Einträge zum Auflösen von Internetnamen (z. B. „www.bing.com“) angeben.

Sie können weiterhin Ihr eigenes DNS und benutzerdefinierte DNS-Server verwenden. Mit iDNS können Sie Internet-DNS-Namen auflösen und eine Verbindung mit anderen virtuellen Computern im gleichen virtuellen Netzwerk herstellen, müssen aber keine benutzerdefinierten DNS-Einträge erstellen.

## <a name="what-doesnt-idns-do"></a>Was ist mit iDNS nicht möglich?

iDNS ermöglicht Ihnen nicht, einen DNS-Eintrag für einen Namen zu erstellen, der von außerhalb des virtuellen Netzwerks aufgelöst werden kann.

In Azure haben Sie die Option, eine DNS-Namensbezeichnung anzugeben, die einer öffentlichen IP-Adresse zugeordnet ist. Sie können die Bezeichnung (das Präfix) auswählen, aber Azure wählt das Suffix, das auf der Region basiert, in der Sie die öffentliche IP-Adresse erstellen.

![Beispiel für eine DNS-Namensbezeichnung](media/azure-stack-understanding-dns-in-tp2/image3.png)

In der Abbildung oben erstellt Azure einen A-Eintrag im DNS für die DNS-Namensbezeichnung, angegeben unter der Zone **westus.cloudapp.azure.com**. Das Präfix und Suffix werden kombiniert, um einen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (Fully Qualified Domain Name, FQDN) zu bilden, der von überall im öffentlichen Internet aufgelöst werden kann.

Azure Stack unterstützt iDNS nur für die Registrierung interner Namen, sodass Folgendes nicht möglich ist:

- Erstellen eines DNS-Eintrags unter einer vorhandenen gehosteten DNS-Zone (z.B. „local.azurestack.external“).
- Erstellen einer DNS-Zone (z.B. „contoso.com“).
- Erstellen eines Eintrags unter Ihrer eigenen benutzerdefinierten DNS-Zone.
- Unterstützen des Erwerbs von Domänennamen.

## <a name="next-steps"></a>Nächste Schritte

[DNS in Azure Stack](azure-stack-dns.md)
