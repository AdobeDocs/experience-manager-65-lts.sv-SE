---
title: Ange timeoutvärden för användning med Acrobat Reader DC-tillägg
description: Lär dig hur du anger timeoutvärden som ska användas med Acrobat Reader DC-tillägg.
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# Ange timeoutvärden för användning med Acrobat Reader DC-tillägg  {#setting-timeout-values-for-use-with-acrobat-reader-dc-extensions}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

När du arbetar med många PDF-filer i Acrobat Reader DC Extensions ska du se till att följande tidsgräns är korrekt inställd för att förhindra att jobben tajmar ut och misslyckas:

**Tidsgräns för dokumentborttagning**

Det här värdet kan anges i administrationskonsolen. Klicka på Inställningar > Systeminställningar > Konfigurationer och ange ett värde för Standardtidsgräns för dokumentborttagning.

**Timeout för användarhanterarens AEM-formulär:** Det här värdet kan anges genom redigering av filen config.xml. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler och sedan på Exportera. Öppna den exporterade filen config.xml och redigera följande rader:

&lt;entry key=&quot;assertionValidityInMinutes&quot; value=&quot;600&quot;/>

&lt;entry key=&quot;SessionTimeout&quot; value=&quot;600&quot;/>

Spara och importera sedan filen config.xml tillbaka till administrationskonsolen.

**Tidsgräns för programserversession:** Det här värdet kan anges på programservern. Mer information finns i dokumentationen som medföljer programservern.
