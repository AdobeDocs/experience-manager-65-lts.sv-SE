---
title: Säker informationsleverans i stora volymer
description: Dokumentsäkerhet gör det möjligt att koppla licenser till användare i stället för till dokument i massproduktionsmiljöer.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 5df8c609-8007-4422-9bf8-5bae6d53b9b7
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# Säker informationsleverans i stora volymer {#high-volume-secure-information-delivery}

I en massproduktionsmiljö, t.ex. en som genererar säkra månadsfakturor för ett telekomföretag, kan det bli en resurskrävande process att skapa licenser som är specifika för varje dokument. I sådana fall kan dokumentsäkerheten användas för att koppla licenser till användare i stället för till dokument. Licensen som skapas för en användare används för alla dokument som är skyddade för den användaren.

En fördel med detta är att databasstorleken för dokumentsäkerhet inte växer linjärt med dokumenten, utan med antalet användare. Eftersom du bara behöver skapa licensen en gång för en användare blir det dessutom snabbare att skydda dokumenten med hjälp av dessa profiler. Funktioner som offlineåtkomst, förfallodatum för dokument och återkallande stöds för alla sådana dokument.

Dokumentsäkerhet har även stöd för abstrakta principer. Abstrakta principer är principmallar som innehåller alla principattribut, t.ex. dokumentskyddsinställningar och användningsrättigheter, men som inte innehåller någon lista med huvudprinciper. Administratörer kan skapa valfritt antal profiler utifrån den abstrakta profilen med olika huvudkonton som ska ha tillgång till dokumenten. Ändringar som görs i den abstrakta profilen påverkar inte de faktiska profiler som genereras från de abstrakta profilerna.

Om det finns en månadsfakturagenerering för ett telekomföretag skapar du en abstrakt policy, skapar användare och skapar sedan unika licenser för varje användare. Licenserna tillämpas senare på dokument för varje användare.

Det går bara att skapa en abstrakt profil via Java SDK för dokumentsäkerhet. Du kan dock administrera profilerna som du skapar utifrån den abstrakta profilen från dokumentsäkerhetswebbsidorna. Profiler som skapas med den här metoden har samma beteende som de som skapas från webbsidor för dokumentsäkerhet.

Mer information finns i [Programmering med AEM-formulär](https://www.adobe.com/go/learn_aemforms_programming_63).
