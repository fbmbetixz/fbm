Dokumentasi NextLevelContainer: Kontainer IoC/DI Tingkat Lanjut untuk PHP
NextLevelContainer adalah implementasi Kontainer Inversi Kontrol (IoC) dan Penyuntikan Dependensi (DI) yang sangat canggih dan berkinerja tinggi untuk PHP. Dirancang dengan modularitas dan ekstensibilitas sebagai inti, kontainer ini menyediakan fondasi yang kuat untuk membangun aplikasi PHP yang kompleks, mudah diuji, dan mudah dipelihara.
Dengan fitur-fitur seperti autowiring berbasis atribut, manajemen siklus hidup, monitoring, snapshotting, manajemen rahasia, multi-tenancy, dan kebijakan keamanan yang dapat dikonfigurasi, NextLevelContainer memberdayakan pengembang untuk mengelola dependensi aplikasi dengan presisi dan kontrol yang belum pernah ada sebelumnya.
Fitur Utama
 * Autowiring Cerdas: Secara otomatis menyelesaikan dependensi melalui type hinting dan atribut.
 * Injeksi Berbasis Atribut (PHP 8+): Gunakan atribut #[Inject], #[InjectTag], #[PropertyInject], dan #[SetterInject] untuk deklarasi injeksi yang eksplisit dan bersih.
 * Manajemen Siklus Hidup: Dukungan untuk singletons, scoped instances, dan objek Disposable untuk pembersihan sumber daya yang tepat.
 * Monitoring & Diagnostik: Lacak metrik resolusi layanan, periksa kesehatan layanan, dan dapatkan log diagnostik.
 * Snapshotting Kontainer: Ekspor dan impor status kontainer, memungkinkan persistensi atau replikasi kontainer.
 * Manajemen Rahasia: Ikatan dan resolusi rahasia yang aman melalui provider, menjaga kredensial sensitif di luar kode inti.
 * Multi-Tenancy: Kemampuan untuk mengelola kontainer yang terisolasi untuk setiap tenant atau konteks aplikasi.
 * Ekstensi Kontainer: Perluas fungsionalitas kontainer dengan hook beforeResolve dan afterResolve kustom.
 * Aturan Arsitektur: Terapkan aturan yang dapat disesuaikan untuk memvalidasi struktur binding kontainer, mendorong praktik terbaik.
 * Kebijakan Keamanan: Kontrol namespace tepercaya dan izin closure anonim untuk mencegah injeksi kode yang tidak diinginkan.
 * Tipe Ketat & Kedaluwarsa: Terapkan validasi tipe yang ketat pada instance yang diselesaikan dan kelola masa pakai objek dengan konfigurasi kedaluwarsa.
 * Delegasi Kontainer: Dukungan untuk hierarki kontainer induk-anak dengan kebijakan delegasi yang dapat dikonfigurasi.
 * Generasi Dokumentasi: Hasilkan dokumentasi binding kontainer secara otomatis.
Instalasi (Konseptual)
Meskipun ini adalah implementasi mandiri, dalam proyek nyata Anda akan menggunakan Composer untuk mengelola autoloading kelas.
 * Struktur Direktori: Pastikan struktur direktori Anda mengikuti standar PSR-4. Semua antarmuka terletak di folder Interfaces/.
   your-project/
└── src/
    └── Core/
        └── Container/
            ├── Attributes/
            │   ├── Inject.php
            │   └── ...
            ├── Exceptions/
            │   ├── ContainerException.php
            │   └── ...
            ├── Interfaces/
            │   ├── BindingRegistry.php
            │   └── ...
            ├── Traits/
            │   ├── BindingApiTrait.php
            │   └── ...
            ├── AllowAllPolicy.php
            ├── ConfigValidator.php
            ├── ContainerBuilder.php
            ├── ContainerFactory.php
            ├── DefaultContextManager.php
            ├── DefaultDocGenerator.php
            ├── DefaultEventDispatcher.php
            ├── DefaultMonitoring.php
            ├── DefaultSecretManager.php
            ├── DefaultSnapshotManager.php
            ├── InMemoryBindingRegistry.php
            ├── NextLevelContainer.php
            ├── RuleValidator.php
            └── SecurityPolicy.php

 * Konfigurasi Composer: Tambahkan namespace ke composer.json Anda:
   {
    "autoload": {
        "psr-4": {
            "Core\\Container\\": "src/Core/Container/"
        }
    }
}

 * Jalankan Composer: Setelah memperbarui composer.json, jalankan perintah ini di terminal Anda:
   composer dump-autoload

Konsep Inti
Binding Layanan
Binding adalah cara Anda memberi tahu kontainer bagaimana membuat instance layanan.
bind(string $id, callable|string|Factory $factory, array $options = [])
Mendaftarkan layanan dengan ID unik.
 * $id: Pengenal unik untuk layanan (misalnya, 'app.logger', 'UserRepository').
 * $factory:
   * callable: Sebuah closure atau callable lain yang mengembalikan instance layanan. Kontainer akan meneruskan dirinya sendiri sebagai argumen pertama.
   * string: Nama kelas atau antarmuka yang akan di-autowire.
   * Core\Container\Interfaces\Factory: Sebuah objek yang mengimplementasikan __invoke().
 * $options: Array opsi, seperti 'scope'.
<!-- end list -->
<?php

namespace App\Examples;

use Core\Container\NextLevelContainer;
use Core\Container\InMemoryBindingRegistry;
use Core\Container\DefaultMonitoring;
use Core\Container\DefaultEventDispatcher;
use Core\Container\DefaultSnapshotManager;
use Core\Container\DefaultSecretManager;
use Core\Container\DefaultContextManager;
use Core\Container\DefaultDocGenerator;
use Psr\Log\NullLogger;

class MyService {
    public function doSomething(): string { return "Doing something!"; }
}

$container = new NextLevelContainer(
    new InMemoryBindingRegistry(),
    new DefaultMonitoring(),
    new DefaultEventDispatcher(),
    new DefaultSnapshotManager(),
    new DefaultSecretManager(),
    new DefaultContextManager(),
    new DefaultDocGenerator(),
    new NullLogger()
);

// Binding dasar dengan closure
$container->bind('my_service', function() {
    return new MyService();
});

// Binding dengan nama kelas (akan di-autowire)
$container->bind('another_service', MyService::class);

// Binding nilai langsung (akan diubah menjadi closure internal)
$container->bind('app.name', 'My Awesome App');

// Mengambil layanan
$service = $container->get('my_service');
echo $service->doSomething() . "\n"; // Output: Doing something!
echo $container->get('app.name') . "\n"; // Output: My Awesome App

singleton(string $id, callable|string|Factory $factory, array $options = [])
Mendaftarkan layanan sebagai singleton. Hanya satu instance layanan yang akan dibuat dan digunakan kembali di seluruh siklus hidup kontainer. Ini adalah shortcut untuk bind() dengan opsi ['scope' => 'singleton'].
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class DatabaseConnection {
    public string $id;
    public function __construct() {
        $this->id = uniqid('db_'); // Untuk menunjukkan instance yang sama
        echo "DatabaseConnection created: " . $this->id . "\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->singleton('db_connection', DatabaseConnection::class);

$db1 = $container->get('db_connection'); // Output: DatabaseConnection created: db_...
$db2 = $container->get('db_connection'); // Tidak ada output lagi

echo "DB1 ID: " . $db1->id . "\n"; // Output: DB1 ID: db_...
echo "DB2 ID: " . $db2->id . "\n"; // Output: DB2 ID: db_...
// db1 dan db2 akan memiliki ID yang sama

scope(string $id, callable|string|Factory $factory, string $scopeName = 'request', array $options = [])
Mendaftarkan layanan dengan cakupan kustom. Instance akan dibuat sekali per cakupan yang ditentukan. Cakupan default adalah 'request'.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class RequestService {
    public string $id;
    public function __construct() {
        $this->id = uniqid('req_');
        echo "RequestService created: " . $this->id . "\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->scope('request_service', RequestService::class, 'request');

// Dalam konteks 'request' pertama
$service1_req1 = $container->get('request_service'); // Output: RequestService created: req_...
$service2_req1 = $container->get('request_service'); // Tidak ada output lagi
echo "Service1 ID (Req1): " . $service1_req1->id . "\n";
echo "Service2 ID (Req1): " . $service2_req1->id . "\n";
// service1_req1 dan service2_req1 akan memiliki ID yang sama

// Reset cakupan 'request'
$container->resetScope('request');
echo "Scope 'request' reset.\n";

// Dalam konteks 'request' kedua
$service3_req2 = $container->get('request_service'); // Output: RequestService created: req_... (ID baru)
echo "Service3 ID (Req2): " . $service3_req2->id . "\n";
// service3_req2 akan memiliki ID yang berbeda dari sebelumnya

alias(string $id, string $target)
Membuat alias untuk layanan yang ada. Mengakses alias akan menyelesaikan layanan target.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class LoggerService {}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('app.logger', LoggerService::class);
$container->alias('logger', 'app.logger'); // Buat alias 'logger' untuk 'app.logger'

$loggerInstance = $container->get('logger');
var_dump($loggerInstance instanceof LoggerService); // Output: bool(true)

tag(string|string[] $ids, string $tag)
Menandai satu atau beberapa layanan dengan tag tertentu. Layanan yang ditandai dapat diselesaikan secara bersamaan berdasarkan tagnya.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

interface Plugin {}
class PluginA implements Plugin {}
class PluginB implements Plugin {}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('plugin.a', PluginA::class);
$container->bind('plugin.b', PluginB::class);
$container->bind('some.other.service', stdClass::class);

$container->tag(['plugin.a', 'plugin.b'], 'app.plugins'); // Tag kedua layanan dengan 'app.plugins'

echo "Resolving plugins by tag:\n";
foreach ($container->resolveByTag('app.plugins') as $plugin) {
    echo "  - " . get_class($plugin) . "\n";
}
// Output:
// Resolving plugins by tag:
//   - App\Examples\PluginA
//   - App\Examples\PluginB

Resolusi Layanan
get(string $id)
Mengambil instance layanan dari kontainer. Jika layanan adalah singleton atau scoped, instance yang sama akan dikembalikan jika sudah ada. Jika tidak, instance baru akan dibuat. Ini adalah metode yang paling umum digunakan untuk mengambil layanan.
make(string $id, array $args = [])
Membuat instance baru dari layanan, mengabaikan cakupan singleton/scoped dan memungkinkan argumen konstruktor/factory kustom. Argumen harus berupa array asosiatif (kunci-nilai).
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class ReportGenerator {
    public string $format;
    public function __construct(string $format = 'pdf') {
        $this->format = $format;
        echo "ReportGenerator created with format: " . $this->format . "\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('report.generator', ReportGenerator::class);

// Membuat instance baru dengan argumen kustom
$pdfReport = $container->make('report.generator', ['format' => 'pdf']); // Output: ReportGenerator created with format: pdf
$csvReport = $container->make('report.generator', ['format' => 'csv']); // Output: ReportGenerator created with format: csv

var_dump($pdfReport->format); // Output: string(3) "pdf"
var_dump($csvReport->format); // Output: string(3) "csv"

resolveByTag(string $tag)
Mengambil semua layanan yang terkait dengan tag tertentu sebagai Generator. Ini efisien untuk koleksi layanan yang besar.
// Lihat contoh di bagian `tag()` di atas.

Autowiring & Injeksi
NextLevelContainer secara otomatis menyelesaikan dependensi melalui type hinting dan mendukung injeksi berbasis atribut untuk kontrol yang lebih granular.
Injeksi Konstruktor (Type Hinting)
Jika sebuah kelas memiliki type hint pada parameter konstruktornya, kontainer akan mencoba menyelesaikan dependensi tersebut secara otomatis.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

interface MailerInterface {}
class SmtpMailer implements MailerInterface {}

class UserService {
    private MailerInterface $mailer;
    public function __construct(MailerInterface $mailer) { // Akan di-autowire
        $this->mailer = $mailer;
        echo "UserService created with " . get_class($mailer) . "\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind(MailerInterface::class, SmtpMailer::class); // Bind antarmuka ke implementasi
$container->bind(UserService::class, UserService::class);

$userService = $container->get(UserService::class);
// Output: UserService created with App\Examples\SmtpMailer

Injeksi Berbasis Atribut
#[Inject(string $id)] (Injeksi Parameter)
Menginjeksi layanan berdasarkan ID eksplisit pada parameter konstruktor atau metode.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Core\Container\Attributes\Inject;
use Psr\Log\NullLogger;

class Config { public string $dbHost = 'localhost'; }
class Database {
    public string $host;
    public function __construct(#[Inject('app.config')] Config $config) {
        $this->host = $config->dbHost;
        echo "Database created with host: " . $this->host . "\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('app.config', Config::class);
$container->bind(Database::class, Database::class);

$db = $container->get(Database::class); // Output: Database created with host: localhost

#[InjectTag(string $tag)] (Injeksi Koleksi Parameter)
Menginjeksi semua layanan dengan tag tertentu sebagai array atau iterable pada parameter konstruktor atau metode.
<?php

namespace App\Examples;

use Core\Container\ContainerBuilder;
use Core\Container\Attributes\InjectTag;
use Psr\Log\NullLogger;

interface Reporter {}
class PdfReporter implements Reporter {}
class CsvReporter implements Reporter {}

class ReportProcessor {
    public array $reporters;
    public function __construct(#[InjectTag('reporters')] iterable $reporters) {
        $this->reporters = iterator_to_array($reporters);
        echo "ReportProcessor created with " . count($this->reporters) . " reporters.\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('pdf_reporter', PdfReporter::class);
$container->bind('csv_reporter', CsvReporter::class);
$container->tag(['pdf_reporter', 'csv_reporter'], 'reporters');

$processor = $container->get(ReportProcessor::class);
// Output: ReportProcessor created with 2 reporters.
var_dump(get_class($processor->reporters[0])); // Output: string(24) "App\Examples\PdfReporter"

#[PropertyInject(string $id)] (Injeksi Properti)
Menginjeksi layanan ke properti publik setelah instance objek dibuat. Fitur ini harus diaktifkan dalam opsi binding ('property_injection' => true).
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\Attributes\PropertyInject;
use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class EventLogger {}

class AnalyticsService {
    #[PropertyInject('event.logger')]
    public EventLogger $logger; // Properti akan diinjeksi

    public function __construct() {
        echo "AnalyticsService constructed.\n";
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('event.logger', EventLogger::class);
$container->bind(AnalyticsService::class, AnalyticsService::class, ['options' => ['property_injection' => true]]);

$analytics = $container->get(AnalyticsService::class);
// Output: AnalyticsService constructed.
var_dump($analytics->logger instanceof EventLogger); // Output: bool(true)

#[SetterInject] (Injeksi Setter)
Memanggil metode publik (setter) setelah instance objek dibuat, dan menginjeksi dependensi ke parameter metode setter tersebut. Fitur ini harus diaktifkan dalam opsi binding ('setter_injection' => true).
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\Attributes\SetterInject;
use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;
use Psr\Log\LoggerInterface;

class AuditService {
    private ?LoggerInterface $logger = null;

    #[SetterInject]
    public function setLogger(LoggerInterface $logger) { // Metode ini akan dipanggil
        $this->logger = $logger;
        echo "Logger injected into AuditService: " . get_class($logger) . "\n";
    }

    public function getLogger(): ?LoggerInterface {
        return $this->logger;
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind(LoggerInterface::class, NullLogger::class);
$container->bind(AuditService::class, AuditService::class, ['options' => ['setter_injection' => true]]);

$audit = $container->get(AuditService::class);
// Output: Logger injected into AuditService: Psr\Log\NullLogger
var_dump($audit->getLogger() instanceof NullLogger); // Output: bool(true)

Manajemen Siklus Hidup
Disposable Interface
Layanan yang mengimplementasikan Core\Container\Interfaces\Disposable akan memiliki metode dispose() mereka dipanggil ketika kontainer dimatikan (shutdown()) atau ketika cakupan mereka direset (resetScope()). Ini penting untuk membersihkan sumber daya seperti koneksi database atau file handle.
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\Interfaces\Disposable;
use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;

class TempFileHandler implements Disposable {
    public string $filePath;
    public function __construct() {
        $this->filePath = sys_get_temp_dir() . '/' . uniqid('temp_');
        file_put_contents($this->filePath, 'Temporary data');
        echo "TempFileHandler created for: " . $this->filePath . "\n";
    }

    public function dispose(): void {
        if (file_exists($this->filePath)) {
            unlink($this->filePath);
            echo "TempFileHandler disposed, file deleted: " . $this->filePath . "\n";
        }
    }
}

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->singleton('file_handler', TempFileHandler::class);

$handler = $container->get('file_handler'); // Output: TempFileHandler created for: /tmp/temp_...

// Ketika aplikasi dimatikan
$container->shutdown(); // Output: TempFileHandler disposed, file deleted: /tmp/temp_...

resetScope(string $scope)
Mereset semua instance yang di-cache untuk cakupan tertentu. Instance Disposable dalam cakupan tersebut akan dibuang.
// Lihat contoh di bagian `scope()` di atas.

shutdown()
Mematikan kontainer, mereset semua cakupan yang ada, dan membuang semua singleton yang mengimplementasikan Disposable.
seal()
Menyegel kontainer, mencegah perubahan lebih lanjut pada binding dan alias. Ini adalah praktik yang baik di lingkungan produksi setelah semua binding diatur.
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerBuilder;
use Core\Container\Exceptions\ContainerException;
use Psr\Log\NullLogger;

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger());
$container = $builder->build();

$container->bind('service.a', stdClass::class);
$container->seal(); // Segel kontainer

try {
    $container->bind('service.b', stdClass::class);
} catch (ContainerException $e) {
    echo "Caught exception: " . $e->getMessage() . "\n";
    // Output: Caught exception: Cannot register new bindings. The container is sealed.
}

Fitur Lanjutan
Struktur Konfigurasi (Array)
ContainerFactory::create() dapat menerima array konfigurasi yang komprehensif untuk membangun kontainer.
<?php

// src/config/container.php
// Pastikan semua kelas yang direferensikan dalam konfigurasi ini ada dan di-autoload.

use Core\Container\AllowAllPolicy;
use Core\Container\InMemoryBindingRegistry;
use Core\Container\DefaultMonitoring;
use Core\Container\DefaultEventDispatcher;
use Core\Container\DefaultSnapshotManager;
use Core\Container\DefaultSecretManager;
use Core\Container\DefaultContextManager;
use Core\Container\DefaultDocGenerator;
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Psr\Container\ContainerInterface;

// Asumsi kelas-kelas ini ada di namespace App\
// namespace App\Repositories; class UserRepository {}
// namespace App\Services; class StripeGateway {}
// namespace App\Services; class PayPalGateway {}
// namespace App\Decorators; class CachedUserRepository {}
// namespace App\Rules; class NoDirectServiceInstantiationRule {}
// namespace App\Rules; class RepositoryMustImplementInterfaceRule {}

return [
    'core_settings' => [
        'strict_overwrite' => true, // Melarang penimpaan binding
        'logger_class' => Logger::class, // Contoh: Gunakan Monolog
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Services',
                'App\\Repositories',
                // Tambahkan namespace lain yang berisi kelas factory tepercaya Anda
            ],
            'allow_anonymous_closures' => false, // Melarang closure anonim sebagai factory
        ],
        'seal_in_production' => true, // Segel kontainer di lingkungan produksi (jika is_production() true)
    ],
    'dependencies_implementations' => [
        'registry' => [
            'class' => InMemoryBindingRegistry::class,
            'enabled' => true,
        ],
        'monitoring' => [
            'class' => DefaultMonitoring::class,
            'enabled' => true,
        ],
        'events' => [
            'class' => DefaultEventDispatcher::class,
            'enabled' => true,
        ],
        'snapshot_manager' => [
            'class' => DefaultSnapshotManager::class,
            'enabled' => true,
        ],
        'secret_manager' => [
            'class' => DefaultSecretManager::class,
            'enabled' => true,
        ],
        'context_manager' => [
            'class' => DefaultContextManager::class,
            'enabled' => true,
        ],
        'doc_generator' => [
            'class' => DefaultDocGenerator::class,
            'enabled' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'app.config' => [
                'factory' => function() { return ['app_name' => 'My App', 'version' => '1.0']; },
                'options' => ['scope' => 'singleton'],
                'tags' => ['config'],
                'strict_type' => 'array', // Contoh: memastikan factory mengembalikan array
                'expiry' => ['ttl' => 3600, 'sliding' => true], // Kedaluwarsa setelah 1 jam, diperpanjang setiap akses
            ],
            'user.repository' => [
                'factory' => \App\Repositories\UserRepository::class, // Asumsi kelas ini ada
                'options' => ['scope' => 'request', 'property_injection' => true],
                'tags' => ['repository'],
            ],
            'logger' => [
                'factory' => function(ContainerInterface $c) {
                    // Contoh: Menggunakan Monolog, menginjeksi handler
                    $logger = new Logger('app');
                    $logger->pushHandler($c->get('logger.handler.file'));
                    return $logger;
                },
                'options' => ['scope' => 'singleton'],
            ],
            'logger.handler.file' => [
                'factory' => function() {
                    return new StreamHandler('php://stderr', Logger::DEBUG);
                },
            ],
            'payment.gateway.stripe' => [
                'factory' => \App\Services\StripeGateway::class, // Asumsi kelas ini ada
                'tags' => ['payment.gateway'],
            ],
            'payment.gateway.paypal' => [
                'factory' => \App\Services\PayPalGateway::class, // Asumsi kelas ini ada
                'tags' => ['payment.gateway'],
            ],
        ],
        'aliases' => [
            'UserRepository' => 'user.repository',
            'Log' => 'logger',
        ],
        'tags_only' => [
            // Menambahkan tag ke binding yang sudah ada atau yang dibuat secara otomatis
            'database.drivers' => [
                'MySqlDriver', // Asumsi kelas ini ada
                'PostgreSqlDriver', // Asumsi kelas ini ada
            ],
        ],
        'decorators_only' => [
            'user.repository' => [
                function($instance, ContainerInterface $c) {
                    // Contoh dekorator: Menambahkan caching ke UserRepository
                    // Asumsi kelas App\Decorators\CachedUserRepository dan cache.service ada
                    return new \App\Decorators\CachedUserRepository($instance, $c->get('cache.service'));
                },
            ],
        ],
    ],
    'features' => [
        'architectural_rules' => [
            'enabled' => true,
            'rules' => [
                \App\Rules\NoDirectServiceInstantiationRule::class, // Asumsi kelas ini ada
                \App\Rules\RepositoryMustImplementInterfaceRule::class, // Asumsi kelas ini ada
            ],
        ],
        'parent_container' => [
            'enabled' => true,
            'delegation_policy_class' => AllowAllPolicy::class, // Atau kebijakan kustom
        ],
    ],
];

Catatan Penting untuk core_settings.security_policy.allow_anonymous_closures:
Jika Anda mengatur 'allow_anonymous_closures' ke false dalam security_policy, maka semua factory yang berupa closure anonim (seperti fn() => "Welcome to the basic app!" atau function() { return new SomeForbiddenClass(); }) tidak akan diizinkan oleh kontainer. Dalam kasus seperti itu, Anda harus memastikan bahwa semua factory Anda adalah nama kelas yang dapat di-autowire, atau objek yang mengimplementasikan Core\Container\Interfaces\Factory, dan kelas-kelas tersebut harus berada di trusted_namespaces Anda.
Monitoring
DefaultMonitoring melacak waktu resolusi, penggunaan memori, dan kegagalan. Ini adalah fitur yang sangat powerful untuk debugging dan observabilitas di lingkungan produksi.
 * setHealthcheck(string $id, callable $probe): Menetapkan fungsi pemeriksaan kesehatan untuk layanan.
 * addMetricsPusher(callable $cb): Menambahkan callback untuk mendorong metrik ke sistem eksternal.
 * dumpResolveStats(string $sort = null): Mengembalikan statistik resolusi untuk semua layanan.
 * getDiagnostics(): Mengembalikan log detail setiap resolusi.
 * clearDiagnostics(): Membersihkan log diagnostik.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Psr\Log\NullLogger;

class MyHealthyService {
    public function __construct() { /* ... */ }
    public function isHealthy(): bool {
        // Logika pemeriksaan kesehatan
        return true;
    }
}

$config = [/* ... konfigurasi kontainer Anda ... */];
$container = ContainerFactory::create($config);

// Menetapkan healthcheck untuk layanan
$container->setHealthcheck('my.healthy.service', function(MyHealthyService $instance) {
    return $instance->isHealthy();
});

// Menambahkan pusher metrik (misalnya, ke Prometheus atau layanan monitoring lainnya)
$container->addMetricsPusher(function(string $id, array $stats) {
    // Kirim $id dan $stats ke sistem monitoring eksternal Anda
    echo "Metrics for {$id}: Count={$stats['count']}, AvgTime={$stats['avg_time']}\n";
});

// Mengambil layanan (ini akan memicu pencatatan metrik)
$service = $container->get('my.healthy.service');

// Mendump statistik resolusi
$stats = $container->dumpResolveStats('total_time');
print_r($stats);

// Mendump status kesehatan
$health = $container->dumpHealthStatus();
print_r($health);

// Mendump diagnostik
$diagnostics = $container->getDiagnostics();
print_r($diagnostics);

Snapshotting Kontainer
Memungkinkan Anda mengekspor status kontainer ke file atau array dan mengimpornya kembali. Berguna untuk caching, persistensi, atau deployment yang cepat.
 * exportSnapshot(string $file): Mengekspor status ke file JSON.
 * importSnapshot(string $file): Mengimpor status dari file JSON.
 * exportSnapshotArray(): Mengekspor status sebagai array.
 * importSnapshotArray(array $state): Mengimpor status dari array.
 * diffWith(NextLevelContainer $other): Membandingkan status dua kontainer.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Psr\Log\NullLogger;

class CachedData { public string $value; public function __construct() { $this->value = uniqid('data_'); } }

$config = [
    'service_definitions' => [
        'bindings' => [
            'my.cached.data' => [
                'factory' => CachedData::class,
                'options' => ['scope' => 'singleton']
            ]
        ]
    ]
];

// Kontainer pertama
$container1 = ContainerFactory::create($config);
$data1 = $container1->get('my.cached.data');
echo "Container 1 Data ID: " . $data1->value . "\n";

// Ekspor snapshot
$snapshotFile = sys_get_temp_dir() . '/container_snapshot.json';
$container1->exportSnapshot($snapshotFile);
echo "Snapshot exported to: " . $snapshotFile . "\n";

// Kontainer kedua
$container2 = ContainerFactory::create($config);
$container2->importSnapshot($snapshotFile); // Impor status dari snapshot

$data2 = $container2->get('my.cached.data');
echo "Container 2 Data ID: " . $data2->value . "\n";

// Data ID harus sama karena diimpor dari snapshot
var_dump($data1->value === $data2->value); // Output: bool(true)

// Bandingkan dua kontainer (sebelum dan sesudah impor)
$diff = $container1->diffWith($container2);
// Diff akan menunjukkan bahwa tidak ada perubahan signifikan pada binding/singletons
// jika diimpor dengan sukses.
print_r($diff);

Manajemen Rahasia
Pisahkan rahasia dari konfigurasi kontainer.
 * bindSecret(string $id, string $key, callable $provider): Mengikat ID rahasia ke kunci dan provider yang akan mengambil nilai rahasia.
 * getSecret(string $id): Mengambil nilai rahasia.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Core\Container\Attributes\Inject;
use Psr\Log\NullLogger;

// Contoh provider rahasia (bisa dari variabel lingkungan, file, vault, dll.)
function getEnvSecret(string $key): ?string {
    return $_ENV[$key] ?? null;
}

// Set variabel lingkungan untuk contoh
$_ENV['DB_PASSWORD'] = 'super_secret_password';

$config = []; // Konfigurasi kontainer lainnya
$container = ContainerFactory::create($config);

// Bind rahasia 'db.password' menggunakan provider getEnvSecret
$container->bindSecret('db.password', 'DB_PASSWORD', 'App\Examples\getEnvSecret');

// Coba ambil rahasia
$dbPassword = $container->get('db.password');
echo "Database Password: " . $dbPassword . "\n"; // Output: Database Password: super_secret_password

// Layanan yang membutuhkan rahasia
class DatabaseConnector {
    public string $password;
    public function __construct(#[Inject('db.password')] string $password) {
        $this->password = $password;
        echo "DatabaseConnector created with password: " . $this->password . "\n";
    }
}

$container->bind('db.connector', DatabaseConnector::class);
$connector = $container->get('db.connector');

Manajemen Konteks & Binding Dinamis
Memungkinkan layanan untuk diselesaikan secara berbeda berdasarkan konteks aplikasi saat ini (misalnya, pengguna yang masuk, request HTTP).
 * pushContext(array $ctx): Mendorong konteks baru ke stack.
 * popContext(): Mengeluarkan konteks dari stack.
 * bindDynamic(string $id, callable $resolver): Mengikat resolver dinamis.
 * resolveDynamic(string $id, ContainerInterface $container): Menyelesaikan binding dinamis.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Psr\Log\NullLogger;
use Psr\Container\ContainerInterface;

class CurrentUser {
    public ?string $id = null;
    public ?string $role = null;
    public function __construct(?string $id = null, ?string $role = null) {
        $this->id = $id;
        $this->role = $role;
    }
}

$config = [];
$container = ContainerFactory::create($config);

// Bind dynamic 'current.user'
$container->bindDynamic('current.user', function(array $ctx, ContainerInterface $container) {
    return new CurrentUser($ctx['user_id'] ?? null, $ctx['user_role'] ?? null);
});

// Skenario 1: Tanpa konteks pengguna
$user1 = $container->get('current.user');
echo "User 1 ID: " . ($user1->id ?? 'Guest') . ", Role: " . ($user1->role ?? 'None') . "\n"; // Output: User 1 ID: Guest, Role: None

// Skenario 2: Dengan konteks pengguna sederhana
$container->pushContext(['user_id' => 'user-123']);
$user2 = $container->get('current.user');
echo "User 2 ID: " . ($user2->id ?? 'Guest') . ", Role: " . ($user2->role ?? 'None') . "\n"; // Output: User 2 ID: user-123, Role: None
$container->popContext(); // Hapus konteks

// Skenario 3: Konteks bertingkat
$container->pushContext(['request_id' => 'req-abc']);
echo "Pushed request context.\n";

$container->pushContext(['user_id' => 'user-456', 'user_role' => 'admin']);
echo "Pushed user context.\n";

$user3 = $container->get('current.user');
echo "User 3 ID: " . ($user3->id ?? 'Guest') . ", Role: " . ($user3->role ?? 'None') . "\n"; // Output: User 3 ID: user-456, Role: admin

// Konteks lain yang mungkin bergantung pada konteks sebelumnya
$container->bindDynamic('request.context.data', function(array $ctx, ContainerInterface $container) {
    return ['request_id' => $ctx['request_id'] ?? null, 'timestamp' => time()];
});
$requestData = $container->get('request.context.data');
echo "Request Data: " . json_encode($requestData) . "\n"; // Output: Request Data: {"request_id":"req-abc","timestamp":...}

$container->popContext(); // Hapus konteks pengguna
echo "Popped user context.\n";

$user4 = $container->get('current.user');
echo "User 4 ID: " . ($user4->id ?? 'Guest') . ", Role: " . ($user4->role ?? 'None') . "\n"; // Output: User 4 ID: Guest, Role: None (karena user context sudah di-pop)

$container->popContext(); // Hapus konteks request
echo "Popped request context.\n";

Multi-Tenancy
NextLevelContainer mendukung multi-tenancy dengan memungkinkan Anda mengkloning kontainer untuk setiap tenant, memastikan isolasi singleton dan scoped instances. Setiap tenant akan memiliki instance layanan spesifik tenant yang terisolasi, sementara layanan yang didelegasikan ke parent container akan tetap dibagikan.
 * forTenant(string $tenantId): Mengembalikan instance kontainer yang dikloning untuk tenant tertentu.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Core\Container\ContainerBuilder;
use Core\Container\AllowAllPolicy;
use Psr\Log\NullLogger;
use Psr\Log\LoggerInterface;
use Psr\Container\ContainerInterface;

class TenantConfig {
    public string $tenantId;
    public string $dbName;
    public function __construct(string $tenantId, string $dbName) {
        $this->tenantId = $tenantId;
        $this->dbName = $dbName;
        echo "TenantConfig created for {$tenantId} (DB: {$dbName})\n";
    }
}

class TenantLogger extends \Psr\Log\AbstractLogger {
    private string $tenantId;
    public function __construct(string $tenantId) {
        $this->tenantId = $tenantId;
    }
    public function log($level, \Stringable|string $message, array $context = []): void {
        echo "[TENANT-{$this->tenantId}] [{$level}] {$message}\n";
    }
}

// File: src/App/Extensions/TenantMonitoringExtension.php (sama seperti sebelumnya)
use Core\Container\Interfaces\ContainerExtension;
// ... (kelas TenantMonitoringExtension)

// 1. Konfigurasi Kontainer Utama
$mainConfig = [
    'core_settings' => [
        'logger_class' => NullLogger::class, // Logger untuk kontainer utama
        'strict_overwrite' => true,
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples', // Pastikan namespace contoh diizinkan
                'App\\Extensions',
            ],
            'allow_anonymous_closures' => true, // Izinkan closure untuk contoh ini
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            // Binding yang mungkin dibagikan atau diinjeksi ke kontainer tenant
            'shared.utility' => ['factory' => stdClass::class, 'options' => ['scope' => 'singleton']],
        ],
    ],
    'features' => [
        'parent_container' => [
            'enabled' => true,
            'delegation_policy_class' => AllowAllPolicy::class, // Izinkan delegasi ke induk
        ],
    ],
];

// 2. Buat Kontainer Utama dengan Ekstensi
$mainBuilder = new ContainerBuilder();
$mainBuilder->withLogger(new NullLogger()); // Logger untuk builder
$mainBuilder->withExtension(new \App\Extensions\TenantMonitoringExtension()); // Tambahkan ekstensi
$mainContainer = $mainBuilder->build();

// 3. Fungsi untuk membuat konfigurasi kontainer tenant
$createTenantConfig = function(string $tenantId): array {
    return [
        'core_settings' => [
            'logger_class' => NullLogger::class, // Tenant bisa punya logger sendiri
            'strict_overwrite' => true,
            'security_policy' => [
                'trusted_namespaces' => [
                    'App\\Examples', // Pastikan namespace contoh diizinkan
                ],
                'allow_anonymous_closures' => true,
            ],
        ],
        'service_definitions' => [
            'bindings' => [
                // Layanan spesifik tenant
                TenantConfig::class => [
                    'factory' => fn() => new TenantConfig($tenantId, "db_" . strtolower($tenantId)),
                    'options' => ['scope' => 'singleton']
                ],
                // Logger spesifik tenant
                LoggerInterface::class => [
                    'factory' => fn() => new TenantLogger($tenantId),
                    'options' => ['scope' => 'singleton']
                ],
            ],
        ],
    ];
};

// 4. Inisialisasi Kontainer untuk Tenant A
echo "\n--- Processing Tenant A ---\n";
// Dapatkan kontainer tenant dari mainContainer, ini akan mengkloning mainContainer
$tenantAContainer = $mainContainer->forTenant('tenant-A');
$tenantAContainer->pushContext(['tenant_id' => 'tenant-A']); // Set konteks tenant

// Konfigurasi binding spesifik tenant A
$tenantAContainer = ContainerFactory::create($createTenantConfig('tenant-A'), $mainContainer);

$tenantAConfig = $tenantAContainer->get(TenantConfig::class); // Output: TenantConfig created for tenant-A (DB: db_tenant-a)
$tenantALogger = $tenantAContainer->get(LoggerInterface::class);
$tenantALogger->info("This is a log from Tenant A."); // Output: [TENANT-tenant-A] [info] This is a log from Tenant A.

// Resolusi layanan bersama dari kontainer induk
$sharedUtilityA = $tenantAContainer->get('shared.utility');
echo "Shared utility for Tenant A: " . spl_object_id($sharedUtilityA) . "\n";

$tenantAContainer->popContext(); // Hapus konteks

// 5. Inisialisasi Kontainer untuk Tenant B
echo "\n--- Processing Tenant B ---\n";
// Dapatkan kontainer tenant dari mainContainer
$tenantBContainer = $mainContainer->forTenant('tenant-B');
$tenantBContainer->pushContext(['tenant_id' => 'tenant-B']); // Set konteks tenant

// Konfigurasi binding spesifik tenant B
$tenantBContainer = ContainerFactory::create($createTenantConfig('tenant-B'), $mainContainer);

$tenantBConfig = $tenantBContainer->get(TenantConfig::class); // Output: TenantConfig created for tenant-B (DB: db_tenant-b)
$tenantBLogger = $tenantBContainer->get(LoggerInterface::class);
$tenantBLogger->info("This is a log from Tenant B."); // Output: [TENANT-tenant-B] [info] This is a log from Tenant B.

// Resolusi layanan bersama dari kontainer induk
$sharedUtilityB = $tenantBContainer->get('shared.utility');
echo "Shared utility for Tenant B: " . spl_object_id($sharedUtilityB) . "\n";

$tenantBContainer->popContext(); // Hapus konteks

// Verifikasi isolasi dan berbagi
echo "\n--- Verification ---\n";
var_dump($tenantAConfig->tenantId === 'tenant-A'); // true
var_dump($tenantBConfig->tenantId === 'tenant-B'); // true
var_dump($tenantAConfig !== $tenantBConfig); // true (instance berbeda)
var_dump($sharedUtilityA === $sharedUtilityB); // true (instance yang sama dari kontainer induk)

// Periksa diagnostik dari main container (yang memiliki ekstensi monitoring)
echo "\n--- Main Container Diagnostics ---\n";
print_r($mainContainer->getDiagnostics());
// Anda akan melihat log resolusi dari kedua tenant yang dicatat oleh TenantMonitoringExtension.

Ekstensi Kontainer
Implementasikan Core\Container\Interfaces\ContainerExtension untuk menambahkan fungsionalitas kustom ke kontainer. Ekstensi dapat mendaftarkan binding dan aturan ke builder kontainer yang sedang dibangun, bukan hanya ke kontainer itu sendiri.
 * register(NextLevelContainer $container, ContainerBuilder $builder): Dipanggil selama proses build kontainer.
 * beforeResolve(string $id, NextLevelContainer $container): Dipanggil sebelum layanan diselesaikan.
 * afterResolve(string $id, mixed $instance, NextLevelContainer $container): Dipanggil setelah layanan berhasil diselesaikan.
<!-- end list -->
<?php

namespace App\Extensions;

use Core\Container\Interfaces\ContainerExtension;
use Core\Container\ContainerBuilder;
use Core\Container\NextLevelContainer;
use Psr\Log\LoggerInterface;
use Psr\Log\NullLogger; // Tambahkan ini jika belum ada

class LoggingExtension implements ContainerExtension {
    private LoggerInterface $logger;

    public function register(NextLevelContainer $container, ContainerBuilder $builder): void {
        // Ekstensi dapat menginjeksi logger ke dirinya sendiri
        // Penting: LoggerInterface harus sudah terikat di kontainer utama atau builder
        // Jika tidak, Anda mungkin perlu mengikatnya terlebih dahulu di builder atau config
        try {
            $this->logger = $container->get(LoggerInterface::class);
        } catch (\Psr\Container\NotFoundExceptionInterface $e) {
            // Fallback jika logger belum terikat
            $this->logger = new NullLogger();
            $this->logger->warning("LoggerInterface not found, using NullLogger for LoggingExtension.");
        }
        $this->logger->info("LoggingExtension registered.");

        // Contoh: Ekstensi bisa menambahkan binding baru ke builder
        $builder->withBindingRegistry(new \Core\Container\InMemoryBindingRegistry()); // Contoh, jika ekstensi ingin registry kustom
    }

    public function beforeResolve(string $id, NextLevelContainer $container): void {
        $this->logger->debug("Attempting to resolve: {$id}");
    }

    public function afterResolve(string $id, mixed $instance, NextLevelContainer $container): void {
        $this->logger->debug("Successfully resolved: {$id} (" . get_debug_type($instance) . ")");
    }
}

Untuk menggunakannya:
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Core\Container\ContainerBuilder; // Tambahkan ini
use App\Extensions\LoggingExtension;
use Psr\Log\NullLogger; // Atau logger lain
use Psr\Log\LoggerInterface;

$config = [
    'core_settings' => [
        'logger_class' => NullLogger::class, // Pastikan logger dikonfigurasi
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples', // Namespace untuk contoh kelas
                'App\\Extensions', // Namespace untuk ekstensi
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'my.service' => [ 'factory' => stdClass::class ]
        ]
    ]
];

$builder = new ContainerBuilder();
$builder->withLogger(new NullLogger()); // Pastikan logger di builder juga ada
$builder->withExtension(new LoggingExtension());
$container = $builder->build();

// Resolusi layanan akan memicu hook ekstensi
$service = $container->get('my.service');
// Anda akan melihat pesan log dari LoggingExtension

Aturan Arsitektur
Tegakkan aturan pada binding kontainer Anda dengan mengimplementasikan Core\Container\Interfaces\ArchitecturalRule.
<?php

namespace App\Rules;

use Core\Container\Interfaces\ArchitecturalRule;
use Core\Container\Exceptions\ArchitecturalViolationException;

class NoDirectServiceInstantiationRule implements ArchitecturalRule {
    public function validate(string $id, array $binding, array $allBindings): void {
        // Contoh: Melarang binding yang menggunakan closure anonim sederhana
        // yang secara langsung menginstansiasi kelas tertentu.
        // Ini adalah contoh yang sangat sederhana dan mungkin perlu disempurnakan.
        if (is_callable($binding['factory']) && !($binding['factory'] instanceof \Core\Container\Interfaces\Factory)) {
            $reflection = new \ReflectionFunction($binding['factory']);
            $source = file_get_contents($reflection->getFileName());
            $startLine = $reflection->getStartLine() - 1;
            $endLine = $reflection->getEndLine();
            $lines = array_slice(explode("\n", $source), $startLine, $endLine - $startLine + 1);
            $closureCode = implode("\n", $lines);

            if (preg_match('/new\s+SomeForbiddenClass/i', $closureCode)) {
                throw new ArchitecturalViolationException(
                    "Service '{$id}' directly instantiates 'SomeForbiddenClass'. Use binding for 'SomeForbiddenClass' instead."
                );
            }
        }
    }
}

// Contoh kelas yang akan dilarang
class SomeForbiddenClass {}

Untuk menggunakannya, tambahkan aturan ke konfigurasi kontainer Anda:
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Core\Container\Exceptions\ContainerException;
use Psr\Log\NullLogger;
use App\Rules\NoDirectServiceInstantiationRule; // Tambahkan ini
use App\Rules\SomeForbiddenClass; // Tambahkan ini

$config = [
    'core_settings' => [
        'logger_class' => NullLogger::class,
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples',
                'App\\Rules', // Namespace untuk aturan
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'forbidden.service' => [
                'factory' => function() { return new SomeForbiddenClass(); }
            ]
        ]
    ],
    'features' => [
        'architectural_rules' => [
            'enabled' => true,
            'rules' => [
                NoDirectServiceInstantiationRule::class,
            ],
        ],
    ],
];

try {
    $container = ContainerFactory::create($config);
} catch (ContainerException $e) {
    echo "Container build failed due to architectural rule violation: " . $e->getMessage() . "\n";
    // Output: Container build failed due to architectural rule violation: Service 'forbidden.service' directly instantiates 'SomeForbiddenClass'. Use binding for 'SomeForbiddenClass' instead.
}

Kebijakan Keamanan
Kontrol namespace mana yang diizinkan untuk menyediakan factory layanan, dan apakah closure anonim diizinkan.
 * addTrustedNamespace(string $namespace): Menambahkan namespace yang diizinkan.
 * allowAnonymousClosures(bool $allow = true): Mengizinkan closure anonim sebagai factory.
<!-- end list -->
// Lihat contoh array konfigurasi di atas (`core_settings.security_policy`)

Tipe Ketat & Kedaluwarsa
 * setStrictType(string $id, string $type): Memastikan instance yang diselesaikan adalah tipe yang ditentukan.
 * setExpiry(string $id, int $ttl, bool $sliding = false): Mengatur waktu hidup untuk layanan.
Catatan Penting untuk strict_type: Jika tipe instance yang diselesaikan tidak cocok dengan strict_type yang ditentukan, sebuah \TypeError akan dilemparkan, bukan Core\Container\Exceptions\ContainerException. Ini sesuai dengan perilaku PHP untuk ketidakcocokan tipe.
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Psr\Log\NullLogger;

interface CacheInterface {}
class RedisCache implements CacheInterface {}

$config = [
    'core_settings' => [
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples',
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'app.cache' => [
                'factory' => RedisCache::class,
                'strict_type' => CacheInterface::class, // Memastikan ini adalah CacheInterface
                'expiry' => ['ttl' => 60, 'sliding' => true], // Kedaluwarsa setelah 60 detik, diperpanjang
            ],
            'invalid.cache' => [
                'factory' => stdClass::class, // Ini akan gagal strict_type
                'strict_type' => CacheInterface::class,
            ]
        ]
    ]
];

$container = ContainerFactory::create($config);

// Resolusi yang berhasil
$cache = $container->get('app.cache');
var_dump($cache instanceof RedisCache); // Output: bool(true)

// Resolusi yang gagal karena pelanggaran tipe ketat
try {
    $invalidCache = $container->get('invalid.cache');
} catch (\TypeError $e) {
    echo "Caught TypeError: " . $e->getMessage() . "\n";
    // Output: Caught TypeError: Instance for 'invalid.cache' must be of type 'App\Examples\CacheInterface', but is stdClass
}

// Memperpanjang kedaluwarsa secara otomatis saat diakses (jika sliding)
sleep(5); // Tunggu 5 detik
$cache = $container->get('app.cache'); // Ini akan memperpanjang waktu kedaluarsa
echo "Cache accessed, expiry extended.\n";

Kebijakan Delegasi (Kontainer Induk)
Ketika kontainer anak tidak dapat menyelesaikan layanan, ia dapat mendelegasikannya ke kontainer induknya. Kebijakan delegasi menentukan kapan ini diizinkan.
 * withParent(ContainerInterface $parent, ?DelegationPolicy $policy = null): Mengatur kontainer induk.
 * canDelegate(string $id, ContainerInterface $child, ContainerInterface $parent): Metode dalam implementasi DelegationPolicy.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Core\Container\AllowAllPolicy;
use Psr\Log\NullLogger;
use Psr\Container\ContainerInterface;

// Kontainer Induk
$parentConfig = [
    'core_settings' => [
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples',
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'shared.service' => ['factory' => stdClass::class, 'options' => ['scope' => 'singleton']],
            'parent.only.service' => ['factory' => function() { return 'Parent Only'; }]
        ]
    ]
];
$parentContainer = ContainerFactory::create($parentConfig);

// Kontainer Anak
$childConfig = [
    'core_settings' => [
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples',
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'child.service' => ['factory' => function() { return 'Child Specific'; }]
        ]
    ],
    'features' => [
        'parent_container' => [
            'enabled' => true,
            'delegation_policy_class' => AllowAllPolicy::class, // Izinkan semua delegasi
        ],
    ],
];
$childContainer = ContainerFactory::create($childConfig, $parentContainer);

// Resolusi di kontainer anak
echo "Child service: " . $childContainer->get('child.service') . "\n"; // Output: Child service: Child Specific

// Resolusi layanan yang ada di induk (didelegasikan)
echo "Shared service (from parent): " . get_class($childContainer->get('shared.service')) . "\n"; // Output: Shared service (from parent): stdClass
echo "Parent only service (from parent): " . $childContainer->get('parent.only.service') . "\n"; // Output: Parent only service (from parent): Parent Only

// Resolusi layanan yang hanya ada di anak (parent tidak bisa melihat anak)
try {
    $parentContainer->get('child.service');
} catch (\Psr\Container\NotFoundExceptionInterface $e) {
    echo "Parent cannot resolve child service: " . $e->getMessage() . "\n";
    // Output: Parent cannot resolve child service: No binding, class, secret, dynamic binding, or tenant found for [child.service] in container or its parents.
}

Generasi Dokumentasi
Secara otomatis menghasilkan daftar binding kontainer.
 * generateDocs(string $format = 'markdown'): Menghasilkan dokumentasi dalam format Markdown atau JSON.
<!-- end list -->
<?php

namespace App\Examples; // Perbarui namespace

use Core\Container\ContainerFactory;
use Psr\Log\NullLogger;

$config = [
    'core_settings' => [
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Examples',
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'app.logger' => ['factory' => NullLogger::class, 'tags' => ['system.logging']],
            'app.mailer' => ['factory' => stdClass::class, 'tags' => ['communication']],
        ],
        'aliases' => [
            'Logger' => 'app.logger'
        ]
    ]
];

$container = ContainerFactory::create($config);

$markdownDocs = $container->generateDocs('markdown');
echo "--- Markdown Documentation ---\n";
echo $markdownDocs;
/* Output contoh:
--- Markdown Documentation ---
- **app.logger** _(tags: system.logging)_
- **app.mailer** _(tags: communication)_
- **Logger**
*/

$jsonDocs = $container->generateDocs('json');
echo "\n--- JSON Documentation ---\n";
echo $jsonDocs;
/* Output contoh:
--- JSON Documentation ---
[
    "- **app.logger** _(tags: system.logging)_",
    "- **app.mailer** _(tags: communication)_",
    "- **Logger**"
]
*/

Contoh Implementasi
Contoh Dasar: Aplikasi Konsol Sederhana
<?php

// File: src/App/Services/Greeter.php
namespace App\Services;

class Greeter {
    public function greet(string $name): string {
        return "Hello, " . $name . " from Greeter!";
    }
}

// File: public/index.php (atau main.php)
require_once __DIR__ . '/vendor/autoload.php'; // Pastikan autoloader Composer dimuat

use Core\Container\ContainerFactory;
use App\Services\Greeter;
use Psr\Log\NullLogger;

// 1. Konfigurasi Kontainer
$config = [
    'core_settings' => [
        'logger_class' => NullLogger::class,
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Services', // Izinkan namespace layanan Anda
            ],
            'allow_anonymous_closures' => true, // Izinkan factory closure anonim
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            'app.greeter' => [
                'factory' => Greeter::class, // Menggunakan nama kelas untuk autowiring
                'options' => ['scope' => 'singleton'], // Jadikan singleton
            ],
            'app.message' => [
                'factory' => fn() => "Welcome to the basic app!", // Binding dengan closure sederhana
            ],
        ],
        'aliases' => [
            'Greeter' => 'app.greeter', // Alias untuk akses mudah
        ],
    ],
];

// 2. Buat Kontainer
$container = ContainerFactory::create($config);

// 3. Ambil Layanan dan Gunakan
$greeter = $container->get('Greeter'); // Menggunakan alias
echo $greeter->greet('World') . "\n";

$message = $container->get('app.message');
echo $message . "\n";

// Verifikasi singleton
$greeter2 = $container->get('app.greeter');
var_dump($greeter === $greeter2); // Output: bool(true)

Contoh Menengah: Aplikasi Web Sederhana dengan Middleware
<?php

// File: src/App/Services/AuthService.php
namespace App\Services;

class AuthService {
    public function isAuthenticated(): bool {
        return (bool)($_SESSION['user_id'] ?? false);
    }
    public function login(string $userId): void {
        $_SESSION['user_id'] = $userId;
    }
    public function logout(): void {
        unset($_SESSION['user_id']);
    }
}

// File: src/App/Middleware/AuthMiddleware.php
namespace App\Middleware;

use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Core\Container\Attributes\Inject;
use App\Services\AuthService;
use Nyholm\Psr7\Response; // Tambahkan ini

class AuthMiddleware implements MiddlewareInterface {
    private AuthService $authService;

    public function __construct(#[Inject(AuthService::class)] AuthService $authService) {
        $this->authService = $authService;
    }

    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface {
        if (!$this->authService->isAuthenticated()) {
            // Contoh respons sederhana untuk tidak terautentikasi
            $response = new Response(401); // Gunakan Nyholm\Psr7\Response
            $response->getBody()->write("Unauthorized");
            return $response;
        }
        return $handler->handle($request);
    }
}

// File: src/App/Controllers/DashboardController.php
namespace App\Controllers;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Core\Container\Attributes\Inject;
use App\Services\AuthService;
use Nyholm\Psr7\Response;

class DashboardController {
    private AuthService $authService;

    public function __construct(#[Inject(AuthService::class)] AuthService $authService) {
        $this->authService = $authService;
    }

    public function show(ServerRequestInterface $request): ResponseInterface {
        $response = new Response(200);
        $response->getBody()->write("Welcome to the Dashboard, User " . ($_SESSION['user_id'] ?? 'Guest') . "!");
        return $response;
    }
}

// File: public/index.php
// Asumsi Anda memiliki PSR-7 dan PSR-15 (misalnya, nyholm/psr7, nyholm/psr7-server, dan relay/relay) terinstal
// composer require nyholm/psr7 nyholm/psr7-server relay/relay monolog/monolog

require_once __DIR__ . '/vendor/autoload.php';

use Core\Container\ContainerFactory;
use Core\Container\ContainerBuilder;
use Psr\Log\NullLogger;
use App\Services\AuthService;
use App\Middleware\AuthMiddleware;
use App\Controllers\DashboardController;
use Nyholm\Psr7Server\ServerRequestCreator;
use Nyholm\Psr7Server\ServerRequestCreatorInterface;
use Nyholm\Psr7\Factory\Psr17Factory;
use Relay\Relay;
use Psr\Http\Server\RequestHandlerInterface; // Tambahkan ini
use Psr\Http\Message\ServerRequestInterface; // Tambahkan ini
use Psr\Http\Message\ResponseInterface; // Tambahkan ini


session_start(); // Mulai sesi untuk AuthService

// 1. Konfigurasi Kontainer
$config = [
    'core_settings' => [
        'logger_class' => NullLogger::class,
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Services',
                'App\\Middleware',
                'App\\Controllers',
                'Nyholm\\Psr7', // Untuk factory PSR-7
                'Nyholm\\Psr7Server', // Untuk factory PSR-7 Server
            ],
            'allow_anonymous_closures' => true,
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            AuthService::class => ['factory' => AuthService::class, 'options' => ['scope' => 'singleton']],
            AuthMiddleware::class => ['factory' => AuthMiddleware::class],
            DashboardController::class => ['factory' => DashboardController::class],
            // PSR-7 Factories
            Psr17Factory::class => ['factory' => Psr17Factory::class, 'options' => ['scope' => 'singleton']],
            ServerRequestCreatorInterface::class => [
                'factory' => function(\Psr\Container\ContainerInterface $c) {
                    $psr17Factory = $c->get(Psr17Factory::class);
                    return new ServerRequestCreator(
                        $psr17Factory, // ServerRequestFactory
                        $psr17Factory, // UriFactory
                        $psr17Factory, // UploadedFileFactory
                        $psr17Factory  // StreamFactory
                    );
                },
                'options' => ['scope' => 'singleton']
            ],
        ],
        'tags_only' => [
            'app.middleware' => [ // Tag middleware yang akan dijalankan
                AuthMiddleware::class,
                // Middleware lain bisa ditambahkan di sini
            ],
        ],
    ],
];

// 2. Buat Kontainer
$container = ContainerFactory::create($config);

// 3. Siapkan Request dan Response
$requestCreator = $container->get(ServerRequestCreatorInterface::class);
$request = $requestCreator->fromGlobals();

// 4. Buat Relay (Dispatcher Middleware)
$middlewareQueue = [];
foreach ($container->resolveByTag('app.middleware') as $middleware) {
    $middlewareQueue[] = $middleware;
}

// Tambahkan handler akhir (controller)
$middlewareQueue[] = new class($container) implements RequestHandlerInterface { // Gunakan RequestHandlerInterface
    private ContainerInterface $container; // Gunakan ContainerInterface
    public function __construct(ContainerInterface $container) { // Gunakan ContainerInterface
        $this->container = $container;
    }
    public function handle(ServerRequestInterface $request): ResponseInterface {
        // Contoh routing sederhana: selalu ke DashboardController
        $controller = $this->container->get(DashboardController::class);
        return $controller->show($request);
    }
};

$relay = new Relay($middlewareQueue);

// 5. Jalankan Aplikasi
// Simulasikan login untuk menguji AuthMiddleware
if (!isset($_SESSION['user_id'])) {
    $authService = $container->get(AuthService::class);
    $authService->login('testuser-123');
    echo "User logged in for this request.\n";
}

$response = $relay->handle($request);

// 6. Kirim Respons
foreach ($response->getHeaders() as $name => $values) {
    foreach ($values as $value) {
        header(sprintf('%s: %s', $name, $value), false);
    }
}
http_response_code($response->getStatusCode());
echo $response->getBody();

Contoh Lanjutan: Aplikasi Multi-Tenant dengan Ekstensi Kustom
Dalam skenario ini, kita akan memiliki kontainer utama dan kontainer terpisah untuk setiap tenant. Setiap kontainer tenant akan memiliki layanan yang unik untuk tenant tersebut, dan kita akan menggunakan ekstensi untuk memantau resolusi tenant.
<?php

// File: src/App/Services/TenantConfig.php
namespace App\Services;

class TenantConfig {
    public string $tenantId;
    public string $dbName;
    public function __construct(string $tenantId, string $dbName) {
        $this->tenantId = $tenantId;
        $this->dbName = $dbName;
        echo "TenantConfig created for {$tenantId} (DB: {$dbName})\n";
    }
}

// File: src/App/Services/TenantLogger.php
namespace App\Services;

use Psr\Log\LoggerInterface;
use Psr\Log\AbstractLogger;

class TenantLogger extends AbstractLogger {
    private string $tenantId;
    public function __construct(string $tenantId) {
        $this->tenantId = $tenantId;
    }
    public function log($level, \Stringable|string $message, array $context = []): void {
        echo "[TENANT-{$this->tenantId}] [{$level}] {$message}\n";
    }
}

// File: src/App/Extensions/TenantMonitoringExtension.php
namespace App\Extensions;

use Core\Container\Interfaces\ContainerExtension;
use Core\Container\ContainerBuilder;
use Core\Container\NextLevelContainer;
use Psr\Log\LoggerInterface;
use Psr\Log\NullLogger; // Tambahkan ini

class TenantMonitoringExtension implements ContainerExtension {
    private LoggerInterface $mainLogger;

    public function register(NextLevelContainer $container, ContainerBuilder $builder): void {
        try {
            $this->mainLogger = $container->get(LoggerInterface::class);
        } catch (\Psr\Container\NotFoundExceptionInterface $e) {
            $this->mainLogger = new NullLogger();
            $this->mainLogger->warning("LoggerInterface not found in main container, using NullLogger for TenantMonitoringExtension.");
        }
        $this->mainLogger->info("TenantMonitoringExtension registered on main container.");
    }

    public function beforeResolve(string $id, NextLevelContainer $container): void {
        // Cek apakah ini kontainer tenant
        $currentContext = $container->context->current();
        $tenantId = $currentContext['tenant_id'] ?? 'N/A';
        if ($tenantId !== 'N/A') {
            $this->mainLogger->debug("[Tenant: {$tenantId}] Resolving: {$id}");
        }
    }

    public function afterResolve(string $id, mixed $instance, NextLevelContainer $container): void {
        $currentContext = $container->context->current();
        $tenantId = $currentContext['tenant_id'] ?? 'N/A';
        if ($tenantId !== 'N/A') {
            $this->mainLogger->debug("[Tenant: {$tenantId}] Resolved: {$id}");
        }
    }
}

// File: public/index.php (atau main.php)
require_once __DIR__ . '/vendor/autoload.php';

use Core\Container\ContainerFactory;
use Core\Container\ContainerBuilder;
use Core\Container\AllowAllPolicy;
use Psr\Log\NullLogger;
use Psr\Log\LoggerInterface;
use Psr\Container\ContainerInterface; // Tambahkan ini
use App\Services\TenantConfig;
use App\Services\TenantLogger;
use App\Extensions\TenantMonitoringExtension;

// 1. Konfigurasi Kontainer Utama
$mainConfig = [
    'core_settings' => [
        'logger_class' => NullLogger::class, // Logger untuk kontainer utama
        'strict_overwrite' => true,
        'security_policy' => [
            'trusted_namespaces' => [
                'App\\Services', // Namespace untuk layanan tenant
                'App\\Extensions', // Namespace untuk ekstensi
                'App\\Examples', // Namespace untuk contoh
            ],
            'allow_anonymous_closures' => true, // Izinkan closure untuk contoh ini
        ],
    ],
    'service_definitions' => [
        'bindings' => [
            // Binding yang mungkin dibagikan atau diinjeksi ke kontainer tenant
            'shared.utility' => ['factory' => stdClass::class, 'options' => ['scope' => 'singleton']],
        ],
    ],
    'features' => [
        'parent_container' => [
            'enabled' => true,
            'delegation_policy_class' => AllowAllPolicy::class, // Izinkan delegasi ke induk
        ],
    ],
];

// 2. Buat Kontainer Utama dengan Ekstensi
$mainBuilder = new ContainerBuilder();
$mainBuilder->withLogger(new NullLogger()); // Logger untuk builder
$mainBuilder->withExtension(new TenantMonitoringExtension()); // Tambahkan ekstensi
$mainContainer = $mainBuilder->build();

// 3. Fungsi untuk membuat konfigurasi kontainer tenant
$createTenantConfig = function(string $tenantId): array {
    return [
        'core_settings' => [
            'logger_class' => NullLogger::class, // Tenant bisa punya logger sendiri
            'strict_overwrite' => true,
            'security_policy' => [
                'trusted_namespaces' => [
                    'App\\Services', // Pastikan namespace contoh diizinkan
                ],
                'allow_anonymous_closures' => true,
            ],
        ],
        'service_definitions' => [
            'bindings' => [
                // Layanan spesifik tenant
                TenantConfig::class => [
                    'factory' => fn() => new TenantConfig($tenantId, "db_" . strtolower($tenantId)),
                    'options' => ['scope' => 'singleton']
                ],
                // Logger spesifik tenant
                LoggerInterface::class => [
                    'factory' => fn() => new TenantLogger($tenantId),
                    'options' => ['scope' => 'singleton']
                ],
            ],
        ],
    ];
};

// 4. Inisialisasi Kontainer untuk Tenant A
echo "\n--- Processing Tenant A ---\n";
// Dapatkan kontainer tenant dari mainContainer, ini akan mengkloning mainContainer
$tenantAContainer = $mainContainer->forTenant('tenant-A');
$tenantAContainer->pushContext(['tenant_id' => 'tenant-A']); // Set konteks tenant

// Konfigurasi binding spesifik tenant A
$tenantAContainer = ContainerFactory::create($createTenantConfig('tenant-A'), $mainContainer);

$tenantAConfig = $tenantAContainer->get(TenantConfig::class); // Output: TenantConfig created for tenant-A (DB: db_tenant-a)
$tenantALogger = $tenantAContainer->get(LoggerInterface::class);
$tenantALogger->info("This is a log from Tenant A."); // Output: [TENANT-tenant-A] [info] This is a log from Tenant A.

// Resolusi layanan bersama dari kontainer induk
$sharedUtilityA = $tenantAContainer->get('shared.utility');
echo "Shared utility for Tenant A: " . spl_object_id($sharedUtilityA) . "\n";

$tenantAContainer->popContext(); // Hapus konteks

// 5. Inisialisasi Kontainer untuk Tenant B
echo "\n--- Processing Tenant B ---\n";
// Dapatkan kontainer tenant dari mainContainer
$tenantBContainer = $mainContainer->forTenant('tenant-B');
$tenantBContainer->pushContext(['tenant_id' => 'tenant-B']); // Set konteks tenant

// Konfigurasi binding spesifik tenant B
$tenantBContainer = ContainerFactory::create($createTenantConfig('tenant-B'), $mainContainer);

$tenantBConfig = $tenantBContainer->get(TenantConfig::class); // Output: TenantConfig created for tenant-B (DB: db_tenant-b)
$tenantBLogger = $tenantBContainer->get(LoggerInterface::class);
$tenantBLogger->info("This is a log from Tenant B."); // Output: [TENANT-tenant-B] [info] This is a log from Tenant B.

// Resolusi layanan bersama dari kontainer induk
$sharedUtilityB = $tenantBContainer->get('shared.utility');
echo "Shared utility for Tenant B: " . spl_object_id($sharedUtilityB) . "\n";

$tenantBContainer->popContext(); // Hapus konteks

// Verifikasi isolasi dan berbagi
echo "\n--- Verification ---\n";
var_dump($tenantAConfig->tenantId === 'tenant-A'); // true
var_dump($tenantBConfig->tenantId === 'tenant-B'); // true
var_dump($tenantAConfig !== $tenantBConfig); // true (instance berbeda)
var_dump($sharedUtilityA === $sharedUtilityB); // true (instance yang sama dari kontainer induk)

// Periksa diagnostik dari main container (yang memiliki ekstensi monitoring)
echo "\n--- Main Container Diagnostics ---\n";
print_r($mainContainer->getDiagnostics());
// Anda akan melihat log resolusi dari kedua tenant yang dicatat oleh TenantMonitoringExtension.

Kesimpulan
NextLevelContainer menawarkan solusi IoC/DI yang sangat kuat dan fleksibel untuk aplikasi PHP modern. Dengan desain modular, fitur-fitur canggih seperti multi-tenancy, manajemen rahasia, dan kebijakan keamanan, serta kemampuan ekstensibilitas yang kuat, kontainer ini dapat menjadi tulang punggung yang andal untuk mengelola dependensi di proyek Anda, mulai dari aplikasi kecil hingga sistem skala enterprise.
Manfaatkan fitur-fitur ini untuk membangun kode yang lebih bersih, lebih mudah diuji, dan lebih mudah dipelihara.
