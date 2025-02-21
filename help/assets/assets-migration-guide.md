---
title: Migrera resurser i grupp
description: Beskriver hur du hämtar resurser till  [!DNL Adobe Experience Manager], använder metadata, genererar återgivningar och aktiverar dem för publiceringsinstanser.
contentOwner: AG
role: Architect, Admin
feature: Migration,Renditions,Asset Management
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1721'
ht-degree: 6%

---

# Så här migrerar du resurser i grupp {#assets-migration-guide}

När du migrerar resurser till [!DNL Adobe Experience Manager] finns det flera steg att tänka på. Extrahering av resurser och metadata från det aktuella hemmet ligger utanför det här dokumentets omfång eftersom det varierar mycket mellan implementeringar, men i det här dokumentet beskrivs hur du överför dessa resurser till [!DNL Experience Manager], använder deras metadata, genererar återgivningar och aktiverar dem för publiceringsinstanser.

## Förutsättningar {#prerequisites}

Granska och implementera vägledningen i [Assets tips för prestandajustering](performance-tuning-guidelines.md) innan du utför något av stegen i den här metoden. Många av stegen, som att konfigurera maximalt antal samtidiga jobb, förbättrar serverns stabilitet och prestanda under inläsning avsevärt. Andra steg, som att konfigurera ett fildatalager, är mycket svårare att utföra efter att systemet har lästs in med resurser.

>[!NOTE]
>
>Följande verktyg för resursmigrering ingår inte i [!DNL Experience Manager] och stöds inte av Adobe:
>
>* ACS AEM Tools Tag Maker
>* ACS AEM Tools CSV Asset Importer
>* ACS Commons - massarbetsflödeshanterare
>* Snabbåtgärdshanteraren för ACS-kommandon
>* Syntetiskt arbetsflöde
>
>Programvaran är öppen källkod och täcks av [Apache v2-licensen](https://adobe-consulting-services.github.io/pages/license.html). Om du vill ställa en fråga eller rapportera ett problem går du till [GitHub Issues for ACS AEM Tools](https://github.com/Adobe-Consulting-Services/acs-aem-commons/issues) eller [ACS AEM Commons](https://github.com/Adobe-Consulting-Services/acs-aem-tools/issues).

## Migrera till [!DNL Experience Manager] {#migrating-to-aem}

Migrering av resurser till [!DNL Experience Manager] kräver flera steg och bör visas i en fasad process. Flyttningsfaserna är följande:

1. Inaktivera arbetsflöden.
1. Läs in taggar.
1. Ingest assets.
1. Bearbeta återgivningar.
1. Aktivera resurser.
1. Aktivera arbetsflöden.

![chlimage_1-223](assets/chlimage_1-223.png)

### Inaktivera arbetsflöden {#disabling-workflows}

Innan du startar migreringen inaktiverar du dina startprogram för arbetsflödet [!UICONTROL DAM Update Asset]. Det är bäst att importera alla resurser till systemet och sedan köra arbetsflödena gruppvis. Om du redan är aktiv under migreringen kan du schemalägga att dessa aktiviteter körs på ledig tid.

### Läs in taggar {#loading-tags}

Du kanske redan har en tagg-taxonomi på plats som du tillämpar på dina bilder. Verktyg som CSV-resursimporteraren och [!DNL Experience Manager]-stöd för metadataprofiler kan automatisera processen att använda taggar på resurser, men taggarna måste läsas in i systemet. Med funktionen [ACS AEM Tools Tag Maker](https://adobe-consulting-services.github.io/acs-aem-tools/features/tag-maker/index.html) kan du fylla i taggar med hjälp av ett Microsoft Excel-kalkylblad som är inläst i systemet.

### Ingående resurser {#ingesting-assets}

Prestanda och stabilitet är viktiga frågor när du ska hämta in resurser i systemet. Eftersom du läser in en stor mängd data i systemet bör du se till att systemet fungerar bra för att minimera den tid som krävs och undvika att överbelasta systemet, vilket kan orsaka en systemkrasch, särskilt i system som redan är i produktion.

Det finns två sätt att läsa in resurser i systemet: en push-baserad metod med HTTP eller en pull-baserad metod med JCR API:erna.

#### Skicka via HTTP {#pushing-through-http}

Adobe Managed Services-team använder ett verktyg som heter Glutton för att läsa in data i kundmiljöer. Glutton är ett litet Java-program som läser in alla resurser från en katalog till en annan katalog i en [!DNL Experience Manager]-distribution. I stället för Glutton kan du också använda verktyg som Perl-skript för att publicera resurserna i databasen.

Det finns två nackdelar med att använda metoden att gå igenom https:

1. Resurserna måste överföras via HTTP till servern. Detta kräver en hel del extraarbete och är tidskrävande, vilket gör att det tar längre tid att utföra migreringen.
1. Om du har taggar och anpassade metadata som måste användas för resurserna, kräver den här metoden en andra anpassad process som du måste köra för att använda dessa metadata för resurserna när de har importerats.

Det andra sättet att importera resurser är att hämta resurser från det lokala filsystemet. Om du inte kan få en extern enhet eller nätverksresurs monterad på servern för att utföra en pull-baserad metod är det bästa alternativet att publicera resurserna via HTTP.

#### Hämta från det lokala filsystemet {#pulling-from-the-local-filesystem}

[ACS AEM Tools CSV-resursimporteraren](https://adobe-consulting-services.github.io/acs-aem-tools/features/csv-asset-importer/index.html) hämtar resurser från filsystemet och metadata för resurser från en CSV-fil för resursimporten. Experience Manager Asset Manager API används för att importera resurserna till systemet och använda de konfigurerade metadataegenskaperna. Resurser monteras helst på servern via en nätverksfilmontering eller via en extern enhet.

Eftersom resurser inte behöver överföras via ett nätverk förbättras prestandan avsevärt, och den här metoden anses generellt vara det mest effektiva sättet att läsa in resurser i databasen. Eftersom verktyget har stöd för metadatainmatning kan du dessutom importera alla resurser och metadata i ett enda steg i stället för att skapa ett andra steg för att använda metadata via ett separat verktyg.

### Bearbeta återgivningar {#processing-renditions}

När du har läst in resurserna i systemet måste du bearbeta dem via arbetsflödet [!UICONTROL DAM Update Asset] för att extrahera metadata och generera återgivningar. Innan du utför det här steget måste du duplicera och ändra arbetsflödet för [!UICONTROL DAM Update Asset] så att det passar dina behov. Det färdiga arbetsflödet innehåller många steg som kanske inte är nödvändiga för dig, till exempel Dynamic Media PTIFF-generering eller [!DNL InDesign Server]-integrering.

När du har konfigurerat arbetsflödet efter dina behov finns det två alternativ:

1. Det enklaste sättet är [ACS Commons&#39; massarbetsflödeshanterare](https://adobe-consulting-services.github.io/acs-aem-commons/features/bulk-workflow-manager.html). Med det här verktyget kan du köra en fråga och bearbeta resultatet av frågan via ett arbetsflöde. Det finns även alternativ för att ange gruppstorlekar.
1. Du kan använda [ACS Commons Fast Action Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/fast-action-manager.html) tillsammans med [syntetiska arbetsflöden](https://adobe-consulting-services.github.io/acs-aem-commons/features/synthetic-workflow.html). Även om det här arbetssättet är mycket mer involverat kan du ta bort overheadkostnaden för arbetsflödesmotorn [!DNL Experience Manager] samtidigt som du optimerar användningen av serverresurser. Dessutom förbättrar Fast Action Manager resultatet ytterligare genom att övervaka serverresurser dynamiskt och begränsa belastningen på systemet. Exempel på skript finns på funktionssidan för ACS Commons.

### Aktivera resurser {#activating-assets}

För distributioner som har en publiceringsnivå måste du aktivera resurserna i publiceringsgruppen. Adobe rekommenderar att du kör mer än en publiceringsinstans, men det är effektivast att replikera alla resurser till en publiceringsinstans och sedan klona den instansen. När du aktiverar ett stort antal resurser kan du behöva ingripa efter att ha aktiverat ett träd. Därför: När aktiveringar utlöses läggs objekt till i kön för Samling-jobb/händelse. När storleken på den här kön börjar bli över cirka 40 000 objekt tar det dramatiskt lång tid att bearbeta. När storleken på den här kön överstiger 100 000 objekt börjar systemstabiliteten försämras.

Du kan lösa det här problemet genom att använda [Snabb åtgärdshanterare](https://adobe-consulting-services.github.io/acs-aem-commons/features/fast-action-manager.html) för att hantera resursreplikering. Detta fungerar utan att använda Sling-köerna, vilket sänker overheadkostnaderna samtidigt som arbetsbelastningen begränsas för att förhindra att servern blir överbelastad. Ett exempel på hur du använder FAM för att hantera replikering visas på funktionens dokumentationssida.

Andra alternativ för att hämta resurser till publiceringsgruppen är bland annat att använda [vlt-rcp](https://jackrabbit.apache.org/filevault/rcp.html) eller [oak-run](https://github.com/apache/jackrabbit-oak/tree/trunk/oak-run), som ingår i Jackrabbit. Ett annat alternativ är att använda ett verktyg med öppen källkod för din [!DNL Experience Manager]-infrastruktur som heter [Grabbit](https://github.com/TWCable/grabbit) och som anger att den har snabbare prestanda än VLT.

För någon av dessa metoder är caveat att resurserna i författarinstansen inte visas som aktiverade. Om du vill hantera flaggning av dessa resurser med rätt aktiveringsstatus måste du också köra ett skript som markerar resurserna som aktiverade.

>[!NOTE]
>
>Adobe underhåller eller stöder inte Grabbit.

### Klona publicering {#cloning-publish}

När resurserna har aktiverats kan du klona publiceringsinstansen och skapa så många kopior som behövs för distributionen. Det är ganska enkelt att klona en server, men det finns några viktiga steg att komma ihåg. Så här klonar du publiceringen:

1. Säkerhetskopiera källinstansen och datalagret.
1. Återställ säkerhetskopian av instansen och datalagret till målplatsen. Följande steg hänvisar alla till den nya instansen.
1. Utför en filsystemssökning under `crx-quickstart/launchpad/felix` för `sling.id`. Ta bort den här filen.
1. Under datalagrets rotsökväg letar du reda på och tar bort alla `repository-XXX`-filer.
1. Redigera `crx-quickstart/install/org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config` och `crx-quickstart/launchpad/config/org/apache/jackrabbit/oak/plugins/blob/datastore/FileDataStore.config` för att peka på platsen för datalagret i den nya miljön.
1. Starta miljön.
1. Uppdatera konfigurationen för alla replikeringsagenter på författaren/författarna så att de pekar på rätt publiceringsinstanser eller push-agenter för dispatcher på den nya instansen för att peka på rätt dispatchers för den nya miljön.

### Aktivera arbetsflöden {#enabling-workflows}

När migreringen är klar bör startarna för arbetsflödena för [!UICONTROL DAM Update Asset] återaktiveras så att de stöder generering av återgivningar och metadataextrahering för den dagliga systemanvändningen.

## Migrera över [!DNL Experience Manager] distributioner {#migrating-between-aem-instances}

Även om det inte är nästan lika vanligt behöver du ibland migrera stora mängder data från en [!DNL Experience Manager]-distribution till en annan. När du till exempel utför en [!DNL Experience Manager]-uppgradering måste du uppgradera maskinvaran eller migrera till ett nytt datacenter, till exempel med en AMS-migrering.

I det här fallet är dina resurser redan ifyllda med metadata och återgivningar har redan genererats. Du kan helt enkelt fokusera på att flytta resurser från en instans till en annan. När du migrerar mellan distributionen av [!DNL Experience Manager] utför du följande steg:

1. Inaktivera arbetsflöden: Eftersom du migrerar återgivningar tillsammans med våra resurser vill du inaktivera arbetsflödets startprogram för arbetsflödet [!UICONTROL DAM Update Asset].

1. Migrera taggar: Eftersom du redan har taggar inlästa i källdistributionen [!DNL Experience Manager] kan du skapa dem i ett innehållspaket och installera paketet på målinstansen.

1. Migrera resurser: Det finns två verktyg som rekommenderas för att flytta resurser från en [!DNL Experience Manager]-distribution till en annan:

   * Med **Vault Remote Copy** eller vlt rcp kan du använda VLT i ett nätverk. Du kan ange en käll- och målkatalog och hämta alla databasdata från en instans och läsa in dem i en annan. Vlt rcp finns på [https://jackrabbit.apache.org/filevault/rcp.html](https://jackrabbit.apache.org/filevault/rcp.html)
   * **Grabbit** är ett verktyg för innehållssynkronisering med öppen källkod som har utvecklats av Time Warner Cable för deras [!DNL Experience Manager]-implementering. Eftersom den använder kontinuerliga dataströmmar har den en lägre fördröjning jämfört med vlt rcp och kräver en hastighetsförbättring på två till tio gånger snabbare än vlt rcp. Grabbit har även stöd för synkronisering av deltainnehåll, vilket gör att det kan synkronisera ändringar efter att en första migreringspass har slutförts.

1. Aktivera resurser: Följ instruktionerna för [aktivering av resurser](#activating-assets) som dokumenterats för den första migreringen till [!DNL Experience Manager].

1. Klona publicering: Precis som med en ny migrering är det effektivare att läsa in en enda publiceringsinstans och klona den än att aktivera innehållet på båda noderna. Se [Klona publicering.](#cloning-publish)

1. Aktivera arbetsflöden: När du har slutfört migreringen aktiverar du starkarna för arbetsflödet [!UICONTROL DAM Update Asset] igen så att återgivningsgenerering och metadataextrahering stöds för den dagliga systemanvändningen.
