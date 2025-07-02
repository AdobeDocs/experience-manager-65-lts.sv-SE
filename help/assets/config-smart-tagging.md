---
title: Konfigurera resurstaggning med Smart Content Service
description: Lär dig hur du konfigurerar smart taggning och förbättrad smart taggning i  [!DNL Adobe Experience Manager] med hjälp av Smart Content Service.
role: Admin
feature: Tagging,Smart Tags
solution: Experience Manager, Experience Manager Assets
exl-id: be7c294c-149b-4825-8376-573f9e2987e2
source-git-commit: 1cedead501597fb655c2c7b87336b29cbf048294
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 14%

---

# Förbered [!DNL Assets] för smart taggning {#configure-asset-tagging-using-the-smart-content-service}

Innan du kan börja tagga dina resurser med smarta innehållstjänster måste du integrera [!DNL Experience Manager Assets] med Adobe Developer Console för att använda den smarta tjänsten i [!DNL Adobe Sensei]. När konfigurationen är klar kan du utbilda tjänsten med några bilder och en tagg.
Innan du använder tjänsten för smart innehåll bör du kontrollera följande:

* [Integrera med Adobe Developer Console](#integrate-adobe-io).
* [Logga in på tjänsten för smart innehåll](#training-the-smart-content-service).
* Installera den senaste [[!DNL Experience Manager] Service Pack](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=sv-SE).

>[!IMPORTANT]
>
>Mer information om konfigurationen av smarta taggar i AEM 6.5 finns i [Förbered Assets för smart taggning](https://experienceleague.adobe.com/sv/docs/experience-manager-65/content/assets/administer/config-smart-tagging).

**Nya användare**

Smarta innehållstjänster är inte längre tillgängliga för nya [!DNL Experience Manager Assets] On-Premise-användare.

**Befintliga användare**

Befintliga lokala användare, som redan har den här funktionen aktiverad, kan fortsätta använda smarta innehållstjänster.

## Integrera med Adobe Developer Console {#integrate-adobe-io}

När du integrerar med Adobe Developer Console autentiserar servern [!DNL Experience Manager] dina tjänstinloggningsuppgifter med Adobe Developer Console gateway innan du vidarebefordrar din begäran till Smart Content Service. För att kunna integrera behöver du ett Adobe ID-konto som har administratörsbehörighet för organisationen och som har köpts och aktiverats för din organisation.

Så här konfigurerar du tjänsten Smart Content:

1. Skapa en integrering i [Adobe Developer Console](#create-adobe-io-integration).

1. Skapa [IMS-konfiguration för tekniskt konto](#create-ims-account-config) med API-nyckeln och andra autentiseringsuppgifter från Adobe Developer Console.

1. [Konfigurera Smart Content-tjänsten](#configure-smart-content-service).

1. [Testa konfigurationen](#validate-the-configuration).

### Integrering med Adobe Developer Console {#create-adobe-io-integration}

Om du vill använda API:er för smarta innehållstjänster skapar du en integrering i Adobe Developer Console för att få [!UICONTROL API Key] (som genereras i fältet [!UICONTROL CLIENT ID] för Adobe Developer Console-integrering), [!UICONTROL ORGANIZATION ID] och [!UICONTROL CLIENT SECRET] för [!UICONTROL Assets Smart Tagging Service Settings] av molnkonfigurationen i [!DNL Experience Manager].

1. Gå till [https://developer.adobe.com](https://developer.adobe.com/) i en webbläsare. Välj rätt konto och verifiera att den associerade organisationsrollen är system **administrator**.

1. Skapa ett projekt med valfritt namn. Klicka på **[!UICONTROL Add API]**.

1. På sidan **[!UICONTROL Add an API]** markerar du **[!UICONTROL Experience Cloud]** och väljer **[!UICONTROL Smart Content]**. Klicka på **[!UICONTROL Next]**.

1. Välj **[!UICONTROL OAuth Server-to-Server]**. Klicka på **[!UICONTROL Next]**.
Mer information om hur du gör den här konfigurationen finns i Developer Console-dokumentationen, beroende på dina krav:

   * Översikt
      * [Server till server-autentisering](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)

   * Skapa en ny OAuth-autentiseringsuppgift:
      * [Implementeringshandbok för autentiseringsuppgifter för OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation)

   * Migrera en befintlig JWT-autentiseringsuppgift till en OAuth-autentiseringsuppgift:
      * [Migrerar från JWT-autentiseringsuppgifter (Service Account) till autentiseringsuppgifter för OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration)


1. Välj **[!UICONTROL Select product profiles]** på sidan **[!UICONTROL Smart Content Services]**. Klicka på **[!UICONTROL Save configured API]**.

   En sida visar mer information om konfigurationen. Håll den här sidan öppen för att kopiera och lägga till dessa värden i [!UICONTROL Assets Smart Tagging Service Settings] av molnkonfigurationen i [!DNL Experience Manager] för att konfigurera smarta taggar.

   ![OAuth-autentiseringsuppgifter i Developer Console](assets/ims-configuration-developer-console.png)

### Skapa konfiguration av tekniskt IMS-konto {#create-ims-account-config}

Du måste skapa en teknisk kontokonfiguration för IMS enligt stegen nedan:

1. I användargränssnittet för [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Security]** > **[!UICONTROL Adobe IMS Configurations]**.

1. Klicka på **[!UICONTROL Create]**.

1. Använd följande värden i dialogrutan Konfiguration av IMS-konto:

   ![Konfigurationsfönstret för Adobe IMS](assets/adobe-ims-config.png)

   | Fält | Beskrivning |
   | -------- | ---------------------------- |
   | Molnlösning | Välj **[!UICONTROL Smart Tags]** i listrutan. |
   | Titel | Lägg till namnet på det konfigurerande IMS-kontot. |
   | Auktoriseringsserver | Lägg till `https://ims-na1.adobelogin.com` |
   | Klient-ID | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Klienthemlighet | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Omfång | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |
   | Organisations-ID | Tillhandahålls via [Adobe Developer-konsolen](https://developer.adobe.com/console/). |

1. Markera konfigurationen som du har skapat och klicka på **[!UICONTROL Check Health]**.

1. Bekräfta dialogrutan för att kontrollera hälsa och klicka på Stäng när konfigurationen är i felfritt läge.

### Skapa en ny konfiguration {#configure-smart-content-service}

Om du vill konfigurera integreringen använder du värdena för fälten [!UICONTROL TECHNICAL ACCOUNT ID], [!UICONTROL ORGANIZATION ID], [!UICONTROL CLIENT SECRET] och [!UICONTROL CLIENT ID] från Adobe Developer Console-integreringen. Om du skapar en molnkonfiguration för smarta taggar kan API-begäranden från distributionen [!DNL Experience Manager] autentiseras.

1. I [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Cloud Service]** > **[!UICONTROL Smart Tag]** för att öppna [!UICONTROL Smart Tag Configurations].

1. Klicka på **[!UICONTROL Create]** om du vill skapa en ny konfiguration. Annars klickar du på **[!UICONTROL Properties]** för att uppdatera den befintliga konfigurationen.

1. Fyll i följande fält:

   ![Konfiguration av smarta taggar](assets/smart-tags-config.png)

   | Fält | Beskrivning |
   | -------- | ---------------------------- |
   | Titel | Lägg till namnet på det konfigurerande IMS-kontot. |
   | Associerad Adobe IMS-konfiguration | Välj konfiguration i listrutan. |
   | Tjänst-URL | `https://smartcontent.adobe.io/<region where your Experience Manager author instance is hosted>`. Exempel: `https://smartcontent.adobe.io/apac`. Du kan ange `na`, `emea` eller `apac` som de områden där din Experience Manager-författarinstans finns. |

   >[!NOTE]
   >
   >Om Experience Manager hanterade tjänst etableras före 1 september 2022 använder du följande tjänst-URL:
   >`https://mc.adobe.io/marketingcloud/smartcontent`

1. Klicka på **[!UICONTROL Save & Close]**.

### Validera konfigurationen {#validate-the-configuration}

När du har slutfört konfigurationen kan du använda en JMX MBean för att validera konfigurationen. Följ de här stegen för att validera.

1. Gå till din [!DNL Experience Manager]-server på `https://[aem_server]:[port]`.

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** för att öppna OSGi-konsolen. Klicka på **[!UICONTROL Main]>[!UICONTROL JMX]**.

<!--
1. Click `com.day.cq.dam.similaritysearch.internal.impl`. It opens **[!UICONTROL SimilaritySearch Miscellaneous Tasks]**.-->

1. Klicka på `com.day.cq.dam.similaritysearch.internal.impl (SCS)`.

   ![Mbean-fönster](assets/mbean.png)

1. Klicka på `validateConfigs()`. I dialogrutan **[!UICONTROL Validate Configurations]** klickar du på **[!UICONTROL Invoke]**.

Valideringsresultaten visas i samma dialogruta.

### Aktivera smart taggning i arbetsflödet [!UICONTROL DAM Update Asset] (valfritt) {#enable-smart-tagging-in-the-update-asset-workflow-optional}

1. Gå till [!DNL Experience Manager] > **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** i **[!UICONTROL Models]**.

1. Välj arbetsflödesmodellen **[!UICONTROL DAM Update Asset]** på sidan **[!UICONTROL Workflow Models]**.

1. Klicka på **[!UICONTROL Edit]** i verktygsfältet.

1. Expandera sidopanelen för att visa stegen. Dra steget **[!UICONTROL Smart Tag Asset]** som finns i avsnittet med DAM-arbetsflöden och placera det efter steget **[!UICONTROL Process Thumbnails]**.

   ![Lägg till resurssteget för smarta taggar efter steget med processminiatyrer i arbetsflödet för DAM-uppdatering av resurser](assets/smart-tag-in-dam-update-asset-workflow.png)

1. Öppna egenskaperna för steget för att ändra informationen. Under **[!UICONTROL Advanced Settings]** kontrollerar du att alternativet **[!UICONTROL Handler Advance]** är markerat.

   ![Konfigurera arbetsflödet för DAM-uppdatering och lägg till smart tagg](assets/smart-tag-step-properties-workflow1.png)

1. Välj **[!UICONTROL Arguments]** på fliken **[!UICONTROL Ignore Errors]** om du vill att arbetsflödet ska slutföras även om steget med automatisk taggning misslyckas.

   Om du dessutom vill tagga resurser när de överförs, oavsett om smart taggning är aktiverat för mappar, väljer du **[!UICONTROL Ignore Smart Tag Flag]**.

   ![Konfigurera arbetsflödet för DAM-uppdatering av resurs för att lägga till smart tagg och markera hanterarframsteg](assets/smart-tag-step-properties-workflow2.png)

1. Klicka på den ![färdiga ikonen](assets/do-not-localize/check-ok-done-icon.png) för att stänga processsteget.

1. Klicka på **[!UICONTROL Sync]** för att spara arbetsflödet.

## Utbilda tjänsten Smart Content {#training-the-smart-content-service}

För att Smart Content Service ska känna igen din företagsklonomi kan du köra den på en uppsättning resurser som redan innehåller taggar som är relevanta för ditt företag. För att effektivt märka upp varumärkesbilderna kräver Smart Content Service att utbildningsbilderna följer vissa riktlinjer. Efter utbildning kan tjänsten tillämpa samma taxonomi på en liknande uppsättning resurser.

Du kan utbilda tjänsten flera gånger för att förbättra dess förmåga att använda relevanta taggar. Efter varje utbildningscykel kör du ett taggningsarbetsflöde och kontrollerar om dina resurser är taggade på rätt sätt.

Du kan utbilda Smart Content Service regelbundet eller efter behov.

>[!NOTE]
>
>Utbildningsarbetsflödet kan endast användas på mappar.

### Riktlinjer för utbildning {#guidelines-for-training}

För bästa resultat uppfyller bilderna i din utbildningsuppsättning följande riktlinjer:

**Kvantitet och storlek:** Minst 30 bilder per tagg. Minst 500 pixlar på den längre sidan.

**Sammanhang**: Bilder som används för en viss tagg liknar varandra visuellt.

Det är till exempel ingen bra idé att tagga alla dessa bilder som `my-party` (för träning) eftersom de inte är visuellt lika.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/coherence.png)

**Täckning**: Använd tillräcklig variation i bilderna i kursen. Tanken är att ge några exempel som är ganska olika, men som ändå är ganska olika, så att Experience Manager lär sig att fokusera på rätt saker. Om du använder samma tagg på bilder som ser olika ut bör du ta med minst fem exempel av varje typ.

För taggen *model-down-pose* kan du t.ex. inkludera fler utbildningsbilder som liknar den markerade bilden nedan så att tjänsten kan identifiera liknande bilder mer exakt under taggningen.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/coverage_1.png)

**Distraktion/obstruktion**: Tjänsten tränar bättre på bilder som har mindre störning (framträdande bakgrunder, icke-relaterade komponenter, t.ex. objekt/personer med huvudmotivet).

För taggen *casual-shoe* är till exempel den andra bilden inte en bra träningskandidat.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/distraction.png)

**Fullständighet:** Om en bild kvalificerar sig för mer än en tagg lägger du till alla tillämpliga taggar innan du inkluderar bilden för träning. För taggar, till exempel `raincoat` och `model-side-view`, lägger du till båda taggarna i den kvalificerade resursen innan du inkluderar den för utbildning.

![Illustrativa bilder som exempel på riktlinjer för utbildning](/help/assets/assets/do-not-localize/completeness.png)

>[!NOTE]
>
>Möjligheten för Smart Content Service att lära sig taggar och använda dem på andra bilder beror på kvaliteten på de bilder du använder i utbildningen. För bästa resultat rekommenderar Adobe att du använder visuellt liknande bilder för att utbilda tjänsten för varje tagg.

### Periodisk utbildning {#periodic-training}

Du kan aktivera tjänsten Smart Content Service för att med jämna mellanrum utbilda resurser och tillhörande taggar i en mapp. Öppna sidan [!UICONTROL Properties] i resursmappen, välj **[!UICONTROL Enable Smart Tags]** på fliken **[!UICONTROL Details]** och spara ändringarna.

![enable_smart_tags](assets/enable_smart_tags.png)

När det här alternativet har valts för en mapp kör [!DNL Experience Manager] ett utbildningsarbetsflöde automatiskt för att utbilda Smart Content Service i mappresurserna och deras taggar. Som standard körs utbildningsarbetsflödet varje vecka kl. 12.30 på lördagar.

### On-demand-utbildning {#on-demand-training}

Du kan utbilda tjänsten för smart innehåll när det behövs från arbetsflödeskonsolen.

1. I gränssnittet [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj arbetsflödet **[!UICONTROL Workflow Models]** på sidan **[!UICONTROL Smart Tags Training]** och klicka sedan på **[!UICONTROL Start Workflow]** i verktygsfältet.
1. I dialogrutan **[!UICONTROL Run Workflow]** bläddrar du till nyttolastmappen som innehåller de taggade resurserna för att utbilda tjänsten.
1. Ange en rubrik för arbetsflödet och lägg till en kommentar. Klicka sedan på **[!UICONTROL Run]**. Resurserna och taggarna skickas in för utbildning.

   ![workflow_dialog](assets/workflow_dialog.png)

>[!NOTE]
>
>När materialet i en mapp har behandlats för utbildning behandlas endast de ändrade resurserna i efterföljande utbildningscykler.

### Visa utbildningsrapporter {#viewing-training-reports}

Om du vill kontrollera om Smart Content Service är utbildad i dina taggar i övningsresurserna kan du läsa rapporten om utbildningsarbetsflödet i rapportkonsolen.

1. I gränssnittet [!DNL Experience Manager] går du till **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Reports]**.
1. Klicka på **[!UICONTROL Asset Reports]** på sidan **[!UICONTROL Create]**.
1. Välj rapporten **[!UICONTROL Smart Tags Training]** och klicka sedan på **[!UICONTROL Next]** i verktygsfältet.
1. Ange en titel och beskrivning för rapporten. Under **[!UICONTROL Schedule Report]** låter du alternativet **[!UICONTROL Now]** vara markerat. Om du vill schemalägga rapporten till ett senare tillfälle väljer du **[!UICONTROL Later]** och anger ett datum och en tid. Klicka sedan på **[!UICONTROL Create]** i verktygsfältet.
1. På sidan **[!UICONTROL Asset Reports]** markerar du rapporten som du skapat. Om du vill visa rapporten klickar du på **[!UICONTROL View]** i verktygsfältet.
1. Granska informationen i rapporten.

   Rapporten visar träningsstatusen för de taggar du har tränat. Den gröna färgen i kolumnen **[!UICONTROL Training Status]** anger att smarta innehållstjänster har tränats för taggen. Gul färg anger att tjänsten inte är helt tränad för en viss tagg. I det här fallet lägger du till fler bilder med just den taggen och kör träningsarbetsflödet för att träna tjänsten helt för taggen.

   Om du inte ser dina taggar i den här rapporten kör du utbildningsarbetsflödet igen för dessa taggar.

1. Om du vill hämta rapporten markerar du den i listan och klickar på **[!UICONTROL Download]** i verktygsfältet. Rapporten hämtas som ett Microsoft Excel-kalkylblad.

## Begränsningar {#limitations}

* Förbättrade smarta taggar bygger på inlärningsmodeller för bilder och taggar. Dessa modeller är inte alltid perfekta när det gäller att identifiera taggar. Den aktuella versionen av Smart Content Service har följande begränsningar:

   * Oförmåga att identifiera små skillnader i bilder. Till exempel tunna eller jämna skjortor.
   * Oförmåga att identifiera taggar baserat på små mönster/delar av en bild. Till exempel logotyper på T-shirts.
   * Taggning stöds i de språkområden som [!DNL Experience Manager] stöds i.

* Om du vill söka efter resurser med smarta taggar (vanliga eller förbättrade) använder du [!DNL Assets] Omnissearch (fulltextsökning). Det finns inget separat sökpredikat för smarta taggar.

>[!MORELIKETHIS]
>
>* [Översikt och utbildning av smarta taggar](enhanced-smart-tags.md)
>* [Felsökning av smarta taggar för OAuth-autentiseringsuppgifter](config-oauth.md)
>* [Videosjälvstudiekurs om smarta taggar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html?lang=sv-SE)
