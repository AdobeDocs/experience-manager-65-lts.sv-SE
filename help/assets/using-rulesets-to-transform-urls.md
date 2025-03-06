---
title: Använd regeluppsättningar för att omforma URL:er
description: Du kan distribuera regeluppsättningar i Dynamic Media för att omvandla URL:er. Regeluppsättningar är instruktioner skrivna i ett skriptspråk (t.ex. JavaScript) som utvärderar XML-data och utför vissa åtgärder om dessa data uppfyller vissa villkor.
contentOwner: Rick Brough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
topic-tags: dynamic-media
content-type: reference
role: User, Admin,Developer
feature: Configuration,Rulesets
solution: Experience Manager, Experience Manager Assets
exl-id: 8f005ada-275a-444f-9883-64d847fd9959
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '674'
ht-degree: 0%

---

# Använd regeluppsättningar för att omforma URL:er {#using-rulesets-to-transform-urls}

Du kan distribuera regeluppsättningar i Dynamic Media för att omvandla URL:er. Regeluppsättningar är instruktioner skrivna i ett skriptspråk (t.ex. JavaScript) som utvärderar XML-data och utför vissa åtgärder om dessa data uppfyller vissa villkor. Varje regel består av minst ett villkor och minst en åtgärd. En regel utvärderar XML-data mot villkoren, och om ett villkor är uppfyllt utförs rätt åtgärd. Exempel på regeluppsättningar är följande:

* Lägga till ett MIME-typsuffix. Många tjänster och webbplatser kräver bildsuffix, som att lägga till `.jpg` till en URL.
* Skapa en mappsökväg till URL:en för sökmotoroptimering (SEO).

  Se [Så här stöder Adobe Dynamic Media Classic SEO](/help/assets/assets/s7_seo.pdf).

* Lägga till metadata i URL:en för sökmotoroptimering.

  Se [Så här stöder Adobe Dynamic Media Classic SEO](/help/assets/assets/s7_seo.pdf).

* Ställa in innehållets disposition för att utlösa en hämtning.
* Förenkla bildhanteringen genom att ange URL:er för personalisering. Gör till exempel `rgb{XX,YY,ZZ}` till RTF-klar `\redXX\greenYY\blueZZ`

* Begär att vissa tecken ska kodas, till exempel `$`, `{` och `}`, och vissa tecken ska avkodas mot ImageServer. Facebook fungerar till exempel inte så bra med URL:er som innehåller specialtecken.

  Se [Ta bort specialtecken från URL:er](https://helpx.adobe.com/experience-manager/scene7/kb/base/scene7-rulesets/remove-special-characters-urls.html).

När det gäller Dynamic Media kan webbplatser som använder ett XML-baserat system för att hantera resursinformation överföra XML-filer till Dynamic Media. Du kan ange en av dessa filer som förbearbetningsregeluppsättningsfil för att hantera Dynamic Media-resurser. Den här filen omstrukturerar standardformatet för URL-protokoll så att det uppfyller affärslogiken i system som integreras med Dynamic Media. Du anger en XML-fil som ska fungera som sökväg till definitionsfilen för regeluppsättningen.

>[!CAUTION]
>
>Var försiktig när du använder regeluppsättningar. De kan förhindra att dynamiskt medieinnehåll visas på webbplatsen.

Det finns exempellinjaler som kan hjälpa dig att skapa en egen linjaluppsättning.
Se [Referens för regeluppsättning](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/rule-set-reference/c-rule-set-reference.html).

Precis som när du skapar alla regeluppsättningar måste du se till att XML-filen är giltig innan du överför den med ett XML-valideringsprogram som xmlvalid.
Se även [Felsöka regeluppsättningar](https://helpx.adobe.com/experience-manager/scene7/kb/base/scene7-rulesets/scene7-ruleset-troubleshooting.html).

Kontrollera också först att du testar regeluppsättningen i en staging-miljö som inte påverkar produktionsmiljön.
Produktionsmiljöer och staging-miljöer kräver normalt olika inloggningar.

Se [Adobe Dynamic Media Classic-datorprogrammet för inloggningsinformation](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#sign-in-dmc-app).

<!-- OBSOLETE INFORMATION * **NA staging environment** login page: [https://s7sps1-staging.scene7.com/IpsWeb/](https://s7sps1-staging.scene7.com/IpsWeb/)
* **EMEA staging environment** login page: [https://s7sps3-staging.scene7.com/IpsWeb/](https://s7sps3-staging.scene7.com/IpsWeb/)
* **JAPAC staging environment** login page: [https://s7sps5-staging.scene7.com/IpsWeb/](https://s7sps5-staging.scene7.com/IpsWeb/) -->

Se även [Använd resursen i stället för bilden är i en regeluppsättning](https://helpx.adobe.com/experience-manager/scene7/kb/base/scene7-rulesets/ruleset-asset-instead-image.html).

**Så här distribuerar du XML-regeluppsättningar:**

1. Logga in på ditt [Dynamic Media Classic-datorprogram](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/signing-out.html#sign-in-dmc-app).

   Dina inloggningsuppgifter och inloggningsuppgifter tillhandahölls av Adobe vid tidpunkten för etableringen. Om du inte har den här informationen kan du kontakta Adobe kundsupport.

1. Överför regeluppsättningsfilen genom att göra följande:

   * Välj **[!UICONTROL Upload]** i fältet Global navigering.
   * På sidan **[!UICONTROL Upload]**, nära det övre vänstra hörnet, väljer du **[!UICONTROL Browse]**.
   * Bläddra till regeluppsättningsfilen (XML) i dialogrutan **[!UICONTROL Open]**.
   * Markera filen och välj sedan **[!UICONTROL Open]**.
   * Till höger på sidan **[!UICONTROL Upload]** väljer du en målmapp för regeluppsättningsfilen.
   * Kontrollera att **[!UICONTROL Publish After Uploading]** är markerat i närheten av sidans nederkant.
   * Välj **[!UICONTROL Submit Upload]** längst ned till höger på sidan.
   * I fältet Global navigering väljer du **[!UICONTROL Jobs]** för att kontrollera överföringsjobbets status. När kolumnen **[!UICONTROL Status]** på sidan **[!UICONTROL Job]** säger Överföring klar fortsätter du till nästa steg.

1. Välj **[!UICONTROL Setup]** > **[!UICONTROL Application Setup]** > **[!UICONTROL Publish Setup]** > **[!UICONTROL Image Server]** i navigeringsfältet nära sidans överkant.
1. På sidan **[!UICONTROL Image Server Publish]**, under gruppen **[!UICONTROL Catalog Management]**, letar du reda på **[!UICONTROL Rule Set Definition File Path]** och väljer sedan **[!UICONTROL Select]**.
1. På sidan **[!UICONTROL Select Rule Set Definition File (XML)]** bläddrar du till regeluppsättningsfilen och väljer **[!UICONTROL Select]** i det nedre högra hörnet på sidan.
1. Välj **[!UICONTROL Close]** i det nedre högra hörnet på sidan Inställningar.
1. Kör ett Image Server-publiceringsjobb.

   Regeluppsättningsvillkoren tillämpas på begäranden till dynamiska mediabildsservrar.

   Om du ändrar regeluppsättningsfilen tillämpas ändringarna omedelbart när du överför och publicerar den uppdaterade regeluppsättningsfilen igen.
