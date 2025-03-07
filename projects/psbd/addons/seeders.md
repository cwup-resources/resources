# **Seedery w systemach baz danych**
Seedery to skrypty, które wypełniają bazę danych przykładowymi danymi. Są przydatne podczas tworzenia aplikacji, testowania i wypełniania bazy danych w środowisku deweloperskim.
Factory to narzędzie, które pomaga w tworzeniu obiektów modeli w aplikacji. Aby generować przykładowe dane, factory korzysta z Faker'a, który generuje losowe dane w zdefiniowany sposób z róznych kategorii (adres, dane bankowe, dane osobowe itp.) dla różnych języków i lokalizacji.

## **Laravel: Factory + Seeder + Faker**

Laravel ma wbudowaną obsługę factory, która świetnie współpracuje z Fakerem.

### **1. Tworzenie Factory**
```bash
php artisan make:factory UserFactory --model=User
```

### **2. Edycja pliku `database/factories/UserFactory.php`**
```php
<?php

namespace Database\Factories;

use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;
use Illuminate\Support\Str;
use Illuminate\Support\Facades\Hash;

class UserFactory extends Factory
{
    protected $model = User::class;

    public function definition()
    {
        return [
            'name' => $this->faker->name,
            'email' => $this->faker->unique()->safeEmail,
            'email_verified_at' => now(),
            'password' => Hash::make('password'),
            'remember_token' => Str::random(10),
        ];
    }
}
```

### **3. Edycja `UserSeeder.php`**
Teraz możemy użyć factory w seederze:

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\User;

class UserSeeder extends Seeder
{
    public function run()
    {
        User::factory(10)->create();
        User::factory()->create(['email' => 'specific_email@example.com']);
    }
}
```

### **4. Uruchamianie seederów**
```bash
php artisan db:seed --class=UserSeeder
```
Jeżeli uruchomimy seeder bez podania klasy, zostanie uruchomiony domyślny seeder `DatabaseSeeder`.

Jeśli chcemy odświeżyć bazę i wypełnić ją przykładowymi danymi, możemy użyć:
```bash
php artisan migrate:fresh --seed
```

---

## **Symfony/Slim: Factory + Fixtures + Faker**

Symfony nie ma wbudowanego systemu factory jak Laravel, ale można użyć **zenstruck/foundry**.

### **1. Instalacja paczki**
```bash
composer require --dev zenstruck/foundry
```

### **2. Generowanie factory**
```bash
php bin/console make:factory UserFactory
```

### **3. Edycja `src/Factory/UserFactory.php`**
```php
<?php

namespace App\Factory;

use App\Entity\User;
use Zenstruck\Foundry\ModelFactory;
use Zenstruck\Foundry\Proxy;

final class UserFactory extends ModelFactory
{
    protected function getDefaults(): array
    {
        return [
            'name' => self::faker()->name(),
            'email' => self::faker()->unique()->safeEmail(),
            'password' => password_hash('password', PASSWORD_BCRYPT),
        ];
    }

    protected static function getClass(): string
    {
        return User::class;
    }
}
```

### **4. Edycja `UserFixtures.php`**
Teraz używamy factory w seederze:

```php
<?php

namespace App\DataFixtures;

use App\Factory\UserFactory;
use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;

class UserFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        UserFactory::createMany(10);
    }
}
```

### **5. Uruchamianie seederów**
```bash
php bin/console doctrine:fixtures:load
```

Teraz baza zostanie wypełniona przykładowymi danymi w sposób bardziej elastyczny dzięki factory.
