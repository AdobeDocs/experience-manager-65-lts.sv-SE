---
title: Konfigurera inloggningsuppgifter för användning med Acrobat Reader DC-tillägg
description: Lär dig hur du konfigurerar autentiseringsuppgifter för användning med Acrobat Reader DC-tillägg.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_acrobat_reader_dc_extensions
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 040a4db1-45e1-4501-8117-d2d41d4a73ea
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Konfigurera inloggningsuppgifter för användning med Acrobat Reader DC-tillägg{#configuring-credentials-for-use-with-acrobat-reader-dc-extensions}

Om du vill lägga in användarrättigheter i PDF-dokument konfigurerar du AEM-formulär med giltiga autentiseringsuppgifter för Acrobat Reader DC Extensions. En autentiseringsuppgift kan ha konfigurerats under installationen av AEM-formulär. Om du inte konfigurerade autentiseringsuppgifterna för Acrobat Reader DC Extensions medan du kör Configuration Manager eller, om du behöver importera en ny eller ersättande autentiseringsuppgift, kan du göra det med hjälp av sidorna Pålitlig lagringshantering.

Om du använder en utvärderingsreferens ersätter du den med en produktionsautentiseringsuppgift när du flyttar till produktionsmiljön. Om du vill uppdatera en inloggningsinformation som har gått ut eller utvärdera den tar du först bort den gamla inloggningsinformationen för Acrobat Reader DC-tillägg.

Mer information om att hämta autentiseringsuppgifter finns i [Förbereder installation av AEM-formulär (en server)](https://helpx.adobe.com/pdf/aem-forms/6-3/prepare-install-single-server.pdf).

Trust Store kan innehålla mer än en autentiseringsuppgift för Acrobat Reader DC Extensions. Ange en av dessa autentiseringsuppgifter som standardautentiseringsuppgifter för Reader Extensions. Standardautentiseringsuppgifterna används när en Workbench-användare inte kan avgöra vilka autentiseringsuppgifter som ska användas när processen skapas. Dessa regler gäller för standardautentiseringsuppgifter:

* Om du importerar en autentiseringsuppgift för Acrobat Reader DC Extensions och Trust Store inte innehåller några andra autentiseringsuppgifter för Acrobat Reader DC Extensions, anges det som standard.
* Om du importerar en autentiseringsuppgift för Acrobat Reader DC Extensions med standardalternativet markerat, tas standardtypen bort från en befintlig standardautentiseringsuppgift. De importerade autentiseringsuppgifterna blir standard.
* Du kan inte ta bort en standardautentiseringsuppgift för Acrobat Reader DC Extensions. Om du vill ta bort standardautentiseringsuppgifterna anger du först en annan autentiseringsuppgift som standard. Ett undantag till den här regeln är att om det bara finns en autentiseringsuppgift kan du ta bort den även om den är standard.
* Du kan inte uppdatera standardautentiseringsuppgifter för Acrobat Reader DC Extensions.

>[!NOTE]
>
>Du kan också importera och ta bort autentiseringsuppgifter via programmering. (Se [Programmering med AEM-formulär](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=sv-SE).)

## Importera autentiseringsuppgifter för Acrobat Reader DC Extensions {#import-a-acrobat-reader-dc-extensions-credential}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Klicka på Importera och välj Autentiseringsuppgifter för Acrobat Reader DC-tillägg under Pålitlig lagringstyp.
1. (Valfritt) Välj Standard om du vill ange att de här autentiseringsuppgifterna är standardautentiseringsuppgifter som ska användas med Acrobat Reader DC-tillägg.
1. Ange en identifierare för autentiseringsuppgifterna i rutan Alias. Den här identifieraren används som visningsnamn för autentiseringsuppgifterna i Acrobat Reader DC-tillägg. Det här aliaset används även för att komma åt inloggningsuppgifterna via programmering med AEM-formulären SDK.

   >[!NOTE]
   >
   >Aliasnamnet konverteras automatiskt till versaler för visning. Aliasnamnet är inte skiftlägeskänsligt när du refererar till det i en process.

1. Klicka på Välj fil för att leta upp autentiseringsuppgifterna, skriv lösenordet för autentiseringsuppgifterna och klicka sedan på OK.

   Om felmeddelandet&quot;Det gick inte att importera autentiseringsuppgifter på grund av felaktigt filformat eller felaktigt lösenord&quot; visas kontrollerar du att lösenordet är giltigt.

## Ta bort autentiseringsuppgifter för Acrobat Reader DC Extensions {#remove-a-acrobat-reader-dc-extensions-credential}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Markera autentiseringsuppgifterna och klicka på Ta bort.

## Ersätta autentiseringsuppgifter för Acrobat Reader DC-tillägg {#replace-a-acrobat-reader-dc-extensions-credential}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Observera det befintliga autentiseringsuppgiftens alias, markera det och klicka sedan på Ta bort.
1. Importera de nya autentiseringsuppgifterna med exakt samma aliasnamn.
