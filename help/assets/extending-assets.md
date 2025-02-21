---
title: Anpassa och utöka  [!DNL Assets]
description: Lär dig hur du kan anpassa och utöka Resursresurs- och Resursredigeraren, som ger användarna ett särskilt skräddarsytt gränssnitt och en uppsättning funktioner.
contentOwner: AG
role: Developer
feature: Developer Tools
solution: Experience Manager, Experience Manager Assets
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# Anpassa och utöka [!DNL Assets] {#customizing-and-extending-assets}

Resursredigeraren är den primära åtkomstpunkten som användare på en Adobe Enterprise Manager-webbplats använder för att hitta, visa och ändra digitala resurser i din databas.

Som [!DNL Experience Manager]-utvecklare kan du anpassa och utöka resursredigeraren på flera olika sätt, så att användarna får ett särskilt anpassat gränssnitt och en uppsättning funktioner.

Följande funktionalitetsaspekter kan anpassas eller förbättras:

* [Utöka resursredigeraren](asseteditorx.md)
* [Utöka Assets Search](searchx.md)
* [Bearbeta Assets med mediehanterare och arbetsflöden](media-handlers.md)
* [Integrera Assets med aktivitetsströmmen](extending-activity-stream.md)
* [Assets proxyutveckling](proxy.md)
* [Bästa tillvägagångssätt för att konfigurera ImageMagick](best-practices-for-imagemagick.md)

## Anpassa utseendet {#customizing-the-look-and-feel}

Följande aspekter av utseendet och känslan i resursredigeraren kan anpassas:

* Logo: Du kan lägga till din egen organisations logotyp i gränssnittet.
* Färger och teckensnitt: Du kan ändra de färger och teckensnitt som används i gränssnittet.
* HTML Code: För mer ingående anpassning kan du ändra den underliggande HTML-koden som definierar gränssnitten.

## Anpassa återgivningar {#customizing-renditions}

I [!DNL Experience Manager Assets]-terminologi är en återgivning det format i vilket en resurs presenteras. I allmänhet kan en viss resurs ha flera renderingar. Fullfärgsbilder kan till exempel ha en återgivning i sin ursprungliga storlek, en annan i en nedskalad storlek och en annan som både är nedskalad och konverterad till gråskala.

De återgivningar som en viss resurs är tillgänglig i kan anpassas och nya återgivningar skapas.
