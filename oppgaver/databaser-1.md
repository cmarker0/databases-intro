# SQL-oppgaver

Tabellene du jobber med:

```sql
CREATE TABLE Users (
    Id        INTEGER  PRIMARY KEY,
    Name      TEXT     NOT NULL
);

CREATE TABLE Post (
    Id        INTEGER  PRIMARY KEY,
    Title     TEXT     NOT NULL,
    Content   TEXT     NOT NULL,
    CreatedAt DATE     NOT NULL,
    Author    INTEGER  NOT NULL REFERENCES Users(Id)
);

CREATE TABLE Comment (
    Id        INTEGER  PRIMARY KEY,
    Content   TEXT     NOT NULL,
    CreatedAt DATE     NOT NULL,
    Author    INTEGER  NOT NULL REFERENCES Users(Id),
    PostId    INTEGER  NOT NULL REFERENCES Post(Id)
);
```

---

## Enkle oppgaver

**1.** Hent alle poster fra `Post`-tabellen.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Post;
```

</details>

---

**2.** Hent bare `Title` og `CreatedAt` fra alle poster.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Title, CreatedAt FROM Post;
```

</details>

---

**3.** Hent alle brukere fra `Users`-tabellen.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Users;
```

</details>

---

**4.** Hent alle kommentarer skrevet etter 1. januar 2024.

- (Hint: `CreatedAt` > 'YYYY-MM-DD')

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Comment
WHERE CreatedAt > '2024-01-01';
```

</details>

---

**5.** Hent alle poster sortert etter `CreatedAt`, nyeste først.

- (Hint: `ORDER BY ... DESC`)

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Post
ORDER BY CreatedAt DESC;
```

</details>

---

**6.** Hent de 5 nyeste postene.

- (Hint: `LIMIT`)

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Post
ORDER BY CreatedAt DESC
LIMIT 5;
```

</details>

---

**7.** Hent alle kommentarer som tilhører posten med `Id = 1`.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT * FROM Comment
WHERE PostId = 1;
```

</details>

---

## Middels oppgaver

**8.** Hent tittel og innhold på alle poster, sammen med navnet på forfatteren.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, Post.Content, Users.Name
FROM Post
INNER JOIN Users ON Post.Author = Users.Id;
```

</details>

---

**9.** Hent alle kommentarer sammen med navnet på brukeren som skrev dem.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Comment.Content, Comment.CreatedAt, Users.Name
FROM Comment
INNER JOIN Users ON Comment.Author = Users.Id;
```

</details>

---

**10.** Hent alle poster skrevet av brukeren med `Id = 2`, inkludert brukerens navn.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, Post.Content, Users.Name
FROM Post
INNER JOIN Users ON Post.Author = Users.Id
WHERE Users.Id = 2;
```

</details>

---

**11.** Hent tittel på poster og innholdet i tilhørende kommentarer (alle kommentarer til alle poster).

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, Comment.Content
FROM Comment
INNER JOIN Post ON Comment.PostId = Post.Id;
```

</details>

---

**12.** Hent alle poster som har minst én kommentar. Vis tittel og forfatterens navn.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, Users.Name
FROM Post
INNER JOIN Users ON Post.Author = Users.Id
INNER JOIN Comment ON Comment.PostId = Post.Id;
```

</details>

---

## Vanskelige oppgaver

**13.** Hent navn på alle brukere som har skrevet minst én post, og antall poster de har skrevet. Sorter etter antall poster, flest først.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Users.Name, COUNT(Post.Id) AS antall_poster
FROM Users
INNER JOIN Post ON Post.Author = Users.Id
GROUP BY Users.Id, Users.Name
ORDER BY antall_poster DESC;
```

</details>

---

**14.** Hent tittel på alle poster sammen med antall kommentarer hver post har fått.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, COUNT(Comment.Id) AS antall_kommentarer
FROM Post
INNER JOIN Comment ON Comment.PostId = Post.Id
GROUP BY Post.Id, Post.Title;
```

</details>

---

**15.** Hent navn og kommentarinnhold for alle kommentarer på poster skrevet av bruker med `Id = 1` (dvs. kommentarer på den brukerens poster).

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Users.Name, Comment.Content
FROM Comment
INNER JOIN Post ON Comment.PostId = Post.Id
INNER JOIN Users ON Comment.Author = Users.Id
WHERE Post.Author = 1;
```

</details>

---

**16.** Finn den posten med flest kommentarer. Vis tittel og antall kommentarer.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Post.Title, COUNT(Comment.Id) AS antall_kommentarer
FROM Post
INNER JOIN Comment ON Comment.PostId = Post.Id
GROUP BY Post.Id, Post.Title
ORDER BY antall_kommentarer DESC
LIMIT 1;
```

</details>

---

**17.** Hent alle brukere som har kommentert på en post de selv ikke har skrevet. Vis brukerens navn og tittelen på posten.

<details>
<summary>🔑 Vis løsningsforslag</summary>

```sql
SELECT Users.Name, Post.Title
FROM Comment
INNER JOIN Users ON Comment.Author = Users.Id
INNER JOIN Post ON Comment.PostId = Post.Id
WHERE Comment.Author != Post.Author;
```

</details>
