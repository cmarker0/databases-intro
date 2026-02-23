# Oppgave: Koble DatabaseService til databasen

I dag skal du oppdatere `DatabaseService` slik at den bruker `ApplicationDbContext` til å lese og skrive data til databasen, i stedet for å lagre data i minnet.

`DatabaseService` implementerer dette interfacet:

```csharp
public interface IDatabaseService
{
    List<Post> GetPosts();
    Post? GetPostById(int postId);
    void AddPost(Post post);
    void AddComment(Comment comment);
    void DeletePost(int postId);
    void UpdatePost(int postId, UpdatePostDTO updatedPost);
    bool PostExists(int id);
    User GetOrCreateUser(string objectId, string name);
}
```

Start med å injisere `ApplicationDbContext` i konstruktoren:

```csharp
public class DatabaseService : IDatabaseService
{
    private readonly ApplicationDbContext _context;

    public DatabaseService(ApplicationDbContext context)
    {
        _context = context;
    }

    // ...
}
```

---

## Oppgave 1: Hent alle poster

Implementer `GetPosts()` slik at den returnerer alle poster fra databasen, sortert etter `CreatedAt` med nyeste først. Inkluder kommentarene til hver post.

- Hint: `_context.Posts`, `.Include(...)`, `.OrderByDescending(...)`, `.ToList()`

---

## Oppgave 2: Hent en post med ID

Implementer `GetPostById(int postId)` slik at den returnerer posten med gitt ID. Returner `null` hvis posten ikke finnes. Inkluder kommentarene og forfatteren.

- Hint: `.Include(...)`, `.FirstOrDefault(...)`

---

## Oppgave 3: Sjekk om en post finnes

Implementer `PostExists(int id)` slik at den returnerer `true` hvis det finnes en post med gitt ID, og `false` ellers.

- Hint: `.Any(...)`

---

## Oppgave 4: Legg til en post

Implementer `AddPost(Post post)` slik at den lagrer en ny post i databasen. Sett `CreatedAt` til `DateTime.UtcNow` før du lagrer.

- Hint: `_context.Posts.Add(...)`, `_context.SaveChanges()`

---

## Oppgave 5: Legg til en kommentar

Implementer `AddComment(Comment comment)` slik at den lagrer en ny kommentar i databasen. Sett `CreatedAt` til `DateTime.UtcNow` før du lagrer.

- Hint: `_context.Comments.Add(...)`, `_context.SaveChanges()`

---

## Oppgave 6: Hent eller opprett bruker

Implementer `GetOrCreateUser(string objectId, string name)` slik at den:

1. Forsøker å hente brukeren med gitt `objectId` fra databasen.
2. Hvis brukeren ikke finnes, oppretter den en ny bruker med `Id = objectId` og `Name = name`, lagrer den, og returnerer den.
3. Returnerer brukeren uansett.

- Hint: `_context.Users.FirstOrDefault(...)`, `_context.Users.Add(...)`, `_context.SaveChanges()`

---

## Oppgave 7: Oppdater en post

Implementer `UpdatePost(int postId, UpdatePostDTO updatedPost)` slik at den oppdaterer `Title` og `Content` på posten med gitt ID. Gjor ingenting hvis posten ikke finnes.

- Hint: Hent posten med `.FirstOrDefault(...)`, oppdater feltene direkte, og kall `_context.SaveChanges()`

---

## Oppgave 8: Slett en post

Implementer `DeletePost(int postId)` slik at den sletter posten med gitt ID. Gjor ingenting hvis posten ikke finnes.

- Hint: `.FirstOrDefault(...)`, `_context.Posts.Remove(...)`, `_context.SaveChanges()`
