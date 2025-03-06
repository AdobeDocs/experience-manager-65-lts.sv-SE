---
title: Importera och exportera konfigurationsfilen
description: Lär dig hur du importerar och exporterar konfigurationsfilen för att redigera serverinställningar eller konfigurera en annan instans av AEM-formulärsprodukten.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_user_management
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 92fdcee3-2007-4bbc-be4b-426d65b8dbc1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Importera och exportera konfigurationsfilen {#importing-and-exporting-the-configuration-file}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Använd sidan Manuell konfiguration för att hämta en kopia av konfigurationsinställningarna i XML-format. Inställningarna i den här filen styr alla serverinställningar. Du kan sedan redigera filen och överföra den tillbaka till servern. Du kan också använda filen för att konfigurera en annan instans av AEM-formulärsprodukten.

För att undvika säkerhetsrisker inkluderas inte det binda lösenordsvärdet för katalogservern i en exporterad konfigurationsfil. Uppdatera lösenordet i XML-filen innan du importerar filen till ett nytt system.

>[!NOTE]
>
>Om du importerar konfigurationsfilen konfigureras AEM-formulär om baserat på informationen i filen. Det är bara systemadministratörer eller konsulter som känner till AEM blankettprodukt och XML som bör överväga att ändra konfigurationsfilen. De kan behöva redigera konfigurationsfilen, t.ex. för att konfigurera om en skadad inställning.

**Exportera konfigurationsinformationen**

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler.
1. Klicka på Exportera. Om du använder Microsoft Internet Explorer uppmanas du att ange var filen ska sparas. Om du använder Firefox sparas filen på skrivbordet.

**Importera konfigurationsinformationen**

1. I administrationskonsolen klickar du på Inställningar > Användarhantering > Konfiguration > Importera och exportera konfigurationsfiler.
1. Klicka på Bläddra för att hitta konfigurationsfilen, klicka på Importera och sedan på OK.
