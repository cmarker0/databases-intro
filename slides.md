---
theme: default
colorSchema: dark
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
fonts:
  sans: JetBrains Mono
  mono: JetBrains Mono
---

# Introduksjon til Databaser

---
layout: default
---

# Hva skal vi lære i dag?


- 📋 **Tabeller** – Grunnsteinen i en database
- 🔤 **Kolonnetyper** – Hvilke typer data kan vi lagre?
- 🔗 **Relasjoner** – Hvordan henger data sammen?
- 🔑 **Primær- og fremmednøkler** – Kobling mellom tabeller
- 🔍 **SELECT** – Hente og filtrere data
- 🔗 **INNER JOIN** – Hente data fra flere tabeller samtidig
- ✏️ **Oppgaver** – Opprette tabeller og skrive spørringer

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

- **Rader** – én enhet av data
- **Kolonner** – en egenskap ved dataene
- **Celler** – verdien der en rad og kolonne møtes

</div>
<div class="mt-2">
<div class="info-block blue">

💡 Tenk på en tabell som et **Excel-ark** – men med strenge regler for hva slags data som kan legges inn!

</div>
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

Dette er viktig fordi det:
- Sikrer at riktig type data lagres
- Gjør databasen mer effektiv
- Forhindrer feil (f.eks. tekst der det skal være tall)

---
layout: default
---

# Vanlige kolonnetyper (1/2)

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

**Tekst**

| Type | Beskrivelse |
| ------ | ------------- |
| `CHAR(n)` | Fast lengde tekst |
| `VARCHAR(n)` | Variabel lengde tekst (maks n tegn) |
| `TEXT` | Ubegrenset tekst |

</div>
<div>

**Tall**

| Type | Beskrivelse |
| ------ | -------------|
| `INTEGER` | Heltall (…-1, 0, 1, 2…) |
| `DECIMAL(p,s)` | Desimaltall med presisjon |
| `FLOAT` | Flyttall |

</div>
</div>

---
layout: default
---

# Vanlige kolonnetyper (2/2)

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

**Dato og tid**

| Type | Beskrivelse |
| ------ | ------------- |
| `DATE` | Dato (YYYY-MM-DD) |
| `TIME` | Klokkeslett |
| `DATETIME` | Dato og tid kombinert |

</div>
<div>

**Andre**

| Type | Beskrivelse |
| ------ | ------------- |
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

<div class="info-block green">

💡 **NB:** I systemer med begrenset lagringskapasitet er det viktig å velge rett type på teskt-verdiene. Men i det daglige så vil det være nok med `TEXT`.

</div>

---
layout: section
---

# Relasjoner

---
layout: default
---

# Hva er relasjoner? (1/2)

En relasjonsdatabase lar oss koble tabeller sammen slik at vi unngår å gjenta de samme dataene mange ganger. Dette kalles **normalisering**.

**Uten relasjoner (dårlig):**

| ordre_id | kunde_navn | kunde_epost       | vare    |
| -------- | ---------- | ----------------- | ------- |
| 1        | Kari N.    | kari@skole.no     | Bok     |
| 2        | Kari N.    | kari@skole.no     | Penn    |
| 3        | Ola H.     | ola@skole.no      | Linjal  |

Data om Kari gjentas på hver rad - dette er sløsing og kan gi inkonsistens.

---
layout: default
---

# Hva er relasjoner? (2/2)

<div class="grid grid-cols-3 gap-6 mt-4">
<div>

Tabell: `kunder`

| id | navn   | epost         |
|----|--------|---------------|
| 1  | Kari N.| kari@skole.no |
| 2  | Ola H. | ola@skole.no  |

</div>
<div>

Tabell: `produkter`

| id | navn   | pris  |
|----|--------|-------|
| 1  | Bok    | 99    |
| 2  | Penn   | 15    |
| 3  | Linjal | 25    |

</div>
<div>

Tabell: `ordrer`

| id | kunde_id | produkt_id |
|----|----------|------------|
| 1  | 1        | 1          |
| 2  | 1        | 2          |
| 3  | 2        | 3          |

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

```sql {all|2}
CREATE TABLE studenter (
    id        INTEGER     PRIMARY KEY AUTO_INCREMENT,  -- primærnøkkel!
    fornavn   VARCHAR(50) NOT NULL,
    etternavn VARCHAR(50) NOT NULL,
    epost     VARCHAR(100)
);
```

<div class="mt-6">

**Regler for primærnøkler:**

- Må være **unik** – ingen to rader kan ha samme verdi
- Kan **ikke** være `NULL` (tom/ukjent)
- Hver tabell bør ha minst **én** primærnøkkel
- Ofte brukes et automatisk incrementerende tall: `SERIAL` / `AUTO_INCREMENT`

</div>

---
layout: default
---

# Fremmednøkkel (Foreign Key)

En **fremmednøkkel** er en kolonne som refererer til primærnøkkelen i en annen tabell – og skaper dermed en kobling mellom tabellene.

```sql {all|2,9}
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

<div class="info-block green">

🔗 <code>klasse_id</code> i `studenter`-tabellen peker på `id` i `klasser`-tabellen. Databasen sørger for at du ikke kan legge inn en `klasse_id` som ikke finnes!

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

<div class="mt-8 text-center">

`studenter.klasse_id` → `klasser.id`

Kari og Ola går i klasse **1A**, Ingrid går i klasse **1B**

</div>

---
layout: default
---

# Oppsummering

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

📋 **Tabeller**
Grunnstrukturen i en database. Rader = poster, kolonner = egenskaper.

🔤 **Kolonnetyper**
Hver kolonne har en type: `INTEGER`, `VARCHAR`, `DATE`, `BOOLEAN`, osv.

🔗 **Relasjoner**
Unngå å gjenta data – koble tabeller i stedet for å duplisere informasjon.

</div>
<div>

🔑 **Primærnøkkel**
Unik identifikator for hver rad i en tabell. Kan ikke være `NULL`.

🔗 **Fremmednøkkel**
Refererer til en primærnøkkel i en annen tabell og skaper en kobling.

</div>
</div>

---
layout: center
---

# Oppgave 1 *(5 minutter)*

<div class="mt-8">

I deres prosjekt har dere følgende tabeller `Post`, `Comment` og `User`.

Skriv SQL-spørringer som oppretter disse tabellene.


<div>

| **Post**    | **Comment** | **User** |
| ----------- | ---------   |--------- |
| Id          | Id          | Id       |
| Content     | Content     | Name     |
| CreatedAt   | CreatedAt   |          |
| Author      | Author      |          |
| Title       | PostId      |          |
| Comments    |             |          |

</div>
</div>

---
layout: default
---

# Løsningsforslag: Oppgave 1

```sql
CREATE TABLE User (
    Id        INTEGER       PRIMARY KEY,
    Name      TEXT          NOT NULL
);

CREATE TABLE Post (
    Id        INTEGER       PRIMARY KEY,
    Content   TEXT          NOT NULL,
    CreatedAt DATE          NOT NULL,
    Author    INTEGER       NOT NULL REFERENCES User(Id),
    Title     TEXT          NOT NULL
);

CREATE TABLE Comment (
    Id        INTEGER       PRIMARY KEY,
    Content   TEXT          NOT NULL,
    CreatedAt DATE          NOT NULL,
    Author    INTEGER       NOT NULL REFERENCES User(Id),
    PostId    INTEGER       NOT NULL REFERENCES Post(Id)
);
```

---
layout: section
---

# Hente data med SELECT

---
layout: default
---

# SELECT – grunnleggende syntaks

`SELECT` brukes til å hente data fra en tabell.<br/>For å hente alle kolonner bruker man `*`

```sql
SELECT *
FROM studenter;
```


<div class="mt-6">

**Resultat:**

| id | fornavn | etternavn | epost           | fodselsar |
|----|---------|-----------|-----------------|-----------|
| 1  | Kari    | Nordmann  | kari@skole.no   | 2005      |
| 2  | Ola     | Hansen    | ola@skole.no    | 2004      |
| 3  | Ingrid  | Berg      | ingrid@skole.no | 2005      |

</div>

---
layout: default
---

# SELECT – filtrere med WHERE

`WHERE` lar deg filtrere hvilke rader som returneres.

```sql
SELECT fornavn, etternavn, fodselsar
FROM studenter
WHERE fodselsar = 2005;
```

<div class="mt-6">

**Resultat:**

| fornavn | etternavn | fodselsar |
|---------|-----------| ----------|
| Kari    | Nordmann  | 2005      |
| Ingrid  | Berg      | 2005      |

</div>

<div class="info-block blue">

💡 Du kan kombinere betingelser med `AND` og `OR`, og bruke operatorer som `=`, `>`, `<`, `!=`, `LIKE`.

</div>

---
layout: default
---

# SELECT – sortere og begrense

`ORDER BY` sorterer resultatet, `LIMIT` begrenser antall rader.

```sql
SELECT fornavn, etternavn, fodselsar
FROM studenter
ORDER BY fodselsar DESC
LIMIT 2;
```

<div class="mt-6">

**Resultat (nyeste først, maks 2 rader):**

| fornavn | etternavn | fodselsar |
|---------|-----------|-----------|
| Kari    | Nordmann  | 2005      |
| Ingrid  | Berg      | 2005      |

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

<div class="info-block purple">

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

| id    | navn   |
| ----- | ------ |
| **1** | 1A     |
| **2** | 1B     |
| 3     | 2A ❌  |

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

<div class="info-block yellow">

💡 Du kan lenke så mange `INNER JOIN` som du trenger – bare sørg for at du alltid kobler via nøklene!

</div>

---
layout: center
---

# Oppgave 2!

<div class="mt-8">

Bruk tabellene du opprettet i forrige oppgave (`Post`, `Comment`, `User`).

Skriv en SQL-spørring som henter ut **alle poster skrevet av en bestemt bruker**, inkludert brukerens navn.

<div class="mt-6">

**💡 Tips:**
- Bruk `SELECT` for å velge hvilke kolonner du vil ha med
- Bruk `INNER JOIN` for å koble `Post` og `User`
- Bruk `WHERE` for å filtrere på en bestemt bruker

</div>
</div>

---
layout: default
---

# Løsningsforslag: Oppgave 2

```sql
SELECT *
 -- Eller spesifikt:
SELECT
    Post.Title,
    Post.Content,
    Post.CreatedAt,
    User.Name
FROM Post
INNER JOIN User ON Post.Author = User.Id
WHERE User.Name = 'Kari Nordmann';
```

---
layout: center
---

# Flere oppgaver i `oppgaver.md` frem til lunsj.