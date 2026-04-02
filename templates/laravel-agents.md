# Laravel Project Guidelines

**For PHP/Laravel Projects**

---

## Project Structure

```
project/
├── app/
│   ├── Console/
│   ├── Exceptions/
│   ├── Http/
│   │   ├── Controllers/
│   │   ├── Middleware/
│   │   └── Requests/
│   ├── Models/
│   ├── Providers/
│   ├── Repositories/
│   ├── Services/
│   └── Policies/
├── bootstrap/
├── config/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeders/
├── routes/
├── storage/
│   ├── app/
│   ├── framework/
│   └── logs/
└── tests/
    ├── Feature/
    └── Unit/
```

## Architecture Patterns

### Repository Pattern
```php
// Use Repository pattern for complex queries
interface UserRepositoryInterface {
    public function findByEmail(string $email): ?User;
    public function search(array $filters): Collection;
}

class UserRepository implements UserRepositoryInterface {
    public function __construct(private User $model) {}
    
    public function findByEmail(string $email): ?User {
        return $this->model->where('email', $email)->first();
    }
}
```

### Service Layer
```php
// Use Service classes for business logic
class OrderService {
    public function __construct(
        private OrderRepository $orders,
        private InventoryService $inventory
    ) {}
    
    public function processOrder(array $data): Order {
        // Business logic here
    }
}
```

### Form Requests for Validation
```php
// Use Form Requests for complex validation
class CreateUserRequest extends FormRequest {
    public function rules(): array {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|string|min:8',
        ];
    }
    
    public function messages(): array {
        return [
            'email.unique' => 'This email is already registered.',
        ];
    }
}
```

### Policies for Authorization
```php
// Use Policies for authorization
class UserPolicy {
    public function view(User $user, User $model): bool {
        return $user->id === $model->id || $user->isAdmin();
    }
}
```

## Code Style & Standards

### PSR-12 Compliance
- Follow PSR-4 autoloading
- Use PSR-3 logging interface
- Implement PSR-6 caching when possible
- Follow PSR-7 for HTTP messages

### Type Hints & Return Types
```php
class UserController extends Controller
{
    public function show(int $id): User
    {
        return User::findOrFail($id);
    }
    
    public function update(UpdateUserRequest $request, int $id): User
    {
        $user = User::findOrFail($id);
        $user->update($request->validated());
        return $user->fresh();
    }
}
```

### PHPDoc Standards
```php
/**
 * Retrieve a user by their email address.
 *
 * @param string $email The user's email address
 * @return User|null The user instance or null if not found
 * @throws InvalidArgumentException If email format is invalid
 */
public function findByEmail(string $email): ?User
```

## Database & Eloquent

### Eloquent Best Practices
```php
// Use eager loading to prevent N+1 queries
$users = User::with(['posts', 'comments'])->get();

// Use whereHas for filtering relations
$users = User::whereHas('posts', function ($query) {
    $query->where('published', true);
})->get();

// Use chunking for large datasets
User::chunk(200, function ($users) {
    foreach ($users as $user) {
        // Process user
    }
});

// Useupsert for bulk updates
User::upsert(
    $data,
    ['email'],
    ['name', 'updated_at']
);
```

### Database Migrations
```php
// Always include down() method
public function up(): void
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->timestamp('email_verified_at')->nullable();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}

public function down(): void
{
    Schema::dropIfExists('users');
}

// Add indexes for performance
$table->index(['email', 'status']);
$table->foreignId('user_id')->constrained()->onDelete('cascade');
```

### Query Optimization
- Use `select()` to specify needed columns
- Add database indexes for frequently queried columns
- Use `explain()` to analyze query performance
- Implement database-level caching for expensive queries

## Testing

### Pest/ PHPUnit Configuration
```php
// Use factories for test data
test('users can be created', function () {
    $user = User::factory()->create([
        'name' => 'Test User',
    ]);
    
    expect($user->name)->toBe('Test User');
});

// Use proper test assertions
test('validation errors are returned', function () {
    $response = $this->post('/api/users', [
        'email' => 'invalid-email',
    ]);
    
    $response->assertStatus(422)
             ->assertJsonValidationErrors(['email']);
});
```

### Test Coverage Goals
- Minimum 80% on business logic
- 100% on Models, Services, Repositories
- Integration tests for API endpoints
- Feature tests for critical user journeys

## Artisan Commands

### Common Commands
```bash
# Development
php artisan serve
php artisan tinker
php artisan queue:work
php artisan schedule:run

# Database
php artisan migrate:fresh --seed
php artisan db:seed
php artisan migrate:rollback

# Code Generation
php artisan make:model -mcr # Model, Migration, Controller, Resource
php artisan make:request CreateUserRequest
php artisan make:policy UserPolicy
php artisan make:service UserService

# Testing
php artisan test
php artisan test --coverage
```

## Security Best Practices

### CSRF Protection
- Always use `@csrf` in forms
- Verify CSRF token for API requests
- Use `VerifyCsrfToken` middleware

### SQL Injection Prevention
- Use Eloquent or Query Builder (automatically sanitized)
- Never use `DB::raw()` with user input
- Use parameter binding in raw queries

### XSS Prevention
- Use `{{ $var }}` (escaped output) by default
- Use `{!! $var !!}` only when trusted
- Use Laravel's `e()` helper for manual escaping

### Authentication
- Use Laravel Sanctum for SPA/API authentication
- Implement proper password hashing (bcrypt/argon2)
- Use rate limiting for login attempts
- Implement proper session management

## Performance Optimization

### Caching
```php
// Use cache for expensive operations
return Cache::remember('users.active', 3600, function () {
    return User::where('active', true)->get();
});

// Use cache tags for granular invalidation
Cache::tags(['users'])->put('active', $users, 3600);
```

### Queue Jobs
```php
// Use queues for long-running tasks
class ProcessPodcast implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;
    
    public function handle(): void
    {
        // Time-consuming processing
    }
}
```

### Database Optimization
- Use database-level caching
- Implement read/write splitting for high-traffic apps
- Use proper indexing strategies
- Monitor query performance with Laravel Debugbar

## API Design

### RESTful Conventions
```php
// Resource Controllers
Route::resource('users', UserController::class);

// Standard HTTP status codes
200 - OK
201 - Created
204 - No Content
400 - Bad Request
401 - Unauthorized
403 - Forbidden
404 - Not Found
422 - Unprocessable Entity
500 - Internal Server Error
```

### Response Format
```json
{
    "data": {
        "id": 1,
        "name": "John Doe"
    },
    "message": "User created successfully",
    "success": true
}
```

### Error Response Format
```json
{
    "success": false,
    "message": "Validation failed",
    "errors": {
        "email": ["The email field is required."]
    },
    "code": 422
}
```

## Additional Resources

- [Laravel Documentation](https://laravel.com/docs)
- [Laravel Best Practices](https://github.com/alexeymezenin/laravel-best-practices)
- [PHP-FIG Standards](https://www.php-fig.org/)
- [OWASP PHP Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/PHP_Cheat_Sheet.html)
