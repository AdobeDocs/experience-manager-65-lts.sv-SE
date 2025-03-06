---
title: Strategier för säkerhetskopiering av bevakade mappar
description: I det här dokumentet beskrivs hur bevakade mappar påverkas av olika scenarier för säkerhetskopiering och återställning, begränsningar och resultat för dessa scenarier och hur du minimerar dataförluster.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/aem_forms_backup_and_recovery
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
exl-id: 5955deb0-9d1c-4b61-a202-41ef03a23cf8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 0%

---

# Strategier för säkerhetskopiering av bevakade mappar {#backup-strategies-for-watched-folders}

Det här innehållet beskriver hur bevakade mappar påverkas av olika scenarier för säkerhetskopiering och återställning, begränsningarna och resultatet av dessa scenarier samt hur du minimerar dataförluster.

*Bevakad mapp* är ett filsystemsbaserat program som anropar konfigurerade tjänståtgärder som ändrar filen i någon av följande mappar i den bevakade mapphierarkin:

* Indata
* Scen
* Utdata
* Fel
* Bevara

En användare eller ett klientprogram släpper först filen eller mappen i indatamappen. Tjänståtgärden flyttar sedan filen till scenmappen för bearbetning. När tjänsten har utfört den angivna åtgärden sparas den ändrade filen i utdatamappen. Källfiler som bearbetats har flyttats till mappen preserve och filer som inte kunde bearbetas har flyttats till felmappen. När attributet `Preserve On Failure` för den bevakade mappen är aktiverat flyttas misslyckade bearbetade källfiler till mappen för bevarande. (Se [Konfigurera bevakade mappslutpunkter](/help/forms/using/admin-help/configuring-watched-folder-endpoints.md#configuring-watched-folder-endpoints).)

Du kan säkerhetskopiera bevakade mappar i filsystemet.

>[!NOTE]
>
>Säkerhetskopieringen är oberoende av säkerhetskopierings- och återställningsprocessen för databasen eller dokumentlagringen.

## Hur bevakade mappar fungerar {#how-watched-folders-work}

Det här innehållet beskriver den bevakade mappfilsbearbetningen. Det är viktigt att förstå denna process innan man utarbetar en återhämtningsplan. I det här exemplet är attributet `Preserve On Failure` för den bevakade mappen aktiverat. Filerna bearbetas i den ordning som de kommer fram.

I följande tabell beskrivs filhanteringen för fem exempelfiler (fil1, fil2, fil3, fil4, fil5) under hela processen. I tabellen representerar x-axeln tid, till exempel Tid 1 eller T1, och y-axeln representerar mappar i den bevakade mapphierarkin, till exempel Indata.

<table>
 <thead>
  <tr>
   <th><p>Mapp</p></th>
   <th><p>T1</p></th>
   <th><p>T2</p></th>
   <th><p>T3</p></th>
   <th><p>T4</p></th>
   <th><p>T5</p></th>
   <th><p>T6</p></th>
   <th><p>T7</p></th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td><p>Indata</p></td>
   <td><p>file1, file2, file3, file4</p></td>
   <td><p>file2, file3, file4</p></td>
   <td><p>file3, file4</p></td>
   <td><p>file4</p></td>
   <td><p>tom</p></td>
   <td><p>file5</p></td>
   <td><p>tom</p></td>
  </tr>
  <tr>
   <td><p>Scen</p></td>
   <td><p>tom</p></td>
   <td><p>fil1</p></td>
   <td><p>fil2</p></td>
   <td><p>file3</p></td>
   <td><p>file4</p></td>
   <td><p>tom</p></td>
   <td><p>file5</p></td>
  </tr>
  <tr>
   <td><p>Utdata</p></td>
   <td><p>tom</p></td>
   <td><p>tom</p></td>
   <td><p>fil1_ut</p></td>
   <td><p>fil1_ut, fil2_ut</p></td>
   <td><p>fil1_ut, fil2_ut</p></td>
   <td><p>file1_out, file2_out, file4_out</p></td>
   <td><p>file1_out, file2_out, file4_out</p></td>
  </tr>
  <tr>
   <td><p>Fel</p></td>
   <td><p>tom</p></td>
   <td><p>tom</p></td>
   <td><p>tom</p></td>
   <td><p>tom</p></td>
   <td><p>file3_failed, file3 </p></td>
   <td><p>file3_failed, file3 </p></td>
   <td><p>file3_failed, file3 </p></td>
  </tr>
  <tr>
   <td><p>Bevara</p></td>
   <td><p>tom</p></td>
   <td><p>tom</p></td>
   <td><p>fil1 </p></td>
   <td><p>fil1, fil2 </p></td>
   <td><p>fil1, fil2 </p></td>
   <td><p>file1, file2, file4 </p></td>
   <td><p>file1, file2, file4 </p></td>
  </tr>
 </tbody>
</table>

I följande text beskrivs filhanteringen för varje gång:

**T1:** De fyra exempelfilerna placeras i indatamappen.

**T2:** Tjänståtgärden flyttar fil1 till scenmappen för manipulering.

**T3:** Tjänståtgärden flyttar fil2 till scenmappen för manipulering. Resultatet av fil1 placeras i utdatamappen och fil1 flyttas till mappen preserve.

**T4:** Tjänståtgärden placerar fil3 i scenmappen för manipulering. Resultatet av fil2 placeras i utdatamappen och fil2 placeras i den reserverade mappen.

**T5:** Tjänståtgärden placerar fil4 i scenmappen för manipulering. Hanteringen av filen3 misslyckas och den placeras i felmappen av tjänståtgärden.

**T6:** Tjänsten placerar fil5 i indatamappen. Resultatet av filen 4 placeras i utdatamappen och filen 4 placeras i mappen preserve.

**T7:** Tjänståtgärden placerar fil5 i scenmappen för manipulering.

## Säkerhetskopiera bevakade mappar {#backing-up-watched-folders}

Vi rekommenderar att du säkerhetskopierar hela det bevakade mappfilsystemet till ett annat filsystem.

## Återställer bevakade mappar {#restoring-watched-folders}

I det här avsnittet beskrivs hur du återställer bevakade mappar. Bevakade mappar anropar ofta kortvariga processer som slutförs på en minut. I sådana fall förhindrar inte en återställning av den bevakade mappen med en säkerhetskopiering som görs varje timme att data går förlorade.

Om t.ex. en säkerhetskopia tas vid T1-tidpunkten och servern misslyckas vid T7, ändras redan filen 1, filen 2, filen 3 och filen 4. Att återställa den bevakade mappen med en säkerhetskopia som tagits på T1 förhindrar inte dataförlust.

Om en senare säkerhetskopiering har gjorts kan du återställa filerna. När du återställer filerna bör du tänka på vilken mapp i mapphierarkin den aktuella filen finns i:

**Scen:** Filer i den här mappen bearbetas igen när den bevakade mappen har återställts.

**Indata:** Filer i den här mappen bearbetas igen när den bevakade mappen har återställts.

**Resultat:** Filer i den här mappen bearbetas inte.

**Utdata:** Filer i den här mappen bearbetas inte.

**Bevara:** Filer i den här mappen bearbetas inte.

## Strategier för att minimera dataförluster {#strategies-to-minimize-data-loss}

Följande strategier kan minimera dataförlust för utdata och indatamappar när en bevakad mapp återställs:

* Säkerhetskopiera utdata och felmappar ofta, t.ex. en timme, för att undvika förlust av resultat och felfiler.
* Säkerhetskopiera indatafilerna i en annan mapp än den bevakade mappen. Detta garanterar att filen är tillgänglig efter återställning om du inte kan hitta filerna i utdata- eller felmappen. Kontrollera att filnamnsschemat är konsekvent.

  Om du till exempel sparar utdata med `%F.`*filnamnstillägget* får utdatafilen samma namn som indatafilen. Detta hjälper dig att avgöra vilka indatafiler som ska ändras och vilka som ska skickas igen. Om du bara ser filen file1_out i resultatmappen och inte filen2_out, file3_out och file4_out, måste du skicka filen 2, file3 och file4 igen.

* Om den bevakade säkerhetskopian av mappen som är tillgänglig är äldre än den tid det tar att bearbeta jobbet, bör du låta systemet skapa en bevakad mapp och automatiskt placera filerna i indatamappen.
* Om den senaste tillgängliga säkerhetskopian inte är tillräckligt aktuell är säkerhetskopieringstiden kortare än den tid det tar att bearbeta filerna och den bevakade mappen återställs, har filen manipulerats i någon av följande steg:

   * **Steg 1:** I indatamappen
   * **Steg 2:** har kopierats till scenmappen men processen har inte anropats än
   * **Steg 3:** har kopierats till scenmappen och processen anropas
   * **Steg 4:** Redigering pågår
   * **Steg 5:** Resultat returnerades

  Om filerna finns på scen 1 ändras de. Om filerna finns i scen 2 eller 3 placerar du dem i indatamappen så att ändringarna utförs igen.

  >[!NOTE]
  >
  >Om en fil manipuleras mer än en gång förhindras dataförlust, men resultatet kan dupliceras.

## Slutsats {#conclusion}

På grund av den bevakade mappens dynamiska och ständigt föränderliga natur bör bevakade mappar återställas med filer som säkerhetskopieras inom en dag. Det bästa är att säkerhetskopiera resultaten, lagra indatamappen på en server och spåra indatafiler så att du kan skicka jobbet igen om fel uppstår.
