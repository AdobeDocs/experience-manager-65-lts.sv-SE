---
title: Ange filplatser för utdata
description: Lär dig hur du anger filplatser för utdata för vissa typer av filer, till exempel Content Root URI, XCI Configuration File, Cache och Default.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_output
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: aff0274a-bbb7-4062-afaf-7f9c31f57cb1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Ange filplatser för utdata {#specify-file-locations-for-output}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Du kan ange var utdata ska söka efter vissa typer av filer som krävs.

1. Klicka på Tjänster > Utdata i administrationskonsolen.
1. Ange lämpliga alternativ under Platser.
1. Klicka på Spara.

## Platsinställningar {#locations-settings}

**Innehållsrots-URI:** URI:n eller den absoluta platsen för databasen som formulär hämtas från. Detta värde kombineras med parametern sForm, som anges via API:t, för att skapa den absoluta sökvägen till det formulär som hämtas. Det här värdet kan referera till en katalog eller en webbplats som är tillgänglig via HTTP.

Standardvärdet är en tom sträng.

**XCI-konfigurationsfil:** Den relativa eller absoluta platsen för XCI-konfigurationsfilen som Output-tjänsten använder för återgivning. För ett relativt värde antas XCI-filen finnas i AEM-formulärens distribuerbara EAR-fil.

Standardvärdet är `com/adobe/formServer/PA/pa_output.xci`.

**Cacheplats:** Anger platsen för cacheminnet för utdatadisk. När du ändrar den här inställningen återställs all befintlig cacheinformation från den aktuella platsen och en ny cache skapas på den nya platsen. Välj något av följande alternativ:

**Standardplats:** Det här är standardvalet. När det här alternativet är markerat skapas cachen på en plats som är beroende av den programserver som du använder:

* **JBoss:** `[JBoss Home]\server\[install type]\svcdata\Output\Cache`
* **WebLogic:** `[WebLogic Home]\user_projects\domains\[aem-forms domain Name]\adobe\[Forms Server name]\Output\Cache`
* **WebSphere:** `[IBM Home]\WebSphere\AppServer\installedApps\adobe\server1\Output\Cache`

**Tillfällig LC-katalog:** Cachen skapas i en underkatalog till den tillfälliga AEM-formulärkatalogen, som anges i administrationskonsolen under Inställningar > Systeminställningar > Konfigurationer > Plats för tillfällig katalog. Underkatalogen heter `adobeoutput_[servername]`.

>[!NOTE]
>
>Om du använder ett tillfälligt rensningsverktyg och tar bort de här katalogerna påverkar inte funktionerna avsevärt, men prestanda kan påverkas avsevärt under en kort tid tills det nya cacheminnet skapas. För att undvika det här problemet ska du inte ta bort de här katalogerna när du rensar den tillfälliga AEM-formulärkatalogen.
