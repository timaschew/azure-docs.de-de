---
title: Festlegen von Umleitungs-URLs zu b2clogin.com für Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Verwenden von b2clogin.com in Umleitungs-URLs für Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7b460efbdc50c5b243c3ef78bad568b720e75e59
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635556"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Festlegen von Umleitungs-URLs zu b2clogin.com für Azure Active Directory B2C

Wenn Sie einen Identitätsanbieter für die Registrierung und Anmeldung in Ihrer Azure Active Directory B2C-Anwendung (Azure AD) einrichten, müssen Sie eine Umleitungs-URL angeben. In der Vergangenheit wurde login.microsoftonline.com verwendet, nun sollten Sie allerdings b2clogin.com verwenden.

Die Verwendung von b2clogin.com bietet zusätzliche Vorteile, z.B.:

- Cookies werden nicht mehr mit anderen Microsoft-Diensten gemeinsam verwendet.
- Ihre URLs enthalten keinen Verweis mehr auf Microsoft. Beispiel: `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`.

Sehen Sie sich diese Einstellungen, die bei Verwendung von „b2clogin.com“ möglicherweise geändert werden müssen:

- Legen Sie die Umleitungs-URLs in den Anwendungen Ihrer Identitätsanbieter für die Verwendung von „b2clogin.com“ fest. 
- Legen Sie in Ihrer Azure AD B2C-Anwendung fest, dass für Richtlinienverweise und Tokenendpunkte „b2clogin.com“ verwendet werden soll. 
- Wenn Sie MSAL verwenden, müssen Sie die **ValidateAuthority**-Eigenschaft auf `false` festlegen.
- Stellen Sie sicher, dass Sie alle **zulässigen Ursprünge** ändern, die Sie in den CORS-Einstellungen zur [Anpassung der Benutzeroberfläche](active-directory-b2c-ui-customization-custom-dynamic.md) definiert haben.  

## <a name="change-redirect-urls"></a>Ändern von Umleitungs-URLs

Um b2clogin.com zu verwenden, suchen Sie in den Einstellungen Ihrer Identitätsanbieteranwendung nach der Liste vertrauenswürdiger URLs für Umleitungen zu Azure AD B2C, und ändern Sie sie.  Derzeit ist dort wahrscheinlich eine Umleitung an eine Website unter login.microsoftonline.com eingerichtet. 

Sie müssen die Umleitungs-URL ändern, damit `your-tenant-name.b2clogin.com` autorisiert ist. Ersetzen Sie dabei unbedingt `your-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten, und entfernen Sie `/te` (sofern in der URL enthalten). Es gibt geringfügige Abweichungen von dieser URL für die einzelnen Identitätsanbieter. Die genaue URL finden Sie auf der entsprechenden Seite.

Informationen zum Einrichten von Identitätsanbietern finden Sie in den folgenden Artikeln:

- [Microsoft-Konto](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [Benutzerdefiniertes OIDC](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Aktualisieren Ihrer Anwendung

Die Azure AD B2C-Anwendung verweist wahrscheinlich an mehreren Stellen auf `login.microsoftonline.com`, z.B. in Ihren Richtlinienverweisen und den Tokenendpunkten.  Vergewissern Sie sich, dass Ihr Autorisierungsendpunkt, Ihr Tokenendpunkt und Ihr Aussteller aktualisiert wurden und `your-tenant-name.b2clogin.com` verwenden.  

## <a name="set-the-validateauthority-property"></a>Festlegen der ValidateAuthority-Eigenschaft

Wenn Sie MSAL verwenden, legen Sie **ValidateAuthority** auf `false` fest. Das folgende Beispiel zeigt eine Möglichkeit, wie Sie die Eigenschaft festlegen können:

In [MSAL für .Net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

In [MSAL für Javascript](https://github.com/AzureAD/microsoft-authentication-library-for-js):

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
