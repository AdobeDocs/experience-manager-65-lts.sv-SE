---
title: Känna igen giltiga och utgångna certifikat i PDF-dokument
description: Lär dig hur du känner igen giltiga och utgångna certifikat i PDF-dokument.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_acrobat_reader_dc_extensions
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: f7402f0d-7c19-4a56-8630-208faa197f94
source-git-commit: bc91f56d447d1f2c26c160f5c414fd0e6054f84c
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# Känna igen giltiga och utgångna certifikat i PDF-dokument {#recognizing-valid-and-expired-certificates-in-pdf-documents}

När ett PDF-dokument som har användningsrättigheter som används av Reader Extensions öppnas i Adobe Reader visas ett statusfält som beskriver de specifika användningsrättigheter som är aktiverade i PDF-dokumentet.

När det digitala certifikatet som anger användningsrättigheterna för ett PDF-dokument upphör att gälla och PDF-dokumentet öppnas i Adobe Reader visas en dialogruta som informerar användaren om att PDF-dokumentet har användningsbehörighet, men dessa rättigheter är inaktiverade. Även om meddelandet anger att PDF-dokumentet har ändrats eller manipulerats är detta inte nödvändigtvis fallet. Det här meddelandet visas när ett certifikat upphör att gälla eller när ett dokument ändras. I Adobe Reader 7.0.x eller senare kan du inte avgöra vilket fall som är problemet.

När du har stängt dialogrutan öppnas PDF-dokumentet. De användningsrättigheter som tillämpades med Acrobat Reader DC-tillägg är inte tillgängliga som förväntat. Om PDF-dokumentet är ett interaktivt formulär är formulärfälten låsta och användaren kan inte ändra formulärdata.
