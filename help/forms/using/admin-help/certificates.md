---
title: Hantera certifikat
description: Lär dig hur du importerar och exporterar ett certifikat och redigerar dess pålitlighetsinställningar.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_certificates_and_credentials
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 0%

---

# Hantera certifikat {#managing-certificates}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Med pålitlighetslagerhanteringen kan du importera, redigera och ta bort certifikat som du litar på på servern för validering av digitala signaturer och certifikatautentisering. Du kan importera och exportera valfritt antal certifikat. När ett certifikat har importerats kan du redigera pålitlighetsinställningarna och förtroendearkivets typ. Tänk på följande alternativ när du kombinerar typer av förtroendearkiv:

* **Lita på certifikatautentisering med certifikatutfärdare:** Välj även Lita på identitet för CRL-validering.
* **Lita på certifikatautentisering med ICA:** Välj endast Lita på identitet. En ICA ska inte vara betrodd för certifikatautentisering. Om du litar på ICA för certifikatautentisering blir ICA en certifikatutfärdare för sökvägsuppbyggnad. Om ICA är betrott för både certifikatautentisering och identitet ignoreras certifikatutfärdarens leverantörscertifikat eftersom ICA blir certifikatutfärdare.
* **Förtroende för OCSP Server med HTTP:** Om OSCP-svarsservern finns på en HTTP-plats måste du även välja Lita på SSL-anslutningar. Om OSCP-svaranden kräver CRL-validering måste du även välja Lita på identitet.
* **Adobe-rot:** Välj inte SSL-anslutningar eller OCSP Server Trust Store-typer. Adobe Root är inte betrodd för SSL-anslutningar och OCSP Server. Adobe utfärdar inte OCSP- eller SSL-certifikat. Adobe Root är implicit tillförlitligt med alias name=&quot;ADOBEROT&quot;.

Endast X509v3-certifikat stöds. Den här certifikattypen kan anges i en binär DER-kodad fil (.cer-fil) eller en textfil som innehåller en Base64-kodad version av samma DER-kodade certifikat (inklusive X509-certifikat i PEM-format (Privacy Enhanced Mail)).

Certifikat som krävs för att slutföra en signaturverifiering måste finnas i samma arkiv (HSM eller databas).

Du kan också importera och ta bort certifikat med Trust Manager API. Mer information finns i&quot;Importera certifikat med Trust Manager API&quot; och&quot;Ta bort certifikat med Trust Manager API&quot; i [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63).

## Importera ett certifikat {#import-a-certificate}

1. Klicka på **[!UICONTROL Settings > Trust Store Management > Certificates]** i administrationskonsolen.
1. Klicka på Importera och välj något av följande alternativ under Pålitlig lagringstyp:

   * **Lita på SSL-anslutningar:** Anger att AEM-formulär kan använda certifikat för att ansluta till externa system via SSL.
   * **Förtroende för certifikatsignatur:** Anger att certifikat är pålitliga i dokumentsigneringsåtgärder för certifikatförfattardigitala signaturer.
   * **Förtroende för signatur:** Anger att certifikat är betrodda i dokumentsigneringsåtgärder för digitala signaturer som inte är författare.
   * **Förtroende för certifikatautentisering:** Anger att AEM-formulär använder certifikat för autentisering av användare med certifikat eller smartkortsautentisering.
   * **Förtroende för OCSP-server:** Anger att AEM-formulär kan använda certifikat för att ansluta till externa OCSP-svarare
   * **Lita på identitet:** Anger att certifikat kan användas för att lita på annan information än den som anges ovan.

   >[!NOTE]
   >
   >Förtroendearkivet litar implicit på ett Adobe-rotcertifikat för certifikatautentisering, signatur, certifikatsignatur och identitet.

1. Ange certifikatets identifierare i rutan Alias.
1. Klicka på **[!UICONTROL Browse]** för att hitta certifikatet och klicka sedan på **[!UICONTROL OK]**.

## Exportera ett certifikat {#export-a-certificate}

1. Klicka på **[!UICONTROL Settings > Trust Store Management > Certificates]** i administrationskonsolen.
1. Klicka på aliasnamnet för det certifikat som ska exporteras. Sidan **[!UICONTROL Certificate Details]** visas.
1. Klicka på **[!UICONTROL Export]**, följ instruktionerna för att exportera certifikatet och klicka sedan på **[!UICONTROL OK]**.

## Redigera pålitlighetsinställningar och förtroendearkivets typ {#edit-a-certificate-s-trust-settings-and-trust-store-type}

1. Klicka på **[!UICONTROL Settings > Trust Store Management > Certificates]** i administrationskonsolen.
1. Klicka på aliasnamnet för det certifikat som ska redigeras.
1. Klicka på **[!UICONTROL Update Certificate]**.
1. Om du vill ändra certifikatets aliasnamn skriver du ett nytt namn i rutan Alias.
1. Välj lämplig typ av förtroendearkiv om du vill uppdatera certifikatets typ av förtroendearkiv.
1. Om du vill uppdatera principbegränsningarna skriver du principinformationen i rutan Certifikatprofiler och klickar sedan på **[!UICONTROL OK]**.

## Ta bort ett certifikat {#delete-a-certificate}

1. Klicka på **[!UICONTROL Settings > Trust Store Management > Certificates]** i administrationskonsolen.
1. Markera kryssrutorna för de certifikat som ska tas bort, klicka på **[!UICONTROL Delete]** och sedan på **[!UICONTROL OK]**.
