---
title: Grunderna för hantering av certifikat och autentiseringsuppgifter
description: Lär dig mer om grunderna för hantering av certifikat och autentiseringsuppgifter.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_certificates_and_credentials
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 8aeacdb7-68a7-476f-a725-f9ad7406cc9c
source-git-commit: 02b9eb98d1fdf1b090166a6ae7c0a4379487d2e1
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Grunderna för hantering av certifikat och autentiseringsuppgifter {#basics-of-managing-certificates-and-credentials}

En *autentiseringsuppgift* innehåller din privata nyckelinformation som behövs för att signera eller identifiera dokument. Ett *certifikat* är information om offentlig nyckel som du konfigurerar för förtroende. AEM-formulär använder certifikat och autentiseringsuppgifter för flera syften:

* I Acrobat Reader DC-tillägg används en autentiseringsuppgift för att aktivera användarrättigheter för Adobe Reader i PDF-dokument. (Se [Konfigurera autentiseringsuppgifter för användning med Acrobat Reader DC-tillägg](/help/forms/using/admin-help/configuring-credentials-acrobat-reader-dc.md#configuring-credentials-for-use-with-acrobat-reader-dc-extensions).)
* Du kan konfigurera Rights Management så att autentiseringsuppgifter endast visas för användning i Acrobat från betrodda utfärdare. (Se [Konfigurera visningsinställningar för Rights Management](/help/forms/using/admin-help/configuring-client-server-options.md#configure-document-security-display-settings).) Gemensamt namn (CN) måste finnas i certifikatet.
* Signaturtjänsten får åtkomst till certifikat och autentiseringsuppgifter. Mer information om signaturtjänsten finns i [Tjänstreferens](https://www.adobe.com/go/learn_aemforms_services_65).

**Skapar en parnyckel**

AEM-formulär använder sin Trust Store för att lagra och hantera certifikat, autentiseringsuppgifter och listor över återkallade certifikat. Dessutom kan du använda en oberoende HSM-enhet (Hardware Security Module) för att lagra privata nycklar.

AEM-formulär har inget alternativ för att generera nyckelpar. Du kan dock generera den med verktyg som Java-nyckelverktyg och importera den i AEM forms Trust Store. Mer information om Java-nyckelverktyget finns i:

[https://docs.oracle.com/javase/tutorial/security/toolsign/step3.html](https://docs.oracle.com/javase/tutorial/security/toolsign/step3.html)

[https://docs.oracle.com/cd/E19798-01/821-1841/gjrgy/index.html](https://docs.oracle.com/cd/E19798-01/821-1841/gjrgy/index.html)

Följande signaturtyper stöds och kan importeras i AEM-formulär:

* XML-signatur
* XMLTimeStampToken
* RFC 3161 TimeStampToken
* PKCS#7
* PKCS#1
* DSA-signaturer

**Hantera förlorad eller komprometterad nyckel**

Om du misstänker att nyckeln har gått förlorad eller har komprometterats ska du göra följande:

1. Informera certifikatutfärdaren så att de lägger till den kompromissade nyckeln i listan över återkallade certifikat för att återkalla nyckeln.
1. Hämta en ny nyckel och dess certifikat från den attesterande myndigheten.
1. Signera dokument som signerats med den kompromissade nyckeln igen med den nya nyckeln.
