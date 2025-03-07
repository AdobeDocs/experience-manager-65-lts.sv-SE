---
title: Hantera lokala autentiseringsuppgifter
description: Lär dig hur du hanterar lokala autentiseringsuppgifter med pålitlighetslagerhanteringen. AEM-formulär har stöd för RSA- och DSA-autentiseringsuppgifter i PKCS12-standardformulär.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_certificates_and_credentials
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
hide: true
hidefromtoc: true
exl-id: d297ab09-2b92-442a-8b19-ffee86e24bb9
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---

# Hantera lokala autentiseringsuppgifter {#managing-local-credentials}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Lokala autentiseringsuppgifter är privata nyckelautentiseringsuppgifter som lagras i Trust Store Management. En *lokal autentiseringsuppgift* identifierar var användarens DES-autentiseringsuppgifter lagras. Med Trust Store Management kan du importera och hantera dina lokala inloggningsuppgifter med exempelvis befintliga PFX-filer så att du kan importera, redigera och ta bort lokala inloggningsuppgifter.

AEM-formulär har stöd för RSA- och DSA-referenser på upp till 4 096 bitar i det vanliga PKCS12-formatet (.pfx- och .p12-filer).

Du kan importera och exportera valfritt antal autentiseringsuppgifter. Om du vill ersätta en inloggningsinformation som har gått ut med samma alias tar du bort inloggningsinformationen och importerar sedan den nya inloggningsinformationen med samma alias.

Information och instruktioner om Acrobat Reader DC-tillägg finns i [Konfigurera autentiseringsuppgifter för Acrobat Reader DC-tillägg](/help/forms/using/admin-help/configuring-credentials-acrobat-reader-dc.md#configuring-credentials-for-use-with-acrobat-reader-dc-extensions).

## Importera en autentiseringsuppgift {#import-a-credential}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Klicka på Importera. Välj något av följande alternativ under Pålitlig lagringstyp:

   * **Dokumentsigneringsreferens:** En autentiseringsuppgift som används för att utfärda en digital signatur för ett dokument.
   * **Acrobat Reader DC Extensions Credential:** Ett digitalt certifikat som är specifikt för Acrobat Reader DC Extensions som gör att Adobe Reader-användningsrättigheter kan aktiveras i de PDF-dokument som skapas.
   * **Standard:** Anger att det här är standardautentiseringsuppgifterna som ska användas med Acrobat Reader DC-tillägg.

   Mer information om att hämta autentiseringsuppgifter finns i [Förbereder installation av AEM-formulär](https://helpx.adobe.com/pdf/aem-forms/6-3/prepare-install-single-server.pdf).

1. Ange en identifierare för autentiseringsuppgifterna i rutan Alias. Den här identifieraren används som visningsnamn för autentiseringsuppgifterna i Acrobat Reader DC-tillägg och Signature-tjänsten. Det här aliaset används även för att komma åt inloggningsuppgifterna via programmering med AEM-formulären SDK.

   >[!NOTE]
   >
   >Aliasnamnet konverteras automatiskt till versaler för visning. Aliasnamnet är inte skiftlägeskänsligt när du refererar till det i en process.

1. Klicka på Bläddra för att hitta autentiseringsuppgifterna, skriv lösenordet för autentiseringsuppgifterna och klicka sedan på OK.

   Om felmeddelandet&quot;Det gick inte att importera autentiseringsuppgifter på grund av felaktigt filformat eller felaktigt lösenord&quot; visas kontrollerar du att lösenordet är giltigt.

## Exportera en autentiseringsuppgift {#export-a-credential}

Autentiseringsuppgifterna exporteras som P12-filer i PKCS#12-format.

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Klicka på aliasnamnet för de autentiseringsuppgifter som du vill exportera och klicka sedan på Exportera.
1. Skriv lösenordet i rutan Lösenord. Det här lösenordet är nytt och används för att kryptera de exporterade autentiseringsuppgifterna.
1. Klicka på Exportera, följ instruktionerna så att du kan exportera inloggningsuppgifterna och klicka sedan på OK.

## Redigera en autentiseringsuppgiftens alias eller förtroendearkivtyp {#edit-a-credential-s-alias-or-trust-store-type}

När en autentiseringsuppgift har importerats kan du redigera dess aliasnamn och typ av förtroendearkiv.

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Klicka på aliasnamnet för den inloggningsinformation som du vill redigera.
1. Klicka på Uppdatera autentiseringsuppgifter.
1. Redigera aliasnamnet och förtroendearkivets typ efter behov och klicka på OK.

## Ta bort en autentiseringsuppgift {#delete-a-credential}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lokala autentiseringsuppgifter.
1. Markera kryssrutorna för de autentiseringsuppgifter som ska tas bort.
1. Klicka på Ta bort och sedan på OK.
