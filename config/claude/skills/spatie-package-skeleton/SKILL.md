---
name: spatie-package-skeleton
description: Guide for creating PHP and Laravel packages using Spatie's package-skeleton-laravel and package-skeleton-php templates. Use when the user wants to create a new PHP or Laravel package, scaffold a package. Also use when building customizable packages — covers proven patterns for extensibility (events, configurable models/jobs, action classes) instead of config option creep.

metadata:
  author: Spatie
  tags:
    - php
    - laravel
    - package
    - skeleton
    - open-source
---

# Creating PHP & Laravel Packages with Spatie Skeletons

## Choosing the Right Skeleton

| Use case | Skeleton | Repo |
|----------|----------|------|
| Laravel-specific package (service providers, facades, config, migrations, commands, views) | `package-skeleton-laravel` | `spatie/package-skeleton-laravel` |
| Framework-agnostic PHP package (no Laravel dependency) | `package-skeleton-php` | `spatie/package-skeleton-php` |

**Rule of thumb:** If your package needs Laravel's service container, config files, Blade views, migrations, or Artisan commands, use the Laravel skeleton. Otherwise, use the PHP skeleton.

## Step 1: Create the Repository

Use GitHub's template feature via `gh`:

```bash
# For a Laravel package:
gh repo create vendor/package-name --template spatie/package-skeleton-laravel --public --clone

# For a PHP package:
gh repo create vendor/package-name --template spatie/package-skeleton-php --public --clone
```

Then `cd` into the new directory.

## Step 2: Run the Configure Script

```bash
php ./configure.php
```

This interactive script replaces all placeholder values throughout the skeleton files. It will auto-detect sensible defaults from your git config and GitHub CLI.

### Laravel Skeleton Prompts

| Prompt | Default | Notes |
|--------|---------|-------|
| Author name | `git config user.name` | |
| Author email | `git config user.email` | |
| Author username | Guessed from git history, `gh auth status`, or remote URL | |
| Vendor name | Guessed from GitHub org API | |
| Vendor slug | Lowercase vendor name | Used in Composer name |
| Vendor namespace | PascalCase vendor | PHP namespace |
| Package name | Current folder name | |
| Class name | TitleCase of package name | Main class name |
| Package description | | One-liner for composer.json |
| Enable PhpStan? | Yes | Adds Larastan + PHPStan workflow |
| Enable Laravel Pint? | Yes | Code style fixer |
| Enable Dependabot? | Yes | Automated dependency updates |
| Use Ray for debugging? | Yes | Adds `spatie/laravel-ray` as dev dep |
| Use changelog updater workflow? | Yes | Auto-updates CHANGELOG.md on release |

### PHP Skeleton Prompts

| Prompt | Default | Notes |
|--------|---------|-------|
| Author name | `git config user.name` | |
| Author email | `git config user.email` | |
| Author username | Guessed from remote URL | |
| Vendor name | | |
| Vendor namespace | PascalCase vendor | |
| Package name | Current folder name | |
| Class name | TitleCase of package name | |
| Package description | | |
| Testing library | pest / phpunit | Choose one |
| Code style library | pint / php-cs-fixer | Choose one |

### What the Configure Script Does

1. **String replacement** across all files — replaces placeholders like `:vendor_name`, `:package_name`, `Skeleton`, `VendorName`, etc.
2. **Renames files** — e.g. `src/Skeleton.php` becomes `src/YourClassName.php`
3. **Removes unused files** based on your choices (e.g. PHPStan files if declined)
4. **Deletes itself** (`configure.php`) and the setup section from `README.md`

## Step 3: Install Dependencies

```bash
composer install
```

## Post-Setup: Directory Structure

### Laravel Package

```
src/
  YourClass.php                    # Main package class
  YourClassServiceProvider.php     # Service provider (uses spatie/laravel-package-tools)
  Facades/YourClass.php            # Facade
  Commands/YourClassCommand.php    # Artisan command stub
config/
  your-package.php                 # Published config file
database/
  factories/ModelFactory.php       # Factory template (commented out)
  migrations/create_table.php.stub # Migration stub
resources/views/                   # Blade views
tests/
  TestCase.php                     # Extends Orchestra\Testbench\TestCase
  ArchTest.php                     # Architecture tests (no dd/dump/ray)
  ExampleTest.php                  # Starter test
  Pest.php                         # Pest config binding TestCase
```

### PHP Package

```
src/
  YourClassClass.php    # Main class (note: the skeleton appends "Class" to the name)
tests/
  ArchTest.php          # Architecture tests (Pest only)
  ExampleTest.php       # Starter test
  Pest.php              # Pest config (if Pest chosen)
```

## Key Files to Edit First

### Laravel Package

**1. Service Provider** (`src/YourClassServiceProvider.php`):
Uses `spatie/laravel-package-tools`. Configure what your package provides:

```php
use Spatie\LaravelPackageTools\Package;
use Spatie\LaravelPackageTools\PackageServiceProvider;

class YourClassServiceProvider extends PackageServiceProvider
{
    public function configurePackage(Package $package): void
    {
        $package
            ->name('your-package')
            ->hasConfigFile()
            ->hasViews()
            ->hasMigration('create_your_package_table')
            ->hasCommand(YourClassCommand::class);
    }
}
```

Remove any methods you don't need (e.g. `->hasViews()` if you have no views, `->hasCommand()` if no commands).

**2. Main class** (`src/YourClass.php`): Implement your package's core logic.

**3. Config file** (`config/your-package.php`): Add your configuration options.

**4. Migration stub** (`database/migrations/`): Define your table schema or delete if not needed.

**5. Command** (`src/Commands/YourClassCommand.php`): Implement or delete if not needed.

**6. Facade** (`src/Facades/YourClass.php`): Already wired up. Only modify if the facade should resolve a different class.

### PHP Package

**1. Main class** (`src/YourClassClass.php`): Rename and implement. Consider renaming to drop the "Class" suffix if it reads awkwardly.

## Delete What You Don't Need

The skeletons include everything. After running configure, delete files you won't use:

- **No database?** Delete `database/` directory and remove `->hasMigration()` from the service provider
- **No commands?** Delete `src/Commands/` and remove `->hasCommand()` from the service provider
- **No views?** Delete `resources/views/` and remove `->hasViews()` from the service provider
- **No facade?** Delete `src/Facades/` and remove the facade alias from `composer.json`'s `extra.laravel.aliases`
- **No config?** Delete `config/` and remove `->hasConfigFile()` from the service provider

## Testing

```bash
# Run tests
composer test

# Run code style fixer
composer format

# Run static analysis (Laravel skeleton with PhpStan enabled)
composer analyse
```

The Laravel skeleton uses **Pest** with **Orchestra Testbench** for testing in a simulated Laravel environment. The `TestCase.php` base class:
- Registers your service provider
- Sets up an in-memory SQLite database
- Configures factory discovery

The PHP skeleton uses your chosen test framework (Pest or PHPUnit) directly.

## GitHub Actions (CI)

Both skeletons include workflows for:
- **Running tests** — matrix of PHP versions, dependency strategies (lowest/stable), and OS (ubuntu/windows)
- **Code style fixing** — auto-commits fixes via Pint or CS Fixer
- **Dependabot auto-merge** — auto-merges minor/patch dependency updates

The Laravel skeleton additionally includes:
- **PHPStan** workflow (if enabled)
- **Changelog updater** (if enabled) — auto-updates CHANGELOG.md when a GitHub release is created

## composer.json Essentials

After setup, verify these fields in `composer.json`:
- `name`: `vendor/package-name`
- `description`: Clear one-liner
- `require.php`: Minimum PHP version (default `^8.4`)
- `require.illuminate/contracts`: Laravel version constraints (default `^11.0|^12.0`)
- `extra.laravel.providers`: Auto-discovery of your service provider
- `extra.laravel.aliases`: Auto-discovery of your facade

## README

The skeleton provides a good README template. Update these sections:
- Installation instructions (already templated)
- Usage examples with actual code
- API documentation
- Testing instructions

## Common Patterns

### Registering Bindings in Service Provider

For complex packages, override `packageRegistered()` or `packageBooted()`:

```php
public function packageRegistered(): void
{
    $this->app->singleton(YourClass::class, function () {
        return new YourClass(config('your-package'));
    });
}
```

### Publishing Assets

`spatie/laravel-package-tools` handles publishing. Users install with:

```bash
# Publish config
php artisan vendor:publish --tag="your-package-config"

# Publish migrations
php artisan vendor:publish --tag="your-package-migrations"

# Publish views
php artisan vendor:publish --tag="your-package-views"
```

### Adding More Commands

Register additional commands in the service provider:

```php
$package
    ->hasCommands([
        FirstCommand::class,
        SecondCommand::class,
    ]);
```

### Adding Routes

```php
$package->hasRoute('web');
// Loads routes/web.php from your package
```

### Adding Installable Command

```php
use Spatie\LaravelPackageTools\Commands\InstallCommand;

$package->hasInstallCommand(function (InstallCommand $command) {
    $command
        ->publishConfigFile()
        ->publishMigrations()
        ->askToRunMigrations()
        ->askToStarRepoOnGitHub('vendor/package-name');
});
```

## Proven Package Patterns: Customizability

These patterns apply to both packages and projects. The core principle: **avoid adding tiny config options**. Instead, give users full control by letting them extend classes and override behavior.

### Anti-Pattern: Config Option Creep

Don't keep adding small config options for every customization request:

```php
// DON'T: This escalates quickly and becomes unmanageable
return [
    'log_transformers' => [
        'enabled' => true,
        'start_message' => 'Transforming starting',
        'end_message' => 'Transformer ended',
        'log_level' => 'info',
        'log_channel' => 'transformer',
    ],
    'model_connection_name' => 'default',
    'model_table_name' => 'my_table',
    'enable_soft_deletes' => true,
    'job_queue_name' => 'default',
    'extra_http_headers' => [],
];
```

Problems: difficult to manage, confusing for users, not fun to maintain or review.

### Pattern 1: Use Events Instead of Logging/Hook Config Options

When users want to add behavior at certain points (logging, notifications, etc.), fire events and let them listen:

```php
// In your package code:
event(new TransformerStarting($transformer, $url));

$transformer->transform();

event(new TransformerEnded($transformer, $url, $result));
```

Users can listen to these events to add any behavior they want:

```php
Event::listen(function (TransformerStarting $event) {
    Log::info("Transformer {$event->transformer->type()} starting…");
});
```

This replaces any need for config options around logging, notifications, or side effects.

### Pattern 2: Register Models in the Config File

Instead of adding config options for connection name, table name, soft deletes, etc., let users specify their own model class:

```php
// In config file:
return [
    /*
     * The model that stores the transformation results.
     *
     * You can use your own model by extending the default model.
     */
    'model' => Spatie\AiTransformer\Models\TransformationResult::class,
];
```

Users extend the model to customize anything:

```php
class CustomTransformationResult extends TransformationResult
{
    use SoftDeletes;

    protected $connection = 'custom_connection';
}
```

And specify it in config:

```php
'model' => App\Models\CustomTransformationResult::class,
```

**Important:** In your package code, always resolve the model from config rather than referencing the class directly:

```php
// DON'T:
TransformationResult::find($id);

// DO:
$model = config('your-package.model');
$model::find($id);
```

### Pattern 3: Register Jobs in the Config File

Instead of adding config options for queue name, timeout, encryption, etc., let users specify their own job class:

```php
// In config file:
return [
    /*
     * The job in which a transformation will be processed.
     *
     * You can extend the default job and specify your own job
     * here to customize the package's behavior.
     */
    'process_transformer_job' => Spatie\AiTransformer\Jobs\ProcessTransformerJob::class,
];
```

Users extend the job to customize queue behavior:

```php
class CustomJob extends ProcessTransformerJob implements ShouldBeEncrypted
{
    public $queue = 'my-custom-queue';

    public int $timeout = 120;
}
```

### Pattern 4: Use Action Classes for Overridable Behavior

Wrap small pieces of functionality in action classes — single-purpose classes with one method (usually `execute`):

```php
class FetchUrlContentAction
{
    public function execute(string $url): string
    {
        return Http::get($url)->body();
    }
}
```

Register all action classes in the config file:

```php
return [
    /*
     * The actions that will perform low-level operations of the package.
     *
     * You can extend the default actions and specify your own actions here
     * to customize the package's behavior.
     */
    'actions' => [
        'fetch_url_content' => Spatie\UrlAiTransformer\Actions\FetchUrlContentAction::class,
        // other actions...
    ],
];
```

Users override by extending and registering their custom action:

```php
class CustomFetchUrlContentAction extends FetchUrlContentAction
{
    public function execute(string $url): string
    {
        return Http::withHeaders(['extra' => 'custom_value'])
            ->get($url)
            ->body();
    }
}
```

Inside the package, resolve actions from config:

```php
$fetchUrlContentActionClass = config('your-package.actions.fetch_url_content');
$fetchUrlContentAction = new $fetchUrlContentActionClass;
$fetchUrlContentAction->execute('https://example.com');
```

Consider adding a Config helper class to simplify this:

```php
Config::getActionClass('fetch_url_content')->execute($url);
```

