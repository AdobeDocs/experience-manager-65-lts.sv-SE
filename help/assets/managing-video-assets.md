---
title: Hantera videomaterial
description: Överför, förhandsgranska, kommentera och publicera videomaterial i  [!DNL Adobe Experience Manager].
contentOwner: AG
role: User
feature: Asset Management
hide: true
solution: Experience Manager, Experience Manager Assets
exl-id: e2b9b13b-c00c-4bfc-8512-84188e90c0ed
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '4998'
ht-degree: 7%

---

# Hantera videomaterial {#manage-video-assets}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/manage-video-assets.html?lang=sv-SE) |
| AEM 6.5 | Den här artikeln |

Videoformatet är en viktig del av ett företags digitala resurser. [!DNL Adobe Experience Manager] erbjuder mogna erbjudanden och funktioner för att hantera hela livscykeln för dina videoresurser efter att de har skapats.

Lär dig hur du hanterar och redigerar videoresurserna i [!DNL Adobe Experience Manager Assets]. Videokodning och transkodning, till exempel FFmpeg-transkodning, är möjlig med [!DNL Dynamic Media]-integrering.

## Överföra och förhandsgranska videomaterial {#upload-and-preview-video-assets}

[!DNL Adobe Experience Manager Assets] genererar förhandsvisningar för videoresurser med filnamnstillägget MP4. Om resursens format inte är MP4 installerar du FFmpeg-paketet för att generera en förhandsvisning. FFmpeg skapar videoåtergivningar av typen OGG och MP4. Du kan förhandsgranska återgivningarna i användargränssnittet för [!DNL Assets].

1. Navigera till den plats där du vill lägga till digitala resurser i mappen eller undermapparna för digitala resurser.
1. Om du vill överföra resursen klickar du på **[!UICONTROL Create]** i verktygsfältet och väljer **[!UICONTROL Files]**. Du kan också dra en fil till användargränssnittet. Mer information finns i [Överför resurser](manage-assets.md#uploading-assets).
1. Om du vill förhandsgranska en video i kortvyn klickar du på alternativet **[!UICONTROL Play]** ![play](assets/do-not-localize/play.png) i videoresursen. Du kan bara pausa eller spela upp video i kortvyn. Alternativen [!UICONTROL Play] och [!UICONTROL Pause] är inte tillgängliga i listvyn.

1. Om du vill förhandsgranska videon på sidan med resursinformation klickar du på **[!UICONTROL Edit]** på kortet. Videon spelas upp i webbläsarens inbyggda videospelare. Du kan spela upp, pausa, styra volymen och zooma videon till helskärm.

   ![Videouppspelningskontroller](assets/video-playback-controls.png)

## Konfiguration för att överföra resurser som är större än 2 GB {#configuration-to-upload-assets-that-are-larger-than-gb}

Som standard kan du inte överföra resurser som är större än 2 GB i [!DNL Assets] eftersom filstorleken är begränsad. Du kan dock skriva över den här gränsen genom att gå till CRXDE Lite och skapa en nod under katalogen `/apps`. Noden måste ha samma nodnamn, katalogstruktur och jämförbara nodegenskaper i ordningen.

Förutom konfigurationen för [!DNL Assets] kan du ändra följande konfigurationer för att överföra stora resurser:

* Öka tokens förfallotid. Se [!UICONTROL Adobe Granite CSRF Servlet] i webbkonsolen på `https://[aem_server]:[port]/system/console/configMgr`. Mer information finns i [CSRF-skydd](/help/sites-developing/csrf-protection.md).
* Öka `receiveTimeout` i Dispatcher-konfigurationen. Mer information finns i [Experience Manager Dispatcher-konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=sv-SE#renders-options).

>[!NOTE]
>
>Användargränssnittet [!DNL Experience Manager] Classic har ingen begränsning för filstorlek på 2 GB. Slutgiltigt arbetsflöde för stor video stöds inte heller helt.

Utför följande steg i katalogen `/apps` om du vill konfigurera en större filstorleksgräns.

1. I [!DNL Experience Manager] klickar du på **[!UICONTROL Tools]** > **[!UICONTROL General]** > **[!UICONTROL CRXDE Lite]**.
1. Navigera till `/libs/dam/gui/content/assets/jcr:content/actions/secondary/create/items/fileupload` i CRXDE Lite. Om du vill visa katalogfönstret klickar du på `>>`.
1. Klicka på **[!UICONTROL Overlay Node]** i verktygsfältet. Du kan också välja **[!UICONTROL Overlay Node]** på snabbmenyn.
1. I dialogrutan **[!UICONTROL Overlay Node]** klickar du på **[!UICONTROL OK]**.

   ![Överläggsnod](assets/overlay-node-path.png)

1. Uppdatera webbläsaren. Överläggsnoden `/apps/dam/gui/content/assets/jcr:content/actions/secondary/create/items/fileupload` har valts.
1. Ange lämpligt värde i byte på fliken **[!UICONTROL Properties]** för att öka storleksgränsen till önskad storlek. Om du till exempel vill öka storleksgränsen till 30 GB anger du värdet `32212254720`.

1. Klicka på **[!UICONTROL Save All]** i verktygsfältet.
1. I [!DNL Experience Manager] klickar du på **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]**.
1. På sidan [!DNL Adobe Experience Manager] [!UICONTROL Web Console Bundles], under kolumnen Namn i tabellen, letar du reda på och klickar på **[!UICONTROL Adobe Granite Workflow External Process Job Handler]**.
1. På sidan [!UICONTROL Adobe Granite Workflow External Process Job Handler] anger du sekunder för både **[!UICONTROL Default Timeout]** och **[!UICONTROL Max Timeout]** fält till `18000` (fem timmar). Klicka på **[!UICONTROL Save]**.
1. I [!DNL Experience Manager] klickar du på **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. Välj **[!UICONTROL Dynamic Media Encode Video]** på sidan Arbetsflödesmodeller och klicka sedan på **[!UICONTROL Edit]**.
1. Dubbelklicka på **[!UICONTROL Dynamic Media Video Service Process]**-komponenten på arbetsflödessidan.
1. I dialogrutan [!UICONTROL Step Properties], på fliken **[!UICONTROL Common]**, expanderar du **Avancerade inställningar**.
1. I fältet **[!UICONTROL Timeout]** anger du värdet `18000` och klickar sedan på **[!UICONTROL OK]** för att återgå till arbetsflödessidan **[!UICONTROL Dynamic Media Encode Video]**.
1. Klicka på **[!UICONTROL Save]** i närheten av sidans överkant, nedanför sidrubriken [!UICONTROL Dynamic Media Encode Video].

## Publicera videomaterial {#publish-video-assets}

Efter publiceringen kan du inkludera videomaterialet på en webbsida som en URL eller bädda in resurserna direkt. Mer information finns i [publicera dynamiska medieresurser](/help/assets/publishing-dynamicmedia-assets.md).

## Publicera videor på YouTube {#publishing-videos-to-youtube}

Du kan publicera lokala Experience Manager-videoresurser direkt i en YouTube-kanal som du tidigare har skapat.

Om du vill publicera videomaterial till YouTube skapar du Experience Manager Assets med taggar. Du kopplar dessa taggar till en YouTube-kanal. Om videoresursens tagg matchar taggen för en YouTube-kanal publiceras videon till YouTube. Publicera till YouTube sker tillsammans med en normal publicering av videon så länge en associerad tagg används.

YouTube gör sin egen kodning. Den ursprungliga videofilen som överfördes till Experience Manager publiceras till YouTube i stället för den videoåtergivning som Dynamic Medias kodning har skapat. Även om det inte krävs för att bearbeta videofilmer med Dynamic Media, förväntas de göra det om en visningsförinställning behövs för uppspelning.

När du åsidosätter videobearbetningsprofilen och publicerar direkt till YouTube innebär det helt enkelt att videomaterialet i Experience Manager Asset inte får någon miniatyrbild som kan visas. Det innebär också att om du kör i `dynamicmedia`- eller `dynamicmedia_scene7`-körningslägen så fungerar inte videoklipp som inte är kodade med någon av resurstyperna för dynamiska media.

När du publicerar videomaterial till YouTube-servrar utför du följande uppgifter för att säkerställa säker server-till-server-autentisering med YouTube:

1. [Konfigurera inställningar för Google Cloud](#configuring-google-cloud-settings)
1. [Create a YouTube channel](#creating-a-youtube-channel)
1. [Lägga till taggar för publicering](#adding-tags-for-publishing)
1. [Aktivera YouTube Publish Replication Agent](#enabling-the-youtube-publish-replication-agent)
1. [Konfigurera YouTube i Experience Manager](#setting-up-youtube-in-aem)
1. [(Valfritt) Automatisera inställningen av YouTube standardegenskaper för överförda videofilmer](#optional-automating-the-setting-of-default-youtube-properties-for-your-uploaded-videos)
1. [Publicera videor i din YouTube-kanal](#publishing-videos-to-your-youtube-channel)
1. [(Valfritt) Verifiera den publicerade videon på YouTube](/help/assets/video.md#optional-verifying-the-published-video-on-youtube)
1. [Länka YouTube URL:er till ditt webbprogram](#linking-youtube-urls-to-your-web-application)

Du kan även [avpublicera videoklipp för att ta bort dem från YouTube](#unpublishing-videos-to-remove-them-from-youtube).

### Konfigurera inställningar för Google Cloud {#configuring-google-cloud-settings}

Du behöver ett Google-konto för att kunna publicera till YouTube. Om du har ett GMAIL-konto har du redan ett Google-konto. Om du inte har något Google-konto kan du enkelt skapa ett. Du behöver kontot eftersom du behöver inloggningsuppgifter för att publicera videoresurser på YouTube. Om du redan har skapat ett konto hoppar du över den här uppgiften och fortsätter direkt till [Skapa en YouTube-kanal](#creating-a-youtube-channel).

Kontot som används med Google Cloud och Google-kontot som används för YouTube behöver inte vara samma.

Google ändrar regelbundet användargränssnittet. Stegen för att publicera videofilmer till YouTube kan därför variera något från vad som beskrivs nedan. Denna caveat gäller även YouTube när du försöker kontrollera om videoklipp har överförts till det.

>[!NOTE]
>
>Följande steg var korrekta när detta skrevs. Google uppdaterar dock regelbundet sina webbplatser utan föregående meddelande. De här stegen kan därför vara något annorlunda.

Så här konfigurerar du inställningarna för Google Cloud:

1. Skapa ett Google-konto.

   Om du redan har ett Google-konto går du vidare till nästa steg.

1. Gå till [https://cloud.google.com/](https://cloud.google.com/).
1. På Google Cloud-sidan uppe till höger klickar du på **[!UICONTROL Console]**.

   Om det behövs använder **[!UICONTROL Sign in]** dina Google-kontouppgifter för att se alternativet **[!UICONTROL Console]**.

1. Klicka på listrutan Projekt till höger om **[!UICONTROL Google Cloud Platform]** på sidan Kontrollpanel för att öppna dialogrutan Välj ett projekt.
1. Välj **[!UICONTROL New Project]** i dialogrutan Välj ett projekt.

   ![6_5_googleaccount-newproject](assets/6_5_googleaccount-newproject.png)

1. Skriv namnet på det nya projektet i fältet Projektnamn i dialogrutan Nytt projekt.

   Ditt projekt-ID baseras på ditt projektnamn. Välj därför projektnamnet noggrant. Det går inte att ändra det efter att det har skapats. Du måste även ange samma projekt-ID igen när du konfigurerar YouTube i Experience Manager senare. Överväg att skriva ned det.

1. Klicka på **[!UICONTROL Create]**.

1. Gör något av följande:

   * Välj **[!UICONTROL Explore and enable APIs]** på ditt projekts Dashboard på kortet Komma igång.
   * Välj **[!UICONTROL Go to APIs overview]** på ditt projekts Dashboard på API-kortet.

   ![6_5_googleaccount-apis-enable2](assets/6_5_googleaccount-apis-enable2.png)

1. Välj **[!UICONTROL Enable APIs and Services]** i början av API:erna och tjänsterna.
1. På sidan API-bibliotek, till vänster, under **[!UICONTROL Category]**, väljer du **[!UICONTROL YouTube]**. Välj **[!UICONTROL YouTube Data API]** till höger på sidan.
1. Välj **[!UICONTROL Enable]** på sidan YouTube Data API v3.

   ![6_5_googleaccount-apis-enable3](assets/6_5_googleaccount-apis-enable3.png)

1. Om du vill använda API:t måste du ha autentiseringsuppgifter. Om det behövs klickar du på **[!UICONTROL Create Credentials]**.

   ![6_5_googleaccount-apis-createcredentials](assets/6_5_googleaccount-apis-createcredentials.png)

1. Gör följande på sidan **[!UICONTROL Add credentials to your project]**, steg 1:

   * I listrutan **[!UICONTROL Which API are you using?]** väljer du **[!UICONTROL YouTube Data API v3]**.

   * Välj **[!UICONTROL Web Server (for example, node.js, Tomcat)]** i listrutan **[!UICONTROL Where are you calling the API from?]**

   * I listrutan **[!UICONTROL What data are you accessing?]** väljer du **[!UICONTROL User data]**.

   ![6_5_googleaccount-apis-createcredentials2](assets/6_5_googleaccount-apis-createcredentials2.png)

1. Välj **[!UICONTROL What credentials do I need?]**
1. I steg 2 på sidan **[!UICONTROL Add credentials to your project]** anger du ett unikt namn i fältet Namn under rubriken **[!UICONTROL Create an OAuth 2.0 client ID]**. Du kan också använda standardnamnet som anges av Google.
1. Under rubriken **[!UICONTROL Authorized JavaScript origins]** anger du följande sökväg i textfältet, ersätter din egen domän och portnummer i sökvägen och trycker sedan på **[!UICONTROL Enter]** för att lägga till sökvägen i listan:

   `https://<servername.domain>:<port_number>`

   Exempel: `https://1a2b3c.mycompany.com:4321`

   **Obs!**: Sökvägsexemplet ovan är endast avsett som exempel.

   ![6_5_googleaccount-apis-createcredentials-oauth](assets/6_5_googleaccount-apis-createcredentials-oauth.png)

1. Under rubriken **[!UICONTROL Authorized redirect URIs]** anger du följande sökväg i textfältet, ersätter din egen domän och portnummer i sökvägen och trycker sedan på **[!UICONTROL Enter]** för att lägga till sökvägen i listan:

   `https://<servername.domain>:<port_number>/etc/cloudservices/youtube.youtubecredentialcallback.json`

   Exempel: `https://1a2b3c.mycompany.com:4321/etc/cloudservices/youtube.youtubecredentialcallback.json`

   **Obs!**: Sökvägsexemplet ovan är endast avsett som exempel.

1. Klicka på **[!UICONTROL Create OAuth client ID]**.
1. På sidan **[!UICONTROL Add credentials to your project]**, steg 3, under rubriken **[!UICONTROL Set up the OAuth 2.0 consent screen]**, väljer du den Gmail-e-postadress som du för närvarande använder.

   ![6_5_googleaccount-apis-createcredentials-consentscreen](assets/6_5_googleaccount-apis-createcredentials-consentscreen.png)

1. Under rubriken **[!UICONTROL Product name shown to users]**, i textfältet, anger du det du vill visa på godkännandeskärmen.

   Medgivandeskärmen visas för Experience Manager-administratören när de autentiserar till YouTube. Experience Manager kontaktar YouTube för att få tillstånd.

1. Klicka på **[!UICONTROL Continue]**.
1. Välj **[!UICONTROL Download]** under rubriken **[!UICONTROL Download credentials]** på sidan Lägg till inloggningsuppgifter för ditt projekt.

   ![6_5_googleaccount-apis-createcredentials-downloadcredentials](assets/6_5_googleaccount-apis-createcredentials-downloadcredentials.png)

1. Spara filen `client_id.json`.

   Du behöver den här hämtade JSON-filen när du konfigurerar YouTube i Adobe Experience Manager senare.

1. Klicka på **[!UICONTROL Done]**.

   Logga ut från ditt Google-konto. Skapa nu en YouTube-kanal.

### Create a YouTube channel {#creating-a-youtube-channel}

Du måste ha en eller flera kanaler för att kunna publicera videofilmer på YouTube. Om du redan har skapat en YouTube-kanal kan du hoppa över den här uppgiften och gå till [Lägg till taggar för publicering](/help/assets/video.md#adding-tags-for-publishing).

>[!WARNING]
>
>Kontrollera att du redan har konfigurerat en eller flera kanaler i YouTube *innan* du lägger till kanaler under YouTube-inställningar i Experience Manager (se [Konfigurera YouTube i Experience Manager](#setting-up-youtube-in-aem) nedan). Om du inte konfigurerar en eller flera kanaler får du ingen varning om att det inte finns några kanaler. Google-autentisering sker dock fortfarande när du lägger till en kanal, men det finns inget alternativ för att välja vilken kanal videon skickas till.

**Så här skapar du en YouTube-kanal:**

1. Gå till [https://www.youtube.com](https://www.youtube.com/) och logga in med autentiseringsuppgifterna för ditt Google-konto.
1. Klicka på din profilbild i det övre högra hörnet på YouTube-sidan (kan också visas som en bokstav i en enfärgad cirkel) och klicka sedan på **[!UICONTROL YouTube settings]** (den runda kugghjulsikonen).
1. Klicka på **[!UICONTROL See all my channels or create a channel]** på sidan Översikt under rubriken Ytterligare funktioner.
1. Klicka på **[!UICONTROL Create a new channel]** på sidan Kanaler.
1. På sidan Varumärkeskonto anger du ett företagsnamn eller ett annat kanalnamn som du väljer där du vill publicera videoresurserna i fältet Märkeskontonamn. Klicka sedan på **[!UICONTROL Create]**.

   Kom ihåg namnet som du anger här eftersom du måste ange det igen när du konfigurerar YouTube i Experience Manager.

1. (Valfritt) Lägg till fler kanaler om det behövs.

   Nu kan du lägga till taggar för publicering.

### Lägga till taggar för publicering {#adding-tags-for-publishing}

Experience Manager kopplar taggar till en eller flera YouTube-kanaler för att publicera dem till dina videofilmer på YouTube. Mer information om hur du lägger till taggar för publicering finns i [Administrera taggar](/help/sites-administering/tags.md).

Om du tänker använda standardtaggarna i Experience Manager kan du hoppa över den här uppgiften och gå till [Aktivera YouTube Publish-replikeringsagenten](#enabling-the-youtube-publish-replication-agent).

### Aktivera YouTube Publish-replikeringsagenten {#enabling-the-youtube-publish-replication-agent}

När du har aktiverat YouTube Publish-replikeringsagenten väljer du **[!UICONTROL Test Connection]** om du vill testa anslutningen till Google Cloud-kontot. Anslutningsresultaten visas på en flik i webbläsaren. Om du har lagt till YouTube-kanaler visas en lista över dem som en del av testet.

1. Klicka på Experience Manager logotyp i det övre vänstra hörnet av Experience Manager och klicka sedan på **[!UICONTROL Tools]** > **[!UICONTROL Deployment]** > **[!UICONTROL Replication]** > **[!UICONTROL Agents on Author]** i den vänstra listen.
1. På sidan Agents of Author klickar du på **[!UICONTROL YouTube Publish]**.
1. Klicka på **[!UICONTROL Edit]** till höger om Inställningar i verktygsfältet.
1. Markera kryssrutan **[!UICONTROL Enabled]** så att du kan aktivera replikeringsagenten.
1. Klicka på **[!UICONTROL OK]**.

   Konfigurera nu YouTube i Experience Manager.

### Konfigurera YouTube i Experience Manager {#setting-up-youtube-in-aem}

Från och med Experience Manager 6.4 introducerades en ny pekgränssnittsmetod för att konfigurera YouTube-publicering i Experience Manager. Baserat på den installerade instansen av Experience Manager som du använder gör du något av följande:

* Information om hur du konfigurerar YouTube i Experience Manager före 6.4 finns i [Konfigurera YouTube i Experience Manager före 6.4](/help/assets/video.md#setting-up-youtube-in-aem-before).
* Information om hur du konfigurerar YouTube i Experience Manager 6.4 eller senare finns i [Konfigurera YouTube i Experience Manager 6.4 och senare](#setting-up-youtube-in-aem-and-later).

#### Konfigurera YouTube i Experience Manager 6.4 och senare {#setting-up-youtube-in-aem-and-later}

1. Se till att du loggar in på din instans av Dynamic Media som administratör.
1. I det övre vänstra hörnet väljer du Experience Manager-logotypen och sedan väljer du **[!UICONTROL Tools]**(hammikon) > **[!UICONTROL Cloud Services]** > **[!UICONTROL YouTube Publishing Configuration]** i den vänstra listen.
1. Välj **[!UICONTROL global]** (markera inte det).

1. Välj **[!UICONTROL Create]** i det övre högra hörnet på den globala sidan.
1. På sidan Skapa YouTube-konfiguration anger du Googles projekt-ID under Inställningar för Google Cloud-plattform i fältet **[!UICONTROL Application Name]**.

   Du angav projekt-ID:t när du konfigurerade Google Cloud-inställningarna tidigare.
Lämna sidan Skapa YouTube-konfiguration öppen. Om en stund kommer du tillbaka till den.

   ![6_5_youtubepublish-createUtubeConfiguration](assets/6_5_youtubepublish-createyoutubeconfiguration.png)

1. Öppna JSON-filen som du hämtade och sparade tidigare i aktiviteten [Konfigurera Google Cloud-inställningar](/help/assets/video.md#configuring-google-cloud-settings) med en vanlig textredigerare.
1. Markera och kopiera hela JSON-texten.
1. Återgå till dialogrutan YouTube-kontoinställningar Klistra in JSON-texten i fältet **[!UICONTROL JSON Config]**.
1. Välj **[!UICONTROL Save]** i sidans övre högra hörn.

   Konfigurera nu YouTube-kanaler i Experience Manager.

1. Välj **[!UICONTROL Add Channel]**.
1. I fältet Kanalnamn anger du namnet på kanalen som du skapade i aktiviteten **[!UICONTROL Adding one or more channels to YouTube]** tidigare.

   Om du vill kan du lägga till en beskrivning.

1. Välj **[!UICONTROL Add]**.
1. YouTube/Google-autentisering visas. Om du inte redan är inloggad på Google Cloud-kontot hoppar du över det här steget.

   * Ange det användarnamn och lösenord för Google som är kopplat till Google projekt-ID och JSON-texten ovan.
   * Beroende på hur många kanaler ditt konto har visas två eller flera objekt. Välj en kanal. Markera inte e-postadressen, den är inte en kanal.
   * På nästa sida väljer du **[!UICONTROL Accept]** för att tillåta åtkomst till den här kanalen.

1. Välj **[!UICONTROL Allow]**.

   Konfigurera taggar för publicering.

1. **[!UICONTROL Setting up tags for publishing]** - På sidan Molntjänster > YouTube väljer du pennikonen för att redigera listan med taggar som du vill använda.
1. Markera listruteikonen (cirkumflex upp och ned) så att du kan visa listan med tillgängliga taggar i Experience Manager.
1. Markera en eller flera taggar så att du kan lägga till dem.

   Om du vill ta bort en tagg som du har lagt till markerar du den och väljer **[!UICONTROL X]**.

1. När du är klar med att lägga till de taggar du vill ha väljer du **[!UICONTROL Save]**.

   Nu kan du publicera videor i din YouTube-kanal.

#### Konfigurera YouTube i Experience Manager före 6.4 {#setting-up-youtube-in-aem-before}

1. Se till att du loggar in på din instans av Dynamic Media som administratör.

1. I det övre vänstra hörnet väljer du Experience Manager-logotypen och sedan väljer du **[!UICONTROL Tools]** (hammikon) > **[!UICONTROL Deployment]** > **[!UICONTROL Cloud Services]** i den vänstra listen.
1. Välj **[!UICONTROL Configure now]** under Tredjepartstjänster, under YouTube.
1. I dialogrutan Skapa konfiguration anger du en rubrik (obligatoriskt) och ett namn (valfritt) i respektive fält.
1. Välj **[!UICONTROL Create]**.
1. I dialogrutan YouTube-kontoinställningar anger du Googles projekt-ID i fältet **[!UICONTROL Application Name]**.

   Du angav projekt-ID:t när du [konfigurerade Google Cloud-inställningarna](/help/assets/video.md#configuring-google-cloud-settings) tidigare.
Lämna dialogrutan YouTube Kontoinställning öppen. Du kommer att gå tillbaka till den om en stund.

1. Använd en vanlig textredigerare för att öppna JSON-filen som du hämtade och sparade tidigare i uppgiften Konfigurera inställningar för Google Cloud.
1. Markera och kopiera hela JSON-texten.
1. Återgå till dialogrutan YouTube-kontoinställningar Klistra in JSON-texten i fältet **[!UICONTROL JSON Config]**.
1. Välj **[!UICONTROL OK]**.

   Konfigurera nu YouTube-kanaler i Experience Manager.

1. Till höger om **[!UICONTROL Available Channels]** väljer du **+** (plusteckenikon).
1. I dialogrutan YouTube-kanalinställningar, i fältet Titel, anger du namnet på kanalen som du skapade i uppgiften **[!UICONTROL Adding one or more channels to YouTube]** tidigare.

   Om du vill kan du lägga till en beskrivning.

1. Välj **[!UICONTROL OK]**.
1. YouTube/Google-autentisering visas. Om du inte redan är inloggad på Google Cloud-kontot hoppar du över det här steget.

   * Ange det användarnamn och lösenord för Google som är kopplat till Google projekt-ID och JSON-texten ovan.
   * Beroende på hur många kanaler ditt konto har visas två eller flera objekt. Välj en kanal. Markera inte e-postadressen, den är inte en kanal.
   * På nästa sida väljer du **[!UICONTROL Accept]** för att tillåta åtkomst till den här kanalen.

1. Välj **[!UICONTROL Allow]**.

   Konfigurera taggar för publicering.

1. **[!UICONTROL Setting up tags for publishing]** - På sidan Molntjänster > YouTube väljer du pennikonen för att redigera listan med taggar som du vill använda.
1. Markera listruteikonen (cirkumflex upp och ned) så att du kan visa listan med tillgängliga taggar i Experience Manager.
1. Markera en eller flera taggar så att du kan lägga till dem.

   Om du vill ta bort en tagg som du har lagt till markerar du den och väljer **X**.

1. När du är klar med att lägga till de taggar du vill ha väljer du **[!UICONTROL OK]**.

   Nu kan du publicera videor i din YouTube-kanal.

### (Valfritt) Automatisera inställningen av YouTube standardegenskaper för överförda videofilmer {#optional-automating-the-setting-of-default-youtube-properties-for-your-uploaded-videos}

Du kan också automatisera inställningen av YouTube-egenskaper vid överföring av videofilmer genom att skapa en metadatabearbetningsprofil i Experience Manager.

Om du vill skapa en profil för metadatabearbetning kopierar du först värden från fälten **[!UICONTROL Field Label]**, **[!UICONTROL Map to property]** och **[!UICONTROL Choices]**, som alla finns i metadatascheman för video. Sedan skapar du en YouTube-metadatabearbetningsprofil för video genom att lägga till dessa värden.

Så här automatiserar du inställningen av YouTube standardegenskaper för överförda videofilmer:

1. I det övre vänstra hörnet väljer du Experience Manager logotyp och klickar sedan på **[!UICONTROL Tools]** (hammikon) > **[!UICONTROL Assets]** > **[!UICONTROL Metadata Schemas]** i den vänstra listen.
1. Klicka på **[!UICONTROL default]**. (Lägg inte till en bockmarkering i markeringsrutan till vänster om &quot;standard&quot;.)
1. Markera rutan till vänster om **[!UICONTROL video]** på sidan **[!UICONTROL default]** och välj sedan **[!UICONTROL Edit]**.
1. Välj fliken **[!UICONTROL Advanced]** på sidan Redigerare för metadatamodeller.
1. Under rubriken YouTube-publicering klickar du på **[!UICONTROL YouTube Category]**.
1. Till höger på sidan, under fliken **[!UICONTROL Settings]**, gör du följande:

   * Markera och kopiera värdet i textfältet **[!UICONTROL Map to property]**.
Klistra in det kopierade värdet i textredigeraren. Du kommer att behöva det här värdet senare när du skapar din metadatabearbetningsprofil. Låt textredigeraren vara öppen.

   * Under **[!UICONTROL Choices]** markerar och kopierar du standardvärdet som du vill använda (till exempel Folk &amp; bloggar eller Vetenskap och teknik).
Klistra in det kopierade värdet i textredigeraren. Du kommer att behöva det här värdet senare när du skapar din metadatabearbetningsprofil. Låt textredigeraren vara öppen.

1. Välj **[!UICONTROL YouTube Privacy]** under rubriken YouTube Publishing.
1. Till höger på sidan, under fliken **[!UICONTROL Settings]**, gör du följande:

   * Markera och kopiera värdet i textfältet **[!UICONTROL Map to property]**.
Klistra in det kopierade värdet i textredigeraren. Du kommer att behöva det här värdet senare när du skapar din metadatabearbetningsprofil. Låt textredigeraren vara öppen.

   * Under **[!UICONTROL Choices]** markerar och kopierar du standardvärdet som du vill använda. Observera att alternativen grupperas i par om två. Det undre fältet i paret är standardvärdet som du vill kopiera, till exempel public, unlisted eller private.
Klistra in det kopierade värdet i textredigeraren. Du kommer att behöva det här värdet senare när du skapar din metadatabearbetningsprofil. Låt textredigeraren vara öppen.

1. Klicka på **[!UICONTROL Cancel]** i det övre högra hörnet på sidan för redigering av metadatamatchning.
1. I det övre vänstra hörnet av Experience Manager väljer du Experience Manager logotyp och klickar sedan på **[!UICONTROL Tools]** (hammikon) > **[!UICONTROL Assets]** > **[!UICONTROL Metadata Profiles]** till vänster.

1. Klicka på **[!UICONTROL Create]** i det övre högra hörnet på sidan Metadataprofiler.
1. I dialogrutan Lägg till metadataprofil i textfältet **[!UICONTROL Profile title]** anger du namnet `YouTube Video` och sedan klickar du på **[!UICONTROL Create]**.
1. Klicka på fliken **[!UICONTROL Advance]** på sidan Redigerare för metadataprofil.
1. Lägg till de kopierade YouTube Publishing-värdena i profilen genom att göra följande:

   * Klicka på fliken **[!UICONTROL Build Form]** till höger på sidan.
   * (Valfritt) Dra komponenten **[!UICONTROL Section Header]** till vänster och släpp den i formulärområdet.
   * (Valfritt) Klicka på **[!UICONTROL Field Label]** för att markera komponenten.
   * (Valfritt) Skriv `YouTube Publishing` till höger på sidan under fliken Inställningar i textfältet Fältetikett.
   * Klicka på fliken **[!UICONTROL Build Form]** och dra sedan komponenten **[!UICONTROL Multi Value Text]** och släpp den under rubriken **[!UICONTROL YouTube Publishing]** som du skapade.

   * Klicka på **[!UICONTROL Field Label]** så att komponenten är markerad.
   * Till höger på sidan, under fliken Inställningar, klistrar du in de YouTube Publishing-värden (Field Label-värde och Map to property-värde) som du kopierade tidigare i deras respektive fält i formuläret. Klistra in alternativvärdet i fältet Standardvärde.

1. Lägg till de kopierade sekretessvärdena för YouTube till profilen genom att göra följande:

   * Klicka på fliken **[!UICONTROL Build Form]** till höger på sidan.
   * (Valfritt) Dra komponenten **[!UICONTROL Section Header]** till vänster och släpp den i formulärområdet.
   * (Valfritt) Klicka på **[!UICONTROL Field Label]** för att markera komponenten.
   * (Valfritt) Skriv `YouTube Privacy` till höger på sidan under fliken Inställningar i textfältet Fältetikett.
   * Klicka på fliken **[!UICONTROL Build Form]** och dra sedan komponenten **[!UICONTROL Multi Value Text]** och släpp den under rubriken **[!UICONTROL YouTube Privacy]** som du skapade.

   * Klicka på **[!UICONTROL Field Label]** så att komponenten är markerad.
   * Till höger på sidan, under fliken Inställningar, klistrar du in de YouTube Publishing-värden (Field Label-värde och Map to property-värde) som du kopierade tidigare i deras respektive fält i formuläret. Klistra in alternativvärdet i fältet Standardvärde.

1. Klicka på **[!UICONTROL Save]** i det övre högra hörnet på sidan.
1. Använd metadataprofilen för YouTube Publishing på de mappar där du ska överföra videoklipp. Du måste ha angett både Metadataprofil och Videoprofil.

   Se [Metadataprofiler](/help/assets/metadata-config.md#metadata-profiles) och [Videoprofiler](/help/assets/video-profiles.md).

### Publicera videor i din YouTube-kanal {#publishing-videos-to-your-youtube-channel}

Nu kopplar du taggarna som du lade till tidigare till videoresurser. På så sätt får Experience Manager veta vilka mediefiler som ska publiceras i din YouTube-kanal.

>[!NOTE]
>
>När du kör i läget Dynamic Media - Scene7 publiceras inte publicering direkt till YouTube. När läget Dynamic Media - Scene7 har konfigurerats finns det två publiceringsalternativ att välja mellan: **[!UICONTROL Immediately]** eller **[!UICONTROL Upon Activation]**.
>
>**[!UICONTROL Publish Immediately]** betyder att den överförda resursen - efter att den har synkroniserats med IPS - publiceras automatiskt till leveranssystemet. Det gäller Dynamic Media, men inte YouTube. Om du vill publicera till YouTube måste du publicera via Experience Manager Author.

>[!NOTE]
>
>Experience Manager använder arbetsflödet **[!UICONTROL Publish to YouTube]** för att publicera innehåll från YouTube, vilket gör att du kan övervaka förloppet och visa felinformation.
>
>Se [Övervaka videokodning och YouTube publiceringsförlopp](#monitoring-video-encoding-and-youtube-publishing-progress).
>
>Mer detaljerad förloppsinformation finns i YouTube-loggen som replikeras. Tänk dock på att en sådan övervakning kräver administratörsåtkomst.

**Så här publicerar du videofilmer i din YouTube-kanal:**

1. I Experience Manager navigerar du till en videoresurs som du vill publicera i din YouTube-kanal.
1. Välj videoresurs (den adaptiva videouppsättningen).
1. Klicka på **[!UICONTROL Properties]** i verktygsfältet.
1. Klicka på **[!UICONTROL Open Selection Dialog]** till höger om fältet Taggar på fliken Grundläggande under rubriken Metadata.
1. På sidan Välj taggar navigerar du till de taggar du vill använda och markerar sedan en eller flera taggar.

   Kom ihåg att taggarna måste kopplas till YouTube-kanalen.

1. Klicka på **[!UICONTROL Select]** i det övre högra hörnet på sidan.
1. Klicka på **[!UICONTROL Save and Close]** i det övre högra hörnet på egenskapssidan för videon.
1. Klicka på **[!UICONTROL Quick Publish]** i verktygsfältet.

   Se även [Använda Publiceringshantering med Experience Manager Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/publication-management-feature-video-use.html?lang=sv-SE).

   Du kan även verifiera den publicerade videon på din YouTube-kanal.

### (Valfritt) Verifiera den publicerade videon på YouTube {#optional-verifying-the-published-video-on-youtube}

Du kan också övervaka förloppet för din YouTube-publicering (eller avpublicering).

Se [Övervaka videokodning och YouTube publiceringsförlopp](#monitoring-video-encoding-and-youtube-publishing-progress).

Publiceringstiderna kan variera avsevärt beroende på olika faktorer, bland annat formatet för den primära källvideon, filstorleken och överföringstrafiken. Publiceringsprocessen kan ta från några minuter till flera timmar. Högre upplösningsformat återges dessutom mycket långsammare. 720p och 1080p tar till exempel längre tid att visa än 480p.

Efter åtta timmar om du fortfarande ser ett statusmeddelande som säger **[!UICONTROL Uploaded (processing, please wait)]** kan du försöka ta bort videon från Adobe webbplats och överföra den igen.

### Länka YouTube URL:er till ditt webbprogram {#linking-youtube-urls-to-your-web-application}

Du kan hämta en YouTube URL-sträng som genereras av Dynamic Media när du har publicerat videon. När du kopierar YouTube-URL:en markeras den i Urklipp så att du kan klistra in den på sidor på webbplatsen eller i programmet.

>[!NOTE]
>
>YouTube-URL:en kan inte kopieras förrän du har publicerat videoresursen till YouTube.

**Så här länkar du YouTube URL:er till ditt webbprogram:**

1. Navigera till den *YouTube-publicerade*-videoresurs vars URL du vill kopiera och markera den.

   Kom ihåg att YouTube URL:er endast är tillgängliga för kopiering av *efter* att du först *har publicerat* videomaterialet till YouTube.

1. Klicka på **[!UICONTROL Properties]** i verktygsfältet.
1. Klicka på fliken **[!UICONTROL Advanced]**.
1. Under rubriken YouTube Publishing (Publicering), i YouTube URL List, markerar och kopierar du URL-texten till webbläsaren för att förhandsgranska resursen eller lägga till den på webbinnehållssidan.

### Avpublicera videofilmer så att du kan ta bort dem från YouTube {#unpublishing-videos-to-remove-them-from-youtube}

När du avpublicerar en videoresurs i Experience Manager tas videon bort från YouTube.

>[!CAUTION]
>
>Om du tar bort en video direkt från YouTube är Experience Manager inte medveten om det och fortsätter att uppföra sig som om videon fortfarande publicerats till YouTube. Avpublicera alltid en videoresurs från YouTube via Experience Manager.

>[!NOTE]
>
>Experience Manager använder arbetsflödet **[!UICONTROL Unpublish from YouTube]** för att ta bort innehåll från YouTube, vilket gör att du kan övervaka förloppet och visa felinformation.
>
>Se [Övervaka videokodning och YouTube publiceringsförlopp](#monitoring-video-encoding-and-youtube-publishing-progress).

**Så här avpublicerar du videoklipp för att ta bort dem från YouTube:**

1. Navigera till de videoresurser som du vill avpublicera från din YouTube-kanal.
1. Välj en eller flera publicerade videoresurser i ett resursurvalsläge.
1. Klicka på **[!UICONTROL Manage Publication]** i verktygsfältet. Välj ikonen med tre punkter (. .) i verktygsfältet så att **[!UICONTROL Manage Publication]** öppnas.
1. Välj **[!UICONTROL Unpublish]** på sidan Hantera publikation.
1. Välj **[!UICONTROL Next]** i det övre högra hörnet på sidan.
1. Välj **[!UICONTROL Unpublish]** i det övre högra hörnet på sidan.

## Övervaka videokodning och YouTube publiceringsförlopp {#monitoring-video-encoding-and-youtube-publishing-progress}

När du överför en ny video till en mapp där videokodning används, eller publicerar videon på YouTube, kan du övervaka hur videokodningen/YouTube-publiceringen fortskrider. Det faktiska publiceringsförloppet för YouTube är endast tillgängligt via loggarna. Misslyckandet eller framgången listas dock på ytterligare sätt som beskrivs i följande procedur. Dessutom får du e-postmeddelanden när en YouTube-publiceringsarbetsgång eller videokodning har slutförts eller avbrutits.

### Övervaka förlopp {#monitoring-progress}

1. Visa kodningsförloppet för video i resursmappen:

   * I kortvyn visas videokodningsförloppet för resursen i procent. Om ett fel uppstår visas även den här informationen på resursen.

   ![chlimage_1-429](assets/chlimage_1-429.png)

   * I listvyn visas videokodningsförloppet i kolumnen **[!UICONTROL Processing Status]**. Om ett fel uppstår visas det här meddelandet i samma kolumn.

   ![chlimage_1-430](assets/chlimage_1-430.png)

   Den här kolumnen visas inte som standard. Om du vill aktivera kolumnen väljer du **[!UICONTROL View Settings]** i listrutan Vyer, lägger till kolumnen **[!UICONTROL Processing Status]** och klickar på **[!UICONTROL Update]**.

   ![chlimage_1-431](assets/chlimage_1-431.png)

1. Visa förloppet i tillgångsinformationen. När du klickar på en resurs öppnar du den nedrullningsbara menyn och väljer **[!UICONTROL Timeline]**. Om du vill begränsa det till arbetsflödesaktiviteter som kodning eller YouTube-publicering väljer du **[!UICONTROL Workflows]**.

   ![chlimage_1-432](assets/chlimage_1-432.png)

   All arbetsflödesinformation, till exempel kodning, visas på tidslinjen. För YouTube-publicering innehåller tidslinjen för arbetsflödet även namnet på YouTube-kanalen och YouTube video-URL:en. Dessutom visas felmeddelanden på tidslinjen i arbetsflödet när publiceringen är klar.

   >[!NOTE]
   >
   >Det kan ta lång tid innan fel-/felmeddelanden slutligen spelas in på grund av flera arbetsflödeskonfigurationer på **[!UICONTROL retries]**, **[!UICONTROL retry delay]** och **[!UICONTROL timeout]** från [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr), till exempel:
   >
   >* Konfiguration av Apache Sling-jobbkö
   >* Extern processhanterare för Adobe Granite-arbetsflöde
   >* Timeoutkö för Granite-arbetsflöde
   >
   >Du kan justera egenskaperna **[!UICONTROL retries]**, **[!UICONTROL retry delay]** och **[!UICONTROL timeout]** i dessa konfigurationer.

1. Information om pågående arbetsflöden finns i Arbetsflödesinstanser som är tillgängliga i **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Instances]**.

   >[!NOTE]
   >
   >Du behöver administratörsbehörighet för att komma åt menyn **[!UICONTROL Tools]**.

   ![chlimage_1-433](assets/chlimage_1-433.png)

   Markera instansen och välj **[!UICONTROL Open History]**.

   ![chlimage_1-434](assets/chlimage_1-434.png)

   I området Arbetsflödesinstanser kan du även göra uppehåll i, avsluta eller byta namn på arbetsflöden. Mer information finns i [Administrera arbetsflöden](/help/sites-administering/workflows-administering.md).

1. Information om misslyckade jobb finns i Arbetsflödesfel i **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Failures]**. I listan **[!UICONTROL Workflow Failure]** visas alla misslyckade arbetsflödesaktiviteter.

   >[!NOTE]
   >
   >Du behöver administratörsbehörighet för att komma åt menyn **[!UICONTROL Tools]**.

   ![chlimage_1-435](assets/chlimage_1-435.png)

   >[!NOTE]
   >
   >Det kan ta lång tid innan felmeddelandet slutligen spelas in på grund av flera arbetsflödeskonfigurationer på **[!UICONTROL retries]**, **[!UICONTROL retry delay]** och **[!UICONTROL timeout]** från [https://localhost:4502/system/console/configMgr](https://localhost:4502/system/console/configMgr), till exempel:
   >
   >* Konfiguration av Apache Sling-jobbkö
   >* Extern processhanterare för Adobe Granite-arbetsflöde
   >* Timeoutkö för Granite-arbetsflöde
   >
   >Du kan justera egenskaperna **[!UICONTROL retries]**, **[!UICONTROL retry delay]** och **[!UICONTROL timeout]** i dessa konfigurationer.

1. Information om slutförda arbetsflöden finns i Arbetsflödesarkiv som är tillgängligt från **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Archive]**. **[!UICONTROL Workflow Archive]** visar alla slutförda arbetsflödesaktiviteter.

   >[!NOTE]
   >
   >Du behöver administratörsbehörighet för att komma åt menyn **[!UICONTROL Tools]**.

   ![chlimage_1-436](assets/chlimage_1-436.png)

1. Du får e-postmeddelanden om avbrutna eller misslyckade arbetsflödesjobb. Dessa e-postmeddelanden kan konfigureras av en administratör. Se [Konfigurera e-postmeddelanden](#configuring-e-mail-notifications).

#### Konfigurera e-postmeddelanden {#configuring-e-mail-notifications}

>[!NOTE]
>
>Du behöver administratörsbehörighet för att komma åt menyn **[!UICONTROL Tools]**.

Hur du konfigurerar meddelanden beror på om du vill ha meddelanden för kodningsjobb eller YouTube publiceringsjobb:

* För kodningsjobb kan du komma åt konfigurationssidan för alla e-postmeddelanden för Experience Manager-arbetsflöden på **[!UICONTROL Tools]** > **[!UICONTROL Operations]** > **[!UICONTROL Web Console]** och genom att söka efter **[!UICONTROL Day CQ Workflow Email Notification Service]**. Se [Konfigurera e-postmeddelanden i Experience Manager](/help/sites-administering/notification.md). Du kan markera eller avmarkera kryssrutorna för **[!UICONTROL Notify on Abort]** eller **[!UICONTROL Notify on Complete]** utifrån detta.

* Gör följande för publiceringsjobb i YouTube:

1. I Experience Manager väljer du **[!UICONTROL Tools]** > **[!UICONTROL Workflow]** > **[!UICONTROL Models]**.
1. På sidan Arbetsflödesmodeller väljer du **[!UICONTROL Publish to YouTube]** och sedan **[!UICONTROL Edit]** i verktygsfältet.
1. I det övre högra hörnet av arbetsflödessidan Publicera till YouTube väljer du **[!UICONTROL Edit]**.
1. Håll muspekaren över YouTube Upload-komponenten och välj sedan en gång för att visa det textbundna verktygsfältet.

   ![6_5_publishtoyoutubeworkflow](assets/6_5_publishtoyoutubeworkflow.png)

1. I det textbundna verktygsfältet väljer du ikonen Konfiguration (skiftnyckel). Klicka på fliken **[!UICONTROL Arguments]**.

   ![6_5_publishtoyoutubeworkflow-configurationicon](assets/6_5_publishtoyoutubeworkflow-configurationicon.png)

1. I dialogrutan YouTube Upload Process - Step Properties väljer du fliken **[!UICONTROL Arguments]**.

   ![6_5_publishtoyoutubeworkflow-arguments-tab](assets/6_5_publishtoyoutubeworkflow-arguments-tab.png)

1. Du kan markera eller avmarkera följande kryssrutor:

   * Publicera start
   * Publiceringsfel
   * Slutförd publicering - innehåller information om kanaler och URL:er

   Om du avmarkerar en kryssruta innebär det att du inte får det angivna e-postmeddelandet från YouTube Publish-arbetsflödet.

   >[!NOTE]
   >
   >Dessa e-postmeddelanden är specifika för YouTube och är utöver de allmänna e-postmeddelandena för arbetsflöden. Du kan därför få två uppsättningar e-postmeddelanden - det allmänna meddelandet som är tillgängligt i **[!UICONTROL Day CQ Workflow Email Notification Service]** och ett som är specifikt för YouTube beroende på dina konfigurationsinställningar.

1. När du är klar markerar du ikonen **[!UICONTROL Done]** (bockmarkering) i dialogrutans övre högra hörn.
1. Välj **[!UICONTROL Sync]** på arbetsflödessidan Publicera till YouTube, i det övre högra hörnet.

## Kommentera videomaterial {#annotate-video-assets}

1. I konsolen [!DNL Assets] väljer du **[!UICONTROL Edit]** på resurskortet för att visa sidan med resursinformation.
1. Om du vill spela upp videon klickar du på **[!UICONTROL Preview]**.
1. Klicka på **[!UICONTROL Annotate]** om du vill kommentera videon. En anteckning läggs till vid en viss tidpunkt (bildruta) i videon. När du gör anteckningar kan du rita på arbetsytan och ta med en kommentar med ritningen. Kommentarerna sparas automatiskt. Klicka på **[!UICONTROL Close]** om du vill avsluta anteckningsguiden.

   ![Rita och kommentera i en videobildruta](assets/annotate-video.png)

1. Gå till en viss punkt i videon, ange tiden i sekunder i **textfältet** och klicka på **Hoppa**. Om du till exempel vill hoppa över de första 20 sekunderna av videon anger du 20 i textfältet.

   ![Söka efter en tid i en video att hoppa över efter angivna sekunder](assets/seek-in-video.png)

1. Klicka på en anteckning om du vill visa den i tidslinjen. Klicka på **[!UICONTROL Delete]** om du vill ta bort anteckningen från tidslinjen.

   ![Visa anteckningar och information på tidslinjen](assets/timeline-view-annotation.png)

>[!MORELIKETHIS]
>
>* [Hantera digitala resurser i Experience Manager Assets](/help/assets/manage-assets.md)
>* [Hantera samlingar i Experience Manager Assets](/help/assets/manage-collections.md)
>* [Videodokumentation för dynamiska media](/help/assets/video.md).
