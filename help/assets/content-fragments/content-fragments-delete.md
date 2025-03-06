---
title: Innehållsfragment - Ta bort överväganden
description: Granska dessa viktiga aspekter innan du definierar din princip för borttagning av innehållsfragment i AEM. Content Fragments är ett kraftfullt verktyg för att leverera headless-innehåll, och konsekvenserna av att ta bort dem måste noggrant övervägas.
feature: Content Fragments
role: User
solution: Experience Manager, Experience Manager Assets
exl-id: 1460872b-415f-4392-a480-c442790fd0d9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 7%

---

# Innehållsfragment - Ta bort överväganden {#content-fragments-delete-considerations}

Granska dessa viktiga aspekter innan du definierar din princip för borttagning av innehållsfragment i AEM. Content Fragments är ett kraftfullt verktyg för att leverera headless-innehåll, och konsekvenserna av att ta bort dem måste noggrant övervägas.

## Behörigheter - ta bort eller inte ta bort {#permissions-delete-or-not-delete}

Möjligheten att ta bort innehåll är kraftfull, men potentiellt känslig, och många branscher måste begränsa och styra hur dessa behörigheter distribueras.

När det gäller borttagningsbehörigheter måste innehållsfragment beaktas på två nivåer:

1. **Innehållsfragmentet som en enskild entitet.**

   * **Använd skiftläge**: En användare som behöver redigera/uppdatera ett innehållsfragment - **och ta bort ett helt fragment**.
   * **Behörigheter**: [Ta bort](/help/sites-administering/security.md#actions)-behörigheten kan [tilldelas via användar- och/eller grupphantering](/help/sites-administering/security.md#managing-permissions).

2. **De flera underentiteter som utgör ett innehållsfragment, till exempel variationer, undernoder.**

   Den grundläggande åtgärden i innehållsfragmentredigeraren kräver att sådana tillfälliga underelement kan tas bort. Till exempel när du ändrar variationer, även när du redigerar metadata eller hanterar associerat innehåll.

   * **Använd skiftläge**: En användare som behöver redigera/uppdatera ett innehållsfragment - **utan att kunna ta bort ett helt fragment**.
   * **Behörigheter**: Se [Behörigheter krävs endast för redigeringsfunktioner](#permissions-required-for-editor-functionality-only).

>[!NOTE]
>
>När en användare inte har några [Delete](/help/sites-administering/security.md#actions)-behörigheter fungerar redigeraren för innehållsfragment i *skrivskyddat* läge.

>[!NOTE]
>
>Se även [Så här granskar du användarhanteringsåtgärder i AEM](/help/sites-administering/audit-user-management-operations.md).

## Behörigheter krävs endast för redigeringsfunktionen {#permissions-required-for-editor-functionality-only}

Användare som behöver redigera/uppdatera ett innehållsfragment, **utan att kunna ta bort ett helt fragment**, måste tilldelas specifika behörigheter, eftersom grundläggande användning av redigeraren för innehållsfragment kräver att tillfälliga underelement kan tas bort.

Till exempel när du ändrar variationer, även när du redigerar metadata eller hanterar associerat innehåll.

>[!NOTE]
>
>De borttagningsbehörigheter som krävs för att redigera/uppdatera ett innehållsfragment ingår i borttagningsbehörigheten [som tilldelats via användar- och/eller grupphantering](/help/sites-administering/security.md#managing-permissions).

Behörigheterna som behövs för att redigera/uppdatera ett fragment måste tillämpas på antingen noden som innehåller innehållsfragmentet eller en lämplig överordnad nod (på alla nivåer under `/content/dam`). När den tilldelas till en sådan överordnad nod tillämpas behörigheterna på alla noder i den grenen.

En mapp som till exempel kommer att innehålla alla innehållsfragment, till exempel:

* `/content/dam/contentfragments`

>[!CAUTION]
>
>Det går också att ange behörigheter för `/content/dam` eftersom alla innehållsfragment lagras här.
>
>Den här åtgärden tillämpar dock samma borttagningsbehörigheter för *alla* andra resurstyper också.

Behörigheten som krävs för att en viss användare och/eller grupp ska kunna redigera/uppdatera ett innehållsfragment är:

>[!NOTE]
>
>I den här listan visas alla behörigheter som krävs, inte bara borttagningsbehörighet.

* För noderna eller mapparna för innehållsfragment:

   * `jcr:addChildNodes`, `jcr:modifyProperties`

* För noden `jcr:content`för alla innehållsfragment:

   * `jcr:addChildNodes`, `jcr:modifyProperties` och `jcr:removeChildNodes`

* För alla noder under `jcr:content` i alla innehållsfragment:

   * `jcr:addChildNodes`, `jcr:modifyProperties` och `jcr:removeChildNodes`, `jcr:removeNode`

Dessa `remove`-behörigheter måste [administreras med åtkomstkontrollistor i CRXDE Lite](/help/sites-administering/user-group-ac-admin.md#access-right-management).

Behörigheterna `add` och `modify` kan också administreras i CRXDE Lite eller med användarhanteringskonsolen.

Definitionen av behörigheten `remove` för en grupp `content-authors-no-delete`:

![cf-delete-03](assets/cf-delete-03.png)
