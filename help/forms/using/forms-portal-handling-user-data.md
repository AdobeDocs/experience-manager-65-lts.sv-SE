---
title: Forms Portal | Hantera användardata
description: Lär dig hur du hanterar användardata som åtkomst, borttagning och datalager på AEM Forms Portal.
contentOwner: vishgupt
topic-tags: grdp
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: f9eb6e61-5669-4cb7-a528-f1c09e455a60
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# Forms Portal | Hantera användardata {#forms-portal-handling-user-data}

[!DNL AEM Forms]-portalen innehåller komponenter som du kan använda för att lista adaptiva formulär, HTML5-formulär och andra Forms-resurser på sidan [!DNL AEM Sites]. Dessutom kan du konfigurera den så att den visar utkast och skickade adaptiva formulär och HTML5-formulär för en inloggad användare. Mer information om Forms Portal finns i [Introduktion till att publicera formulär på en portal](/help/forms/using/introduction-publishing-forms.md).

När en inloggad användare sparar ett anpassat formulär som ett utkast eller skickar det, visas de på flikarna Utkast och Inskickat på Forms Portal. Data för ifyllda eller inskickade formulär lagras i datalagret som är konfigurerat för AEM-distribution. Utkast och inskickade data från anonyma användare visas inte på Forms Portal-sidan, men data lagras i det konfigurerade datalagret. Se [Konfigurera lagringstjänster för utkast och överföringar](/help/forms/using/configuring-draft-submission-storage.md).

## Användardata och datalager {#user-data-and-data-stores}

Forms Portal lagrar data för utkast och skickade formulär i följande scenarier:

* Den sändningsåtgärd som har konfigurerats i det adaptiva formuläret är **Forms Portal Submit Action**.
* För andra skicka-åtgärder än **Forms Portal Submit Action** aktiveras alternativet **[!UICONTROL Store data in Forms Portal]** i **[!UICONTROL Submission]** -egenskaperna för den adaptiva formulärbehållaren.

För varje utkast och skickat formulär för inloggade och anonyma användare lagras följande data i Forms Portal:

* Formulärmetadata som formulärnamn, formulärsökväg, utkast- eller överförings-ID, sökväg till bilagor och användar-ID
* Bifogade formulär som databyte
* Formulärdata som databyte

Beroende på den konfigurerade datalagringens beständighet lagras utkast och skickade formulärdata på följande platser.

<table>
 <tbody>
  <tr>
   <td><p><strong>Typ av beständighet</strong></p> </td>
   <td><p><strong>Datalager</strong></p> </td>
   <td><p><strong>Plats</strong></p> </td>
  </tr>
  <tr>
   <td><p>Standard</p> </td>
   <td><p>AEM-databas med instanser för författare och publicering</p> </td>
   <td><p><code>/content/forms/fp/</code></p> </td>
  </tr>
  <tr>
   <td><p>Fjärr</p> </td>
   <td><p>AEM-databas med författare och AEM fjärrinstanser</p> </td>
   <td><p><code>/content/forms/fp/</code></p> </td>
  </tr>
  <tr>
   <td><p>Databas</p> </td>
   <td><p>AEM databas med Author-instans och databastabeller</p> </td>
   <td>Databastabeller <code>data</code>, <code>metadata</code> och <code>additionalmetadata</code></td>
  </tr>
 </tbody>
</table>

## Få åtkomst till och ta bort användardata {#access-and-delete-user-data}

Du kan komma åt formulärdata för inloggade och anonyma användare i konfigurerade datalager och vid behov ta bort dem.

### AEM-instanser {#aem-instances}

Alla utkast och skickade formulärdata i AEM-instanser (författare, publicering eller fjärranvändare) för inloggade och anonyma användare lagras i noden `/content/forms/fp/` i den tillämpliga AEM-databasen. Varje gång en inloggad eller anonym användare sparar ett utkast eller skickar ett formulär genereras ett `draft ID`- eller `submission ID`-, `user data ID`- och ett slumpmässigt `ID`-värde för varje bifogad fil (om tillämpligt). Det är kopplat till respektive utkast eller inlämning.

#### Åtkomst till användardata {#access-user-data}

När en inloggad användare sparar ett utkast eller skickar ett formulär skapas en underordnad nod med användar-ID:t. Utkast och inskickade data för Sarah Rose vars användar-ID är `srose` lagras till exempel i noden `/content/forms/fp/srose/` i AEM-databasen. I användar-ID-noden ordnas data i en hierarkisk struktur.

I följande tabell förklaras hur data för alla utkast av `srose` lagras i AEM-databasen.

>[!NOTE]
>
>En exakt struktur som `drafts` replikeras för skickade formulär för `srose` under noden `/content/forms/fp/srose/submit/`.
>
>Alla utkast och inskickat material från `anonymous`-användare lagras under noden `/content/forms/fp/anonymous/`, som organiserar utkast och inskickat material för alla anonyma användare under noderna `draft` och `submit`.

| Nod | Beskrivning |
|---|---|
| `/content/forms/fp/srose/drafts` | Behållarnoddata för alla utkast av användaren |
| `/content/forms/fp/srose/drafts/attachments/` | Ordnar alla bilagor för användaren baserat på utkast-ID |
| `/content/forms/fp/srose/drafts/attachments/<ID>` | Innehåller en bifogad fil för det valda ID:t i binärt format |
| `/content/forms/fp/srose/drafts/metadata/` | Ordnar formulärmetadata för användaren baserat på utkast-ID |
| `/content/forms/fp/srose/drafts/metadata/<draft ID>` | Innehåller formulärmetadata för det valda utkast-ID:t |
| `/content/forms/fp/srose/drafts/data/` | Organiserar formulärdata för användaren baserat på användarens data-ID |
| `/content/forms/fp/srose/drafts/data/<user data ID>` | Innehåller formulärdata för det valda användar-ID:t i binärt format |

#### Ta bort användardata {#delete-user-data}

Om du vill ta bort användardata från utkast och inskickade data för en inloggad användare från AEM-system helt måste du ta bort noden `user ID` för en viss användare från författarnoden. Ta bort data manuellt från alla tillämpliga AEM-instanser.

Utkast och överföringsdata för alla anonyma användare lagras i de gemensamma `drafts`- och `submit`-noderna under `/content/forms/fp/anonymous`. Det finns ingen metod för att hitta data för en viss anonym användare om inte viss identifierbar information är känd. I det här fallet kan du söka efter information som identifierar den anonyma användaren i AEM-databasen och manuellt ta bort noden som innehåller den från alla tillämpliga AEM-instanser för att ta bort data från AEM-systemet. Om du vill ta bort data för alla anonyma användare kan du ta bort noden `anonymous` för att ta bort utkast och skicka data för alla anonyma användare.

### Databas {#database}

När AEM har konfigurerats för att lagra data i en databas lagras Forms Portal-utkast och överföringsdata i följande databastabeller för både inloggade och anonyma användare:

* data
* metadata
* extra metadata

#### Åtkomst till användardata {#access-user-data-1}

Kör följande databaskommando om du vill komma åt utkast och skicka data för en inloggad och anonym användare i databastabellerna. Ersätt `logged-in user` med det användar-ID vars data du vill komma åt eller med `anonymous` för anonyma användare i frågan.

```sql
select * from metadata, data, additionalmetadatatable where metadata.owner = 'logged-in user' and metadata.id = additionalmetadatatable.id and metadata.userdataID = data.id
```

#### Ta bort användardata {#delete-user-data-1}

Om du vill ta bort utkast och skicka data för en inloggad användare från databastabellerna kör du följande databaskommando. Ersätt `logged-in user` med det användar-ID vars data du vill ta bort eller med `anonymous` för anonyma användare i frågan. Om du vill ta bort data för en viss anonym användare från databasen måste du hitta dem med identifierbar information och ta bort dem från databastabeller som innehåller informationen.

```sql
DELETE FROM metadata, data, additionalmetadatatable USING metadata INNER JOIN data ON metadata.userdataID = data.id INNER JOIN additionalmetadatatable ON metadata.id = additionalmetadatatable.id WHERE metadata.owner = 'logged-in user'
```
