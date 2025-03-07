---
title: Övervakningshändelser
description: När granskningsfunktionen är aktiverad kan du med dokumentsäkerhet övervaka vissa typer av händelser. Du kan enkelt söka efter och sortera händelselistan med dokumentsäkerhet.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: cee9cce0-becd-4822-ac37-094d564f2289
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---

# Övervakningshändelser {#monitoring-events}

När granskningsfunktionen är aktiverad kan du med dokumentsäkerhet övervaka vissa typer av händelser. Vilka händelser du kan se beror på din roll:

**Användare:** Kan visa granskade händelser för sina policyskyddade dokument och för alla skyddade dokument som de tar emot och använder.

**Principuppsättningskoordinatorer:** Kan visa granskade händelser, inklusive dokument- och principhändelser, för dokument som skyddas av principer från sina principuppsättningar.

**Administratörer:** Kan visa granskade händelser som är relaterade till alla principskyddade dokument och användare. Administratörer kan även spåra andra typer av händelser, bland annat användare, dokument, principer och systemhändelser.

>[!NOTE]
>
>Händelser som utförs på en kopia av ett policyskyddat dokument spåras också som händelser i det ursprungliga skyddade dokumentet.

(Se [Alternativ för händelsegranskning](/help/forms/using/admin-help/configuring-client-server-options.md#event-auditing-options).)

En misslyckad händelse registreras om en obehörig användare försöker visa ett dokument eller försöker logga in med ett felaktigt användarnamn eller lösenord.

>[!NOTE]
>
>Misslyckade anonyma åtkomsthändelser för dokument kan loggas om en princip redigeras för att ta bort anonym åtkomst. När en godkänd mottagare försöker få åtkomst till ett dokument som den redigerade profilen skyddar, görs ett försök att få anonym åtkomst, men detta misslyckas.

Om en princip tillåter anonym användaråtkomst men administratören senare inaktiverar anonym åtkomst för dokumentsäkerhet, kommer anonym åtkomst inte att kunna användas för dokument som skyddas med profilen och händelsen kommer inte att loggas.

## Aktivera händelsegranskning {#enable-event-auditing}

Dessa konfigurationskrav måste uppfyllas för att händelsegranskning ska kunna utföras:

* Systemet eller administratören måste aktivera serverns granskningsfunktioner.

  (Se [Konfigurera händelsegranskning och sekretessinställningar](/help/forms/using/admin-help/configuring-client-server-options.md#configuring-event-auditing-and-privacy-settings).)

* Granskning måste vara aktiverat för profilen som du använder för att skydda dokumentet. (Se [Skapa och redigera profiler](/help/forms/using/admin-help/creating-policies.md#creating-and-editing-policies).)

## Söka efter en händelse {#search-for-an-event}

Du kan söka i händelselistan och visa mer detaljerade beskrivningar om händelser. Detaljerade beskrivningar innehåller information om till exempel händelse-ID, beskrivning, IP-adress, organisation, vilken användare som påverkas, datum och tid då händelsen inträffade, nekade aktiviteter och offlinehändelser (när användare försöker använda ett dokument utan att vara anslutna till dokumentsäkerhet).

Du kan söka efter händelser på sidan Händelser genom att använda en kombination av sökvillkor för händelser och de datum som händelserna inträffade. Vilka händelser du kan söka efter beror på din roll:

**Användare:** Kan visa granskade händelser för sina policyskyddade dokument och för alla skyddade dokument som de tar emot och använder. Dessa sökalternativ är tillgängliga:

**Händelser relaterade till
Till mig:** Användare kan hitta händelser för alla principskyddade dokument som de har skapat eller tagit emot. Om en användare till exempel öppnar, visar eller skriver ut ett dokument som en annan person har skyddat, ser användaren bara dessa händelser för det dokumentet.

**Händelser som rör mina dokument:** Användare kan hitta alla händelser som är relaterade till deras egna policyskyddade dokument. Användarna ser händelser som genereras av alla som hanterar deras dokument.

**Principuppsättningskoordinatorer:** Kan visa granskade händelser, inklusive dokument- och principhändelser, för dokument som skyddas av principer från sina principuppsättningar. Dessa alternativ är tillgängliga:

**dokumenthändelser där
Jag är koordinator för principuppsättningen:** Koordinatorer för principuppsättningar som har behörighet att visa händelser kan hitta händelser som är relaterade till dokument som skyddas av principer i deras principuppsättningar.

**Principhändelser där jag är koordinator för principuppsättning:** Koordinatorer för principuppsättningar som har behörighet att visa händelser kan hitta händelser som är relaterade till principer från sina principuppsättningar.

**Administratörer:** Kan visa granskade händelser som är relaterade till alla principskyddade dokument och användare. Administratörer kan även spåra andra typer. Administratörer kan dessutom dela upp händelsesökningar ytterligare efter typ av användare:

**Kända användare:** Användare finns i källkatalogerna eller är registrerade som externa användare.

**Anonyma användare:** Okända användare som har åtkomst till ett dokument som är skyddat med en profil som tillåter anonym åtkomst.

**Systemanvändare:** Serverinitierade händelser, till exempel katalogsynkronisering.

1. Klicka på Händelser på dokumentsäkerhetssidan.
1. Markera de sökvillkor som du vill använda i söklistan. Beroende på vad du har valt i söklistan visas en andra lista med ytterligare sökvillkor. Skriv sökvillkoren i textrutan, om tillämpligt.

   Mer information om de specifika händelsetyperna finns i [Alternativ för händelsegranskning](/help/forms/using/admin-help/configuring-client-server-options.md#event-auditing-options).

1. I listan Användare väljer du den användartyp som utförde händelsen:

   * Om du väljer Känd användare visas en andra sökruta där du måste ange användarens användarnamn eller e-postadress.
   * Om du inte känner till dessa värden klickar du på sökikonen för adressboken för att söka efter användaren med antingen användarnamnet eller e-postadressen.

1. Välj ett datumintervallalternativ i datumlistan. Om du väljer Anpassade datum visas rutor där du anger datumet i formatet åååå/mm/dd, eller där du kan använda datumväljaren för att ange datumintervall:

   * Klicka på kalendern för att öppna datumväljaren.
   * Använd pilarna för att hitta år och månad.
   * Klicka på en dag i månaden i kalendern.
   * Klicka på OK för att stänga datumväljaren.

1. I visningslistan väljer du antalet sökresultat som ska visas per sida.
1. Klicka på Sök.

   Alla misslyckade händelser markeras i listan med en nekad ikon.

1. Om du vill visa information om en händelse klickar du på beskrivningen av händelsen i listan.

## Sortera händelselistan {#sort-the-event-list}

Du kan sortera händelselistan efter kolumnrubriker för att enklare hitta händelser. Triangelikoner bredvid kolumnrubriken anger vilken kolumn som används för att sortera. En uppåtriktad triangel visar stigande ordning, medan en nedåtriktad triangel anger fallande ordning.

1. Klicka på lämplig kolumnrubrik.
1. Om du vill ändra sorteringsordningen klickar du på kolumnrubriken igen.
