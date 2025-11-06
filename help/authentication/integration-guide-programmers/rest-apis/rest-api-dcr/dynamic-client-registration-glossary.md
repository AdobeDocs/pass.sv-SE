---
title: DCR-ordlista (Dynamic Client Registration)
description: DCR-ordlista (Dynamic Client Registration)
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# DCR-ordlista (Dynamic Client Registration) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Det här dokumentet innehåller definitioner för termer som används vid integrering av Adobe Pass Authentication Dynamic Client Registration (DCR).

>[!MORELIKETHIS]
> 
> * [REST API v2-ordlista](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Ordlista {#glossary-terms}

### A {#a}

#### Åtkomsttoken {#access-token}

Åtkomsttoken är en token som genereras av Adobe Pass Authentication som ett resultat av processen [Dynamic Client Registration (DCR)](#dcr) som är avsedd att säkerställa åtkomst till skyddade API:er.

### C {#c}

#### Klientautentiseringsuppgifter {#client-credentials}

Klientens autentiseringsuppgifter är en uppsättning unika värden som genereras under processen [Dynamic Client Registration (DCR)](#dcr) och som ska användas för att erhålla en [åtkomsttoken](#access-token).

#### Anpassat schema {#custom-scheme}

Det anpassade schemat är ett unikt värde som refererar till ett [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)-program som kan genereras och hämtas från Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) och ska användas som en slutgiltig omdirigering i program som körs på iOS-enheter.

### D {#d}

#### DCR {#dcr}

DCR (Dynamic Client Registration) är en auktoriseringsmekanism som definieras av [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) och som baseras på OAuth 2.0-auktoriseringsramverket som beskrivs i [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

DCR levereras till en [programmerare](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) som en Adobe Pass-autentiseringstjänst som ytterligare kan aktivera åtkomst till skyddade API:er.

Mer information finns i dokumentationen [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Registrerat program {#registered-application}

Det registrerade programmet är ett Adobe Pass Authentication-koncept som lagrar information om programmet [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) som måste fortsätta med processen [Dynamic Client Registration (DCR)](#dcr).

### S {#s}

#### Programsats {#software-statement}

Programsatsen är en JSON Web Token (JWT) som kan hämtas från Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) och ska användas som en del av processen [Dynamic Client Registration (DCR)](#dcr).
