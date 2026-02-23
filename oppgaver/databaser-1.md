# SQL-oppgaver

Tabellene du jobber med:

```sql
CREATE TABLE User (
    Id        INTEGER  PRIMARY KEY,
    Name      TEXT     NOT NULL
);

CREATE TABLE Post (
    Id        INTEGER  PRIMARY KEY,
    Title     TEXT     NOT NULL,
    Content   TEXT     NOT NULL,
    CreatedAt DATE     NOT NULL,
    Author    INTEGER  NOT NULL REFERENCES User(Id)
);

CREATE TABLE Comment (
    Id        INTEGER  PRIMARY KEY,
    Content   TEXT     NOT NULL,
    CreatedAt DATE     NOT NULL,
    Author    INTEGER  NOT NULL REFERENCES User(Id),
    PostId    INTEGER  NOT NULL REFERENCES Post(Id)
);
```

---

## Enkle oppgaver

**1.** Hent alle poster fra `Post`-tabellen.

**2.** Hent bare `Title` og `CreatedAt` fra alle poster.

**3.** Hent alle brukere fra `User`-tabellen.

**4.** Hent alle kommentarer skrevet etter 1. januar 2024.

- (Hint: `CreatedAt` > 'YYYY-MM-DD')

**5.** Hent alle poster sortert etter `CreatedAt`, nyeste først.

- (Hint: `ORDER BY ... DESC`)

**6.** Hent de 5 nyeste postene.

- (Hint: `LIMIT`)

**7.** Hent alle kommentarer som tilhører posten med `Id = 1`.

---

## Middels oppgaver

**8.** Hent tittel og innhold på alle poster, sammen med navnet på forfatteren.

**9.** Hent alle kommentarer sammen med navnet på brukeren som skrev dem.

**10.** Hent alle poster skrevet av brukeren med `Id = 2`, inkludert brukerens navn.

**11.** Hent tittel på poster og innholdet i tilhørende kommentarer (alle kommentarer til alle poster).

**12.** Hent alle poster som har minst én kommentar. Vis tittel og forfatterens navn.

---

## Vanskelige oppgaver

**13.** Hent navn på alle brukere som har skrevet minst én post, og antall poster de har skrevet. Sorter etter antall poster, flest først.

**14.** Hent tittel på alle poster sammen med antall kommentarer hver post har fått.

**15.** Hent navn og kommentarinnhold for alle kommentarer på poster skrevet av bruker med `Id = 1` (dvs. kommentarer på den brukerens poster).

**16.** Finn den posten med flest kommentarer. Vis tittel og antall kommentarer.

**17.** Hent alle brukere som har kommentert på en post de selv ikke har skrevet. Vis brukerens navn og tittelen på posten.
