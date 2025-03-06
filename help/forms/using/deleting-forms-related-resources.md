---
title: Ta bort formulär och relaterade resurser
description: Så här tar du bort ett formulär eller en resurs i AEM Forms och hur det påverkar refererade och refererade resurser och XFA-formulär.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-manager
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
exl-id: e9b7e1b1-23a3-4da9-a5b4-c30f70441341
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Ta bort formulär och relaterade resurser {#deleting-forms-and-related-resources}

Du kan ta bort formulären och resurserna för att ta bort dessa resurser från databasen. Borttagningsåtgärden fungerar för alla resurstyper och mappar.

Om du tar bort en resurs från författarinstansen tas även resursen bort från publiceringsinstansen. AEM Forms-servern består av författarinstanser och publiceringsinstanser. Instansen Författare används för att skapa och hantera formulärresurser och -resurser. Publiceringsinstansen innehåller publicerade formulärresurser och relaterade resurser som är tillgängliga för slutanvändare.

## Ta bort ett formulär {#how-to-delete-a-form}

1. Logga in på AEM Forms användargränssnitt genom att gå till `https://[hostname]:'port'/aem/forms.html.`
1. Navigera till och markera det formulär som du vill ta bort. Klicka på Ta bort ![aem6forms_delete2](assets/aem6forms_delete2.png) i verktygsfältet och bekräfta borttagningsåtgärden.

   >[!NOTE]
   >
   >Det går endast att ta bort ett formulär åt gången. Ta bort flera formulär individuellt eller ta bort den överordnade mappen.

1. Innan du tar bort en resurs söker AEM Forms efter referenser och begär en explicit bekräftelse. Klicka på Tvinga borttagning om du vill ta bort resursen oavsett relationsstatus.

   >[!NOTE]
   >
   >Om du tar bort en resurs som refereras av andra resurser kan det orsaka funktionella problem.

   >[!NOTE]
   >
   >Om den markerade resursen är en mapp och den innehåller en sådan resurs i sin hierarki kan du ta bort andra resurser antingen separat eller ta bort hela mappen.

## Effekten av att ta bort ett refererat XFA-formulär {#impact-of-deleting-a-referenced-xfa-form}

I AEM Forms kan en XFA-formulärmall refereras av ett adaptivt formulär eller en annan XFA-formulärmall. En mall kan även referera till en resurs eller en annan XFA-mall.

Det är inte tillrådligt att ta bort ett XFA-formulär som refereras av ett adaptivt formulär, eftersom det kan skada det adaptiva formuläret. När ett adaptivt formulär refererar till ett XFA-formulär är fälten bundna. När XFA har tagits bort kan det adaptiva formuläret inte synkronisera sina fält med XFA-fälten och ett felmeddelande visas för sådana fält. Mer information om effekten av den refererade XFA-borttagningen och om felaktiga AF:er finns i [Uppdatera refererade XFA-formulär](/help/forms/using/get-xdp-pdf-documents-aem.md#p-updating-referenced-xfa-forms-p).

Om du vill ta bort ett sådant XFA-formulär uppdaterar du det adaptiva formuläret och tar bort bindningarna med XFA-fälten.
