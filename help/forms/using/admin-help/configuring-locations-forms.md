---
title: Konfigurera platser för Forms
description: Lär dig hur du konfigurerar plats för AEM-formulär. Du kan ange filplatser för attributet, formulärets plats, PDF-startfilen och cacheplatsen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 0%

---

# Konfigurera platser för Forms {#configuring-locations-for-forms}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan ange URL, URI och filplatser för attribut som webbroten, platsen för de formulär som ska hämtas, den dirigerade PDF-filen som används i PDF-formulärsomformningar och cacheplatsen.

1. Klicka på Tjänster > Forms i administrationskonsolen.
1. Ange lämpliga alternativ under Platser. Alternativen beskrivs nedan.
1. Klicka på Spara.

## Platsinställningar {#locations-settings}

**Bas-URL:** Bas-URL:en där formulärresurser som bilder och skript finns. Det här värdet krävs för HTML-omformningar som innehåller HREF-referenser till externa beroenden, som bilder eller skript. Ett sådant skript är xfasubset.js, vilket krävs för att HTML-formulär ska kunna utföra XFA-intelligens. Värdet måste vara HTTP-motsvarigheten till innehållsrots-URI.

>[!NOTE]
>
>Bas-URL stöder endast HTTP- eller databasprotokoll. Den stöder inte protokoll som file:///. Om du behöver komma åt en resurs, till exempel en anpassad CSS eller en URI för digitala signaturer, använder du lämpligt API-parametervärde för att ange den absoluta platsen.

När en beroendesökväg är absolut, ignoreras värdet för bas-URL. I annat fall kombineras beroendesökvägen med bas-URL:en.

Standardvärdet är en tom sträng.

Följande exempel pekar på samma innehåll (med Content Root URI och Base URL):

`(ContentRootURI)/subdir/image1.jpg`

`(BaseURL)/subdir/image1.jpg`

**FS-webbrot-URI:** URL:en för Forms webbprogram. Du kan lämna den här rutan tom om Forms webbprogram och klientprogram distribueras på samma programserver. URL:en för Forms API-webbroten används.

Om Forms webbprogram och klientprogram inte distribueras till samma programserver anger du URL:en för Forms webbprogram i den här rutan, som i det här exemplet:

`https://<host name>:<port>/FormServer`

Där `host name` och `port` är servernamnet och portnumret för den server som är värd för Forms webbprogram.

Standardvärdet är en tom sträng.

**Webbrot-URI:** Programmets webbrot. Det här värdet kombineras med parametern sTargetURL (när sTargetURL anges som relativ), som anges via AEM-formulären SDK, för att skapa en absolut URL för att komma åt programspecifikt webbinnehåll.

Standardvärdet är en tom sträng.

**Innehållsrots-URI:** URI:n eller den absoluta plats som formulär hämtas från. Detta värde kombineras med parametern sFormQuery, som anges via API:t, för att skapa den absoluta sökvägen till det formulär som hämtas. Det här värdet kan referera till en katalog eller en webbplats som är tillgänglig via HTTP.

Standardvärdet är en tom sträng.

**URI för XCI-konfiguration:** Den relativa eller absoluta plats där XCI-filen som används för återgivning finns. För ett relativt värde antas att XCI-filen finns i den distribuerbara AEM-formulärfilen EAR.

Standardvärdet är `com/adobe/formServer/PA/pa.xci`.

**URI för teckensnittsmappning:** Den relativa eller absoluta platsen för teckensnittsmappningsfilen. För ett relativt värde antas den här filen finnas i den distribuerbara AEM-formulärens EAR-fil.

Teckensnittsmappningsfilen används för att skapa anpassade teckensnittsmappningar för HTML-omformningar i formulär, så att du kan ange vilket teckensnitt som ska användas när ett teckensnitt inte finns på klientdatorn.

Standardvärdet är `com/adobe/formServer/client-font-map.properties`.

Följande post är ett exempel på en post i teckensnittsmappningsfilen:

`Arial=Arial,Helvetica,sans-serif`

**Startfil för PDF:** Den ursprungliga PDF-filen som används i en PDF-formulärsomformning för att optimera leveransen. Startfilen för PDF anger en anpassad PDF-fil (som endast innehåller XFA-ström, bilder och teckensnittsresurser) som bifogas med formulärdesignen och data. Formuläret återges i Acrobat 7 eller senare och gäller för PDF-formulärsomformning.

Standardvärdet är en tom sträng.

**Cacheplats:** Anger platsen för Forms-diskcachen. När du ändrar den här inställningen återställs all befintlig cacheinformation från den aktuella platsen och en ny cache skapas på den nya platsen. Välj något av följande alternativ:

**Standardplats:** Det här är standardvalet. När det här alternativet är markerat skapas cachen på en plats som är beroende av den programserver som du använder:

* **JBoss:** [JBoss Home]\server\[installationstyp]\svcdata\FormServer\Cache
* **WebLogic:** [WebLogic Home]\user_projects\domains\[aem-forms Domain Name]\adobe\[Forms Server name]\FormServer\Cache
* **WebSphere:** [IBM Home]\WebSphere\AppServer\installedApps\adobe\server1\FormServer\Cache

**Tillfällig LC-katalog:** Cachen skapas i en underkatalog till den tillfälliga AEM-formulärkatalogen, som anges i administrationskonsolen under Inställningar > Systeminställningar > Konfigurationer > Plats för tillfällig katalog. Underkatalogen heter adobeform_[servername].

>[!NOTE]
>
>Om du använder ett tillfälligt rensningsverktyg och tar bort de här katalogerna påverkar inte funktionen avsevärt prestanda under en kort tid tills det nya cacheminnet skapas. För att undvika det här problemet ska du inte ta bort de här katalogerna när du rensar den tillfälliga AEM-formulärkatalogen.
