---
title: Erstellen einer Azure IoT Hub-Instanz mit dem Azure IoT Hub Toolkit für Visual Studio Code | Microsoft-Dokumentation
description: Hier wird erläutert, wie Sie mit dem Azure IoT Hub Toolkit für VS Code eine IoT Hub-Instanz erstellen.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: cee71ddfbf1a20cc7417976d60b04bff6f0deac8
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339024"
---
# <a name="create-an-iot-hub-using-the-azure-iot-hub-toolkit-for-visual-studio-code"></a>Erstellen einer IoT Hub-Instanz mit dem Azure IoT Hub Toolkit für Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In diesem Artikel erfahren Sie, wie Sie das [Azure IoT Hub Toolkit für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (zuvor Azure IoT Toolkit) zum Erstellen einer Azure IoT Hub-Instanz verwenden. 

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.

2. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). 

   ![Erweitern von „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klicken Sie auf **...** in der Überschrift des Abschnitts **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header. 

4. Klicken Sie auf **IoT Hub erstellen**.

5. In der unteren rechten Ecke wird ein Popupfenster angezeigt, über das Sie sich zum ersten Mal bei Azure anmelden können.

6. Wählen Sie ein Azure-Abonnement aus. 

7. Wählen Sie eine Ressourcengruppe aus.

8. Wählen Sie einen Standort aus.

9. Wählen Sie einen Tarif aus.

10. Geben Sie einen global eindeutigen Namen für Ihre IoT Hub-Instanz ein.

11. Warten Sie einige Minuten, bis die IoT Hub-Instanz erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun eine IoT Hub-Instanz mit dem Azure IoT Hub Toolkit für Visual Studio Code bereitgestellt. Schauen Sie sich für weiterführende Informationen die folgenden Artikel an:

* [Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub mithilfe der Azure IoT Hub Toolkit-Erweiterung für Visual Studio Code](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)

* [Verwenden der Azure IoT Hub Toolkit-Erweiterung für Visual Studio Code für die Geräteverwaltung mit Azure IoT Hub](iot-hub-device-management-iot-toolkit.md)

* [Siehe die Wiki-Seite zum Azure IoT Hub Toolkit](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
