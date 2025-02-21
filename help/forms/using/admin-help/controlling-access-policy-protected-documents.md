---
title: Styra åtkomsten till profilskyddade dokument
description: Se hur du kan visa, hantera och styra åtkomsten till dina profilskyddade dokument.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/working_with_document_security
products: SG_EXPERIENCEMANAGER/6.5/FORMS
feature: Document Security
solution: Experience Manager, Experience Manager Forms
role: User, Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '2167'
ht-degree: 0%

---

# Styra åtkomsten till profilskyddade dokument {#controlling-access-to-policy-protected-documents}

Du kan styra hur mottagarna använder dina profilskyddade dokument oavsett hur ofta du distribuerar dem.

Du kan göra följande med hjälp av dokumentsidan:

* Sök efter och visa information om policyskyddade dokument. Du kan visa information om dokumentnamn, utgivarnamn, principnamn och datum då profilen tillämpades. Om profilen som skyddade ett dokument tas bort kan du även se det borttagna princip-ID:t under principnamnet. Användarna kan visa och hantera sina egna policyskyddade dokument. Administratörer kan visa och hantera alla policyskyddade dokument.
* Ändra information om profilen som används i ett dokument. Användare kan redigera egna profiler, administratörer kan redigera delade och personliga profiler och koordinatorer för principuppsättningar kan redigera delade principer i de principuppsättningar de har behörighet för. Du kan komma åt profilen som är kopplad till ett dokument direkt från dokumentinformationssidan.
* Återkalla och återskapa åtkomst till ett profilskyddat dokument. Administratörer kan återkalla och återskapa åtkomsten till alla dokument. Koordinatorer för principuppsättningar (som har behörighet att hantera dokument) kan återkalla och återskapa åtkomst till principskyddade dokument som använder delade profiler från sina principuppsättningar. Användarna kan återkalla åtkomsten till sina profilskyddade dokument om de har skapat profilen som skyddar dokumentet eller om profilen är en delad profil som tillåter den funktionen.
* Byt den profil som ska tillämpas på ett dokument. Användare som tillämpar profiler på dokument kan ändra en profil om de har skapat den eller om det är en delad profil som aktiverar den funktionen. Koordinatorer för principuppsättningar kan växla principer från sina principuppsättningar. Administratörer kan växla principer som tillämpas på alla dokument.

När ett dokument skyddas av en profil och du återkallar behörigheter eller växlar den tillämpade profilen, gäller ändringarna enligt följande:

* Om dokumentet är online tillämpas ändringarna omedelbart, såvida inte användaren har dokumentet öppet. I så fall måste användaren stänga dokumentet för att ändringarna ska börja gälla.
* Om en mottagare använder dokumentet offline (till exempel på en bärbar dator) träder ändringarna i kraft nästa gång mottagaren synkroniserar med dokumentsäkerheten genom att öppna ett policyskyddat dokument.

## Visa information om ett dokument {#view-information-about-a-document}

För varje dokument som visas på sidan Dokument kan du se dokumentnamnet, utgivarens namn, principnamnet och datumet då dokumentet var skyddat. Om principen som skyddade ett dokument har tagits bort visas princip-ID:t under Principnamn.

Du kan även visa mer information, som beskrivs nedan, om ett visst dokument på sidan Dokumentinformation:

>[!NOTE]
>
>Använd länken Principnamn på sidan Dokumentinformation för att komma åt profiler som genereras automatiskt i Microsoft Outlook för mottagare av ett dokument som bifogas till ett e-postmeddelande. Dessa profiler visas inte på profilsidan.

**Dokumentnamn:** Det markerade dokumentets namn.

**Dokument-ID:** En unik identifierare som dokumentsäkerhet tilldelar när en profil tillämpas på dokumentet. dokumentskyddet använder det här numret för att spåra dokumentet.

**Dokumentstatus:** Dokumentets status (t.ex. aktiv eller återkallad).

**Utgivare:** Namnet på den användare som bifogade profilen till dokumentet.

**Principnamn:** Namnet på den princip som används för att skydda dokumentet. Du kan klicka på namnet för att öppna profilen. Använd den här länken för att komma åt profiler som Acrobat genererar för mottagare av ett dokument som är kopplat till ett e-postmeddelande i Outlook. Dessa profiler visas inte på sidan Profiler.

**Principtyp:** Typen av princip som tillämpades på dokumentet.

**Publiceringsdatum:** Det datum då profilen tillämpades på dokumentet.

**Relaterade iterationer:** Om dokumentet har relaterade iterationer visas även det här objektet i listan. Klicka på länken om du vill visa en lista med relaterade iterationer för dokumentet.

Användare kan visa information om sina skyddade dokument. Administratörer kan visa information om dokument som alla användare har skyddat med en profil. Koordinatorer för principuppsättningar kan visa information om dokument som skyddas av profiler från sina principuppsättningar.

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Klicka på rätt dokument i listan över dokument. Sidan Dokumentdetalj öppnas och detaljerad information om dokumentet visas. Den här sidan innehåller även alternativ för att återkalla dokumentåtkomst, byta profil och visa händelser som är relaterade till det här dokumentet.

## Visa relaterade iterationer för ett dokument {#view-related-iterations-for-a-document}

Om spårning av relaterade iterationer är aktiverat kan du spåra versioner av ett dokument som olika användare har sparat. Den här funktionen stöds endast av vissa program, som PTC Pro/ENGINEER Wildfire.

Den här funktionen är användbar när flera användare samarbetar och sparar olika versioner av samma dokument. dokumentskydd kan hålla reda på de olika versionerna och därför kan du enkelt visa dokumentinformation för de olika versionerna.

Om den här funktionen är aktiverad kan du visa relaterade versioner av ett dokument från dokumentsidan.

1. Visa dokumentinformationssidan för ett dokument. (Se [Visa information om ett dokument](controlling-access-policy-protected-documents.md#view-information-about-a-document).)
1. Klicka på Visa relaterade iterationer. Alternativet är bara tillgängligt om funktionen är aktiverad. Listan med relaterade iterationer visas. För varje upprepning kan du visa följande information:

   * **Iteration:** Filnamnet. Den kan skilja sig från det ursprungliga filnamnet och har ett versionsnummer tillagt till slutet av det.
   * **Utgivare:** Utgivaren av originaldokumentet.
   * **Skapad av:** Den användare som sparade upprepningen.
   * **Skapad:** Datum och tid då iterationen sparades.
   * **Princip:** Den princip som skyddar upprepningen. Olika iterationer kan skyddas av olika profiler.

1. Om du vill visa dokumentinformationssidan för den iterationen klickar du på filnamnet för en iteration.

## Återkalla och återupprätta åtkomst till dokument {#revoking-and-reinstating-access-to-documents}

Du kan återkalla och återställa åtkomsten till profilskyddade dokument:

**Användare:** Kan återkalla eller återskapa åtkomst till dokument som de skyddar med sina egna personliga principer eller med delade principer för vilka funktionen återkallad är aktiverad för den användare som tillämpar principen. Användare som inte kan återkalla åtkomst till ett dokument eller växla en profil måste kontakta administratören.

**Administratörer:** Kan återkalla eller återskapa åtkomsträttigheter till alla principskyddade dokument, inklusive de som skyddas av personliga eller delade principer. Om en administratör återkallar åtkomsten till ett dokument som är skyddat med en delad princip, kan bara en administratör återskapa åtkomstbehörighet för det dokumentet.

**Principuppsättningskoordinatorer:** Kan återkalla eller återskapa åtkomstbehörigheter för dokument som skyddas av principer från sina principuppsättningar.

När du återkallar eller återställer behörigheter för dokumentåtkomst gäller ändringen vid följande tidpunkter:

* Om dokumentet är online och stängt träder ändringen i kraft nästa gång mottagaren synkroniserar med dokumentsäkerheten genom att öppna ett profilskyddat dokument.
* Om dokumentet är online och öppet börjar ändringen gälla när mottagaren stänger dokumentet.
* Om dokumentet är offline, d.v.s. används utan internetanslutning, t.ex. på en bärbar dator, träder ändringen i kraft nästa gång mottagaren synkroniserar med dokumentsäkerheten.

**Återkalla åtkomst till ett principskyddat dokument**

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Markera kryssrutan bredvid rätt dokument och klicka på Återkalla. Du kan återkalla åtkomsten till flera dokument samtidigt.
1. Välj ett meddelande som ska visas för användare som försöker öppna dokumentet efter att det har återkallats:

   * **Allmänt meddelande:** Anger att författaren har återkallat dokumentet
   * **Dokumentet har avslutats:** Anger att författaren har avslutat dokumentet
   * **Dokumentet har ändrats**: Anger att författaren har ändrat dokumentet

1. (Valfritt) Om det finns en nyare version av dokumentet anger du URL:en och klickar på Testa för att verifiera URL:en.
1. Klicka på OK och sedan på OK igen för att återgå till dokumentsidan.

**Återskapa dokumentbehörigheter**

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Klicka på rätt dokument i listan över dokument.
1. Klicka på Ångra och sedan på OK.

## Växla en profil som tillämpas på ett dokument {#switch-a-policy-that-is-applied-to-a-document}

Användare, principuppsättningskoordinatorer och administratörer kan växla den profil som används för ett principskyddat dokument (du kan bara tillämpa en princip åt gången för ett dokument). Användare kan växla principer som tillämpas på egna policyskyddade dokument om de har skapat profilen eller om profilen är en delad profil som har den funktionen aktiverad. I annat fall måste administratören eller koordinatorn för principuppsättningen ändra principen. Administratörer kan växla principer för alla användares profilskyddade dokument. Koordinatorer för principuppsättningar kan växla principer från sina principuppsättningar.

När du byter ut en profil tillämpas den nya principen på följande sätt:

* Om dokumentet är online och stängt träder ändringen i kraft nästa gång mottagaren synkroniserar med dokumentsäkerheten genom att öppna ett policyskyddat dokument online.
* Om dokumentet är online och öppet börjar ändringen gälla när användaren stänger dokumentet.
* Om dokumentet är offline (används utan en aktiv Internet- eller nätverksanslutning, t.ex. på en bärbar dator) tillämpas ändringen nästa gång användaren synkroniserar med dokumentsäkerheten genom att öppna ett policyskyddat dokument online.

>[!NOTE]
>
>Om du vill tillåta anonym åtkomst till ett policyskyddat dokument som för närvarande inte har denna åtkomst tar du bort den befintliga principen i klientprogrammet och tillämpar sedan en policy som tillåter anonym åtkomst. Om du byter profil måste användarna fortfarande logga in för att få åtkomst till dokumentet.

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Klicka på rätt dokument i listan över dokument.
1. Klicka på Byt profil. En lista med upp till 100 profiler visas.
1. Om profilen du vill använda inte visas väljer du Principnamn eller princip-ID i söklistan, skriver namnet eller ID och klickar på Sök.
1. Klicka på en ny profil i listan.
1. Klicka på Byt profil och sedan på OK för att gå tillbaka till sidan Dokument.

## Söka efter ett dokument {#search-for-a-document}

Du kan söka efter dokument på sidan Dokument genom att använda en kombination av datumintervallvillkor och sökvillkor som är tillgängliga i listan. Dessa villkor omfattar dokumentnamn, principnamn eller alla dokument.

Ytterligare sökalternativ är bara tillgängliga för administratörer:

**Dokument-ID:** Unikt ID-nummer som dokumentsäkerhet tilldelar dokumentet när profilen tillämpas.

**Dokumentnamn:** Dokumentets namn.

**Utgivarnamn:** Namnet på den användare som bifogade profilen till dokumentet. Du kan välja användare från alla domäner eller en angiven domän.

**Princip-ID:** ID-nummer för principen som är kopplad till dokumentet.

**Principnamn:** Namnet på principen som är kopplad till dokumentet.

**Alla dokument:** Alla dokument som skyddas av administratörer och användare. Om du använder alternativet Alla dokument för att söka kan en lång lista med dokument returneras.

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Välj önskat sökvillkor i söklistan.

   Du kan ange villkor som dokument-ID, dokumentnamn, utgivarnamn, princip-ID, principnamn eller alla dokument.

   Om du anger utgivarens namn klickar du på adressboksikonen och anger den domän där du vill söka efter användaren. Klicka sedan på OK för att återgå till söksidan Dokument.

1. (Valfritt) Välj ett datumintervallalternativ i datumlistan. Om du väljer Anpassade datum anger du datumet i formatet åååå/mm/dd i de rutor som visas eller använder datumväljaren för att ange datumintervall:

   * Klicka på kalendern för att öppna datumväljaren.
   * Använd pilarna för att hitta år och månad.
   * Klicka på en dag i månaden i kalendern.
   * Klicka på OK för att stänga datumväljaren.

1. Klicka på Sök.

## Sortera dokumentlistan {#sort-the-document-list}

Du kan sortera listan med dokument efter kolumnrubrik. Triangelikoner bredvid kolumnrubriken anger vilken kolumn som används för att sortera. En uppåtriktad triangel visar stigande ordning, medan en nedåtriktad triangel anger fallande ordning.

1. Klicka på Dokument på dokumentsäkerhetssidan.
1. Klicka på lämplig kolumnrubrik.
1. Om du vill ändra sorteringsordningen klickar du på kolumnen igen.

## Lägg till försättsblad i profilskyddade dokument {#add-cover-page-to-policy-protected-documents}

Om det finns de flesta visningsprogram som inte kommer från Adobe PDF visas den första sidan som en tom sida om du öppnar ett dokument som är skyddat av dokumentskydd. Om du inte öppnar dokumentet avbryts programmet.

Du kan använda stödet för sidan 0 (Brytningsdokument) för att tillåta användare som inte använder Adobe PDF att öppna ett skyddat dokument och visa en försättssida i dokumentet.

>[!NOTE]
>
>När du visar sådana dokument (som innehåller en sida 0) i Adobe Reader/Acrobat eller Mobile Reader öppnas det skyddade dokumentet som standard.

**Lägga till försättsblad i ett policyskyddat dokument**

Använd följande processer i workbench:

**Skydda
Dokument med försättsblad:** Skyddar ett PDF-dokument med den angivna profilen och lägger till en försättssida i dokumentet

**Extrahera skyddat dokument:** Extraherar det policyskyddade PDF-dokumentet från PDF-dokumentet med försättsblad

Använd följande API:er för dokumentsäkerhet:

**protectDocumentWithCoverPage:** Skyddar ett givet PDF med den angivna profilen och returnerar ett dokument med en försättssida och det skyddade dokumentet som en bifogad fil
`//Create a ServiceClientFactory instance ServiceClientFactory factory = ServiceClientFactory.createInstance(connectionProps); //Create a RightsManagementClient object RightsManagementClient rightsClient = new RightsManagementClient(factory); //Reference a PDF document to which a policy is applied FileInputStream fileInputStream = new FileInputStream("C:\\testFile.pdf"); Document inPDF = new Document(fileInputStream); //Reference a Cover Page document FileInputStream coverPageInputStream = new FileInputStream("C:\\CoverPage.pdf"); Document inCoverDoc = new Document(coverPageInputStream); //Create a Document Manager object DocumentManager documentManager = rightsClient.getDocumentManager(); //Apply a policy to the PDF document RMSecureDocumentResult rmSecureDocument = documentManager.protectDocumentWithCoverPage( inPDF, "ProtectedPDF.pdf", "PolicySetName", "PolicyName", null, null, inCoverDoc, true); //Retrieve the policy-protected PDF document Document protectPDF = rmSecureDocument.getProtectedDoc(); //Save the policy-protected PDF document File myFile = new File("C:\\PolicyProtectedDoc.pdf"); protectPDF.copyToFile(myFile);` **extractProtectedDocument:** Extraherar det skyddade dokumentet, som är en bifogad fil i dokumentet med försättsblad. Dokumentet med omslagssidan kan skapas med metoden protectDocumentWithCoverPage
`//Create a ServiceClientFactory instance ServiceClientFactory factory = ServiceClientFactory.createInstance(connectionProps); //Create a RightsManagementClient object RightsManagementClient rightsClient = new RightsManagementClient(factory); //Reference a protected PDF document with a Cover Page FileInputStream fileInputStream = new FileInputStream("C:\\policyProtectedDocWithCoverPage.pdf"); Document inPDF = new Document(fileInputStream); //Create a Document Manager object DocumentManager documentManager = rightsClient.getDocumentManager(); //Apply a policy to the PDF document Document extractedDoc = documentManager.extractProtectedDocument(inPDF); //Save the policy-protected PDF document File myFile = new File("C:\\PolicyProtectedDoc.pdf"); extractedDoc.copyToFile(myFile);`
