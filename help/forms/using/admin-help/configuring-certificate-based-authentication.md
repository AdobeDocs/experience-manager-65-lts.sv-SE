---
title: Konfigurera certifikatbaserad autentisering
description: Importera ett certifikatutfärdarcertifikat (CA) till förtroendearkivet och skapa en certifikatsmappning för certifikatbaserad autentisering.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
hide: true
hidefromtoc: true
exl-id: 56603735-959e-4460-b642-bba63fa20c02
source-git-commit: 103250f3442cf7c2793c51a95b1bf4fbaff71463
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Konfigurera certifikatbaserad autentisering {#configuring-certificate-based-authentication}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Användarhantering utför vanligtvis autentisering med ett användarnamn och lösenord. Användarhantering har även stöd för certifikatbaserad autentisering, som du kan använda för att autentisera användare via Acrobat eller för att autentisera användare programmatiskt. Mer information om programmatisk autentisering av användare finns i [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63).

Om du vill använda certifikatbaserad autentisering importerar du ett certifikatutfärdarcertifikat (CA) som du litar på till förtroendearkivet och skapar sedan en certifikatsmappning.

## Importera certifikatutfärdarcertifikatet {#import-the-ca-certificate}

När du importerar certifikatet väljer du alternativen Lita på för certifikatautentisering och Lita på för identitet samt alla andra alternativ som du behöver. Mer information om hur du importerar certifikat finns i [Hantera certifikat](/help/forms/using/admin-help/certificates.md#managing-certificates).

## Konfigurerar certifikatmappning {#configuring-certificate-mapping}

Om du vill aktivera certifikatbaserad autentisering för användare skapar du en certifikatmappning. En *certifikatsmappning* definierar en karta mellan ett certifikats attribut och attributen för användare i en domän. Du kan mappa mer än ett certifikat till samma domän.

När du testar ett certifikat överför Hantering av användare certifikaten för att kontrollera att det uppfyller följande krav:

* Certifikatet är giltigt.
* Den utfärdare du angav kan verifiera certifikatet.
* Certifikatet innehåller det attribut som krävs för mappning.
* Den angivna mappningen mappar certifikatet till endast en användare i AEM formulärdatabas. Både aktuella och inaktuella (borttagna) användare kontrolleras för att avgöra om de matchar mappningsvillkoren. Därför misslyckas certifieringstestet om mer än en användare, inklusive inaktuella användare, har attributvärdet beaktat.

>[!NOTE]
>
>Du kan inte redigera en befintlig certifikatsmappning.

**Lägg till en certifikatmappning**

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Certifikatmappning.
1. Klicka på Ny certifikatsmappning och välj det certifikatalias som konfigurerats i Pålitlighetslagerhantering i listan För utfärdare.
1. Koppla ett av certifikatets attribut till en användares attribut. Du kan till exempel mappa certifikatets vanliga namn till användarens inloggnings-ID.

   Om innehållet i attributet i certifikatet skiljer sig från innehållet i användarens attribut i databasen för användarhantering, kan du använda ett reguljärt Java-uttryck (regex) för att matcha de två attributen. Om de vanliga namnen på certifikaten till exempel är namn som *Alex Pink (Authentication)* och *Alex Pink (Signing)* och det vanliga namnet i användarhanteringsdatabasen är *Alex Pink* använder du en regex för att extrahera den nödvändiga delen av certifikatattributet (i det här exemplet *Alex Pink*). Det reguljära uttryck du anger måste följa Java regex-specifikationen.

   Du kan omforma uttrycket genom att ange gruppernas ordning i rutan Egen ordning. Den anpassade ordningen används med metoden `java.util.regex.Matcher.replaceAll()`. Beteendet som visas motsvarar metodens beteende, och indatasträngen (den anpassade ordningen) måste anges därefter.

   Om du vill testa regex anger du ett värde i rutan Testparameter och klickar på Testa.

   Du kan använda följande tecken i regex:

   * . (valfritt tecken)
   * &ast; (0 eller fler förekomster)
   * () (ange gruppen inom parentes)
   * \ (används för att kringgå ett regex-tecken till ett vanligt tecken)
   * $n (används för att referera till den n:te gruppen)

   Exempel på reguljära uttryck:

   * Extrahera &quot;Alex Pink&quot; från &quot;Alex Pink (Authentication)&quot;

     **Regex:** (.&ast;) \(Autentisering\)

   * Extrahera &quot;Alex Pink&quot; från &quot;Alex (Authentication) Pink&quot;

     **Regex:** (.&ast;)\(Autentisering\) (.&ast;)

   * Extrahera &quot;Rosa Alex&quot; från &quot;Alex (Authentication) Pink&quot;

     **Regex:** (.&ast;)\(Autentisering\) (.&ast;)

     Anpassad ordning: $2 $1 (returnera den andra gruppen, sammanfogad till den första gruppen, fångad med blankstegstecken)

   * Extrahera &quot;apink@sampleorg.com&quot; från &quot;smtp:apink@sampleorg.com&quot;

     **Regex:** smtp:(.&ast;)

   Mer information om hur du använder reguljära uttryck finns i [Java-självstudiekursen om reguljära uttryck](https://java.sun.com/docs/books/tutorial/essential/regex/).

1. Välj användarens domän i listan För domän.
1. Om du vill testa den här konfigurationen klickar du på Bläddra för att överföra ett exempelanvändarcertifikat, klickar på Testa certifikat och, om konfigurationen är korrekt, klickar du på OK.

**Redigera en befintlig certifikatsmappning**

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration.
1. Klicka på Certifikatmappning.
1. Markera certifikatmappningen för att redigera och redigera dess konfiguration. Du kan uppdatera det reguljära uttrycket och den anpassade ordningen.
1. Om du vill testa ändringarna klickar du på Bläddra för att överföra ett exempelcertifikat, klickar på Testa certifikat och sedan på OK.

**Ta bort en certifikatsmappning**

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Certifikatmappning.
1. Markera kryssrutan för den certifikatkoppling som ska tas bort, klicka på Ta bort och klicka sedan på OK.
