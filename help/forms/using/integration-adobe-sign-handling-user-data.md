---
title: Integrering med Adobe Sign | Hantera användardata
description: Lär dig AEM Forms-integrering med Adobe Sign för e-signaturer i anpassningsbara formulär. Det har stöd för flera signeringsalternativ för olika arbetsflöden.
topic-tags: grdp
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Acrobat Sign
role: Admin, User, Developer
solution: Experience Manager, Experience Manager Forms
exl-id: 90521ad8-703e-402b-81dd-4c06f5894358
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Integrering med Adobe Sign | Hantera användardata {#integration-with-adobe-sign-handling-user-data}

[!DNL AEM Forms] integreras med [!DNL &#x200B; Adobe Sign] för att aktivera e-signaturarbetsflöden i adaptiva formulär för att bearbeta formulär eller avtal för arbetsflöden för juridik, försäljning, lön och personalhantering. Det möjliggör signering för en eller flera användare, sekventiella och samtidiga signeringsarbetsflöden, signering av formulär som anonyma eller inloggade användare samt flera sätt att autentisera användare.

När en eller flera signerare signerar och skickar ett anpassat formulär skapas ett [!DNL Adobe Sign]-avtal som innehåller information om signerarna.

Mer information om [!DNL AEM Forms]-integrering med [!DNL Adobe Sign] finns i [Använda Adobe Sign i ett adaptivt formulär](/help/forms/using/working-with-adobe-sign.md).

## Användardata och datalager {#data}

Det adaptiva formuläret [!DNL Adobe Sign] som har aktiverats innehåller information om signerare och kan innehålla andra användardata som samlats in av det adaptiva formuläret. Tjänsten [!DNL Adobe Sign] sparar användardata med signaturen i avtalet. Avtalet sparas på en [!DNL Adobe Sign]-server som är konfigurerad i [!DNL AEM Forms] molntjänster. Om det adaptiva formuläret dessutom är konfigurerat att använda Forms Portal-åtgärden, sparas avtalsdata i Forms Portals datalager tillsammans med formulärdata.

## Få åtkomst till och ta bort användardata {#access-and-delete-user-data}

Användardata samlas in i avtalet men sparas inte i någon av tjänsttabellerna. Med [!DNL Adobe Sign] kan administratörer göra egna val för att hantera de data som de har kontroll över i tjänsten. Sekretessadministratörer för tjänsten [!DNL Adobe Sign] kan visa eller ta bort avtal baserat på e-postadressen till en begärande.

I [!DNL Adobe Sign] finns ett webbprogram som gör att deltagare kan söka efter avtal och vid behov ta bort dem. Mer information finns i [Adobe Sign - funktion: Ta bort användarinformation](https://helpx.adobe.com/sign/help/adobesign_gdpr_user_deletion.html).

Avtalsdata för adaptiva formulär som har konfigurerats att använda Forms Portal-åtgärden för att skicka sparas också i Forms Portal-datalagret. Information om hur du får åtkomst till och tar bort data från Forms Portal-datalagret finns i [Forms Portal | Hantera användardata ](/help/forms/using/forms-portal-handling-user-data.md).
