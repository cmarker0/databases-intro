---
theme: default
title: Introduksjon til Databaser
info: |
  ## Introduksjon til Databaser
  En innføring i grunnleggende databasekonsepter for studenter.
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Introduksjon til Databaser

Grunnleggende konsepter for deg som er ny til databaser

<div class="pt-12">
  <span class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    La oss begynne! 🗄️
  </span>
</div>

---
layout: default
---

# Hva skal vi lære i dag?

<v-clicks>

- 📋 **Tabeller** – Grunnsteinen i en database
- 🔤 **Kolonnetyper** – Hvilke typer data kan vi lagre?
- 🔗 **Relasjoner** – Hvordan henger data sammen?
- 🔑 **Primær- og fremmednøkler** – Kobling mellom tabeller
- 🔍 **INNER JOIN** – Hente data fra flere tabeller samtidig

</v-clicks>

---
layout: section
---

# Tabeller

---
layout: default
---

# Hva er en tabell?

En tabell er den grunnleggende strukturen for å lagre data i en database.

<div class="grid grid-cols-2 gap-8 mt-6">
<div>

**En tabell består av:**

<v-clicks>

- **Rader** (også kalt poster/records) – én enhet av data
- **Kolonner** (også kalt felt/fields) – en egenskap ved dataene
- **Celler** – verdien der en rad og kolonne møtes

</v-clicks>

</div>
<div class="mt-2">

Tenk på en tabell som et **Excel-ark** – men med strenge regler for hva slags data som kan legges inn!

</div>
</div>

---
layout: default
---

# Eksempel: Tabell med studenter

```sql
-- Opprette tabellen
CREATE TABLE studenter (
    id          INTEGER,
    fornavn     VARCHAR(50),
    etternavn   VARCHAR(50),
    epost       VARCHAR(100),
    fodselsar   INTEGER
);
```

<div class="mt-6">

| id | fornavn | etternavn | epost                  | fodselsar |
|----|---------|-----------|------------------------|-----------|
| 1  | Kari    | Nordmann  | kari@skole.no          | 2005      |
| 2  | Ola     | Hansen    | ola@skole.no           | 2004      |
| 3  | Ingrid  | Berg      | ingrid@skole.no        | 2005      |

</div>

---
layout: section
---

# Kolonnetyper

---
layout: default
---

# Hva er kolonnetyper?

Hver kolonne i en tabell må ha en **datatype** som bestemmer hva slags verdier som kan lagres der.

<v-clicks>

Dette er viktig fordi det:
- Sikrer at riktig type data lagres
- Gjør databasen mer effektiv
- Forhindrer feil (f.eks. tekst der det skal være tall)

</v-clicks>

---
layout: default
---

# Vanlige kolonnetyper

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

**Tekst**
| Type | Beskrivelse |
|------|-------------|
| `CHAR(n)` | Fast lengde tekst |
| `VARCHAR(n)` | Variabel lengde tekst (maks n tegn) |
| `TEXT` | Ubegrenset tekst |

**Tall**
| Type | Beskrivelse |
|------|-------------|
| `INTEGER` | Heltall (…-1, 0, 1, 2…) |
| `DECIMAL(p,s)` | Desimaltall med presisjon |
| `FLOAT` | Flyttall |

</div>
<div>

**Dato og tid**
| Type | Beskrivelse |
|------|-------------|
| `DATE` | Dato (YYYY-MM-DD) |
| `TIME` | Klokkeslett |
| `DATETIME` | Dato og tid kombinert |

**Andre**
| Type | Beskrivelse |
|------|-------------|
| `BOOLEAN` | Sann/Usann |
| `NULL` | Ingen verdi / ukjent |

</div>
</div>

---
layout: default
---

# Kolonnetyper i praksis

```sql
CREATE TABLE produkter (
    id            INTEGER,          -- heltall: 1, 2, 3 ...
    navn          VARCHAR(100),     -- tekst: maks 100 tegn
    beskrivelse   TEXT,             -- lang tekst uten grense
    pris          DECIMAL(10, 2),   -- f.eks. 199.90
    på_lager      BOOLEAN,          -- TRUE eller FALSE
    opprettet     DATE              -- f.eks. 2024-01-15
);
```

<div class="mt-6 p-4 bg-blue-50 rounded-lg text-blue-900">

💡 **Tips:** Velg alltid den typen som passer best til dataene dine. Bruk ikke `TEXT` når `VARCHAR(50)` holder, og ikke `FLOAT` til pengebeløp (bruk `DECIMAL` for å unngå avrundingsfeil!).

</div>

---
layout: section
---

# Relasjoner

---
layout: default
---

# Hva er relasjoner?

En relasjonsdatabase lar oss koble tabeller sammen slik at vi unngår å gjenta de samme dataene mange ganger.

<div class="grid grid-cols-2 gap-8 mt-6">
<div>

**Uten relasjoner (dårlig):**

| ordre_id | kunde_navn | kunde_epost       | vare    |
|----------|------------|-------------------|---------|
| 1        | Kari N.    | kari@skole.no     | Bok     |
| 2        | Kari N.    | kari@skole.no     | Penn    |
| 3        | Ola H.     | ola@skole.no      | Linjal  |

</div>
<div>

**Med relasjoner (bra):**

Tabell: `kunder`

| id | navn   | epost         |
|----|--------|---------------|
| 1  | Kari N.| kari@skole.no |
| 2  | Ola H. | ola@skole.no  |

Tabell: `ordrer`

| id | kunde_id | vare   |
|----|----------|--------|
| 1  | 1        | Bok    |
| 2  | 1        | Penn   |
| 3  | 2        | Linjal |

</div>
</div>

---
layout: section
---

# Primær- og fremmednøkler

---
layout: default
---

# Primærnøkkel (Primary Key)

En **primærnøkkel** er en kolonne (eller kombinasjon av kolonner) som **unikt identifiserer** hver rad i en tabell.

```sql {all|3}
CREATE TABLE studenter (
    id        INTEGER     PRIMARY KEY,  -- primærnøkkel!
    fornavn   VARCHAR(50) NOT NULL,
    etternavn VARCHAR(50) NOT NULL,
    epost     VARCHAR(100)
);
```

<div class="mt-6">

**Regler for primærnøkler:**

<v-clicks>

- Må være **unik** – ingen to rader kan ha samme verdi
- Kan **ikke** være `NULL` (tom/ukjent)
- Hver tabell bør ha **én** primærnøkkel
- Ofte brukes et automatisk incrementerende tall: `SERIAL` / `AUTO_INCREMENT`

</v-clicks>

</div>

---
layout: default
---

# Fremmednøkkel (Foreign Key)

En **fremmednøkkel** er en kolonne som refererer til primærnøkkelen i en annen tabell – og skaper dermed en kobling mellom tabellene.

```sql {all|4,8-9}
CREATE TABLE klasser (
    id    INTEGER     PRIMARY KEY,
    navn  VARCHAR(50) NOT NULL
);

CREATE TABLE studenter (
    id        INTEGER     PRIMARY KEY,
    fornavn   VARCHAR(50) NOT NULL,
    klasse_id INTEGER     REFERENCES klasser(id)  -- fremmednøkkel!
);
```

<div class="mt-4 p-4 bg-green-50 rounded-lg text-green-900">

🔗 `klasse_id` i `studenter`-tabellen peker på `id` i `klasser`-tabellen. Databasen sørger for at du ikke kan legge inn en `klasse_id` som ikke finnes!

</div>

---
layout: default
---

# Primær- og fremmednøkler visualisert

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

**Tabell: `klasser`**

| **id** 🔑 | navn |
|-----------|------|
| 1         | 1A   |
| 2         | 1B   |
| 3         | 2A   |

</div>
<div>

**Tabell: `studenter`**

| **id** 🔑 | fornavn | klasse_id 🔗 |
|-----------|---------|--------------|
| 1         | Kari    | 1            |
| 2         | Ola     | 1            |
| 3         | Ingrid  | 2            |

</div>
</div>

<div class="mt-8 text-center text-gray-600">

`studenter.klasse_id` → `klasser.id`

Kari og Ola går i klasse **1A**, Ingrid går i klasse **1B**

</div>

---
layout: section
---

# INNER JOIN

---
layout: default
---

# Hva er INNER JOIN?

`INNER JOIN` lar oss hente data fra **to eller flere tabeller** i én spørring, ved å koble dem sammen på en felles nøkkel.

<div class="mt-6">

```sql
SELECT kolonne1, kolonne2, ...
FROM tabell1
INNER JOIN tabell2 ON tabell1.nokkel = tabell2.nokkel;
```

</div>

<div class="mt-6 p-4 bg-purple-50 rounded-lg text-purple-900">

🔍 `INNER JOIN` returnerer **kun rader der det finnes en match i begge tabeller**. Rader uten kobling blir utelatt.

</div>

---
layout: default
---

# INNER JOIN – Eksempel

Vi ønsker å vise studentenes navn **og** klassen de tilhører:

```sql
SELECT
    studenter.fornavn,
    studenter.etternavn,
    klasser.navn AS klasse
FROM studenter
INNER JOIN klasser ON studenter.klasse_id = klasser.id;
```

<div class="mt-6">

**Resultat:**

| fornavn | etternavn | klasse |
|---------|-----------|--------|
| Kari    | Nordmann  | 1A     |
| Ola     | Hansen    | 1A     |
| Ingrid  | Berg      | 1B     |

</div>

---
layout: default
---

# INNER JOIN – Visualisert

<div class="flex justify-center items-start gap-8 mt-6">

<div class="text-center">

**studenter**

| id | fornavn | klasse_id |
|----|---------|-----------|
| 1  | Kari    | **1**     |
| 2  | Ola     | **1**     |
| 3  | Ingrid  | **2**     |
| 4  | Per     | NULL ❌   |

</div>

<div class="text-center text-4xl mt-12">⟺</div>

<div class="text-center">

**klasser**

| id  | navn |
|-----|------|
| **1** | 1A |
| **2** | 1B |
| 3   | 2A ❌ |

</div>

<div class="text-center">

**Resultat (INNER JOIN)**

| fornavn | klasse |
|---------|--------|
| Kari    | 1A     |
| Ola     | 1A     |
| Ingrid  | 1B     |

</div>

</div>

<div class="mt-4 text-center text-gray-500 text-sm">
Per (ingen klasse) og 2A (ingen studenter) er ikke med i resultatet
</div>

---
layout: default
---

# INNER JOIN med tre tabeller

Vi kan også koble sammen flere tabeller i én spørring:

```sql
SELECT
    studenter.fornavn,
    klasser.navn      AS klasse,
    laerere.navn      AS laerer
FROM studenter
INNER JOIN klasser ON studenter.klasse_id = klasser.id
INNER JOIN laerere ON klasser.laerer_id   = laerere.id;
```

<div class="mt-4 p-4 bg-yellow-50 rounded-lg text-yellow-900">

💡 Du kan lenke så mange `INNER JOIN` som du trenger – bare sørg for at du alltid kobler via nøklene!

</div>

---
layout: default
---

# Oppsummering

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

<v-clicks>

📋 **Tabeller**
Grunnstrukturen i en database. Rader = poster, kolonner = egenskaper.

🔤 **Kolonnetyper**
Hver kolonne har en type: `INTEGER`, `VARCHAR`, `DATE`, `BOOLEAN`, osv.

🔗 **Relasjoner**
Unngå å gjenta data – koble tabeller i stedet for å duplisere informasjon.

</v-clicks>

</div>
<div>

<v-clicks>

🔑 **Primærnøkkel**
Unik identifikator for hver rad i en tabell. Kan ikke være `NULL`.

🔗 **Fremmednøkkel**
Refererer til en primærnøkkel i en annen tabell og skaper en kobling.

🔍 **INNER JOIN**
Henter rader fra to eller flere tabeller der det finnes match på nøklene.

</v-clicks>

</div>
</div>

---
layout: center
class: text-center
---

# Spørsmål?

<div class="mt-8 text-gray-500">

Prøv gjerne å lage din egen database med tabeller, nøkler og JOIN-spørringer!

</div>

<div class="mt-6">

```sql
-- Kom i gang med din første tabell!
CREATE TABLE mine_data (
    id    INTEGER PRIMARY KEY,
    navn  VARCHAR(100) NOT NULL
);
```

</div>
