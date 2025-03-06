---
title: Konfigurerar fjärrslutpunkter
description: Lär dig hur du konfigurerar fjärrslutpunkter. I det här dokumentet beskrivs hur du aktiverar program som skapats med Flex för att anropa tjänsten med AEM Forms Remoting.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_endpoints
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: d19b7265-42cc-41d9-9897-e7b044c4529c
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Konfigurerar fjärrslutpunkter {#configuring-remoting-endpoints}

En fjärrkontrollsslutpunkt gör att ett program som skapats med Flex kan anropa tjänsten med hjälp av AEM Forms Remoting (borttaget för AEM-formulär). En fjärrslutpunkt skapas automatiskt för varje aktiverad tjänst. Ett Flex-mål som har samma namn som slutpunkten skapas, och Flex-klienter kan skapa fjärrobjekt som pekar på det här målet för att anropa åtgärder på den aktuella tjänsten.

## Tar bort slutpunktsinställningar {#remoting-endpoint-settings}

**Flex klientautentiseringsmetod:** Bestämmer vilken typ av svar som servern skickar tillbaka till klienten när den anropade tjänsten är säkerhetsaktiverad, den anropade åtgärden inte stöder anonyma anrop och klienten skickar antingen inga eller ogiltiga autentiseringsuppgifter.
