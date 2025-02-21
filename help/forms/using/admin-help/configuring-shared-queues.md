---
title: Konfigurera delade köer
description: Med delade köer kan du konfigurera och hantera användarköer effektivt. Lär dig konfigurera delade köer.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 0%

---

# Konfigurera delade köer{#configuring-shared-queues}

Med delade köer kan du konfigurera och hantera användarköer effektivt. En användarkö är helt enkelt alla uppgifter som tilldelats en användare. Mer information finns i [Att göra-listor](https://help.adobe.com/en_US/livecycle/11.0/WorkspaceHelp/WS92d06802c76abadb-2b6ab502126beb6ba2f-7ffc.2.html). Du kan tilldela, ta bort tilldelning och tilldela om användarköer beroende på organisationens behov. Du kan hantera delade köer på två sätt:

**Hantera åtkomst till en användare**

Du kan hantera åtkomsten till en vald användarkö med det här alternativet.

**Hantera åtkomst av en användare**

Med det här alternativet kan du hantera delade köer som tilldelats en vald användare.

## Hantera åtkomst till en vald användarkö {#managing-access-to-a-selected-user-queue}

Med funktionen Hantera åtkomst till en användare kan du hantera åtkomst till en vald användarkö. Du kan bevilja eller återkalla åtkomst till en vald användarkö för andra användare i organisationen. Kara Bowman är till exempel inte på kontoret. Med funktionen Hantera åtkomst till en användare kan Karas kö delas med Akira Tanaka och John Jacobs för slutförande. När Kara kommer tillbaka till kontoret kan du återkalla tillgången till hennes kö från Akira Tanaka och John Jacobs.

När uppgifterna delas kan de utföras av användaren, med åtkomst till kön, med Workspace.

>[!NOTE]
>
>Flex Workspace används inte i AEM formulärrelease.

### Konfigurera åtkomst till en vald användarkö {#configuring-access-to-a-selected-user-queue}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. Logga in på administrationskonsolen med ett administratörskonto.
1. Välj **Tjänster** > **Forms Workflow** > **Delad kö**.

1. På fliken Hantera åtkomst till en användare söker du efter och markerar den användare vars kö du vill dela. I det nedre högra fönstret visas en lista med användare som har åtkomst till den valda användarkön.
1. I rutan längst ned till vänster söker du efter och markerar användaren. Klicka på Dela.
1. Klicka på Spara för att slutföra.

### Återkalla åtkomst till en vald användarkö {#revoking-access-to-a-selected-user-queue}

1. Logga in på administrationskonsolen med ett administratörskonto.
1. Välj **Tjänster** > **Forms Workflow** > **Delad kö**.

1. På fliken Hantera åtkomst till en användare söker du efter och markerar den användare vars kö du vill hantera.
1. I rutan längst ned till höger visas en lista med användare som har åtkomst till den valda användarkön. Markera användaren och klicka på Återkalla.
1. Klicka på Spara för att slutföra.

## Hantera köer som tilldelats en användare {#managing-queues-assigned-to-a-user}

Med funktionen Hantera åtkomst av en användare kan du hantera köer som tilldelats en vald användare. Du kan bevilja eller återkalla åtkomst till användarköer för en vald användare individuellt. Du vill till exempel tilldela användarköerna Akira Tanaka och John Jacobs till Kara Bowman. Med funktionen Hantera åtkomst av en användare kan du söka efter Kara Bowman och ge åtkomst till uppgifter som tilldelats Akira Tanaka och John Jacobs. Vid ett senare tillfälle kan du återkalla Kara Bowmans åtkomst till dessa användarköer.

När du har tilldelats dessa uppgifter kan de utföras av användaren med Workspace.

>[!NOTE]
>
>Flex Workspace används inte i AEM formulärrelease.

### Bevilja åtkomst till en vald användarkö {#granting-access-to-a-selected-user-queue}

1. Logga in på administrationskonsolen med ett administratörskonto.
1. Välj **Tjänster** > **Forms Workflow** > **Delad kö**.

1. På fliken Hantera åtkomst till en användare söker du efter och markerar den användare vars kö du vill dela. I det nedre högra fönstret visas en lista med användare som har åtkomst till den valda användarkön.
1. I rutan längst ned till vänster söker du efter och väljer användarköer som du vill dela med den valda användaren. Klicka på Dela.
1. Klicka på Spara för att slutföra.

### Återkalla åtkomst till en vald användarkö {#revoking_access_to_a_selected_user_queue-1}

1. Logga in på administrationskonsolen med ett administratörskonto.
1. Välj **Tjänster** > **Forms Workflow** > **Delad kö**.

1. På fliken Hantera åtkomst av en användare söker du efter och markerar den användare vars kö du vill hantera.
1. I rutan längst ned till höger visas en lista med användarköer som tilldelats den valda användaren. Markera användarkön och klicka på Återkalla.
1. Klicka på Spara för att slutföra.
