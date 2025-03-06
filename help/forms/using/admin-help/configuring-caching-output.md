---
title: Konfigurera cachning för utdata
description: Tjänsten Output cache-lagrar formulärdesigner, fragment och bilder. Lär dig hur du konfigurerar cachning för utdata.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_output
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: d6390e32-d348-42f2-84d0-9c83aff9ee3a
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 0%

---

# Konfigurera cachning för utdata  {#configuring-caching-for-output}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Output-tjänsten sammanfogar XML-formulärdata med en formulärdesign som skapats i Designer för att skapa ett dokumentutdataflöde i olika format.

Utdatasidan i administrationskonsolen innehåller inställningar som styr hur utdatatjänsten cachelagrar objekt. Du kan justera de här inställningarna för att optimera prestanda för utdatatjänsten.

Utdatatjänsten cachelagrar följande objekt:

* **formulärdesigner:** Output-tjänsten cachelagrar formulärdesigner som hämtas från databasen eller från HTTP-källor. Denna cachelagring förbättrar prestanda eftersom Output-tjänsten hämtar formulärdesignen från cachen i stället för från databasen för efterföljande renderingsbegäranden.
* **fragment och bilder:** Utdatatjänsten kan cachelagra fragment och bilder som används i formulärdesigner. När utdatatjänsten cachelagrar dessa objekt förbättras prestandan eftersom fragmenten och bilderna endast läses från databasen vid den första begäran.

Output lagrar cacheminnet på två platser:

* **i minnet:** Objekt lagras i minnet för snabb åtkomst. Cacheminnet i minnet har en begränsad storlek och tas bort när du startar om servern.
* **på disk:** Objekt lagras i serverns filsystem. Diskcachen har större kapacitet än cacheminnet i minnet och bevaras när du startar om servern. Platsen för diskcachen beror på programservern. Mer information om hur du ändrar platsen för diskcachen finns i [Ange filplatser för utdata](/help/forms/using/admin-help/specify-file-locations-output.md#specify-file-locations-for-output).

## Ange cacheläge {#specifying-the-cache-mode}

Output stöder två cachelagringslägen:

* ovillkorlig
* med cachekontrollpunkten

Om du växlar mellan cachelägen startar du om utdatatjänsten för att ändringen ska börja gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

Tidpunkten för cachekontrollen återställs automatiskt när du växlar mellan lägena.

### Använda ovillkorlig cachelagring {#using-unconditional-caching}

I det här läget validerar Output-tjänsten de resurser (formulärdesign och relaterade resurser som fragment och bilder) som krävs när en begäran tas emot. Utdatatjänsten jämför tidsstämpeln för resurserna i databasen med tidsstämpeln för resurserna i cachen. Om resursen i cachen är äldre uppdateras den av utdatatjänsten.

Det här cacheläget garanterar att de senaste resurserna används. Prestandan påverkas emellertid eftersom den cachelagrade objekten valideras mot databasen med varje begäran. Det här cacheläget är lämpligt för utvecklings- och mellanlagringsmiljöer där resurser uppdateras ofta och prestanda inte är ett primärt problem.

**Ange ovillkorlig cachelagring**

1. Klicka på Tjänster > Utdata i administrationskonsolen.
1. Välj Ovillkorligt under Inställningar för utdatacache och klicka på Spara.

### Använda cachekontrollpunkten {#use-the-cache-check-point}

I det här läget söker utdatatjänsten bara i databasen efter nyare versioner av resurser när tidsstämpeln för den cachelagrade resursen är äldre än tiden för cachekontrollen. Tidpunkten för den senaste cachekontrollen visas på utdatasidan i administrationskonsolen.

Använd det här cache-läget i högpresterande produktionsmiljöer där prestanda är ett problem och resursändringar är ovanliga. Du kan återställa tiden för cachekontrollpunkten när du vill distribuera ändringar som gjorts i databasresurserna.

**Ange användning av en cachekontrollpunkt**

1. I administrationskonsolen klickar du på Tjänster > Utdata.
1. Under Inställningar för kontroll av utdatacache markerar du Endast om den senaste valideringen gjordes före tidpunkten för cachekontrollen och klickar på Spara.

**Återställ cachekontrollpunkten**

1. Klicka på Tjänster > Utdata i administrationskonsolen.
1. Klicka på Cachekontrollpunkt under Inställningar för utdatacache.

### Återställ cacheinnehållet {#reset-the-cache-contents}

Du kan när som helst rensa innehållet i cachen. Efter en cacheåterställning tar den första begäran för varje formulär längre tid eftersom utdatatjänsten utför en fullständig återgivning och skapar nytt cacheinnehåll.

1. Klicka på Tjänster > Utdata i administrationskonsolen.
1. Klicka på Återställ cache under Inställningar för utdatacache.

## Konfigurerar cacheinställningar {#configuring-cache-settings}

Du kan ange inställningar som Output använder för cachning, vilket kan optimera prestandan i din AEM-formulärmiljö.

Om du vill komma åt de här inställningarna i administrationskonsolen klickar du på Tjänster > Utdata.

>[!NOTE]
>
>Diskkraven för cacheminnet ska vara lika med databasen.

### Ange globala cacheinställningar {#specifying-global-cache-settings}

Inställningarna i området **Globala cacheinställningar** påverkar alla typer av cacheminnen. Om du ändrar någon av dessa inställningar startar du om utdatatjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

**Maximal dokumentstorlek för cache (kB):** Den maximala storleken i kilobyte för en formulärdesign eller annan resurs som kan lagras i vilken cache som helst i minnet. Det här är en global inställning som gäller för alla cacheminnen i minnet. Om resursen är större än det här värdet cachelagras den inte i minnet. Standardvärdet är 1 024 kB. Den här inställningen påverkar inte diskcachen.

**Cacheminne för formuläråtergivning aktiverat:** Som standard är det här alternativet markerat, vilket innebär att återgivna formulär cachelagras för efterföljande hämtning. Den här inställningen har liten inverkan på utdatatjänstens prestanda eftersom den inte cachelagrar icke-interaktiva dokument. Det här alternativet har ingen effekt när du använder utdatatjänsten för icke-interaktiva dokument som återges på klienten.

### Cachelagra formulärdesigner {#caching-form-designs}

När utdatatjänsten tar emot en återgivningsbegäran, hämtar den formulärdesignen från databasen eller från en HTTP-källa och cachelagrar den. Denna cachelagring förbättrar prestanda eftersom Output-tjänsten hämtar formulärdesignen från cachen i stället för från databasen för efterföljande renderingsbegäranden.

Utdatatjänsten cachelagrar alltid formulärdesigner på disk. Om formulärdesigner lagras på servern betraktas dessa filer som diskcachen. Utdatatjänsten cachelagrar också formulärdesigner i minnet enligt inställningen i området **I minnesmallscache**. Om du ändrar någon av dessa inställningar startar du om utdatatjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

**Cachestorlek för mallkonfiguration:** Det maximala antalet mallkonfigurationsobjekt som ska behållas i minnet. Standardvärdet är 100. Vi rekommenderar att du anger det här värdet som är större än eller lika med värdet för Mallcachestorlek. Den här inställningen påverkar inte diskcachen.

**Mallcachestorlek:** Det maximala antalet mallinnehållsobjekt som ska behållas i minnet. Standardvärdet är 100. Den här inställningen påverkar inte diskcachen.

**Aktiverad:** Som standard är den här kryssrutan markerad, vilket innebär att formulärmallar cachelagras i minnet. När det här alternativet inte är markerat cachelagras endast formulärmallar på disken.

### Cachelagra fragment och bilder {#caching-fragments-and-images}

Output-tjänsten cachelagrar fragment och bilder som används i formulärdesigner på disk. Detta förbättrar prestandan eftersom fragmenten och bilderna endast läses från databasen vid den första begäran. Därefter läser Output-tjänsten fragment och bilder från diskcachen. Fragment och bilder cachelagras bara på disk och inte i minnet.

Du kan använda följande inställningar för att styra cachning på disk av fragment och bilder. De här inställningarna finns i området **Inställningar för mallresurscache**:

**Cachelagring av resurser** Välj ett av följande alternativ i listan:

**Aktiverad för fragment och bilder:** Utdatatjänsten cachelagrar fragment och bilder. Det här är standardalternativet.

**Aktiverat för fragment:** Utdatatjänsten cachelagrar fragment, men inte bilder.

**Inaktiverad:** Utdatatjänsten cachelagrar inte fragment eller bilder.

**Rensningsintervall (sekunder):** Anger hur ofta utdatatjänsten tar bort gamla ogiltiga cachefiler. Tjänsten Output tar inte bort giltiga cachefiler. Om du ändrar rensningsintervallet startar du om utdatatjänsten för att ändringen ska börja gälla. Om du vill starta om den här tjänsten kan du antingen använda Workbench eller läsa Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler.

## Klusteröverväganden för cacher {#clustering-considerations-for-caches}

I en klustrad miljö behåller varje nod sin egen cacheminne och diskcache. Cacheinnehållet på varje nod beror på vilka formulär som har renderats på den noden.

Platsen för cachen måste vara identisk (samma disk och sökväg) på varje nod i klustret. Placera inte cacheminnet på delad lagring.

Om du använder utdatasidan i administrationskonsolen för att ändra cacheinställningarna för en viss nod, uppdateras cacheinställningarna för andra noder när en begäran går till den noden. Det här beteendet gäller även för knappen Återställ cache. Om du klickar på knappen Återställ cache för en nod tas cachen omedelbart bort från den noden. Cacheminnet på andra noder rensas när en begäran går till den noden.
