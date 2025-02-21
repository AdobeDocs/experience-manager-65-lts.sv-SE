---
title: Hantera HSM-autentiseringsuppgifter
description: Lär dig hur du hanterar HSM-autentiseringsuppgifter. Du kan hantera HSM från sidan Hantering av betrodda lagringsplatser. Du kan visa, kontrollera, uppdatera, återställa och ta bort HSM-komponenter.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/managing_certificates_and_credentials
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms,Document Security
role: User, Developer
source-git-commit: eded255b54ff83f60f73cece8824c778d3a87680
workflow-type: tm+mt
source-wordcount: '1334'
ht-degree: 0%

---

# Hantera HSM-autentiseringsuppgifter {#managing-hsm-credentials}

På sidan Hantering av betrodda lagringsplatser kan du hantera HSM-autentiseringsuppgifter (Hardware Security Module). En HSM är en PKCS#11-enhet från en annan leverantör som du kan använda för att på ett säkert sätt generera och lagra privata nycklar. HSM skyddar åtkomst till och användning av privata nycklar fysiskt.

Klientprogramvaran krävs för att kommunicera med HSM. HSM-klientprogramvaran måste installeras och konfigureras på samma dator som AEM-formulär.

AEM formulär Digitala signaturer kan använda inloggningsuppgifter som lagras på en HSM för att använda digitala signaturer på serversidan. Följ instruktionerna i det här avsnittet för att skapa ett alias för varje HSM-autentiseringsuppgift som digitala signaturer ska använda. Aliaset innehåller alla parametrar som krävs för HSM.

>[!NOTE]
>
>När du har ändrat HSM-konfigurationen startar du om AEM Forms Server.

## Skapa ett alias för en HSM-referens när HSM-enheten är online {#create-an-alias-for-an-hsm-credential-when-the-hsm-device-is-online}

>[!NOTE]
> 
> Kontrollera att användaren har administratörsbehörighet för att komma åt administratörskonsolen.

1. I administrationskonsolen klickar du på Inställningar > Pålitlig lagringshantering > HSM-autentiseringsuppgifter och sedan på Lägg till.
1. I rutan Profilnamn skriver du en sträng som används för att identifiera aliaset. Det här värdet används som en egenskap för vissa åtgärder för digitala signaturer, till exempel åtgärden Signera signaturfält.
1. I rutan PKCS11-bibliotek skriver du den fullständiga sökvägen till HSM-klientbiblioteket på servern. Exempel: `c:\Program Files\LunaSA\cryptoki.dll`. I en klustrad miljö måste sökvägen vara identisk för alla servrar i klustret.
1. Klicka på Testa HSM-anslutning. Om AEM-formulär kan ansluta till HSM-enheten visas ett meddelande om att HSM är tillgängligt. Klicka på Nästa.
1. Använd antingen tokennamnet, kortplats-ID eller indexet för kortplatslista för att identifiera var inloggningsuppgifterna lagras i HSM.

   * **Tokennamn:** Motsvarar namnet på den HSM-partition som ska användas (till exempel HSMPART1).
   * **Kortplats-ID:** Kortplats-ID:t är en kortplats-ID av datatypen long.
   * **Index för platslista:** Om du väljer indexvärde för platslista anger du ett heltal som motsvarar platsens information. Detta är ett nollbaserat index, vilket innebär att om klienten registreras med HSMPART1-partitionen först, kommer HSMPART1 att refereras till med SlotListIndex-värdet 0.

1. I rutan Token Pin skriver du det lösenord som krävs för att få åtkomst till HSM-nyckeln och klickar på Nästa.
1. Välj en autentiseringsuppgift i rutan Autentiseringsuppgifter. Klicka på Spara.

## Skapa ett alias för en HSM-referens när HSM-enheten är offline {#create-an-alias-for-an-hsm-credential-when-the-hsm-device-is-offline}

1. I administrationskonsolen klickar du på Inställningar > Pålitlig lagringshantering > HSM-autentiseringsuppgifter och sedan på Lägg till.
1. I rutan Profilnamn skriver du en sträng som används för att identifiera aliaset. Det här värdet används som en egenskap för vissa åtgärder för digitala signaturer, till exempel åtgärden Signera signaturfält.
1. I rutan PKCS11-bibliotek skriver du den fullständiga sökvägen till HSM-klientbiblioteket på servern. Exempel: `c:\Program Files\LunaSA\cryptoki.dll`. I en klustrad miljö måste sökvägen vara identisk för alla servrar i klustret.
1. Markera kryssrutan Skapa offlineprofil. Klicka på Nästa.
1. I listan HSM-enhet väljer du tillverkaren av den HSM-enhet där inloggningsuppgifterna lagras.
1. I listan Facktyp väljer du Fack-ID, Fackindex eller Tokennamn och anger ett värde i rutan Fackinformation. AEM-formulär använder de här inställningarna för att avgöra var inloggningsuppgifterna lagras på HSM.

   * **Tokennamn:** Motsvarar ett partitionsnamn (till exempel HSMPART1).
   * **Kortplats-ID:** Kortplats-ID är ett heltal som motsvarar kortplatsen, vilket i sin tur motsvarar en partition. Klienten (Forms Server) har till exempel registrerats med HSMPART1-partitionen först. Detta mappar plats 1 till HSMPART1-partitionen för den här klienten. Eftersom HSMPART1 är den första registrerade partitionen är plats-ID 1 och du ställer in fackinformation till 1.

     Kortplats-ID anges klient för klient. Om du registrerade en andra dator till en annan partition (till exempel HSMPART2 på samma HSM-enhet) kopplas fack 1 till HSMPART2-partitionen för den klienten.

   * **Fackindex:** Om du väljer Fackindex anger du ett heltal som motsvarar kortplatsen som Fackinformation. Detta är ett nollbaserat index, vilket innebär att om klienten registreras med HSMPART1-partitionen först mappas fack 1 till HSMPART1 för den här klienten. Eftersom HSMPART1 är den första registrerade partitionen är platsindexet 0.

1. Välj något av följande alternativ och ange sökvägen:

   * **Certifikat**: (Krävs inte om SHA1 används) Klicka på Bläddra och leta reda på sökvägen till den offentliga nyckeln för de autentiseringsuppgifter som du använder.
   * **Certifikat SHA1:** (Krävs inte om ett fysiskt certifikat används) Ange SHA1-värde (tumavtryck) för filen med den offentliga nyckeln (.cer) för de autentiseringsuppgifter som du använder. Kontrollera att inga blanksteg används i SHA1-värdet.

1. I rutan Lösenord anger du det lösenord som krävs för att få åtkomst till HSM-nyckeln för den angivna platsinformationen och klickar sedan på Spara.

## Visa egenskaper för HSM-autentiseringsalias {#view-hsm-credential-alias-properties}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > HSM-autentiseringsuppgifter.
1. Klicka på aliasnamnet för autentiseringsalias för att visa egenskaperna och klicka sedan på OK.

## Kontrollera status för en HSM-autentiseringsuppgift {#check-the-status-of-an-hsm-credential}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > HSM-autentiseringsuppgifter.
1. Klicka i kryssrutan bredvid de autentiseringsuppgifter som du vill kontrollera och klicka sedan på Kontrollera status.

Statuskolumnen återspeglar den aktuella statusen för autentiseringsuppgiften. Om ett fel uppstår visas ett rött X i statuskolumnen. Håll muspekaren över X för att visa ett verktygstips som innehåller orsaken till felet.

## Uppdatera egenskaper för HSM-autentiseringsalias {#update-hsm-credential-alias-properties}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > HSM-autentiseringsuppgifter.
1. Klicka på aliasnamnet för autentiseringsalias.
1. Klicka på Uppdatera autentiseringsuppgifter och uppdatera inställningarna efter behov.

## Återställ alla HSM-anslutningar {#reset-all-hsm-connections}

Återställ de öppna anslutningarna till en HSM-enhet efter eventuella avbrott i nätverkssessionen mellan Forms Server och HSM-enheten. Det kan till exempel uppstå avbrott på grund av ett nätverksavbrott eller att HSM-enheten kopplas från för en programuppdatering. Efter ett avbrott är de befintliga anslutningarna inaktuella och eventuella signeringsbegäranden mot dessa anslutningar misslyckas. Om du använder alternativet Återställ alla HSM-anslutningar tas de gamla anslutningarna bort.

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > HSM-autentiseringsuppgifter.
1. Klicka på Återställ alla HSM-anslutningar.

## Ta bort ett HSM-autentiseringsalias {#delete-an-hsm-credential-alias}

1. I administrationskonsolen klickar du på Inställningar > Lita på arkivhantering > HSM-autentiseringsuppgifter.
1. Markera kryssrutorna för de HSM-autentiseringsuppgifter som du vill ta bort, klicka på Ta bort och sedan på OK.

## Konfigurera fjärr-HSM-stöd {#configure-remote-hsm-support}

AEM-formulär använder en webbtjänstbaserad IPC/RPC-mekanism. På så sätt kan AEM-formulär använda en HSM som är installerad på en fjärrdator. Om du vill använda den här funktionen installerar du webbtjänsten på den fjärrdator där HSM är installerat. Mer information finns i [Konfigurera HSM-stöd för AEM-formulär ES med Sun JDK på 64-bitarsplattformen för Windows](https://kb2.adobe.com/cps/808/cpsid_80835.html).

Den här mekanismen stöder inte onlineskapande av HSM-profiler eller statuskontroller. Det finns dock två sätt att skapa HSM-profiler och utföra statuskontroller:

* Skapa en AEM-formulärklientautentiseringsuppgift genom att skicka den till signerarens certifikat. Följ stegen i [Konfigurera HSM-stöd för AEM-formulär ES med Sun JDK på 64-bitarsplattformen för Windows](https://kb2.adobe.com/cps/808/cpsid_80835.html). Webbtjänstplatsen skickas som en autentiseringsuppgiftsegenskap. Det finns även stöd för HSM-profiler som skapats offline med antingen certifikatutfärdare eller SHA-1-hex. Om du har uppgraderat till AEM-formulär från en tidigare version av AEM-formulär gör du klientändringar eftersom inloggningsuppgifterna medförde certifikat och webbtjänstinformation.
* Webbtjänstens plats anges i administrationskonsolen för signeringstjänsten. (Se [Inställningar för signaturtjänsten](/help/forms/using/admin-help/configure-service-settings.md#signature-service-settings).) Här medförde klienten bara aliaset för HSM-profilen i förtroendearkivet. Du kan använda det här alternativet utan att några klientändringar behöver göras, även om du har uppgraderat till AEM-formulär från en tidigare version av AEM-formulär. Det här alternativet stöder inte HSM-profiler som använder certifikat SHA-1.
