# Fasit: Koble DatabaseService til databasen

```csharp
public class DatabaseService : IDatabaseService
{
    private readonly ApplicationDbContext _context;

    public DatabaseService(ApplicationDbContext context)
    {
        _context = context;
    }
}
```

---

## Oppgave 1: Hent alle poster

```csharp
public List<Post> GetPosts()
{
    return _context.Posts
        .Include(p => p.Comments)
        .OrderByDescending(p => p.CreatedAt)
        .ToList();
}
```

---

## Oppgave 2: Hent en post med ID

```csharp
public Post? GetPostById(int postId)
{
    return _context.Posts
        .Include(p => p.Comments)
        .Include(p => p.Author)
        .FirstOrDefault(p => p.Id == postId);
}
```

---

## Oppgave 3: Sjekk om en post finnes

```csharp
public bool PostExists(int id)
{
    return _context.Posts.Any(p => p.Id == id);
}
```

---

## Oppgave 4: Legg til en post

```csharp
public void AddPost(Post post)
{
    post.CreatedAt = DateTime.UtcNow;
    _context.Posts.Add(post);
    _context.SaveChanges();
}
```

---

## Oppgave 5: Legg til en kommentar

```csharp
public void AddComment(Comment comment)
{
    comment.CreatedAt = DateTime.UtcNow;
    _context.Comments.Add(comment);
    _context.SaveChanges();
}
```

---

## Oppgave 6: Hent eller opprett bruker

```csharp
public User GetOrCreateUser(string id, string name)
{
    var user = _context.Users.FirstOrDefault(u => u.Id == id);
    if (user == null)
    {
        user = new User { Id = id, Name = name };
        _context.Users.Add(user);
        _context.SaveChanges();
    }
    return user;
}
```

---

## Oppgave 7: Oppdater en post

```csharp
public void UpdatePost(int postId, UpdatePostDTO updatedPost)
{
    var post = _context.Posts.FirstOrDefault(p => p.Id == postId);
    if (post != null)
    {
        post.Title = updatedPost.Title;
        post.Content = updatedPost.Content;
        _context.SaveChanges();
    }
}
```

---

## Oppgave 8: Slett en post

```csharp
public void DeletePost(int postId)
{
    var post = _context.Posts.FirstOrDefault(p => p.Id == postId);
    if (post != null)
    {
        _context.Posts.Remove(post);
        _context.SaveChanges();
    }
}
```
