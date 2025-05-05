---
title: Konfigurera Dynamic Media Publish Setup för Image Server
description: Lär dig hur du konfigurerar Dynamic Media Publish Setup för Image Server så att du kan publicera resurser. Du kan också testa dina resurser innan du publicerar dem.
feature: Image Profiles
role: User, Admin
mini-toc-levels: 4
solution: Experience Manager, Experience Manager Assets
exl-id: 9cad4bbc-64f6-4ea2-a9b2-b6b9aaa72f11
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3123'
ht-degree: 0%

---

# Konfigurera Dynamic Media Publish Setup för Image Server

Alternativen för konfiguration av dynamiska publiceringsinställningar är bara tillgängliga om följande är true:

* Du kör Dynamic Media i Scene7-läge. Se [Aktivera dynamiska media i Scene7-läge](/help/assets/config-dms7.md#enabling-dynamic-media-in-scene-mode).
* Du har en *befintlig* **[!UICONTROL Dynamic Media Configuration]** (i **[!UICONTROL Cloud Services]**) i Adobe Experience Manager. Se [Skapa en dynamisk mediekonfiguration i molntjänster](/help/assets/config-dms7.md#configuring-dynamic-media-cloud-services).
* Du är systemadministratör för Experience Manager med administratörsbehörighet.

Erfarna webbplatsutvecklare och programmerare ska använda Dynamic Media Publish Setup. Adobe Dynamic Media rekommenderar användare som ändrar publiceringsinställningar att känna till Adobe Dynamic Media, HTTP-protokollets standarder och konventioner samt grundläggande bildbehandlingsteknik.

På sidan Dynamic Media Publish Setup anges standardinställningar som avgör hur resurser levereras från Adobe Dynamic Media-servrar till webbplatser eller program. Om ingen inställning har angetts levererar Adobe Dynamic Media-servern en resurs enligt en standardinställning som har konfigurerats på sidan Dynamic Media Publish Setup.

Se även [Valfritt - Konfigurera och konfigurera Dynamic Media - Scene7-lägesinställningar](/help/assets/config-dms7.md#optional-setup-and-configuration-of-dynamic-media-scene7-mode-settings) för fler valfria konfigurationsuppgifter.

>[!NOTE]
>
>Vill du uppgradera från Dynamic Media Classic till Dynamic Media på Adobe Experience Manager? Sidan [Allmänna inställningar](/help/assets/dm-general-settings.md) och sidan Publiceringsinställningar i Dynamic Media är förifyllda med de värden som tas från ditt Dynamic Media Classic-konto. Undantag är alla värden som listas under **[!UICONTROL Default upload options]** på sidan Allmänna inställningar. Värdena finns redan i Experience Manager. Alla ändringar du gör under **[!UICONTROL Default upload options]** på någon av de fem flikarna, via Experience Manager användargränssnitt, visas i Dynamic Media, inte i Dynamic Media Classic. Alla andra inställningar och värden på sidan [Allmänna inställningar](/help/assets/dm-general-settings.md) och sidan Publiceringsinställningar behålls mellan Dynamic Media Classic och Dynamic Media på Experience Manager.

**Så här konfigurerar du servern för installationsavbildning för dynamisk mediepublicering:**

1. I Experience Manager Author mode väljer du Experience Manager logotyp för att komma åt den globala navigeringskonsolen.
1. Klicka på verktygsikonen i den vänstra listen och gå sedan till **[!UICONTROL Assets]** > **[!UICONTROL Dynamic Media Publish Setup]**.
1. I listrutan på sidan Image Server väljer du din publiceringskontext för att ange standardinställningar för att leverera bilder från Image-servrar.

| Publiceringskontext | Beskrivning |
| --- | --- |
| Bildredigering | Anger kontexten för publiceringsinställningarna. |
| Testa bildvisning | Anger kontexten för testning av publiceringsinställningar.<br>För nya dynamiska mediekonton anges standardfältet **[!UICONTROL Client address]** automatiskt till `127.0.0.1`.<br>Se [Testa resurser innan du publicerar dem](#test-assets-before-making-public). |

1. Använd de fem flikarna för att konfigurera standardinställningarna för publiceringskontext.

   * Fliken [Säkerhet](#security-tab)
   * Fliken [Kataloghantering](#catalog-management-tab)
   * Fliken [Attribut för begäran](#request-attributes-tab)
   * Fliken [Vanliga miniatyrattribut](#common-thumbnail-attributes-tab)
   * Fliken [Färghanteringsattribut](#color-management-attributes-tab)

   ![Installationssida för dynamisk mediepublicering](/help/assets/assets-dm/dm-publish-setup.png)
   *Sidan Dynamic Media Publish Setup med fliken **[!UICONTROL Request Attributes]**&#x200B;markerad.*<br><br>

1. När du är klar klickar du på **[!UICONTROL Save]** i sidans övre högra hörn.

## Fliken Säkerhet {#security-tab}

>[!NOTE]
>
>Säkerhetsinställningar stöds inte för publiceringskontexten *Bildserver*.

När *Testa bildservern* har angetts som publiceringskontext kan du ange följande säkerhetsinställning:

**[!UICONTROL Client address]** - Gör att du kan ange en eller flera IP-adresser eller IP-adressintervall. När det anges avvisas begäranden till den här bildkatalogen som kommer från en klient till en IP-adress som inte finns med i listan. Den här regeln gäller både för leverans av bilder och återgivna bilder.

![Fliken Säkerhet ](/help/assets/assets-dm/dm-ipallowlist.png)<br>*Fliken Säkerhet med IP-fältet &quot;allow&quot;.*

## Fliken Kataloghantering {#catalog-management-tab}

**[!UICONTROL Rule set definition file path]** - Anger filen som innehåller regeluppsättningsdefinitionerna för bildkatalogen.

Se även parametern [RuleSetFile](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-rulesetfile) i referenshandboken för Dynamic Media Viewer.

>[!NOTE]
>
>Om ditt Dynamic Media Classic-konto har markerat **[!UICONTROL Rule set definition file path]** under **[!UICONTROL Setup]** > **[!UICONTROL Application]** > **[!UICONTROL Publish setup]** hämtar Experience Manager filen. Ditt Dynamic Media-konto hämtar filen från Dynamic Media Classic. Filen lagras sedan och blir tillgänglig i det här fältet när du öppnar sidan **[!UICONTROL Dynamic Media Publish Setup]** för första gången.

## Fliken Begär attribut {#request-attributes-tab}

De här inställningarna gäller standardutseendet för bilder.

| Inställning | Beskrivning |
| --- | --- |
| **[!UICONTROL Reply image size limit]** | Obligatoriskt.<br>För nya Dynamic Media-konton anges standardstorleksgränserna automatiskt till Bredd: `3000` och Höjd: `3000` för både **[!UICONTROL Image Serving]** och **[!UICONTROL Test Image Serving]**.<br>Anger den maximala svarsbildens bredd och höjd som returneras till klienten. Servern returnerar ett fel om en begäran orsakar en svarsbild vars bredd, höjd eller båda är större än den här inställningen.<br>Se även parametern [MaxPix](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-maxpix) i referenshandboken för Dynamic Media Viewer. |
| **[!UICONTROL Request obfuscation mode]** | Aktivera om du vill att base64-kodning ska användas på giltiga begäranden.<br>Se även parametern [RequestObfusaction](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-requestobfuscation) i referenshandboken för Dynamic Media Viewer. |
| **[!UICONTROL Request locking mode]** | Aktivera om du vill att ett enkelt hash-lås ska inkluderas i begäranden.<br>Se även parametern [RequestLock](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-requestlock) i referenshandboken för Dynamic Media Viewer. |
| **[!UICONTROL Default Request Attributes]** | |
| **[!UICONTROL Default image file suffix]** | Obligatoriskt.<br>Standarddatafiltillägg som läggs till i fältvärdena för katalogsökvägen och MaskPath om sökvägen inte innehåller något filsuffix.<br>Se även parametern [DefaultExt](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-defaultext) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Default font face name]** | Anger vilket teckensnitt som ska användas om inget teckensnitt anges i en textlagerbegäran. Om du anger det måste det vara ett giltigt teckensnittsnamnvärde i bildkatalogens teckensnittskarta eller standardkatalogens teckensnittskarta.<br>Se även parametern [DefaultFont](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-defaultfont) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Default image]** | En standardbild returneras som svar på en begäran där den begärda bilden inte hittas.<br>Se även parametern [DefaultImage](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-is-cat-defaultimage) i referenshandboken för dynamiska medievyer.<br>**Obs!** Om ditt Dynamic Media Classic-konto har ett **[!UICONTROL Default image]** som har valts under **[!UICONTROL Setup]** > **[!UICONTROL Application]** > **[!UICONTROL Publish setup]** hämtar Experience Manager filen i gruppen **[!UICONTROL Default Request Attributes]**. Filen lagras sedan och blir tillgänglig i det här fältet när du öppnar sidan **[!UICONTROL Dynamic Media Publish Setup]** för första gången. |
| **[!UICONTROL Default image mode]** | När skjutreglaget är aktiverat (skjutreglage till höger) ersätter **[!UICONTROL Default image]** alla lager som saknas i källbilden med standardbilden och returnerar det sammansatta som vanligt. När skjutreglaget är inaktiverat (skjutreglage till vänster) ersätter standardbilden hela den sammansatta bilden, även om den saknade bilden bara är ett av flera lager.<br>Se även parametern [DefaultImageMode](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-defaultimagemode) i referenshandboken för dynamiska mediavisare. |
| **[!UICONTROL Default view size]** | Obligatoriskt.<br>För nya dynamiska mediekonton anges standardvystorlekarna automatiskt till Bredd: `1280` och Höjd: `1280` för både **[!UICONTROL Image Serving]** och **[!UICONTROL Test Image Serving]**.<br>Servern begränsar svarsbilderna så att de inte är större än den här bredden och höjden om begäran inte uttryckligen anger visningsstorleken med `wid=`, `hei=` eller `scl=`.<br>Se även parametern [DefaultPix](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-defaultpix) i referenshandboken för Dynamic Media Viewer. |
| **[!UICONTROL Default thumbnail size]** | Obligatoriskt.<br>Används i stället för attributet **[!UICONTROL Default view size]** för miniatyrbegäranden (`req=tmb`). Servern begränsar svarsbilder till att inte vara större än den här bredden och höjden om en miniatyrbegäran (`req=tmb`) inte anger storleken explicit med `wid=`, `hei=` eller `scl=`.<br>Se även parametern [DefaultThumbPix](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-defaultthumbpix) i referenshandboken för dynamiska mediavisare. |
| **[!UICONTROL Default background color]** | Anger det RGB-värde som används för att fylla i områden i en svarsbild som inte innehåller verkliga bilddata.<br>Se även parametern [BkgColor](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-bkgcolor) i referenshandboken för Dynamic Media Viewer. |
| **[!UICONTROL JPEG Encoding Attributes]** |  |
| **[!UICONTROL Quality]** | <br>Anger standardattributen för JPEG svarsbilder.<br>För nya dynamiska mediekonton anges standardvärdena **[!UICONTROL Quality]** automatiskt till `80` för både **[!UICONTROL Image Serving]** och **[!UICONTROL Test Image Serving]**.<br>Det här fältet definieras i intervallet 1-100.<br>Se även parametern [ JpegQuality](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-jpegquality) i referenshandboken för dynamiska mediavisare. |
| **[!UICONTROL Chromatically downsampling]** | Aktivera eller inaktivera kromatisk nedsampling, som används av JPEG-kodare. |
| **[!UICONTROL Default resampling mode]** | Anger de standardattribut för omsampling och interpolation som ska användas för skalning av bilddata. Använd när `resMode` inte har angetts i en begäran.<br>För nya Dynamic Media-konton anges automatiskt standardomsamplingslägena till `Sharp2` för både **[!UICONTROL Image Serving]** och **[!UICONTROL Test Image Serving]**.<br>Se även parametern [ResMode](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-is-cat-resmode) i referenshandboken för dynamiska medievyer. |

## Fliken Vanliga miniatyrattribut {#common-thumbnail-attributes-tab}

De här inställningarna gäller för miniatyrbildernas standardutseende och -justering.

| Inställning | Beskrivning |
| --- | --- |
| **[!UICONTROL Default background color for thumbnail]** | Anger det RGB-värde som används för att fylla i en miniatyrbilds utdataområde som inte innehåller verkliga bilddata. Används endast för miniatyrbildsbegäranden (`req=tmb`) och när inställningen **[!UICONTROL Default Thumbnail Type]** är inställd på **[!UICONTROL Fit]** eller **[!UICONTROL Texture]**.<br>Se även parametern [ ThumbBkgColor](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-thumbbkgcolor) i referenshandboken för dynamiska mediavisare. |
| **[!UICONTROL Horizontal alignment]** | Anger den vågräta justeringen av miniatyrbilden i svarsbildsrektangeln som anges av värdena `wid=` och `hei=`.<br>Används endast för miniatyrbegäranden (`req=tmb`) och när inställningen **[!UICONTROL Default Thumbnail Type]** är **[!UICONTROL Fit]**.<br>Det finns tre horisontella justeringar att välja mellan: **[!UICONTROL Center alignment]**, **[!UICONTROL Left alignment]** och **[!UICONTROL Right alignment]**.<br>Se även parametern [ ThumbHorizAlign](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-thumbhorizalign) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Vertical alignment]** | Anger den lodräta justeringen av miniatyrbilden i svarsbildsrektangeln som anges av värdena `wid=` och `hei=`. Används endast för miniatyrbildsbegäranden (`req=tmb`) och när inställningen **[!UICONTROL Default Thumbnail Type]** är **[!UICONTROL Fit]**.<br>Det finns tre lodräta justeringar att välja mellan: **[!UICONTROL Top alignment]**, **[!UICONTROL Center alignment]** och **[!UICONTROL Bottom alignment]**.<br>Se även parametern [ ThumbVertAlign](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-thumbvertalign) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Default cache time to live]** | Ett standardförfallointervall anges i timmar om en viss katalogpost inte innehåller ett giltigt värde för katalogförfallotid. Ange `-1` för att markera som aldrig förfaller. <br>Se även parametern [Förfallotid](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-expiration) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Default thumbnail type]** | Ett standardvärde för miniatyrbildstypen anges om en viss katalogpost inte innehåller ett giltigt katalogvärde för ThumbType. Används endast för miniatyrbildsbegäranden (`req=tmb`).<br>Det finns tre typer av miniatyrer att välja mellan: **[!UICONTROL Crop]**, **[!UICONTROL Fit]** och **[!UICONTROL Texture]**.<br>Se även parametern [ThumbType](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-thumbtype) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Default thumbnail resolution]** | Ett standardvärde för upplösningen av miniatyrbildobjektet anges om en viss katalogpost inte innehåller ett giltigt ThumbRes-katalogvärde. Används endast för miniatyrbildsbegäranden (`req=tmb`) och när inställningen **[!UICONTROL Default thumbnail type]** är inställd på **[!UICONTROL Texture]**.<br>Se även parametern [ ThumbRes](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-thumbres) i referenshandboken för dynamiska mediavisare. |

## Fliken Färghanteringsattribut {#color-management-attributes-tab}

De här inställningarna avgör vilka ICC-färgprofiler som används för bilder.

**Återgivningsmetod för färgkonvertering**
En färgkonverteringsåtergivningsmetod tillåter åsidosättning av standardåtergivningsmetoden för arbetsprofilerna för att bestämma hur källfärgerna justeras. Används när:

1. En av standardprofilerna för ICC är målfärgrymden för en färgkonvertering.
1. Den här profilen karakteriserar en utdataenhet, till exempel en skrivare eller bildskärm.
1. Den angivna återgivningsmetoden gäller även för den här profilen.

Olika återgivningsmetoder använder olika regler för att bestämma hur källfärgerna justeras.

Se även parametern [IccRenderIntent](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-iccrenderintent) i referenshandboken för Dynamic Media Viewer.

>[!NOTE]
>
>I allmänhet bör du använda standardåtergivningsmetoden för den valda färginställningen, som Adobe har testat för att uppfylla branschstandarder. Om du t.ex. väljer en färginställning för Nordamerika eller Europa är standardåtergivningsmetoden för färgkonvertering **[!UICONTROL Relative Colorimetric]**. Om du väljer en färginställning för Japan är standardåtergivningsmetoden för färgkonvertering **[!UICONTROL Perceptual]**.

| Inställning | Egenskaper |
| --- | --- |
| **[!UICONTROL CMYK default color space]** | Anger namnet på ICC-färgprofilen som ska användas som arbetsprofil för CMYK-data. Om **[!UICONTROL None Specified]** väljs inaktiveras färghantering för den här bildkatalogen när det gäller CMYK-källbilder. Alla CMYK-arbetsfärgrymder är enhetsberoende, vilket innebär att de baseras på faktiska kombinationer av bläck och papper. De CMYK-arbetsfärgrymder som Adobe tillhandahåller baseras på standardförhållanden för tryckning.<br> Se även parametern [IccProfileCMYK](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-iccprofilecmyk) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Gray-Scale default color space]** | Anger namnet på ICC-färgprofilen som ska användas som arbetsprofil för gråskaledata. Om **[!UICONTROL None Specified]** väljs inaktiveras färghantering för den här bildkatalogen när källbilder i gråskala används.<br>Se även parametern [ IccProfileGray](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-iccprofilegray) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL RGB default color space]** | Anger namnet på ICC-färgprofilen som ska användas som arbetsprofil för RGB-data. Om **[!UICONTROL None Specified]** väljs inaktiveras färghantering för den här bildkatalogen när bilder från RGB används. I allmänhet är det bäst att välja **[!UICONTROL Adobe RGB]** eller **[!UICONTROL sRGB]** i stället för profilen för en viss enhet (till exempel en bildskärmsprofil). **[!UICONTROL sRGB]** rekommenderas när du förbereder bilder för webben eller mobila enheter, eftersom det definierar färgrymden för standardbildskärmen som används för att visa bilder på webben. **[!UICONTROL sRGB]** är också ett bra alternativ när du arbetar med bilder från vanliga digitalkameror, eftersom sRGB används som standardfärgrymd i de flesta av dessa.<br>Se även parametern [ IccProfileRBG](https://experienceleague.adobe.com/sv/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/attributes/r-iccprofilergb) i referenshandboken för dynamiska medievyer. |
| **[!UICONTROL Color conversion rendering intent]** | **[!UICONTROL Perceptual]** - Avsikten är att bevara den visuella relationen mellan färger så att den uppfattas som naturlig för det mänskliga ögat, även om färgvärdena i sig kan ändras. Den här metoden är lämplig för fotografiska bilder med många ej tryckbara färger. Den här inställningen är standardåtergivningsmetoden för den japanska tryckeribranschen. |
|  | **[!UICONTROL Relative Colorimetric]** - Jämför den extrema högdagern för källfärgrymden med målfärgrymden och ändrar alla färger därefter. Färger utanför färgomfånget ändras till den närmast reproducerbara färgen i målfärgrymden. Med Relativa färgvärden bevaras mer av de ursprungliga färgerna i en bild än med Perceptuell. Den här inställningen är standardåtergivningsmetoden för utskrift i Nordamerika och Europa. |
|  | **[!UICONTROL Saturation]** - Försöker producera levande färger i en bild på bekostnad av färgnoggrannheten. Den här återgivningsmetoden är lämplig för affärsgrafik som diagram och diagram där klara, mättade färger är viktigare än det exakta förhållandet mellan färger. |
|  | **[!UICONTROL Absolute Colorimetric]** - Ändrar inte färger som ligger inom målomfånget. Ej tryckbara färger beskärs. Ingen skalning av färger till målvitpunkten utförs. Den här metoden syftar till att bibehålla färgnoggrannheten på bekostnad av att förhållandet mellan färger bevaras och är lämplig för korrektur för att simulera utdata från en viss enhet. Den här metoden är användbar när du vill förhandsgranska hur pappersfärgen påverkar tryckta färger. |

## Testa resurser innan du gör dem offentliga {#test-assets-before-making-public}

Säker testning hjälper er att definiera en säker testmiljö och bygga en robust affärs-till-business-lösning som bygger på en konfigurerbar uppsättning IP-adresser och intervall. Med den här funktionen kan ni matcha era Adobe Dynamic Media-distributioner med arkitekturen i ert innehållshanteringssystem och affärssystem.

Med Säker testning kan du förhandsgranska testversionen av webbplatsen med opublicerat innehåll.

Om du vill kan du skapa en staging-miljö i stället för att göra resurserna allmänt tillgängliga av följande skäl:

* Förhandsgranska webbplatser innan den offentliga lanseringen (testwebbplatsen).
* Tjäna resurser som kräver begränsad åtkomst, t.ex. e-kataloger som visar priser i B2B-webbprogram.
* Använd material bakom en brandvägg som en del av ett produktinformationshanteringssystem, en kundtjänstapplikation, en utbildningssajt med mera.

>[!NOTE]
>
>Säker testning påverkar inte åtkomsten till Adobe Dynamic Media Classic. Adobe Dynamic Media Classic säkerhet är konsekvent och kräver de vanliga inloggningsuppgifterna för åtkomst till Adobe Dynamic Media Classic och relaterade webbtjänster.

### Så här fungerar säker testning {#how-test-assets-works}

De flesta företag använder internet bakom en brandvägg. Tillgång till Internet är möjlig via vissa vägar och vanligtvis via ett begränsat antal offentliga IP-adresser.

Från ditt företagsnätverk kan du ta reda på din offentliga IP-adress med hjälp av webbplatser som [https://www.whatismyip.com](https://www.whatismyip.com/) eller begära den här informationen från företagets IT-organisation.

Med säker testning skapar Adobe Dynamic Media en dedikerad Image Server för testmiljöer eller interna applikationer. Alla förfrågningar till den här servern kontrollerar den ursprungliga IP-adressen. Om den inkommande begäran inte finns i den godkända listan över IP-adresser returneras ett felsvar. Adobe Dynamic Media Company Administrator konfigurerar den godkända listan över IP-adresser för företagets säkra testmiljö.

Eftersom platsen för den ursprungliga begäran måste bekräftas, dirigeras inte trafiken för tjänsten för säker testning via ett nätverk för innehållsdistribution, till exempel offentlig trafik för Dynamic Media Image Server. Begäranden till tjänsten för säker testning har en något högre fördröjning än de offentliga servrarna för dynamiska mediabilder.

Opublicerade resurser är omedelbart tillgängliga från tjänsterna för säker testning, utan att behöva publicera. På så sätt kan du köra en förhandsgranskning innan resurser publiceras till deras offentliga Image Server.

>[!NOTE]
>
>Tjänster för säker testning använder katalogservern som är konfigurerad med en intern publiceringskontext. Om ditt företag är konfigurerat att publicera till Säker testning blir därför alla överförda resurser i Adobe Dynamic Media omedelbart tillgängliga på säkra testningstjänster. Den här funktionen är sann oavsett om resurserna har markerats för publicering vid överföring.

Tjänster för säker testning har för närvarande stöd för följande resurstyper och funktioner:

* Bilder.
* Vinjetter (renderingsserverförfrågningar).
* Kunderna måste uttryckligen begära stöd för Render Server, vilket är tillgängligt.
* Uppsättningar, inklusive bilduppsättningar, eCatalog, renderingsuppsättningar och medieuppsättningar.
* Adobe Dynamic Media-visningsprogram som standard.
* Adobe Dynamic Media OnDemand JSP pages.
* Statiskt innehåll, till exempel PDF-filer och progressivt levererade videor.
* HTTP-videoströmning.
* Progressiv videoströmning.

Följande tillgångstyper och funktioner stöds för närvarande inte:

* Adobe Dynamic Media Classic Info- eller eCatalog-sökning
* RTMP-videoströmning
* Webb-till-tryck
* UGC-tjänster (användargenererat innehåll)

  >[!IMPORTANT]
  >
  >Från och med 1 maj 2023 är UGC-resurser i Dynamic Media tillgängliga för användning upp till 60 dagar från överföringsdatumet. Efter 60 dagar tas resurserna bort.

  >[!NOTE]
  >
  >Stöd för nya eller befintliga UGC-vektorbildresurser i Adobe Dynamic Media upphörde den 30 september 2021.

### Testa tjänsten för säker testning {#test-secure-testing-service}

Så här ser du till att tjänsten för säker testning fungerar som väntat:

#### Förbered ditt konto

1. Kontakta Adobe kundtjänst och begär att de aktiverar säker testning på ditt konto.
1. I Adobe Experience Manager väljer du **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Dynamic Media Publish Setup]**.
1. På sidan Image Server väljer du **[!UICONTROL Test Image Serving]** i listrutan **[!UICONTROL Publish Context]**.
1. Välj fliken **[!UICONTROL Security]**.
1. För filtret **[!UICONTROL Client address]** väljer du **[!UICONTROL Add]**.
1. Skriv en IP-adress i fältet **[!UICONTROL IP Address]**.
1. Skriv en nätmask i fältet **[!UICONTROL Mask]**.

   >[!NOTE]
   >
   >Om du lägger till mer än en IP-adress och nätmask tillåter det *alla* IP-adresser att göra tillgångsanrop, och alla visas.

1. Gör något av följande:

   * Om du vill lägga till fler IP-adresser upprepar du de tre föregående stegen.
   * Fortsätt till nästa steg.

1. Välj **[!UICONTROL Save]** i det övre högra hörnet på sidan Image Server.
1. Ladda upp bilderna till ditt Adobe Dynamic Media-konto.

<!--    See [Upload files](uploading-files.md#uploading_files). -->

1. Se till att vissa bilder är markerade för publicering och att andra är omarkerade och skicka sedan publiceringsjobbet.

<!--    See [Publish files](publishing-files.md#publishing_files). -->

1. Bestäm namnet på tjänsten för säker testning genom att gå till **[!UICONTROL Tools]** > **[!UICONTROL Assets]** > **[!UICONTROL Dynamic Media General Setting]**.
1. På sidan **[!UICONTROL Server]** hittar du servernamnet till höger om **[!UICONTROL Published Server Name]**.

Kontakta Adobe Care om servernamnet saknas eller om URL:en till servern inte fungerar.

#### Förbered webbplatsvarianter

Du behöver två varianter av en webbplats som länkar de publicerade och opublicerade resurserna:

* Offentlig version - Länka resurser med den traditionella Adobe Dynamic Media URL-syntaxen.
* Mellanlagringsversion - Länka resurser med samma syntax men med namnet på platsen för säker testning.

#### Kör testerna

Utför följande tester:

1. Kontrollera om resurser är synliga inifrån företagets nätverk.

   I företagsnätverket som identifieras av det tidigare definierade IP-adressintervallet visar mellanlagringsversionen av webbplatsen alla bilder, oavsett om de är markerade för publicering eller inte. Därför kan du testa utan att oavsiktligt göra bilder tillgängliga innan du förhandsgranskar eller startar produkten.

   Bekräfta att den offentliga versionen av din webbplats visar publicerade resurser så som de har varit med om Adobe Dynamic Media.

1. Verifiera att icke-publicerade resurser (dvs. omärkta för publicering) är skyddade från åtkomst från tredje part utanför företagets nätverk.

   Få åtkomst till nätverket från utsidan (till exempel från din hemdator eller via en 4G/5G-anslutning) och kontrollera sedan att den offentliga versionen av webbplatsen visar alla publicerade resurser, men inget av det opublicerade innehållet.

   Bekräfta att mellanlagringsversionen inte visar någon resurs eftersom du använder tjänsten för säker testning från en ej godkänd IP-adress.
