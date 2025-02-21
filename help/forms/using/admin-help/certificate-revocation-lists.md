---
title: Hantera listor över återkallade certifikat
description: Lär dig hantera listor över återkallade certifikat. Du kan importera, redigera och ta bort listor över återkallade certifikat (CRL:er) med pålitlighetslagerhantering.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_certificates_and_credentials
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
role: User, Developer
feature: Adaptive Forms
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Hantera listor över återkallade certifikat{#managing-certificate-revocationlists}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

Med pålitlighetslagerhantering kan du importera, redigera och ta bort listor över återkallade certifikat. Bas64 och DER-kodade listor över återkallade certifikat stöds.

## Importera en CRL {#import-a-crl}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lista över återkallade certifikat och sedan på Importera.
1. Ange en identifierare för CRL i rutan Alias.
1. Klicka på Bläddra för att leta upp listan över återkallade certifikat och klicka sedan på OK.

## Exportera en CRL {#export-a-crl}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lista över återkallade certifikat.
1. Klicka på CRL-listans aliasnamn så att du kan exportera och klicka sedan på Exportera.
1. Följ anvisningarna så att du kan exportera CRL:en. CRL-listor exporteras i Base64-kodning.
1. Klicka på OK.

## Ta bort en CRL {#delete-a-crl}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > Lista över återkallade certifikat.
1. Markera kryssrutorna för de listor över återkallade certifikat som ska tas bort, klicka på Ta bort och sedan på OK.
