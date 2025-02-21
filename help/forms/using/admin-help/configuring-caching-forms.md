---
title: Konfigurera cachelagring för Forms
description: Lär dig hur du konfigurerar cacheinställningar och hur du klustertar hänsyn till cacheminnen.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# Konfigurera cachelagring för Forms{#configuring-caching-for-forms}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Forms-tjänsten tar formulärdesigner som skapats i Designer och återger dem i olika format.

Forms-sidan i administrationskonsolen innehåller inställningar som styr hur Forms-tjänsten cachelagrar objekt. Du kan justera de här inställningarna för att optimera prestandan för Forms-tjänsten.

Forms-tjänsten cachelagrar följande objekt:

* **formulärdesigner:** Forms-tjänsten cachelagrar formulärdesigner som hämtas från databasen eller från HTTP-källor. Denna cachning förbättrar prestanda eftersom Forms-tjänsten hämtar formulärdesignen från cachen i stället för från databasen för efterföljande återgivningsbegäranden.
* **fragment och bilder:** Forms-tjänsten kan cachelagra fragment och bilder som används i formulärdesigner. När Forms-tjänsten cachelagrar dessa objekt förbättras prestandan eftersom fragmenten och bilderna endast läses från databasen vid den första begäran.
* **formulär:** Forms-tjänsten cachelagrar de formulär som återges. Den här typen av cachning förbättrar prestanda eftersom Forms-tjänsten inte behöver matcha och återge samma formulär vid efterföljande begäranden.

Forms sparar cacheminnet på två platser:

* **i minnet:** Objekt lagras i minnet för snabb åtkomst. Cacheminnet i minnet har en begränsad storlek och tas bort när du startar om servern.
* **på disk:** Objekt lagras i serverns filsystem. Diskcachen har större kapacitet än cacheminnet i minnet och bevaras när du startar om servern. Platsen för diskcachen beror på programservern. Mer information om hur du ändrar platsen för diskcachen finns i [Konfigurera platser för Forms](/help/forms/using/admin-help/configuring-locations-forms.md#configuring-locations-for-forms).

## Ange cacheläge {#specifying-the-cache-mode}

Forms har stöd för två cachelagringslägen:

* ovillkorlig
* med cachekontrollpunkten

Om du växlar mellan cachelägen startar du om Forms-tjänsten för att ändringen ska börja gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

Tidpunkten för cachekontrollen återställs automatiskt när du växlar mellan lägena.

### Använda ovillkorlig cachelagring {#using-unconditional-caching}

I det här läget validerar Forms-tjänsten de resurser (formulärdesign och relaterade resurser som fragment och bilder) som krävs när en begäran tas emot. Forms-tjänsten jämför tidsstämpeln för resurserna i databasen med tidsstämpeln för resurserna i cachen. Om resursen i cachen är äldre uppdateras den av Forms-tjänsten.

Det här cacheläget garanterar att de senaste resurserna används. Prestanda påverkas emellertid eftersom Forms-tjänsten validerar de cachelagrade objekten mot databasen för varje begäran. Det här cacheläget är lämpligt för utvecklings- och mellanlagringsmiljöer där resurser uppdateras ofta och prestanda inte är ett primärt problem.

**Ange ovillkorlig cachelagring**

1. Klicka på Tjänster > Forms i administrationskonsolen.
1. Under Kontrollinställningar för Forms-cache väljer du Ovillkorligt och klickar på Spara.

### Använda cachekontrollpunkten {#use-the-cache-check-point}

I det här läget söker Forms-tjänsten endast i databasen efter nyare versioner av resurser när tidsstämpeln för den cachelagrade resursen är äldre än tiden för cachekontrollen. Tidpunkten för den senaste cachekontrollen visas på Forms-sidan i administrationskonsolen.

Använd det här cache-läget i högpresterande produktionsmiljöer där prestanda är ett problem och resursändringar är ovanliga. Du kan återställa tiden för cachekontrollpunkten när du vill distribuera ändringar som gjorts i databasresurserna.

**Ange användning av en cachekontrollpunkt**

1. I administrationskonsolen klickar du på Tjänster > Forms.
1. Under Kontrollinställningar för Forms-cache markerar du Endast om den senaste valideringen gjordes före tidpunkten för cachekontrollen och klickar på Spara.

**Återställ cachekontrollpunkten**

1. Klicka på Tjänster > Forms i administrationskonsolen.
1. Klicka på Cachekontrollpunkt under Kontrollinställningar för Forms-cache.

**Återställ cacheinnehållet**

Du kan när som helst rensa innehållet i cachen. Efter en cacheåterställning tar den första begäran för varje formulär längre tid eftersom Forms-tjänsten utför en fullständig återgivning och skapar nytt cacheinnehåll.

1. Klicka på Tjänster > Forms i administrationskonsolen.
1. Klicka på Återställ cache under Inställningar för Forms-cachekontroll.

## Konfigurerar cacheinställningar {#configuring-cache-settings}

Du kan ange inställningar som Forms använder för cachning, vilket kan optimera prestandan i din AEM-formulärmiljö.

Om du vill komma åt de här inställningarna klickar du på Tjänster > Forms i administrationskonsolen.

>[!NOTE]
>
>Diskkraven för cacheminnet ska vara lika med databasen.

### Ange globala cacheinställningar {#specifying-global-cache-settings}

Inställningarna i området **Globala cacheinställningar** påverkar alla typer av cacheminnen. Om du ändrar någon av dessa inställningar startar du om Forms-tjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

**Maximal dokumentstorlek för cache (kB):** Den maximala storleken i kilobyte för en formulärdesign eller annan resurs som kan lagras i vilken cache som helst i minnet. Det här är en global inställning som gäller för alla cacheminnen i minnet. Om en resurs är större än det här värdet cachelagras den inte i minnet. Standardvärdet är 1 024 kB. Den här inställningen påverkar inte diskcachen.

**Cacheminne för formuläråtergivning aktiverat:** Som standard är det här alternativet markerat, vilket innebär att återgivna formulär cachelagras för efterföljande hämtning. Den här inställningen förbättrar prestanda eftersom Forms-tjänsten endast behöver återge ett visst formulär en gång och sedan använder den cachelagrade versionen. Det här alternativet fungerar med formulärdesignens cachelagringsegenskap. Information om hur du konfigurerar det här värdet i formulärdesignen finns i hjälpen för Designer.

### Cachelagra formulärdesigner {#caching-form-designs}

När Forms-tjänsten tar emot en renderingsbegäran, hämtas formulärdesignen från databasen och cachelagras. Denna cachning förbättrar prestanda eftersom Forms-tjänsten hämtar formulärdesignen från cachen i stället för från databasen för efterföljande återgivningsbegäranden.

Forms-tjänsten cachelagrar alltid formulärdesigner på disk. Om formulärdesigner lagras på servern betraktas dessa filer som diskcachen. Forms-tjänsten cachelagrar också formulärdesigner i minnet enligt inställningen i **området Minnesmallscache**. Om du ändrar någon av dessa inställningar startar du om Forms-tjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

**Cachestorlek för mallkonfiguration:** Det maximala antalet mallkonfigurationsobjekt som ska behållas i minnet. Standardvärdet är 100. Vi rekommenderar att du anger det här värdet som är större än eller lika med värdet för Mallcachestorlek. Den här inställningen påverkar inte diskcachen.

**Mallcachestorlek:** Det maximala antalet mallinnehållsobjekt som ska behållas i minnet. Standardvärdet är 100. Den här inställningen påverkar inte diskcachen.

**Aktiverad:** Som standard är den här kryssrutan markerad, vilket innebär att formulärmallar cachelagras i minnet. När det här alternativet inte är markerat cachelagras endast formulärmallar på disken.

### Cachelagra återgivna formulär {#caching-rendered-forms}

Forms-tjänsten cachelagrar återgivna formulär så att de inte behöver matcha och återge samma formulär vid efterföljande förfrågningar. Återgivna formulär cachelagras både på disk och i minnet.

De här inställningarna finns i området **i cache för återgivning av minnesformulär**. Om du ändrar någon av dessa inställningar startar du om Forms-tjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten använder du Workbench eller läser [Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler](/help/forms/using/admin-help/starting-stopping-services.md#start-or-stop-the-services-associated-with-aem-forms-modules) för instruktioner.

**Cachestorlek:** Anger det maximala antalet återgivna formulär som kan finnas i cacheminnet i minnet. Standardvärdet är 100. Den här inställningen påverkar inte diskcachen.

**Aktiverad:** Som standard är det här alternativet markerat, vilket innebär att återgivna formulär cachelagras i minnet. När det här alternativet inte är markerat cachelagras endast återgivna formulär på disken.

### Cachelagra fragment och bilder {#caching-fragments-and-images}

Forms-tjänsten cachelagrar fragment och bilder som används i formulärdesigner på disk. Detta förbättrar prestandan eftersom fragmenten och bilderna endast läses från databasen vid den första begäran. Därefter läser Forms-tjänsten fragment och bilder från diskcachen. Fragment och bilder cachelagras bara på disk och inte i minnet.

Du kan använda följande inställningar för att styra cachning på disk av fragment och bilder. De här inställningarna finns i området **Inställningar för mallresurscache**:

**Cachelagring av resurser** Välj ett av följande alternativ i listan:

**Aktiverad för fragment och bilder:** Forms-tjänsten cachelagrar fragment och bilder. Det här är standardalternativet.

**Aktiverat för fragment:** Forms-tjänsten cachelagrar fragment, men inte bilder.

**Inaktiverad:** Forms-tjänsten cachelagrar inte fragment eller bilder.

**Rensningsintervall (sekunder):** Anger hur ofta Forms-tjänsten tar bort gamla ogiltiga cachefiler. Forms tar inte bort giltiga cachefiler. Om du ändrar rensningsintervallet startar du om Forms-tjänsten så att ändringen börjar gälla. Om du vill starta om den här tjänsten kan du antingen använda Workbench eller läsa Starta eller stoppa de tjänster som är kopplade till AEM formulärmoduler. Standardvärdet är 600 sekunder.

## Klusteröverväganden för cacher {#clustering-considerations-for-caches}

I en klustrad miljö behåller varje nod sin egen cacheminne och diskcache. Cacheinnehållet på varje nod beror på vilka formulär som har renderats på den noden.

Platsen för cachen måste vara identisk (samma disk och sökväg) på varje nod i klustret. Placera inte cacheminnet på delad lagring.

Om du använder Forms-sidan i administrationskonsolen för att ändra cacheinställningarna för en viss nod, uppdateras cacheinställningarna för andra noder när en begäran går till den noden. Det här beteendet gäller även för knappen Återställ cache. Om du klickar på knappen Återställ cache för en nod tas cachen omedelbart bort från den noden. Cacheminnet på andra noder rensas när en begäran går till den noden.
