build-lists: true
theme: Simple
slide-transition: true
[.footer: Laracon EU 2020]

# _Better Testing_ in Laravel

#### Kai Sassnowski // @warsh33p // kai-sassnowski.com

---

[.footer: The Goal: _Properties of Tests_]

## **The Goal**

### i.e. What **properties** do I want my tests to have?

---

[.footer: The Goal: _Properties of Tests_]

## **The Goal**: Properties of Tests

- ~~Prove correctness~~
- Allow me to **refactor**
  - How **confident** am I in my test suite?
- Serve as **excecutable documentation**
- **Simple** to understand

---

[.footer: The Goal: _Properties of Tests_]

## [fit] What about **fast**?

---

## [fit] Reducing <br> **unnecessary details**[^1]

[^1]: https://www.kai-sassnowski.com/post/reducing-unnecessary-details-in-tests

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: all]
[.code-highlight: 4]
[.code-highlight: 6-10]
[.code-highlight: 7,9]
[.code-highlight: 12-17]
[.code-highlight: 14,16]
[.code-highlight: all]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'A really long description that no one will ever read.',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'A really long description that no one will ever read.',
    ]);
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 1-7]
[.code-highlight: 4]
[.code-highlight: 6]
[.code-highlight: 1-7]
[.code-highlight: 9-15]
[.code-highlight: 12]
[.code-highlight: 14]
[.code-highlight: 9-15]

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('9999999999');
}
```

---

## What about **all those strings**?

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 7,9,14,16]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'A really long description that no one will ever read.',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'A really long description that no one will ever read.',
    ]);
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 7,9,14,16]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'My super cool game',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => 'My super cool game',
        'is_public' => 1,
        'description' => 'My super cool game',
    ]);
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 7,9,14,16]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => 'aaaaa',
        'is_public' => 1,
        'description' => 'aaaaa',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => 'aaaaa',
        'is_public' => 1,
        'description' => 'aaaaa',
    ]);
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 7,9,14,16]

[.column]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => 'aaaaa',
        'is_public' => 1,
        'description' => 'aaaaa',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => 'aaaaa',
        'is_public' => 1,
        'description' => 'aaaaa',
    ]);
}
```

[.column]

- What matters is that the strings **match**.
- We're only interested in what the string **represents**
- We **don't care** about their **exact values**

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 4,6,14]

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('9999999999');
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 4,6,14]

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('ABCDEFG10234');
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 12]

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('ABCDEFG10234');
}
```

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: 4-6,12-14]

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('ABCDEFG10234');
}
```

---

[.footer: Reducing _unnecessary_ details]

## The **value** of the string **matters**

```php
/** @test */
public function canBeConstructedWithAValidSerialNumber(): void
{
    $serialNumber = SerialNumber::fromString('ABCDEFG10234');

    $this->assertEquals('ABCDEFG10234', (string) $serialNumber);
}

/** @test */
public function throwsAnExceptionIfSerialNumberIsInvalid(): void
{
    $this->expectException(InvalidArgumentException::class);

    SerialNumber::fromString('9999999999');
}
```

---

[.footer: Reducing _unnecessary_ details]

## Make it **obvious** what's <br> relevant and what isn't

---

[.footer: Reducing _unnecessary_ details]
[.code-highlight: all]
[.code-highlight: 7,9,14,16]

```php
/** @test */
public function creatingANewGame(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)->post(route('games.store'), [
        'name' => '::name::',
        'is_public' => 1,
        'description' => '::description::',
    ]);

    $this->assertDatabaseHas('games', [
        'user_id' => $user->id,
        'name' => '::name::',
        'is_public' => 1,
        'description' => '::description::',
    ]);
}
```

---

[.footer: Reducing _unnecessary_ details]

- `"::description::"` is a string that **represents** a description, but it's actual value doesn't matter.
- `"::name::"` is a string that **represents** a name, but it's actual value doesn't matter.

---

[.footer: Reducing _unnecessary_ details]

## Caveats

- Only works for strings
- Might not work for all string (e.g. emails)

---

## [fit] Testing **Validation**

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 5-8]

```php
class UsersController extends Controller
{
    public function store(Request $request): RedirectResponse
    {
        $this->validate([
            'name' => 'required',
            'email' => ['required', 'email', 'unique:users,email'],
            'password' => ['required', 'confirmed'],
        ]);

        // Good to go, create the user...
    }
}
```

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 5-7]
[.code-highlight: 10]

```php
/** @test */
public function name_is_required()
{
    $response = $this->post(route('users.store'), [
        'email' => 'foo@bar.com',
        'password' => '::password::',
        'password_confirmation' => '::password::',
    ]);

    $response->assertSessionHasErrors('name');
}
```

---

[.footer: _Testing Validation_]

```php
/** @test */
public function name_is_required() { /* ... */ }

/** @test */
public function email_is_required() { /* ... */ }

/** @test */
public function email_must_be_valid() { /* ... */ }

/** @test */
public function email_must_be_unique() { /* ... */ }

/** @test */
public function password_is_required() { /* ... */ }

/** @test */
public function password_has_to_be_confirmed() { /* ... */ }
```

---

[.footer: _Testing Validation_]

## **Data providers** to the rescue

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 3]
[.code-highlight: 5]
[.code-highlight: 7-9]

```php
/**
 * @test
 * @dataProvider validationProvider
 */
public function validationTests(array $payload, string $key)
{
    $response = $this->post(route('users.store'), $payload));

    $response->assertSessionHasErrors($key);
}
```

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 1]
[.code-highlight: 3-8]
[.code-highlight: 10-15]
[.code-highlight: 11-14]
[.code-highlight: 12]
[.code-highlight: 13]
[.code-highlight: all]

```php
public function validationProvider(): Generator
{
    $defaultPayload = [
        'name' => '::name::',
        'email' => 'valid@example.com',
        'password' => '::password::',
        'password_confirmation' => '::password::',
    ];

    yield from [
        'missing name' => [
            'payload' => Arr::except($defaultPayload, 'name'),
            'key' => 'name',
        ],
    ];
}
```

---

[.footer: _Testing Validation_]

![](./validation-output-1.png)

---

[.footer: _Testing Validation_]

> “Ok, missing fields are easy. What about **incorrect values**?”
> — You, probably

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 4,5]

```php
$defaultPayload = [
    'name' => '::name::',
    'email' => 'valid@example.com',
    'password' => '::password::',
    'password_confirmation' => '::password::',
];
```

---

[.footer: _Testing Validation_]
[.code-highlight: 4-5]

```php
$defaultPayload = [
    'name' => '::name::',
    'email' => 'valid@example.com',
    'password' => '::password::',
    'password_confirmation' => '::incorrect-confirmation::',
];
```

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 8-10]
[.code-highlight: 12-17]
[.code-highlight: all]

```php
$defaultPayload = [
    'name' => '::name::',
    'email' => 'valid@example.com',
    'password' => '::password::',
    'password_confirmation' => '::password::',
];

array_merge($defaultPayload, [
    'password_confirmation' => '::incorrect-confirmation::',
]),

// => [
//     'name' => '::name::',
//     'email' => 'valid@example.com',
//     'password' => '::password::',
//     'password_confirmation' => '::incorrect-confirmation::',
// ]

```

---

[.footer: _Testing Validation_]
[.code-highlight: 4-9]
[.code-highlight: 5-7]

```php
yield from [
    // Other data sets...

    'password not confirmed' => [
        'payload' => array_merge($defaultPayload, [
            'password_confirmation' => '::incorrect-confirmation::',
        ]),
        'key' => 'password',
    ],
]
```

---

[.footer: _Testing Validation_]

![](./validation-output-2.png)

---

[.footer: _Testing Validation_]

> “What about **unique emails**?”
> — You, again

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 4-6]
[.code-highlight: 10-11]
[.code-highlight: all]

```php
/** @test */
public function email_must_be_unique()
{
    User::factory()->create([
        'email' => 'test@example.com',
    ]);

    $response = $this->post(route('users.store'), [
        'name' => '::name::',
        // Same as the user above
        'email' => 'test@example.com',
        'password' => '::password::',
        'password_confirmation' => '::password::',
    ]);

    $response->assertSessionHasErrors('email');
}
```

---

[.footer: _Testing Validation_]

## We a need a **setup** step

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 5]
[.code-highlight: 7-9]
[.code-highlight: all]

```php
/**
 * @test
 * @dataProvider validationProvider
 */
public function validationTests(array $payload, string $key, callable $setup = null)
{
    if ($setup !== null) {
        $setup();
    }

    $response = $this->post(route('users.store'), $payload));

    $response->assertSessionHasErrors($key);
}
```

---

[.footer: _Testing Validation_]
[.code-highlight: all]
[.code-highlight: 8-17]
[.code-highlight: 9-10]
[.code-highlight: 11-15]
[.code-highlight: 12-14]

```php
public function validationProvider(): Generator
{
    $defaultPayload = [/* ... */];

    yield from [
        // other data sets here...

        'email already exists' => [
            'payload' => $defaultPayload,
            'key' => 'email',
            'setup' => function () use ($defaultPayload) {
                User::factory()->create([
                    'email' => $defaultPayload['email']
                ]);
            },
        ]
    ]
}
```

---

[.footer: _Testing Validation_]

![](./validation-output-3.png)

---

## [fit] Testing **Middleware**

---

[.footer: _Testing Middleware_]

```php
class AdminMiddleware
{
    public function handle(Request $request, Closure $next)
    {
        // Fancy PHP 8 nullsafe operator
        if (! $request->user()?->is_admin) {
            abort(403);
        }

        return $next($request);
    }
}
```

---

[.footer: _Testing Middleware_]

> “Why not just use a **real route**?”
> — Bob, the hypothetical listener

---

[.footer: _Testing Middleware_]

## The **problem** with using a **real route**

---

[.footer: _Testing Middleware_]
[.code-highlight: all]
[.code-highlight: 5]

### The **problem** with using a **real route**

```php
// lots of other routes...

Route::post('/admin/users', [Admin\UsersController::class, 'store'])
    ->name('admin.users.store')
    ->middleware('admin');

// lots of other routes...
```

---

[.footer: _Testing Middleware_]
[.code-highlight: all]
[.code-highlight: 4]
[.code-highlight: 6-7]
[.code-highlight: 9]
[.code-highlight: all]

### The **problem** with using a **real route**

```php
/** @test */
public function onlyAdminsCanCreateUsers()
{
    $nonAdminUser = User::factory()->create();

    $response = $this->actingAs($nonAdminUser)
        ->post(route('admin.users.store'), […]);

    $response->assertForbidden();
}
```

---

[.footer: _Testing Middleware_]

## This **works**!

---

[.footer: _Testing Middleware_]

## But...

---

[.footer: _Testing Middleware_]

## What are we **actually testing**?

---

[.footer: _Testing Middleware_]

```php
/** @test */
public function onlyAdminsCanCreateUsers()
{
    $nonAdminUser = User::factory()->create();

    $response = $this->actingAs($nonAdminUser)
        ->post(route('admin.users.store'), […]);

    $response->assertForbidden();
}
```

---

[.footer: _Testing Middleware_]

```php
Route::post('/admin/users', [Admin\UsersController::class, 'store'])
    ->name('admin.users.store')
    ->middleware('admin');
```

---

[.footer: _Testing Middleware_]
[.code-highlight: all]
[.code-highlight: 1]
[.code-highlight: all]

```php
Route::middleware(['admin'])->group(function () {
    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');
});
```

---

[.footer: _Testing Middleware_]

```php
Route::middleware(['admin'])->group(function () {
    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');

    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');
});
```

---

[.footer: _Testing Middleware_]

```php
Route::middleware(['admin'])->group(function () {
    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');

    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');

    Route::get('/admin/users', [Admin\UsersController::class, 'index'])
        ->name('admin.users.index');

    Route::get('/admin/users/create', [Admin\UsersController::class, 'create'])
        ->name('admin.users.create');

    Route::post('/admin/users', [Admin\UsersController::class, 'store'])
        ->name('admin.users.store');

    Route::get('/admin/users/{user}/edit', [Admin\UsersController::class, 'edit'])
        ->name('admin.users.edit');

    Route::put('/admin/users/{user}', [Admin\UsersController::class, 'update'])
        ->name('admin.users.update');

    Route::delete('/admin/users/{user}', [Admin\UsersController::class, 'destroy'])
        ->name('admin.users.destroy');
});
```

---

[.footer: _Testing Middleware_]

## Now **what**?

- Should we include that same test for every single route?
  - If yes, what if the middleware **needs to change**?
- If not, do we use one of those routes as a “proof of concept”?
  - If so, **which one**?
  - Why this route?
  - What does this **communicate**?

---

[.footer: _Testing Middleware_]

## What I **want to do**

- Test the middleware at the **HTTP level**…
- …but don't use an actual **application route**

---

[.footer: _Testing Middleware_]
[.code-highlight: 3-10]
[.code-highlight: 7-9]

```php
class AdminMiddlewareTest extends TestCase
{
    public function setUp()
    {
        parent::setUp();

        Route::get('/middleware-test', function () {
            return 'noice';
        })->middleware('admin');
    }
}
```

---

[.footer: _Testing Middleware_]
[.code-highlight: 3-11]
[.code-highlight: 6]
[.code-highlight: 8-10]

```php
public function setUp() {…}

/** @test */
public function adminsCanAccessRoutes()
{
    $admin = User::factory()->admin()->create();

    $this->actingAs($admin)
        ->get('/middleware-test')
        ->assertOk();
}

/** @test */
public function nonAdminsCannotAccessRoute() {…}
```

---

[.footer: _Testing Middleware_]
[.code-highlight: 6-14]
[.code-highlight: 9]
[.code-highlight: 11-13]

```php
public function setUp() {…}

/** @test */
public function adminsCanAccessRoute() {…}

/** @test */
public function nonAdminsCannotAccessRoute()
{
    $nonAdmin = User::factory()->create();

    $this->actingAs($nonAdmin)
        ->get('/middleware-test')
        ->assertForbidden();
}
```

---

## Now that we know the **middleware works**…

---

## …we can simply check if the route **uses it**

---

[.footer: _Testing Middleware_]

## `assertRouteUsesMiddleware`[^2]

```php
/** @test */
public function onlyAdminsCanAccessRoute()
{
    $this->assertRouteUsesMiddleware('admin.users.store', ['admin']);
}
```

[^2]: https://github.com/jasonmccreary/laravel-test-assertions

---

```php
/**
 * @test
 * @dataProvider protectedRoutesProvider
 */
public function onlyAdminsCanAccessRoute(string $route)
{
    $this->assertRouteUsesMiddleware($route, ['admin']);
}

public function protectedRoutesProvider(): Generator
{
    yield from [
        'list users' => ['admin.users.index'],
        'view user create form' => ['admin.users.create']
        'store user' => ['admin.users.store'],
        'view user edit form' => ['admin.users.edit'],
        'update user' => ['admin.users.update'],
        'delete user' => ['admin.users.destroy'],
    ];
}
```

---

[.footer: _Testing Middleware_]

```php
public function setUp()
{
    parent::setUp();

    $this->withoutMiddleware(AdminMiddleware::class);
}
```

---

[.footer: _Testing Middleware_]

## Why **deactivate** the middleware?

---

[.footer: _Testing Middleware_]
[.code-highlight: all]
[.code-highlight: 5-6]
[.code-highlight: 8-10]
[.code-highlight: 12]

```php
class ValidSubscription
{
    public function handle(Request $request, Closure $next)
    {
        /** @var User $user */
        $user = $request->route('user');

        if (!$user->onTrial() && !$user->subscribed('default')) {
            abort(404);
        }

        return $next($request);
    }
}
```

---

[.footer: _Testing Middleware_]
[.code-highlight: all]
[.code-highlight: 4]
[.code-highlight: 5-11]
[.code-highlight: 13]
[.code-highlight: all]

```php
/** @test */
public function allowsRequestsIfValidSubscriptionExistsForTheMenu()
{
    $user = User::factory()->create();
    Subscription::create([
        'user_id' => $user->id,
        'stripe_id' => '::stripe-id::',
        'stripe_status' => 'active',
        'name' => 'default',
        'ends_at' => null,
    ]);

    $this->get($user->get . '/test')->assertOk();
}
```

---

[.footer: _Testing Middleware_]

- It **detracts** from the thing we're actually testing (the route)
- It doesn't add any **additional coverage**

---

[.footer: _Testing Middleware_]

## Summary

- Test middleware separately by defining a **dummy route** in the test's `setUp` method
- Check that your route **uses the middleware** with `assertRouteUsesMiddleware`
- [Optional] **Deactivate** the middleware in your route tests

---

[.footer: Better Testing in Laravel]

## [fit] **Thank You** For Listening

---

# [fit] **Bye**
