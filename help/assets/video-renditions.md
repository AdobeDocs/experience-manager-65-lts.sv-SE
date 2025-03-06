---
title: Videoåtergivningar
description: Lär dig hur du använder Adobe Experience Manager Assets för att generera videoåtergivningar för videoresurser i olika format, som OGG, FLV och så vidare.
contentOwner: rbrough
products: SG_EXPERIENCEMANAGER/6.5/ASSETS
solution: Experience Manager, Experience Manager Assets
feature: Video
role: User
exl-id: da33f43b-7375-46f1-a80f-c1891fd90312
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 1%

---

# Videoåtergivningar {#video-renditions}

Adobe Experience Manager Assets genererar videoåtergivningar för videoresurser i olika format, bland annat OGG, FLV.

Experience Manager Assets har stöd för statiska och dynamiska återgivningar (DM-kodade återgivningar) för medieresurser.

Statiska återgivningar genereras internt med hjälp av FFMPEG (som är installerat och tillgängligt på systemsökvägen) och lagras i innehållsdatabasen.

DM-kodade återgivningar lagras på proxyservern och hanteras vid körning.

Experience Manager Assets har uppspelningsstöd för dessa återgivningar på klientsidan.

Om du vill visa återgivningarna för en viss videoresurs öppnar du resurssidan och väljer ikonen Global navigering. Välj sedan **[!UICONTROL Renditions]** i listan.

![chlimage_1-478](assets/chlimage_1-478.png)

Listan med videoåtergivningar visas på panelen **[!UICONTROL Renditions]**.

![chlimage_1-479](assets/chlimage_1-479.png)

[Konfigurera Dynamic Media Cloud-tjänster](config-dynamic.md) om du vill konfigurera proxyservern för DM-kodade återgivningar.

Om du vill generera videoåtergivningar med önskade parametrar [skapar du en motsvarande videoprofil](video-profiles.md).

När du har konfigurerat proxyservern och skapat videoprofiler kan du ta med den här videoförinställningen i en bearbetningsprofil och använda bearbetningsprofilen i en mapp.

>[!NOTE]
>
>Ljuduppspelning fungerar inte för OGG- och WAV-filer i Microsoft® Internet Explorer 11. Ett fel `Invalid Source` visas på sidan med tillgångsinformation för resurser med tillägget OGG eller WAV.
>
>I MS® Edge och iPad spelas inte OGG-filer upp och ett formatfel som inte stöds uppstår.
