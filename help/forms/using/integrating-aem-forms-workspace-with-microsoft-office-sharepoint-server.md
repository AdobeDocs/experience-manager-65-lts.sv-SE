---
title: Integrera AEM blankettarbetsyta med Microsoft Office SharePoint Server
description: Du kan integrera AEM blankettarbetsyta med Microsoft Office SharePoint Server.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 907e3702-a71b-4e25-b52b-f33cbb43009a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Integrera AEM blankettarbetsyta med Microsoft Office SharePoint Server{#integrating-aem-forms-workspace-with-microsoft-office-sharepoint-server}

**- Krav**

**Krävande kunskaper**
Innan du kan lägga till AEM Forms Workspace i SharePoint Server måste du ha tillgång till SharePoint Server med rätt behörighet och du måste känna till webbadressen för att få tillgång till Workspace. Stegen nedan förutsätter att du har kunskap om SharePoint Server. Mer information om webbdelar i SharePoint Server finns i Webbdelar i Windows SharePoint Services.

**Användarnivå**
Början

Du kan använda AEM Forms Workspace som webbdel i Microsoft Office SharePoint Server (till exempel Microsoft Office SharePoint Server 2007). Användare har tillgång till AEM Forms Workspace genom att ansluta till din SharePoint Server via en webbläsare för att ge en enhetlig upplevelse. I den här artikeln får du lära dig de grundläggande stegen för att visa AEM Forms Workspace som en webbdel i Microsoft Office SharePoint Server. Du kan utföra stegen som beskrivs i den här artikeln för att ge en enhetlig upplevelse så att användare som ansluter till din SharePoint-server kan komma åt AEM Forms Workspace från samma port.

>[!NOTE]
>
>Stegen i den här artikeln är specifika för Microsoft SharePoint Server 2007. Du kan också konfigurera HTML Workspace med andra versioner av Microsoft SharePoint som stöds.

## Integrera AEM Forms Workspace med Microsoft Office SharePoint Server 2007 {#integrate-aem-forms-workspace-with-microsoft-office-sharepoint-server}

Så här integrerar du AEM Forms Workspace i en webbdel:

1. I en webbläsare navigerar du till SharePoint-webbplatsen, till exempel `https://[myMOSSserver]:44299/default.aspx` där `[myMOSSserver]` är namnet eller IP-adressen för SharePoint-servern.

   >[!NOTE]
   >
   >44299 är standardportnumret för SharePoint-servern. Portnumret beror på din installation av SharePoint Server.

1. Klicka på **Webbplatsåtgärder** i det övre högra hörnet på webbsidan och välj **Redigera sida**.
1. Klicka på knappen **Lägg till en webbdel**.
1. I dialogrutan Lägg till webbdelar - webbsida väljer du **Webbdel för sidvisning** under Övrigt och klickar sedan på **Lägg till**.
1. Klicka på **redigera** i rutan Webbdel för sidvisning och välj **Ändra delad webbdel**.

   >[!NOTE]
   >
   >Rutan Webbdel för sidvisningsprogrammet visas under knappen **Lägg till en webbdel** som du klickade på i steg 3 enligt följande bild (bild 1):

   ![Rutan Webbdel för sidvisning i Microsoft Office SharePoint-servern.](assets/page-viewer-web-part-box-in-microsoft-office-sharepoint-server.png)

   Figur 1. - The Page Viewer Web Part box in Microsoft Office SharePoint server.

1. Utför följande åtgärder på sidan för sidvisningsprogrammet:

   1. I rutan Länk anger du URL:en för AEM Forms Workspace, till exempel `https://[AEM_forms_Server]:8080/lc/ws` där `[AEM_forms_Server]` representerar IP-adressen eller namnet för AEM Forms Server.
   1. Klicka på **Utseende** och ändra höjd, bredd och titel så att du kan se hela Workspace användargränssnitt. Du kan till exempel ange höjd och bredd till 6 tum respektive 11 tum.
   1. Klicka på **Testa länk**. Ett nytt webbläsarfönster visas med Workspace.
   1. (Valfritt) Klicka på **Layout** och ändra layouten för Workspace i webbdelen.
   1. (Valfritt) Klicka på **Avancerat** och ändra andra inställningar, till exempel beskrivningen och om Workspace kan minimeras eller stängas i webbdelen.

      Klicka på **Använd**.

1. Klicka på **Avsluta redigeringsläget** och kontrollera att du har åtkomst till Workspace.

När du är klar med stegen ovan ser din SharePoint-webbplats ut ungefär som på följande bild (bild 2):

![AEM Forms Workspace integrerat med Microsoft Office SharePoint Server](assets/aem-forms-workspace.jpg)

Bild 2 - AEM Forms Workspace integrerat med Microsoft Office SharePoint Server
