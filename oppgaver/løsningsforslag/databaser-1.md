# Fasit: SQL-oppgaver

---

## Enkle oppgaver

**1.** Hent alle poster fra `Post`-tabellen.

```sql
SELECT * FROM Post;
```

**2.** Hent bare `Title` og `CreatedAt` fra alle poster.

```sql
SELECT Title, CreatedAt FROM Post;
```

**3.** Hent alle brukere fra `User`-tabellen.

```sql
SELECT * FROM User;
```

**4.** Hent alle kommentarer skrevet etter 1. januar 2024.

```sql
SELECT * FROM Comment
WHERE CreatedAt > '2024-01-01';
```

**5.** Hent alle poster sortert etter `CreatedAt`, nyeste først.

```sql
SELECT * FROM Post
ORDER BY CreatedAt DESC;
```

**6.** Hent de 5 nyeste postene.

```sql
SELECT * FROM Post
ORDER BY CreatedAt DESC
LIMIT 5;
```

**7.** Hent alle kommentarer som tilhører posten med `Id = 1`.

```sql
SELECT * FROM Comment
WHERE PostId = 1;
```

---

## Middels oppgaver

**8.** Hent tittel og innhold på alle poster, sammen med navnet på forfatteren.

```sql
SELECT Post.Title, Post.Content, User.Name
FROM Post
INNER JOIN User ON Post.Author = User.Id;
```

**9.** Hent alle kommentarer sammen med navnet på brukeren som skrev dem.

```sql
SELECT Comment.Content, Comment.CreatedAt, User.Name
FROM Comment
INNER JOIN User ON Comment.Author = User.Id;
```

**10.** Hent alle poster skrevet av brukeren med `Id = 2`, inkludert brukerens navn.

```sql
SELECT Post.Title, Post.Content, User.Name
FROM Post
INNER JOIN User ON Post.Author = User.Id
WHERE User.Id = 2;
```

**11.** Hent tittel på poster og innholdet i tilhørende kommentarer (alle kommentarer til alle poster).

```sql
SELECT Post.Title, Comment.Content
FROM Comment
INNER JOIN Post ON Comment.PostId = Post.Id;
```

**12.** Hent alle poster som har minst én kommentar. Vis tittel og forfatterens navn.

```sql
SELECT Post.Title, User.Name
FROM Post
INNER JOIN User ON Post.Author = User.Id
INNER JOIN Comment ON Comment.PostId = Post.Id;
```

---

## Vanskelige oppgaver

**13.** Hent navn på alle brukere som har skrevet minst én post, og antall poster de har skrevet. Sorter etter antall poster, flest først.

```sql
SELECT User.Name, COUNT(Post.Id) AS antall_poster
FROM User
INNER JOIN Post ON Post.Author = User.Id
GROUP BY User.Id, User.Name
ORDER BY antall_poster DESC;
```

**14.** Hent tittel på alle poster sammen med antall kommentarer hver post har fått.

```sql
SELECT Post.Title, COUNT(Comment.Id) AS antall_kommentarer
FROM Post
INNER JOIN Comment ON Comment.PostId = Post.Id
GROUP BY Post.Id, Post.Title;
```

**15.** Hent navn og kommentarinnhold for alle kommentarer på poster skrevet av bruker med `Id = 1`.

```sql
SELECT User.Name, Comment.Content
FROM Comment
INNER JOIN Post ON Comment.PostId = Post.Id
INNER JOIN User ON Comment.Author = User.Id
WHERE Post.Author = 1;
```

**16.** Finn den posten med flest kommentarer. Vis tittel og antall kommentarer.

```sql
SELECT Post.Title, COUNT(Comment.Id) AS antall_kommentarer
FROM Post
INNER JOIN Comment ON Comment.PostId = Post.Id
GROUP BY Post.Id, Post.Title
ORDER BY antall_kommentarer DESC
LIMIT 1;
```

**17.** Hent alle brukere som har kommentert på en post de selv ikke har skrevet. Vis brukerens navn og tittelen på posten.

```sql
SELECT User.Name, Post.Title
FROM Comment
INNER JOIN User ON Comment.Author = User.Id
INNER JOIN Post ON Comment.PostId = Post.Id
WHERE Comment.Author != Post.Author;
```
