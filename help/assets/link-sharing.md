---
title: Dela resurser via en länk
description: Dela resurser, mappar och samlingar som en URL.
contentOwner: AG
role: User
feature: Link Sharing,Asset Management
hide: true
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '931'
ht-degree: 3%

---

# Dela resurs som en länk {#asset-link-sharing}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/share-assets.html?lang=en) |
| AEM 6.5 | Den här artikeln |

Med [!DNL Adobe Experience Manager Assets] kan du dela resurser, mappar och samlingar som en URL med medlemmar i din organisation och externa entiteter, inklusive partners och leverantörer. Att dela resurser via en länk är ett bekvämt sätt att göra resurser tillgängliga för externa parter utan att de först behöver logga in på [!DNL Assets].

>[!PREREQUISITES]
>
>* Du måste ha `Edit ACL` behörighet för mappen eller resursen som du vill dela som en länk.
>* Konfigurera SMTP-serverinformationen i [Day CQ Mail Service](#configmailservice) om du vill skicka e-postmeddelanden till användarna.

## Dela resurser {#share-assets}

Använd dialogrutan [!UICONTROL Link Sharing] för att generera URL:en för resurser som du vill dela med användare.

* De användare som har administratörsbehörighet eller läsbehörighet på platsen `/var/dam/share` kan visa de länkar som delas med dem.
* De användare som har läsbehörighet på platsen `/var/dam/jobs/download` kan hämta resurser från den delade länken.

1. I användargränssnittet [!DNL Assets] väljer du resursen som ska delas som en länk.

1. Klicka på ikonen **[!UICONTROL Share Link]** ![Dela resurser](assets/do-not-localize/assets_share.png) i verktygsfältet. Länken som skapas efter att du klickat på **[!UICONTROL Share]** visas i förväg i fältet [!UICONTROL Share Link]. Länken skapas inte förrän du väljer **[!UICONTROL Submit]**.

   ![Dialogruta med länkresurs](assets/share-assets-as-link.png)

   *Figur: Dialogrutan där resurser delas som en länk.*

1. Skriv e-post-ID:t för den användare som du vill dela länken med i rutan för e-postadress i dialogrutan **[!UICONTROL Link Sharing]**. Du kan lägga till en eller flera användare.

   >[!NOTE]
   >
   >Om du anger ett e-post-ID för en användare som inte är medlem i din organisation, kommer orden [!UICONTROL External User] att föregås av användarens e-post-ID.

1. Ange ett ämne för resursen som du vill dela i rutan **[!UICONTROL Subject]**.

1. Ange ett valfritt meddelande i rutan **[!UICONTROL Message]**.

1. I fältet **[!UICONTROL Expiration]** anger du ett förfallodatum och en förfallotid för att länken ska sluta fungera. Länkens standardförfallotid är en dag.

   ![Ange förfallodatum för delad länk](assets/Set-shared-link-expiration.png)

1. Om du vill att användarna ska kunna hämta den ursprungliga resursen väljer du **[!UICONTROL Allow download of original file]**. Välj **[!UICONTROL Allow download of renditions of file]** om du bara vill att användarna ska kunna hämta renderingar av de delade resurserna.

1. Klicka på **[!UICONTROL Share]**. Ett meddelande bekräftar att länken delas med användarna via ett e-postmeddelande.

1. Om du vill visa den delade resursen klickar du på länken i e-postmeddelandet som skickas till användaren. Om du vill generera en förhandsgranskning av resursen klickar du på den delade resursen. Klicka på **[!UICONTROL Back]** om du vill stänga förhandsgranskningen. Om du har delat en mapp klickar du på **[!UICONTROL Parent Folder]** för att återgå till den överordnade mappen.

   ![Förhandsgranskning av delad resurs](assets/chlimage_1-546.png)

   >[!NOTE]
   >
   >[!DNL Experience Manager] stöder generering av förhandsgranskning av resurser av endast [de filtyper som stöds](/help/assets/assets-formats.md). Om andra MIME-typer delas kan du bara hämta resurserna och inte förhandsgranska.

1. Om du vill hämta den delade resursen klickar du på **[!UICONTROL Select]** i verktygsfältet, på resursen och sedan på **[!UICONTROL Download]** i verktygsfältet.

   ![Verktygsfältsalternativ för att hämta den delade resursen](assets/chlimage_1-547.png)

1. Om du vill visa resurser som du har delat som länkar går du till användargränssnittet för [!DNL Assets] och klickar på logotypen för [!DNL Experience Manager]. Välj **[!UICONTROL Navigation]**. I navigeringsrutan väljer du **[!UICONTROL Shared Links]** om du vill visa en lista över delade resurser.

1. Om du vill ta bort delningen av en resurs markerar du den och klickar på **[!UICONTROL Unshare]** i verktygsfältet. Ett bekräftelsemeddelande följer. Posten för resursen tas bort från listan.

## Konfigurera CQ-e-posttjänst för dag {#configure-day-cq-mail-service}

1. Gå till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** på startsidan för [!DNL Experience Manager].
1. Leta upp **[!UICONTROL Day CQ Mail Service]** i listan över tjänster.
1. Klicka på **[!UICONTROL Edit]** bredvid tjänsten och konfigurera följande parametrar för **[!UICONTROL Day CQ Mail Service]** med den information som anges mot deras namn:

   * SMTP-serverns värdnamn: e-postserverns värdnamn
   * SMTP-serverport: e-postserverport
   * SMTP-användare: användarnamn för e-postserver
   * SMTP-lösenord: e-postserverlösenord

   ![chlimage_1-263](assets/chlimage_1-548.png)

1. Klicka på **[!UICONTROL Save]**.

## Konfigurera maximal datastorlek {#configure-maximum-data-size}

När du hämtar resurser från den länk som delas med funktionen Länkdelning komprimerar [!DNL Experience Manager] resurshierarkin från databasen och returnerar sedan resursen i en ZIP-fil. I avsaknad av begränsningar för den mängd data som kan komprimeras i en ZIP-fil utsätts stora mängder data för komprimering, vilket leder till minnesfel i JVM. Om du vill skydda systemet från en potentiell denial of service-attack på grund av den här situationen konfigurerar du den maximala storleken med parametern **[!UICONTROL Max Content Size (uncompressed)]** för **[!UICONTROL Day CQ DAM Adhoc Asset Share Proxy Servlet]** i Configuration Manager. Om resursens okomprimerade storlek överskrider det konfigurerade värdet, avvisas begäranden om hämtning av resurser. Standardvärdet är 100 MB.

1. Klicka på logotypen [!DNL Experience Manager] och gå sedan till **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.
1. Leta reda på konfigurationen **[!UICONTROL Day CQ DAM Adhoc Asset Share Proxy Servlet]** på webbkonsolen.
1. Öppna konfigurationen **[!UICONTROL Day CQ DAM Adhoc Asset Share Proxy Servlet]** i redigeringsläge och ändra värdet på parametern **[!UICONTROL Max Content Size (uncompressed)]**.

   ![chlimage_1-264](assets/chlimage_1-549.png)

1. Spara ändringarna.

## Bästa praxis och felsökning {#best-practices-and-troubleshooting}

* Resursmappar eller samlingar som innehåller ett tomt utrymme i namnet kanske inte delas.
* Om användarna inte kan hämta de delade resurserna bör du fråga [!DNL Experience Manager]-administratören om vilka [hämtningsgränser](#configure-maximum-data-size) som är.
* Om du inte kan skicka e-post med länkar till delade resurser eller om de andra användarna inte kan ta emot din e-post, bör du kontakta [!DNL Experience Manager]-administratören om [e-posttjänsten](#configure-day-cq-mail-service) är konfigurerad eller inte.
* Om du inte kan dela resurser med hjälp av länkdelningsfunktionen måste du se till att du har rätt behörighet. Se [Dela resurser](#share-assets).
* Om en delad resurs flyttas till en annan plats slutar länken att fungera. Återskapa länken och dela den på nytt med användarna.

* Om du vill dela länkar från din [!DNL Experience Manager] Author-distribution till externa entiteter måste du se till att du bara visar följande URL:er som används för länkdelning, endast för `GET`-begäranden. Blockera andra URL-adresser av säkerhetsskäl.

   * `http://[aem_server]:[port]/linkshare.html`
   * `http://[aem_server]:[port]/linksharepreview.html`
   * `http://[aem_server]:[port]/linkexpired.html`

  I [!DNL Experience Manager]-gränssnittet öppnar du **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**. Öppna **[!UICONTROL Day CQ Link Externalizer]**-konfigurationen och ändra följande egenskaper i fältet **[!UICONTROL Domains]** med de värden som anges mot `local`, `author` och `publish`. Ange URL:en för de lokala instanserna och författarinstanserna för egenskaperna `local` och `author`. Om du kör en enda [!DNL Experience Manager] Author-instans använder du samma värde för egenskaperna `local` och `author`. Ange URL:en för publiceringsinstansen [!DNL Experience Manager] för publiceringsinstanser.
