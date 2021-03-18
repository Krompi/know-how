<!-- # Laravel-Vorlage. 

## erstmalige einrichtung

    composer create-project --prefer-dist laravel/laravel ./



## Entwicklungsumgebung einrichten

Im Docker-Image sollte schon alles passen.
In der Entwicklungsumgebung sind folgende Schritte zu erledigen:

1. git clone https://github.com/Krompi/krompis-comics.git
2. composer update
3. php artisan key:generate -->


# Checkliste testgetriebene Entwicklung

## Anlegen von Model inklusive Controller, Migration, Factory und Seeder

    php artisan make:model -a <modelName>

## Migration-Skript

Im Vorfeld sollten sich schon Gedanken über das Datenmodell gemacht werden!

Beispiel;

    public function up()
    {
        Schema::create('publishers', function (Blueprint $table) {
            $table->id();
            $table->char('name', 100)->unique();
            $table->char('url', 100)->nullable();
            $table->text('description')->nullable();
            $table->timestamps();
            $table->softDeletes('deleted_at', 0);
        });
    }

Anschließend ausführen

    php artisan migrate

https://laravel.com/docs/8.x/migrations

## Erstellen von Factory

Beispiel:

    namespace Database\Factories;

    use Illuminate\Database\Eloquent\Factories\Factory;
    use App\Models\Publisher;

    class PublisherFactory extends Factory
    {
        protected $model = Publisher::class;

        public function definition()
        {
            return [
                'name' => $this->faker->text(20),
                'url' => $this->faker->url,
                'description'  => $this->faker->text(200)
            ];
        }
    }

https://laravel.com/docs/8.x/database-testing#defining-model-factories

## Erstellen des Seeders

Beispiel

    namespace Database\Seeders;

    use Illuminate\Database\Seeder;
    use App\Models\Publisher;

    class PublisherSeeder extends Seeder
    {
        public function run()
        {
            Publisher::factory()
                ->times(10)
                ->create();
        }
    }

https://laravel.com/docs/8.x/seeding

## Ergänzen des DatabaseSeeder

Beispiel:

    namespace Database\Seeders;

    use Illuminate\Database\Seeder;

    class DatabaseSeeder extends Seeder
    {
        /**
        * Seed the application's database.
        *
        * @return void
        */
        public function run()
        {
            // \App\Models\User::factory(10)->create();
            $this->call([
                PublisherSeeder::class,
            ]);
        }
    }

## Ausführen des Seeders

um die Datenbank mit Testdaten zu füllen wird der DatabaseSeeder ausgeführt:

    php artisan db:seed

Soll nur ein bestimmter Seeder ausgeführt werden:

    php artisan db:seed --class=UserSeeder

## Feature-Tests

Anlegen des Tests

    php artisan make:test <modelName>Test

Danach werden die Testfälle erstellt und anschließend läßt man den Test durchlaufen:

    php artisan test
