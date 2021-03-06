---
title: Informationen zu nicht verwalteten (Seitenblobs) und verwalteten Datenträgern für virtuelle Microsoft Azure-Linux-Computer | Microsoft-Dokumentation
description: Machen Sie sich mit den Grundlagen zu nicht verwalteten (Seitenblobs) und verwalteten Datenträgern für virtuelle Linux-Computer in Azure vertraut.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.component: disks
ms.openlocfilehash: eec7b64836819f840702bb715f4fcc0573a94b00
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251848"
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Informationen zu Datenträgern für virtuelle Azure-Linux-Computer
Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger: einen Datenträger mit dem Linux-Betriebssystem und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die in einem Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden.

In diesem Artikel werden die unterschiedlichen Verwendungsmöglichkeiten der Datenträger erörtert und die verschiedenen Typen von Datenträgern erläutert, die Sie erstellen und nutzen können. Dieser Artikel ist auch für [virtuelle Windows-Computer](../windows/about-disks-and-vhds.md)verfügbar.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Von VMs verwendete Datenträger

Werfen wir einen Blick darauf, wie die Datenträger von den virtuellen Computern verwendet werden.

## <a name="operating-system-disk"></a>Betriebssystem-Datenträger

Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Dieser ist standardmäßig als SATA-Laufwerk registriert und mit „/dev/sda“ bezeichnet. Dieser Datenträger hat eine maximale Kapazität von 2048 Gigabytes (GB).

## <a name="temporary-disk"></a>Temporärer Datenträger

Jede VM verfügt über einen temporären Datenträger. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) verloren gehen, oder wenn Sie [eine VM erneut bereitstellen](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Während eines standardmäßigen Neustarts der VM sollten die Daten auf dem virtuellen Datenträger erhalten bleiben. Es gibt jedoch Fälle, in denen die Daten möglicherweise nicht erhalten bleiben, z.B. beim Verschieben auf einen neuen Host. Entsprechend sollten sich auf dem temporären Datenträger keine Daten befinden, die für das System von entscheidender Bedeutung sind.

Auf virtuellen Linux-Computern lautet der Datenträger in der Regel **/dev/sdb**. Er wird vom Azure Linux-Agent formatiert und in **/mnt/** eingebunden. Die Größe des temporären Datenträgers variiert basierend auf der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer in Azure](../windows/sizes.md).

## <a name="data-disk"></a>Datenträger

Ein Datenträger ist eine VHD, die zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger weist eine maximale Kapazität von 4095 GB auf. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

> [!NOTE]
> Weitere Informationen zu Kapazitäten virtueller Computer finden Sie unter [Größen für virtuelle Computer in Azure](./sizes.md).

Azure erstellt einen Betriebssystem-Datenträger, wenn Sie einen virtuellen Computer aus einem Image erstellen. Wenn das verwendete Image Datenträger enthält, erstellt Azure auch diese Datenträger beim Erstellen des virtuellen Computers. Andernfalls fügen Sie die Datenträger nach dem Erstellen des virtuellen Computers hinzu.

Sie können jederzeit Datenträger einem virtuellen Computer hinzufügen, indem Sie den Datenträger dem virtuellen Computer **anfügen** . Sie können eine VHD verwenden, die Sie in das Speicherkonto hochgeladen oder kopiert haben, oder eine VHD, die Azure für Sie erstellt hat. Beim Anfügen eines Datenträgers wird die VHD-Datei durch eine „Lease“ mit dem virtuellen Computer verknüpft, sodass die VHD nicht aus dem Speicher gelöscht werden kann, solange sie angefügt ist.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

In unseren [häufig gestellten Fragen](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged) erfahren Sie, in welchen Regionen Vorschaugrößen verfügbar sind.

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Anfügen eines Datenträgers](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , um zusätzlichen Speicherplatz für den virtuellen Computer hinzuzufügen.
* [Erstellen einer Momentaufnahme](snapshot-copy-managed-disk.md).
* [Konvertieren in Managed Disks](convert-unmanaged-to-managed-disks.md).