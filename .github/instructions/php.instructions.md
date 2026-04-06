---
description: "PHP 8.3 coding standards and conventions"
applyTo: "**/*.php"
name: "PHP-8.3-Coding-Standards"
---

# PHP 8.3 Instructions

**Purpose:** Secure, maintainable PHP code with strict typing. PSR-12 formatting. Student-friendly, well-documented.

## Declaration & Typing
- First line in each file: `declare(strict_types=1);`
- Use strict type hints on all function parameters and return types: `function getName(int $id): string`
- Use typed properties: `private string $name;`
- Nullable types when appropriate: `?string $middleware`

## Formatting & Style (PSR-12)
- 4-space indentation (no tabs)
- 120 character line limit
- Opening braces on same line: `function test() {`
- Class declarations: `class MyClass implements InterfaceA, InterfaceB {`
- Method visibility always explicit: `public`, `protected`, `private`
- Blank line after `namespace` and after `use` blocks

## Database Security
- **PDO prepared statements exclusively** for all queries
- Never concatenate user input directly into SQL: `$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");`
- Use parameterized queries: `$stmt->execute([$userId]);` or named placeholders: `:id`
- Always validate and sanitize input server-side
- Use type casting for expected types: `(int)$_GET['id']`

## Code Organization
- One class per file; filename matches class name: `UserRepository.php` contains `class UserRepository`
- Use namespaces: `namespace App\Repositories;`
- Group related methods logically; helpers at bottom
- Keep functions/methods under 20 lines when possible

## Error Handling
- Use exceptions for error conditions, not silent failures
- Catch exceptions with specific types; avoid bare `catch (Exception $e)`
- Log errors; never expose internal errors to users
- Validate all external input (POST, GET, files)

## Comments & Teaching
- Comments explain **why**, not **what** the code does
- Docblocks for classes and public methods: `/** @return int */`
- Avoid over-commenting obvious code

## Examples

```php
<?php
declare(strict_types=1);

namespace App\Services;

class UserService {
    private \PDO $pdo;

    public function __construct(\PDO $pdo) {
        $this->pdo = $pdo;
    }

    public function getUserById(int $id): array {
        $stmt = $this->pdo->prepare(
            'SELECT id, name, email FROM users WHERE id = ? LIMIT 1'
        );
        $stmt->execute([$id]);
        
        return $stmt->fetch(\PDO::FETCH_ASSOC) ?: [];
    }

    public function createUser(string $name, string $email): int {
        // Validate input
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new \InvalidArgumentException('Invalid email');
        }

        $stmt = $this->pdo->prepare(
            'INSERT INTO users (name, email) VALUES (?, ?)'
        );
        $stmt->execute([$name, $email]);
        
        return (int) $this->pdo->lastInsertId();
    }
}
```

## Additional Resources
- [PSR-12: Extended Coding Style](https://www.php-fig.org/psr/psr-12/)
- [PDO Prepared Statements](https://www.php.net/manual/en/pdo.prepared-statements.php)
- [PHP 8.3 Type System](https://www.php.net/manual/en/language.types.declarations.php)