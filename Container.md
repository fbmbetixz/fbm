---

# NextLevelContainer
**Kontainer IoC/DI Tingkat Lanjut untuk PHP**

NextLevelContainer adalah implementasi Kontainer Inversi Kontrol (IoC) dan Penyuntikan Dependensi (DI) yang canggih dan berkinerja tinggi untuk PHP. Dirancang dengan modularitas dan ekstensi tinggi, cocok untuk aplikasi skala besar modern.

## Fitur Utama

- **Autowiring Cerdas:** Otomatis menyelesaikan dependensi melalui type hinting dan atribut.
- **Injeksi Berbasis Atribut (PHP 8+):** Gunakan atribut seperti `#[Inject]`, `#[InjectTag]`, `#[PropertyInject]`, dan `#[SetterInject]` untuk deklarasi injeksi eksplisit.
- **Manajemen Siklus Hidup:** Dukungan singleton, scoped instances, disposable objects.
- **Monitoring & Diagnostik:** Lacak metrik resolusi, pemeriksaan kesehatan, log diagnostik.
- **Snapshotting Kontainer:** Ekspor/impor status kontainer (persistensi/repplikasi).
- **Manajemen Rahasia:** Provider rahasia menjaga kredensial sensitif di luar kode inti.
- **Multi-Tenancy:** Isolasi kontainer untuk setiap tenant/konteks aplikasi.
- **Ekstensi Kontainer:** Perluas fungsi dengan custom hook `beforeResolve` dan `afterResolve`.
- **Aturan Arsitektur:** Validasi struktur binding, dorong praktik terbaik.
- **Kebijakan Keamanan:** Kontrol namespace tepercaya & izin closure anonim.
- **Tipe Ketat & Kedaluwarsa:** Validasi tipe dan kelola masa pakai objek.
- **Delegasi Kontainer:** Hierarki kontainer induk-anak dengan kebijakan delegasi.
- **Generasi Dokumentasi:** Otomatis hasilkan dokumentasi binding.

---

## Instalasi

**Contoh struktur direktori (PSR-4):**
```
your-project/
└── src/
    └── Core/
        └── Container/
            ├── Attributes/
            │   ├── Inject.php
            ├── Exceptions/
            ├── Interfaces/
            ├── Traits/
            ├── ContainerBuilder.php
            ├── NextLevelContainer.php
            └── ...
```

**composer.json:**
```json
{
    "autoload": {
        "psr-4": {
            "Core\\Container\\": "src/Core/Container/"
        }
    }
}
```

Jalankan:
```bash
composer dump-autoload
```

---

## Konsep Inti

### Binding Layanan

Daftarkan layanan ke kontainer:
```php
bind(string $id, callable|string|Factory $factory, array $options = [])
```
- `$id` : Pengenal unik layanan (misal: 'app.logger', 'UserRepository').
- `$factory` : Closure, nama kelas, atau objek Factory.
- `$options` : Opsi, misal 'scope'.

**Contoh:**
```php
$container->bind('my_service', function() {
    return new MyService();
});
$container->bind('another_service', MyService::class);
$container->bind('app.name', 'My Awesome App');

$service = $container->get('my_service');
echo $service->doSomething(); // Output: Doing something!
```

### Singleton

```php
singleton(string $id, callable|string|Factory $factory, array $options = [])
```
Hanya satu instance dibuat & digunakan berulang.

**Contoh:**
```php
$container->singleton('db_connection', DatabaseConnection::class);
$db1 = $container->get('db_connection');
$db2 = $container->get('db_connection');
// $db1 dan $db2 adalah instance yang sama
```

---

## Scope

Mendaftarkan layanan dengan cakupan (scope) tertentu, misalnya per request.

```php
scope(string $id, callable|string|Factory $factory, string $scopeName = 'request', array $options = [])
```

**Contoh:**
```php
$container->scope('request_service', RequestService::class, 'request');

// Dalam satu request
$service1_req1 = $container->get('request_service'); // Instance baru
$service2_req1 = $container->get('request_service'); // Instance sama dengan sebelumnya

// Reset scope request
$container->resetScope('request');

// Setelah reset, instance baru akan dibuat
$service3_req2 = $container->get('request_service');
```

---

## Alias

Membuat alias untuk layanan yang sudah ada.

```php
alias(string $id, string $target)
```
**Contoh:**
```php
$container->bind('app.logger', LoggerService::class);
$container->alias('logger', 'app.logger');
$loggerInstance = $container->get('logger'); // Sama dengan get('app.logger')
```

---

## Tag

Menandai satu atau beberapa layanan dengan tag tertentu, sehingga dapat diambil bersama.

```php
tag(string|string[] $ids, string $tag)
```

**Contoh:**
```php
$container->bind('plugin.a', PluginA::class);
$container->bind('plugin.b', PluginB::class);
$container->tag(['plugin.a', 'plugin.b'], 'app.plugins');

foreach ($container->resolveByTag('app.plugins') as $plugin) {
    // $plugin adalah instance PluginA dan PluginB
}
```

---

## Resolusi & Pembuatan Instance

- **get(string $id):** Mengambil instance layanan, menghormati scope/singleton.
- **make(string $id, array $args = []):** Membuat instance baru, melewati argumen konstruktor custom.

**Contoh:**
```php
$pdfReport = $container->make('report.generator', ['format' => 'pdf']);
$csvReport = $container->make('report.generator', ['format' => 'csv']);
```

---

## Autowiring & Injeksi

### Injeksi Konstruktor (Type Hinting)
Jika ada type hint pada konstruktor, kontainer akan mengisi otomatis.

```php
class UserService {
    public function __construct(MailerInterface $mailer) { ... }
}
$container->bind(MailerInterface::class, SmtpMailer::class);
$container->bind(UserService::class, UserService::class);
$userService = $container->get(UserService::class);
```

### Injeksi Berbasis Atribut

#### #[Inject]
Untuk menginjeksi layanan berdasarkan ID eksplisit di parameter konstruktor/metode.

```php
class Database {
    public function __construct(#[Inject('app.config')] Config $config) { ... }
}
```

#### #[InjectTag]
Untuk menginjeksi semua layanan bertag tertentu sebagai array/iterator.

```php
class ReportProcessor {
    public function __construct(#[InjectTag('reporters')] iterable $reporters) { ... }
}
```

#### #[PropertyInject]
Menyuntikkan layanan ke properti publik setelah objek dibuat. Aktifkan dengan opsi `'property_injection' => true`.

```php
class AnalyticsService {
    #[PropertyInject('event.logger')]
    public EventLogger $logger;
}
```

#### #[SetterInject]
Memanggil setter publik setelah objek dibuat. Aktifkan dengan `'setter_injection' => true`.

```php
class AuditService {
    #[SetterInject]
    public function setLogger(LoggerInterface $logger) { ... }
}
```

---

## Manajemen Siklus Hidup

### Disposable Interface

Layanan yang mengimplementasikan `Core\Container\Interfaces\Disposable` akan otomatis dipanggil `dispose()` saat kontainer shutdown/reset.

### Shutdown & Seal

- **resetScope(string $scope):** Reset semua instance dalam scope.
- **shutdown():** Reset semua scope & singleton, dispose semua Disposable.
- **seal():** Kontainer tidak bisa diubah lagi (binding baru, alias baru, dll).

---

## Fitur Lanjutan

### Struktur Konfigurasi (Array)
Anda bisa membangun kontainer menggunakan array konfigurasi komprehensif lewat `ContainerFactory::create()`.

**Contoh:**
```php
$config = [
    'core_settings' => [
        'strict_overwrite' => true,
        'logger_class' => \Monolog\Logger::class,
        'security_policy' => [
            'trusted_namespaces' => ['App\\Services', 'App\\Repositories'],
            'allow_anonymous_closures' => false,
        ],
        'seal_in_production' => true,
    ],
    'service_definitions' => [
        'bindings' => [
            'app.config' => [
                'factory' => fn() => ['app_name' => 'My App', 'version' => '1.0'],
                'options' => ['scope' => 'singleton'],
                'tags' => ['config'],
            ],
            // ... binding lainnya
        ],
        'aliases' => [
            'UserRepository' => 'user.repository',
        ],
    ],
    'features' => [
        'architectural_rules' => [
            'enabled' => true,
            'rules' => [
                \App\Rules\NoDirectServiceInstantiationRule::class,
            ],
        ],
        'parent_container' => [
            'enabled' => true,
            'delegation_policy_class' => \Core\Container\AllowAllPolicy::class,
        ],
    ],
];
$container = ContainerFactory::create($config);
```

---

## Monitoring & Diagnostik

- **setHealthcheck(string $id, callable $probe):** Set pemeriksaan kesehatan untuk layanan.
- **addMetricsPusher(callable $cb):** Push metrik ke sistem eksternal.
- **dumpResolveStats(string $sort = null):** Statistik resolusi layanan.
- **getDiagnostics():** Mendapatkan log detail setiap resolusi.
- **clearDiagnostics():** Bersihkan log diagnostik.

**Contoh:**
```php
$container->setHealthcheck('my.healthy.service', function(MyHealthyService $instance) {
    return $instance->isHealthy();
});
$container->addMetricsPusher(function(string $id, array $stats) {
    // Kirim ke monitoring eksternal
});
$stats = $container->dumpResolveStats('total_time');
$diagnostics = $container->getDiagnostics();
```

---

## Snapshotting Kontainer

Ekspor/impor status kontainer ke file/array untuk persistensi atau deployment cepat.

- **exportSnapshot(string $file):** Ekspor ke file JSON.
- **importSnapshot(string $file):** Impor dari file JSON.
- **exportSnapshotArray():** Ekspor ke array.
- **importSnapshotArray(array $state):** Impor dari array.
- **diffWith(NextLevelContainer $other):** Bandingkan status dua kontainer.

**Contoh:**
```php
$container->exportSnapshot('/tmp/container_snapshot.json');
$container2->importSnapshot('/tmp/container_snapshot.json');
```

---

## Manajemen Rahasia (Secret Management)

Pisahkan rahasia aplikasi dari konfigurasi dan kode.

- **bindSecret(string $id, string $key, callable $provider):** Binding ID rahasia ke provider.
- **getSecret(string $id):** Mengambil nilai rahasia.

**Contoh:**
```php
$container->bindSecret('db.password', 'DB_PASSWORD', function($key) {
    return $_ENV[$key] ?? null;
});
$dbPassword = $container->get('db.password');
```

---

## Manajemen Konteks & Binding Dinamis

Mendukung binding yang bergantung pada konteks (misal: user, request).

- **pushContext(array $ctx):** Tambah konteks ke stack.
- **popContext():** Keluarkan konteks dari stack.
- **bindDynamic(string $id, callable $resolver):** Binding dinamis.
- **resolveDynamic(string $id, ContainerInterface $container):** Resolve secara dinamis.

**Contoh:**
```php
$container->bindDynamic('current.user', function(array $ctx) {
    return new CurrentUser($ctx['user_id'] ?? null, $ctx['user_role'] ?? null);
});
$container->pushContext(['user_id' => 'user-123']);
$user = $container->get('current.user');
```

---

## Multi-Tenancy

NextLevelContainer mendukung multi-tenancy dengan mengkloning kontainer untuk setiap tenant. Singleton, scoped instance, dan konfigurasi tetap terisolasi antar tenant.

**API:**
- `forTenant(string $tenantId): NextLevelContainer`  
  Membuat atau mengambil instance kontainer khusus tenant.

**Contoh:**
```php
// Kontainer utama
$mainContainer = ContainerFactory::create($mainConfig);

// Dapatkan kontainer tenant (tenant-A)
$tenantAContainer = $mainContainer->forTenant('tenant-A');

// Tambahkan konteks$tenantAContainer->pushContext(['tenant_id' => 'tenant-A']);

// Binding dan penggunaan layanan spesifik tenant
$tenantAContainer->bind(TenantConfig::class, fn() => new TenantConfig('tenant-A', 'db_tenant_a'));
$config = $tenantAContainer->get(TenantConfig::class());
$container = $builder->build();
```

---

## Tips & Praktik Terbaik

- **Segel kontainer di produksi:**  
  Setelah semua binding diatur, gunakan `seal()` untuk mencegah modifikasi binding lebih lanjut.
- **Pisahkan rahasia dari konfigurasi:**  
  Gunakan provider rahasia untuk keamanan ekstra.
- **Manfaatkan monitoring:**  
  Pantau healthcheck dan metrik untuk observabilitas aplikasi.
- **Implementasikan aturan arsitektur:**  
  Gunakan fitur rules untuk menjaga konsistensi dan keamanan aplikasi.
- **Gunakan context separation:**  
  Untuk aplikasi multi-user/request, gunakan context stack agar dependency benar-benar terisolasi.

---

## Referensi Cepat API

| Fitur                | Fungsi/Method                                   |
|----------------------|-------------------------------------------------|
| Binding              | bind(), singleton(), scope()                    |
| Alias                | alias()                                         |
| Tagging              | tag(), resolveByTag()                           |
| Resolusi             | get(), make()                                   |
| Autowiring           | Type Hint, #[Inject], #[InjectTag], dsb.        |
| Siklus hidup         | resetScope(), shutdown(), dispose()             |
| Snapshot             | exportSnapshot(), importSnapshot(), diffWith()  |
| Secret management    | bindSecret(), getSecret()                       |
| Konteks dinamis      | pushContext(), popContext(), bindDynamic()      |
| Multi-tenancy        | forTenant()                                     |
| Monitoring           | setHealthcheck(), dumpResolveStats(), dsb.      |

---

## Penutup

NextLevelContainer dirancang untuk fleksibilitas, keamanan, dan kemudahan scaling aplikasi PHP modern, dengan fitur DI/IoC mutakhir, monitoring, snapshotting, multi-tenancy, dan masih banyak lagi.  
Untuk contoh lebih lanjut dan update terbaru, lihat dokumentasi di repo ini.

---