---
title: Använda AEM arbetsflöde för översättning för att lokalisera anpassningsbara formulär och urkunder
description: Lär dig använda AEM arbetsflöden för översättning för att lokalisera adaptiva formulär och urkunder.
content-type: reference
topic-tags: develop
noindex: true
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: d53e4cb3-c632-4589-a37b-a39fd55acd88
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---

# Använda AEM arbetsflöde för översättning för att lokalisera anpassningsbara formulär och urkunder {#using-aem-translation-workflow-to-localize-adaptive-forms-and-document-of-record}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

Lokaliserade formulär hjälper er att nå en större publik över alla länder. Adobe Experience Manager arbetsflöde för översättning hjälper dig att lokalisera anpassningsbara formulär och deras urkunder. Du kan använda **maskinöversättning** eller **mänskliga översättare** för att lokalisera ett anpassat formulär.

I den här artikeln beskrivs hur du använder AEM arbetsflöde för översättning med adaptiva formulär och urkunder.

## Lokalisera ett anpassat formulär och dokument med post med maskinöversättning {#localizing-an-adaptive-form-and-document-of-record-using-machine-translation}

Maskinöversättningstjänsten översätter omedelbart ditt innehåll i adaptiv form och i ett urklippsdokument. AEM Forms är förkonfigurerat för att använda en testversion av Microsoft Translator för maskinöversättning. Utför följande steg för att aktivera maskinöversättning för anpassade formulär och registerdokument:

1. Markera ett formulär i användargränssnittet för AEM Forms och välj alternativet **Lägg till ordlista**.
1. På skärmen **Lägg till ordlista i översättningsprojekt** väljer du alternativet **Skapa ett nytt översättningsprojekt** eller **Lägg till i ett befintligt översättningsprojekt** .
1. Ange titeln i fältet **Projekttitel**. Exempel: `Government Reference Site - German locale.`
1. I fältet **Målspråk** anger du en språkinställning (till exempel `German(de)`) och klickar på **Klar**. Du kan ange flera språkinställningar. Formuläret översätts till alla språk som anges i fältet **Målspråk**.
1. Klicka på **Öppna projekt** i dialogrutan Lexikon tillagd. Öppna det nyskapade projektet på projektskärmen.
1. Klicka på **ellipserna** längst ned i rutan **Översättningssammanfattning**. Skärmen Översättningssammanfattning öppnas.
1. Klicka på ikonen **Redigera** längst upp på skärmen **Översättningssammanfattning**. Öppna fliken **Översättning** och välj Maskinöversättning på skärmen **Översättningsmetod**. Välj lämplig **översättningsprovider** och **molnkonfiguration**. Klicka på ikonen **Klar** längst upp på skärmen.
1. Klicka på ikonen ![aem62forms_down](assets/aem62forms_downarrow.png) på panelen **Översättningsjobb** och klicka sedan på **Start** . Status för rutan ändras till Utkast. När översättningen är klar ändras statusen till **Klar för granskning**. Uppdatera sidan efter några minuter och kontrollera statusen.
1. När statusen har ändrats till **Klar för granskning** i rutan **Översättningsjobb** öppnar du formuläret i ett webbläsarfönster. En lokaliserad version av formuläret visas.

   >[!NOTE]
   >
   >* Innan du öppnar den lokaliserade versionen av formuläret i webbläsarfönstret kontrollerar du att webbläsarens språkområde är inställt så att det matchar formulärets språkområde. Om formuläret t.ex. översätts till tyskt(de)-språk anger du språket för webbläsaren till tyskt(de).
   >* Anpassade formulärkomponenter har inte stöd för RTL-språk (right to left). Till exempel hebreiska.

   Tillsammans med det adaptiva formuläret är det automatiskt genererade postdokumentet också lokaliserat.

   Mer information om inställningar och konfiguration för dokument för post finns i:

[Konfiguration av dokumentmall](/help/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.md#p-document-of-record-template-configuration-p)

[Dokumentinställningar](/help/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.md#p-document-of-record-settings-p)

1. [Anpassa profileringsinformationen för postens ](/help/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.md)-dokument och se till att webbläsarens språkområde är inställt på samma språk som du har lokaliserat det anpassade formuläret till med datorspråket. Webbläsarens språkområde hjälper till att lokalisera varumärkesinformationen i urkunder.
1. Om du vill visa det lokaliserade postdokumentet väljer du Generera förhandsgranskning. Registerdokumentet PDF skapas och öppnas på en ny flik i webbläsaren.

## Lokalisera en anpassningsbar form och dess urkunder med mänsklig översättning {#localizing-an-adaptive-form-and-its-document-of-record-using-human-translation}

I Personlig översättning skickas innehållet till en översättningsleverantör och översätts av professionella översättare. När det är klart returneras det översatta innehållet och importeras till AEM. När översättningsleverantören är integrerad med AEM skickas innehåll automatiskt mellan AEM och översättningsleverantören.

För översättning delas en ordlista som innehåller filer i XLIFF-format med de professionella översättarna. Ordlistan innehåller en separat XLIFF-fil för varje språkområde. Varje XLIFF-fil innehåller text som visas för slutanvändarna och platshållarna för motsvarande lokaliserad text.

Utför följande steg för att lokalisera ett formulär och dess urkunder med personalöversättare:

1. [Anslut AEM till översättningstjänstleverantören](/help/sites-administering/tc-tic.md) och [skapa konfigurationer för översättningsintegreringsramverk](/help/sites-administering/tc-tic.md).

1. [Koppla sidorna på din språkinställning](/help/sites-administering/tc-tic.md) till översättningstjänsten och ramverkskonfigurationerna.

1. [Identifiera den typ av innehåll ](/help/sites-administering/tc-rules.md) som ska översättas.

1. [Förbered innehållet för översättning](/help/sites-administering/tc-prep.md) genom att skapa språkinställningen och skapa rotsidorna för språkkopior.

1. [Skapa översättningsprojekt](/help/sites-administering/tc-manage.md) för att samla in innehållet som ska översättas och förbereda översättningsprocessen.

1. Använd översättningsprojekten för att [hantera innehållsöversättningsprocessen](/help/sites-administering/tc-manage.md).

>[!NOTE]
>
>* Anpassade formulärkomponenter har inte stöd för RTL-språk (right to left). Till exempel hebreiska.
>
