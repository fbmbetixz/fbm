GitHub Copilot Default Instructions - Modul ErrorManager Refactor (ABSOLUT MUTLAK, Full-Logic, Terpandu)
Selamat datang, Copilot! Anda adalah asisten refaktor tingkat ahli untuk modul ErrorManager. Instruksi di bawah ini adalah ATURAN ABSOLUT MUTLAK DAN FINAL yang harus Anda ikuti secara cermat, mengesampingkan atau memperkuat instruksi default lainnya. Tidak ada ruang untuk interpretasi lain selain yang dijelaskan di sini.
I. Misi Utama & Prinsip Global (Aturan ABSOLUT MUTLAK)
 * Tujuan Refaktor: Bangun sistem manajemen error yang modular, bersih, future-proof, dan adapter-friendly untuk PHP 8+. Ini harus mematuhi prinsip desain PHP modern, dengan fokus pada eliminasi static state, dekomposisi ErrorManager menjadi komponen yang testable dan stabil, serta penanganan error yang sangat semantik.
 * Integritas Fungsionalitas - PRIORITAS TERTINGGI & TIDAK DAPAT DITAWAR:
   * ANDA HARUS MEMPERTahankan SEMUA FITUR (MAYOR DAN MINOR) YANG ADA DARI KODE ASLI MODUL ERRORMANAGER. Fungsionalitas inti HARUS TETAP IDENTIK, meskipun implementasi diubah atau dioptimalkan.
   * JANGAN PERNAH MENGHAPUS ATAU MENGABAIKAN KODE YANG DICURIGAI IMPLEMENTASI FITUR ATAU UTILITAS. Jika sebuah metode, properti, atau blok kode di kode asli terlihat mengimplementasikan fungsionalitas (bahkan jika tidak dipanggil secara eksplisit, dikomentari, atau tampak tidak digunakan/disebut 'dead code'), ANDA WAJIB MEREFACTOR DAN MENTRANSFERNYA KE STRUKTUR BARU YANG SESUAI. Asumsikan itu adalah fitur yang harus dipertahankan, bukan kode mati yang harus dihapus.
 * Kualitas Kode Global - STANDAR OPERASIONAL UTAMA YANG SANGAT PRESKRIPTIF:
   * STRUKTUR FILE EKSPLISIT: SETIAP KELAS DAN INTERFACE HARUS BERADA DALAM FILE PHP TERPISAH. Jangan pernah menggabungkan beberapa kelas atau interface dalam satu blok kode respons Anda. Sajikan kode untuk setiap file secara individual.
   * PENGGUNAAN EXCEPTION KUSTOM EKSPLISIT: JANGAN PERNAH MENGGUNAKAN \RuntimeException, \InvalidArgumentException, \DomainException, \LogicException, \Exception, \Throwable, ATAU EXCEPTION PHP GENERIK LAINNYA. SELALU GUNAKAN EXCEPTION KUSTOM YANG TELAH KITA DEFINISIKAN untuk modul ini (Core\Error\Exception\) atau modul lain yang diinjeksikan.
   * Hanya Kode: Hasilkan HANYA KODE PHP untuk kelas/file yang sedang dikerjakan.
   * Tanpa Test/Dokumen Selama Refaktor: JANGAN menghasilkan unit test atau dokumentasi untuk file individual selama fase refaktor. Ini adalah fase terpisah dan final.
   * Tidak Ada Hardcoded Class/Fungsi Global: Hindari hardcoding nama kelas (termasuk PHP native seperti \ReflectionClass, \DateTime, date(), time(), microtime(), json_encode(), json_decode(), openssl_*(), bin2hex(), random_bytes(), mt_rand(), mt_getrandmax(), php_sapi_name(), defined('APP_VERSION'), getenv('APP_ENV')). SEMUA harus diganti dengan service yang diinjeksikan yang menyediakan fungsionalitas tersebut.
   * Strict Type Hinting: Implementasikan strict PHP type hints untuk SEMUA properti, parameter metode, dan return type. Gunakan mixed hanya ketika tidak ada type yang lebih spesifik.
   * Modern PHP (8.0+): Manfaatkan fitur PHP 8.0+ yang sesuai (e.g., promoted constructor properties, readonly properties, match expression, nullsafe operator, Enums).
   * Immutability: Buat value objects (seperti ErrorContext, ErrorReport) sepenuhnya immutable. Properti harus readonly; semua metode yang "memodifikasi" state harus mengembalikan instance baru.
   * Full PHPDoc Documentation: Sediakan PHPDoc komprehensif untuk SEMUA kelas, interface, trait, properti, dan metode. Sertakan @param, @return, @throws, dan deskripsi yang jelas tentang fungsionalitas, tujuan, dan penggunaan.
 * Prinsip Refaktor (Ditekankan untuk ErrorManager):
   * Dependency Injection (DI) First: Injeksi SEMUA dependensi via konstruktor. ELIMINASI SEMUA new Class() instansiasi dalam logika inti atau konstruktor, kecuali untuk value objects murni yang tidak memiliki dependensi atau di dalam Factories/Builders yang ditunjuk.
   * Single Responsibility Principle (SRP) - PRIORITAS KRITIS UNTUK ErrorManager & ErrorReport: Lakukan DEKOMPOSISI MASIF terhadap kelas ErrorManager dan ErrorReport yang ada. Pecah fungsinya menjadi service atau komponen yang lebih kecil dan fokus. Kelas ErrorManager yang baru akan menjadi fasad utama dan orkestrator yang mendelegasikan ke service ini. ErrorReport yang baru akan menjadi value object murni yang hanya menyimpan data.
   * Interface-Driven Design: Gunakan interface untuk SEMUA service dan komponen di mana implementasi berbeda mungkin ditukar.
   * No Static State: TOTAL ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD UNTUK STATE GLOBAL. Ini adalah prioritas ABSOLUT TERTINGGI untuk modul ErrorManager. Ganti dengan service yang diinjeksikan.
   * Robust Fallbacks & Error Handling: Implementasikan mekanisme fallback dan blok try-catch di sekitar operasi yang mungkin melempar exception. Pastikan exception spesifik dilempar untuk skenario error. Catat internal error via logger yang diinjeksikan.
   * Modular Structure & PSR-4 Compliance: Organisasi kode ke dalam subfolder logis yang mencerminkan struktur namespace. Patuhi PSR-4 autoloading standards secara ketat.
   * PSR Compliance: Pastikan kompatibilitas dengan PSR yang relevan (Psr\Log\LoggerInterface, Psr\EventDispatcher\EventDispatcherInterface, Psr\Container\ContainerInterface, Psr\Http\Message\ResponseInterface, Psr\Http\Message\ResponseFactoryInterface).
II. Fase Utama: Refaktor & Implementasi Logika Langsung (Terpandu)
 * KODE ASLI MODUL ERRORMANAGER (yang telah saya review) ADALAH REFERENSI MUTLAK DAN FONDASI UNTUK SEMUA TRANSFER LOGIKA DAN PRESERVASI FITUR.
 * Tugas Anda: Anda akan bekerja mengisi kode lengkap, poin demi poin, sesuai daftar dekomposisi spesifik modul ErrorManager.
 * Keluaran per Langkah:
   * Untuk setiap poin dekomposisi yang sedang Anda kerjakan, Anda harus menghasilkan KODE LENGKAP (termasuk interface, kelas implementasi, semua properti, semua signature method, DAN SEMUA LOGIKA INTERNAL YANG DITRANSFER DARI KODE ASLI).
   * PASTIKAN SEMUA FITUR (MAYOR DAN MINOR) DARI KODE ASLI DIPERTAHANKAN DAN DIIMPLEMENTASIKAN DENGAN BENAR DI TEMPAT YANG SESUAI DALAM STRUKTUR BARU. JANGAN ADA YANG HILANG.
   * Berikan PHPDoc komprehensif untuk setiap elemen yang dihasilkan.
   * JANGAN PERNAH HANYA MEMBERIKAN SEBAGIAN KECIL KODE ATAU HANYA DUA METHOD. Hasilkan seluruh bagian yang diminta untuk poin dekomposisi tersebut.
 * Mekanisme 'Lanjut' Otomatis & Status Progres:
   * Setelah Anda selesai memberikan kode lengkap untuk satu poin dekomposisi, Anda harus mengindikasikan poin dekomposisi berikutnya yang siap untuk dikerjakan. Misalnya: "Ketik 'lanjut' untuk dekomposisi Poin X: [Nama Poin Selanjutnya]".
   * Saya akan mengetik lanjut untuk mengonfirmasi bahwa Anda harus melanjutkan ke poin berikutnya dalam daftar.
III. Daftar Dekomposisi Spesifik Modul ErrorManager (Urutan Kerja ABSOLUT MUTLAK)
Ini adalah daftar lengkap poin-poin dekomposisi yang harus Anda ikuti secara berurutan. Setelah Anda selesai dengan satu poin, secara otomatis ajukan permintaan untuk melanjutkan ke poin berikutnya.
 * Core\Error\Exception\:
   * Buat exception kustom yang sangat spesifik:
     * Core\Error\Exception\ErrorManagerException (base exception)
     * Core\Error\Exception\ConfigurationException (konfigurasi tidak valid)
     * Core\Error\Exception\InitializationException (ErrorManager belum diinisialisasi)
     * Core\Error\Exception\ReportGenerationException (kesalahan saat membuat ErrorReport)
     * Core\Error\Exception\HandlerResolutionException (handler tidak ditemukan/tidak valid)
     * Core\Error\Exception\RenderingException (kesalahan saat merender output error)
     * Core\Error\Exception\ClassificationException (kesalahan saat klasifikasi throwable)
     * Core\Error\Exception\GroupStoreException (kesalahan pada group store)
     * Core\Error\Exception\NotificationException (kesalahan saat notifikasi error).
   * Sertakan PHPDoc lengkap dan konstruktor yang relevan.
 * Core\Error\Contract\ (Interface Umum):
   * Core\Error\Contract\TraceIdGeneratorInterface: Refaktor interface ini.
   * Core\Error\Contract\HasHttpCode: Refaktor interface ini.
   * Core\Error\Contract\TimeProviderInterface: (Dari modul Container Anda, pastikan ini diinjeksikan di mana pun dibutuhkan waktu).
   * Core\Error\Contract\AppVersionProviderInterface: Untuk mendapatkan versi aplikasi.
   * Core\Error\Contract\EnvironmentProviderInterface: Untuk mendapatkan lingkungan aplikasi (misalnya dari APP_ENV).
   * Core\Error\Contract\ResponseFactoryInterface: (Dari modul Middleware, atau buat yang sederhana jika belum ada).
   * Core\Error\Contract\CliOutputInterface: (Dari modul Router).
   * Core\Error\Contract\JsonEncoderInterface / JsonDecoderInterface: Untuk enkapsulasi json_encode/json_decode jika diinginkan.
   * Core\Error\Contract\KryptonInterface: Untuk operasi kriptografi (openssl_*).
   * Core\Error\Contract\RandomBytesGeneratorInterface: Untuk abstraksi random_bytes dan randomness.
 * Core\Error\ValueObject\ (Immutable):
   * Core\Error\ValueObject\ErrorContext: Refaktor kelas ErrorContext yang ada. Properti readonly. Semua metode yang "memodifikasi" context harus mengembalikan instance baru. TRANSFER DAN ADAPTASI LOGIKA get, getDeep, all.
   * Core\Error\ValueObject\ErrorReport: Refaktor kelas ErrorReport yang ada. Properti readonly. Implementasikan \JsonSerializable. INI ADALAH VALUE OBJECT MURNI. Ia TIDAK boleh melakukan instansiasi dependensi, klasifikasi, hashing, atau state tracking di konstruktornya. Semua data harus datang sudah jadi. TRANSFER DAN ADAPTASI LOGIKA toArray, toJson, getHint, getTags, getExtra, getErrorCode, getSource, jsonSerialize.
 * Core\Error\Classifier\ (Throwable Classification):
   * Core\Error\Classifier\ThrowableClassifierInterface
   * Implementasi Core\Error\Classifier\DefaultThrowableClassifier.
     * TRANSFER DAN ADAPTASI LOGIKA classify() dan httpCode() dari ThrowableClassifier lama.
     * Metode ini harus bersifat instance method, bukan static.
 * Core\Error\Group\ (Error Grouping & Rate Limiting):
   * Core\Error\Group\ErrorHasherInterface: Untuk menghasilkan hash grup.
   * Implementasi Core\Error\Group\DefaultErrorHasher: TRANSFER DAN ADAPTASI LOGIKA groupHash dari ErrorGroupStore lama.
   * Core\Error\Group\GroupStoreInterface: Refaktor interface ini.
   * Implementasi Core\Error\Group\InMemoryGroupStore:
     * TRANSFER DAN ADAPTASI LOGIKA updateOccurrence dan allowLog dari ErrorGroupStore lama.
     * PENTING: HARUS menginjeksi TimeProviderInterface (dari modul Container) untuk semua operasi berbasis waktu (date('c'), time()).
     * Properti ($groups, $rateLimit) bersifat private.
 * Core\Error\Handler\ (Exception Handler Management):
   * Core\Error\Handler\ExceptionHandlerMapInterface
   * Implementasi Core\Error\Handler\DefaultExceptionHandlerMap: TRANSFER DAN ADAPTASI LOGIKA register dan getHandler dari ExceptionHandlerMap lama.
     * PENTING: HARUS menginjeksi Psr\Container\ContainerInterface (dari modul Container) untuk resolusi string handler.
     * HARUS menginjeksi Core\Middleware\Handler\MiddlewareHandlerResolverInterface (dari modul Middleware) untuk menyelesaikan string handler (Service@method).
     * JANGAN melempar \RuntimeException. Gunakan HandlerResolutionException jika container tidak disetel atau handler tidak valid.
 * Core\Error\Renderer\ (Error Rendering):
   * Core\Error\Renderer\ErrorRendererInterface: Refaktor interface ini.
   * Implementasi Core\Error\Renderer\JsonErrorRenderer: TRANSFER DAN ADAPTASI LOGIKA render dari JsonRenderer.
   * Implementasi Core\Error\Renderer\HtmlErrorRenderer: TRANSFER DAN ADAPTASI LOGIKA render dari HtmlRenderer.
     * PENTING: HARUS menginjeksi service untuk HTML escaping (misalnya HtmlEncoderInterface) alih-alih htmlspecialchars() langsung.
   * Implementasi Core\Error\Renderer\CliErrorRenderer: TRANSFER DAN ADAPTASI LOGIKA render dari CliRenderer.
     * PENTING: HARUS menginjeksi CliOutputInterface (dari modul Router) atau service pembantu untuk CLI string formatting.
 * Core\Error\Logger\ (Error Logging):
   * Core\Error\Logger\ErrorLoggerInterface
   * Implementasi Core\Error\Logger\DefaultErrorLogger:
     * TRANSFER DAN ADAPTASI LOGIKA log, registerLevel, levelForReport, maskSensitive dari ErrorLogger lama.
     * PENTING: HARUS menginjeksi Psr\Log\LoggerInterface (dari modul Container) untuk mencatat log.
     * PENTING: Logika masking sensitif (maskSensitive) harus didelegasikan ke service terpisah: SensitiveDataMaskerInterface.
 * Core\Error\Event\ (Error Eventing):
   * Core\Error\Event\ErrorEvent: Refaktor kelas ini. Properti readonly. HARUS mengimplementasikan Psr\EventDispatcher\EventInterface.
   * Core\Error\Event\ErrorEventFactoryInterface: Untuk membuat ErrorEvent.
   * Implementasi Core\Error\Event\DefaultErrorEventFactory:
     * HARUS menginjeksi TimeProviderInterface, AppVersionProviderInterface, EnvironmentProviderInterface untuk menambahkan metadata otomatis.
   * Core\Error\Event\ErrorEventDispatcherInterface
   * Implementasi Core\Error\Event\DefaultErrorEventDispatcher:
     * TRANSFER DAN ADAPTASI LOGIKA dispatch dari ErrorEventDispatcher lama.
     * HARUS menginjeksi Psr\EventDispatcher\EventDispatcherInterface (dari modul Container).
 * Core\Error\Collector\ (Error Collection):
   * Core\Error\Collector\ErrorCollectorInterface
   * Implementasi Core\Error\Collector\DefaultErrorCollector: TRANSFER DAN ADAPTASI LOGIKA add, all, clear dari ErrorCollector lama.
 * Core\Error\Breadcrumbs\ (Contextual Breadcrumbs):
   * Core\Error\Breadcrumbs\BreadcrumbsInterface
   * Implementasi Core\Error\Breadcrumbs\DefaultBreadcrumbs: TRANSFER DAN ADAPTASI LOGIKA add, all dari Breadcrumbs lama.
     * HARUS menginjeksi TimeProviderInterface.
 * Core\Error\Notification\ (Notification Matrix):
   * Core\Error\Notification\NotificationMatrixInterface
   * Implementasi Core\Error\Notification\DefaultNotificationMatrix: TRANSFER DAN ADAPTASI LOGIKA register, getHandlers dari NotificationMatrix lama.
 * Core\Error\Main\ (The New ErrorManager Fasad/Orchestrator):
   * Core\Error\Main\ErrorManagerInterface
   * Implementasi Core\Error\Main\DefaultErrorManager.
     * INI ADALAH FASAD UTAMA. Konstruktornya akan menginjeksi SEMUA service yang telah didekomposisi di atas: ThrowableClassifierInterface, ErrorHasherInterface, GroupStoreInterface, ExceptionHandlerMapInterface, ErrorLoggerInterface, ErrorEventDispatcherInterface, ErrorCollectorInterface, BreadcrumbsInterface, NotificationMatrixInterface, TraceIdGeneratorInterface, Psr\Log\LoggerInterface (global), Psr\EventDispatcher\EventDispatcherInterface (global).
     * TRANSFER DAN ADAPTASI LOGIKA inti dari ErrorManager::handle() lama, handleAsResponse(), registerShutdownHandler(). Ini akan mendelegasikan semua tugas ke service yang diinjeksikan.
     * PENTING: ELIMINASI SEMUA PENGGUNAAN static::$instance (init, instance, reset). ErrorManager harus diatur oleh DI container utama Anda.
     * PENTING: ELIMINASI SEMUA new instansiasi dependensi internal di konstruktor atau di mana pun.
     * TRANSFER DAN ADAPTASI LOGIKA normalizeConfig, validateConfig (ke ErrorManagerBuilder atau ConfigurationValidator).
     * TRANSFER DAN ADAPTASI LOGIKA enableDebug, setChannelSelector.
     * TRANSFER DAN ADAPTASI LOGIKA registerHandler, registerRenderer, registerResponseFactory, last (ke service yang tepat).
     * Tangani errorLimit dan errorCount.
     * Untuk registerShutdownHandler(), ini harus menginjeksi ErrorManagerInterface untuk memanggil handle().
 * Core\Error\Builder\ (ErrorManager Builder):
   * Core\Error\Builder\ErrorManagerBuilderInterface
   * Implementasi Core\Error\Builder\DefaultErrorManagerBuilder.
     * Tanggung jawab utama: Menerima array konfigurasi mentah untuk ErrorManager.
     * TRANSFER DAN ADAPTASI LOGIKA normalizeConfig dan validateConfig dari ErrorManager lama.
     * BERTANGGUNG JAWAB untuk instansiasi SEMUA objek dependensi yang dibutuhkan DefaultErrorManager berdasarkan konfigurasi, dan mengembalikan instance DefaultErrorManager yang sudah sepenuhnya terinisialisasi.
     * Harus menginjeksi Psr\Container\ContainerInterface untuk menyelesaikan dependensi melalui container.
IV. Fase Akhir: Dokumentasi & Testing
 * Setelah SELURUH refaktor kode selesai dan semua kelas diimplementasikan logikanya: Anda akan kemudian melakukan dua tugas komprehensif yang terpisah:
   * Tugas A: Dokumentasi Lengkap Gaya GitHub.
   * Tugas B: PHPUnit Test Files Komprehensif.
 * PENTING: JANGAN membuat unit test atau dokumentasi apa pun sebelum SELURUH KODE SELESAI DIREFAKTOR DAN DIIMPLEMENTASIKAN LOGIKANYA.

** CODEBASE ASLI DIBAWAH ADALAH SUMBER REFERENSI DARI TUGAS REFACTOR MODULE **
<?php
// [CONFIG-DRIVEN V3 + VALIDATOR] ErrorManager - Full Configurable, Full Integration, Validated

if (!class_exists('Response')) {
    class Response {
        public function __construct(public string $body, public int $status = 500) {}
    }
}

use Psr\Container\ContainerInterface;
use Psr\EventDispatcher\EventDispatcherInterface;
use Psr\Log\LoggerInterface;
use Psr\Http\Message\ResponseInterface;

// --- Core interfaces & classes

interface TraceIdGeneratorInterface { public function generate(array $context = []): string; }
class DefaultTraceIdGenerator implements TraceIdGeneratorInterface {
    public function generate(array $context = []): string { return $context['traceId'] ?? bin2hex(random_bytes(8)); }
}
class SystemTraceIdGenerator implements TraceIdGeneratorInterface {
    public function generate(array $ctx = []): string { return \Core\TraceHelper::getTraceId(); }
}
interface HasHttpCode { public function getHttpCode(): int; }

class ErrorContext {
    private array $data = [];
    public function __construct(array $data = []) { $this->data = $data; }
    public function get(string $key, $default = null) { return $this->data[$key] ?? $default; }
    public function getDeep(string $key, $default = null) {
        $keys = explode('.', $key); $data = $this->data;
        foreach ($keys as $k) { if (is_array($data) && array_key_exists($k, $data)) $data = $data[$k]; else return $default; }
        return $data;
    }
    public function all(): array { return $this->data; }
}

final class ErrorReport implements \JsonSerializable {
    public readonly string $id;
    public readonly int $code;
    public readonly string $message;
    public readonly string $type;
    public readonly ?\Throwable $exception;
    public readonly array $context;
    public readonly ?ErrorReport $previous;
    public readonly ?string $file;
    public readonly ?int $line;
    public readonly ?string $exception_class;
    public readonly ?string $group_hash;
    public readonly ?int $occurrence_count;
    public readonly ?string $first_seen;
    public readonly ?string $last_seen;

    public function __construct(
        string|array|ErrorContext $message,
        int $code = 0,
        string $type = 'unknown',
        ErrorContext|array $context = [],
        ?\Throwable $exception = null,
        ?ErrorReport $previous = null,
        ?TraceIdGeneratorInterface $traceIdGen = null,
        ?string $group_hash = null,
        ?int $occurrence_count = null,
        ?string $first_seen = null,
        ?string $last_seen = null
    ) {
        $ctx = $context instanceof ErrorContext ? $context : new ErrorContext($context);
        $this->context = $ctx->all();
        $this->message = is_array($message) && isset($message['message']) ? $message['message'] : ($message instanceof ErrorContext ? ($message->get('message') ?? '') : $message);
        $this->code = $code; $this->type = $type; $this->exception = $exception;
        $this->id = $traceIdGen
            ? $traceIdGen->generate($this->context)
            : ($this->context['traceId'] ?? bin2hex(random_bytes(8)));
        $this->previous = $previous;
        $this->file = $exception ? $exception->getFile() : null;
        $this->line = $exception ? $exception->getLine() : null;
        $this->exception_class = $exception ? get_class($exception) : null;
        $this->group_hash = $group_hash;
        $this->occurrence_count = $occurrence_count;
        $this->first_seen = $first_seen;
        $this->last_seen = $last_seen;
    }
    public function toArray(): array {
        return [
            'id' => $this->id, 'code' => $this->code, 'message' => $this->message, 'type' => $this->type,
            'context' => $this->context, 'file' => $this->file, 'line' => $this->line,
            'exception_class' => $this->exception_class, 'previous' => $this->previous?->toArray(),
            'group_hash' => $this->group_hash, 'occurrence_count' => $this->occurrence_count,
            'first_seen' => $this->first_seen, 'last_seen' => $this->last_seen
        ];
    }
    public function toJson(): string { return json_encode($this->toArray(), JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES); }
    public function getHint(): ?string { return $this->context['hint'] ?? null; }
    public function getTags(): array { return $this->context['tags'] ?? []; }
    public function getExtra(): array { return $this->context['extra'] ?? []; }
    public function getErrorCode(): ?string { return $this->context['code'] ?? null; }
    public function getSource(): ?string { return $this->context['source'] ?? null; }
    public function jsonSerialize(): array { return $this->toArray(); }
}

class ThrowableClassifier {
    public static function classify(\Throwable $e): string {
        switch (true) {
            case $e instanceof \InvalidArgumentException:
            case $e instanceof \DomainException:
            case $e instanceof \LogicException: return 'logic';
            case $e instanceof \RuntimeException: return 'infra';
            case $e instanceof \Exception: return 'client';
            default: return 'unknown';
        }
    }
    public static function httpCode(\Throwable $e): int {
        if ($e instanceof HasHttpCode) return $e->getHttpCode();
        return match (true) {
            $e instanceof \InvalidArgumentException => 400,
            $e instanceof \DomainException         => 422,
            $e instanceof \LogicException          => 500,
            $e instanceof \RuntimeException        => 503,
            default                                => 500,
        };
    }
}

// --- GroupStoreInterface (in-memory default)

interface GroupStoreInterface {
    public function groupHash(\Throwable $e): string;
    public function updateOccurrence(string $hash): array;
    public function allowLog(string $hash): bool;
}

class ErrorGroupStore implements GroupStoreInterface {
    private array $groups = [];
    private array $rateLimit = [];
    private int $rateWindow = 60;
    private int $maxPerWindow = 10;
    public function groupHash(\Throwable $e): string {
        $signature = get_class($e) . '|' . $e->getMessage() . '|' . $e->getFile() . ':' . $e->getLine();
        $trace = $e->getTrace();
        for ($i=0;$i<3&&isset($trace[$i]);$i++) {
            $t=$trace[$i];
            $signature .= '|' . ($t['file'] ?? '') . ':' . ($t['line'] ?? '') . ':' . ($t['function'] ?? '');
        }
        return substr(sha1($signature),0,16);
    }
    public function updateOccurrence(string $hash): array {
        $now = date('c');
        if (!isset($this->groups[$hash])) $this->groups[$hash] = ['count'=>1, 'first'=>$now, 'last'=>$now];
        else { $this->groups[$hash]['count']++; $this->groups[$hash]['last'] = $now; }
        return [
            'count'=>$this->groups[$hash]['count'],
            'first'=>$this->groups[$hash]['first'],
            'last'=>$this->groups[$hash]['last']
        ];
    }
    public function allowLog(string $hash): bool {
        $now = time();
        $rl = $this->rateLimit[$hash] ?? ['last'=>0, 'count'=>0];
        if ($now - $rl['last'] > $this->rateWindow) $rl = ['last'=>$now, 'count'=>1];
        else $rl['count']++;
        $this->rateLimit[$hash] = $rl;
        return $rl['count'] <= $this->maxPerWindow;
    }
}

// --- HandlerMap

class ExceptionHandlerMap {
    private array $handlers = [];
    private ?ContainerInterface $container;
    public function __construct(?ContainerInterface $container = null) { $this->container = $container; }
    public function register(string $exceptionClass, callable|string $handler): void { $this->handlers[$exceptionClass] = $handler; }
    public function getHandler(\Throwable $e): ?callable {
        foreach ($this->handlers as $class => $handler) {
            if ($e instanceof $class) {
                if (is_string($handler) && strpos($handler, '@') !== false) {
                    if (!$this->container) throw new \RuntimeException("Container is required for string handler resolution");
                    [$service, $method] = explode('@', $handler);
                    $instance = $this->container->get($service);
                    $handler = [$instance, $method];
                }
                if (!is_callable($handler)) throw new \RuntimeException("Invalid handler for exception: must be callable or 'Service@method'");
                return $handler;
            }
        }
        return null;
    }
}

// --- Renderer

interface ErrorRendererInterface { public function render(ErrorReport $report): mixed; }
class JsonRenderer implements ErrorRendererInterface {
    public function render(ErrorReport $report): string {
        return json_encode(['error' => $report->toArray()], JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);
    }
}
class HtmlRenderer implements ErrorRendererInterface {
    public function render(ErrorReport $report): string {
        $ctx = htmlspecialchars(json_encode($report->context, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES));
        $trace = $report->exception ? nl2br(htmlspecialchars($report->exception->getTraceAsString())) : '';
        return "<h1>Error: {$report->message}</h1>
            <div><b>Type:</b> {$report->type} | <b>Code:</b> {$report->code} | <b>ID:</b> {$report->id}</div>
            <div><b>File:</b> {$report->file} : {$report->line}</div>
            <div><b>Exception Class:</b> {$report->exception_class}</div>
            <div><b>Group:</b> {$report->group_hash} | <b>Count:</b> {$report->occurrence_count} | <b>First:</b> {$report->first_seen} | <b>Last:</b> {$report->last_seen}</div>
            <pre>Context: {$ctx}\n{$trace}</pre>";
    }
}
class CliRenderer implements ErrorRendererInterface {
    public function render(ErrorReport $report): string {
        $str = "ERROR: {$report->message}\nTYPE: {$report->type}\nCODE: {$report->code}\nID: {$report->id}\nFILE: {$report->file}\nLINE: {$report->line}\nEXCEPTION: {$report->exception_class}\n";
        $str .= "GROUP: {$report->group_hash} | COUNT: {$report->occurrence_count} | FIRST: {$report->first_seen} | LAST: {$report->last_seen}\n";
        $str .= "CONTEXT: " . json_encode($report->context, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES) . "\n";
        if ($report->exception) $str .= "TRACE:\n" . $report->exception->getTraceAsString() . "\n";
        return $str;
    }
}

// --- Logger

class ErrorLogger {
    private LoggerInterface $logger;
    private array $levelMap = [];
    private array $maskFields = [];
    public function __construct(LoggerInterface $logger, array $maskFields = []) { $this->logger = $logger; $this->maskFields = $maskFields; }
    public function registerLevel(string $exceptionClass, string $level): void { $this->levelMap[$exceptionClass] = $level; }
    public function log(ErrorReport $report): void {
        $data = $report->toArray();
        if (!empty($this->maskFields)) $data = $this->maskSensitive($data, $this->maskFields);
        $level = $this->levelForReport($report);
        $this->logger->log($level, $report->message, $data);
    }
    private function levelForReport(ErrorReport $report): string {
        if ($report->exception) foreach ($this->levelMap as $class => $level) if ($report->exception instanceof $class) return $level;
        return match ($report->type) {
            'client' => 'warning', 'logic' => 'error', 'infra' => 'critical', default => 'error',
        };
    }
    private function maskSensitive(array $data, array $fields): array {
        foreach ($fields as $field) {
            if (isset($data[$field])) $data[$field] = '[MASKED]';
            if (isset($data['context'][$field])) $data['context'][$field] = '[MASKED]';
        }
        foreach ($data as $k => &$v) if (is_array($v)) $v = $this->maskSensitive($v, $fields);
        return $data;
    }
}

// --- Event dispatcher

class ErrorEvent { public function __construct(public ErrorReport $report, public array $meta = []) {} }
class ErrorEventDispatcher {
    private ?EventDispatcherInterface $bus; private array $meta = [];
    public function __construct(?EventDispatcherInterface $bus = null, array $meta = []) { $this->bus = $bus; $this->meta = $meta; }
    public function dispatch(ErrorReport $report): void {
        if ($this->bus) {
            $type = $report->type ?? 'unknown';
            $meta = array_merge(
                $this->meta,
                [
                    'timestamp' => date('c'),
                    'app_version' => defined('APP_VERSION') ? APP_VERSION : null,
                    'env' => getenv('APP_ENV') ?: null,
                ]
            );
            $this->bus->dispatch("error.{$type}", new ErrorEvent($report, $meta));
        }
    }
}
class ErrorCollector {
    private array $errors = [];
    public function add(ErrorReport $report): void { $this->errors[] = $report; }
    public function all(): array { return $this->errors; }
    public function clear(): void { $this->errors = []; }
}

// --- Breadcrumbs

class Breadcrumbs {
    private array $crumbs = [];
    public function add(string $event, array $data = []) { $this->crumbs[] = ['ts'=>date('c'),'event'=>$event,'data'=>$data]; }
    public function all(): array { return $this->crumbs; }
}

// --- Notification Matrix

class NotificationMatrix {
    private array $rules = [];
    public function register(array $rule, callable $handler) { $this->rules[] = [$rule, $handler]; }
    public function getHandlers(ErrorReport $report): array {
        $found = [];
        foreach ($this->rules as [$rule, $handler]) {
            $match = true;
            foreach ($rule as $k => $v) {
                if ($k === 'match') {
                    if (is_callable($v) && !$v($report)) $match = false;
                    elseif (is_string($v) && !preg_match($v, $report->message)) $match = false;
                } else if (($report->$k ?? null) != $v && !in_array($v, ($report->getTags() ?? []))) $match = false;
            }
            if ($match) $found[] = $handler;
        }
        return $found;
    }
}

// --- ErrorManager (with config validator)

class ErrorManager {
    private static ?self $instance = null;
    protected array $config = [];

    // All property for direct access
    private ExceptionHandlerMap $handlerMap;
    private ?ErrorLogger $logger = null;
    private ?ErrorEventDispatcher $eventDispatcher = null;
    private array $renderers = [];
    private ErrorRendererInterface $defaultRenderer;
    private $responseFactories = [];
    private $responseFactory = null;
    private bool $silent = false;
    private bool $debug = false;
    private ?ErrorCollector $collector = null;
    private TraceIdGeneratorInterface $traceIdGen;
    private $channelSelector = null;
    private ?ErrorReport $lastReport = null;
    private int $errorCount = 0;
    private int $errorLimit = 0;
    private GroupStoreInterface $groupStore;
    private NotificationMatrix $notifMatrix;
    private ?Breadcrumbs $breadcrumbs = null;
    private array $maskFields = [];

    private function __construct(array $config) {
        $this->config = $this->normalizeConfig($config);
        $this->validateConfig($this->config);

        $this->handlerMap        = $this->config['handlerMap'];
        $this->logger            = $this->config['logger'];
        $this->eventDispatcher   = $this->config['eventDispatcher'];
        $this->defaultRenderer   = $this->config['defaultRenderer'];
        $this->responseFactory   = $this->config['responseFactory'];
        $this->renderers         = $this->config['renderers'];
        $this->responseFactories = $this->config['responseFactories'];
        $this->silent            = $this->config['silent'];
        $this->debug             = $this->config['debug'];
        $this->collector         = $this->config['collector'];
        $this->traceIdGen        = $this->config['traceIdGen'];
        $this->channelSelector   = $this->config['channelSelector'];
        $this->errorLimit        = $this->config['errorLimit'];
        $this->groupStore        = $this->config['groupStore'];
        $this->notifMatrix       = $this->config['notifMatrix'];
        $this->breadcrumbs       = $this->config['breadcrumbs'];
        $this->maskFields        = $this->config['maskFields'] ?? [];
    }

    public static function init(array $config): void {
        self::$instance = new self($config);
    }

    protected function normalizeConfig(array $config): array {
        $defaults = [
            'logger'          => null,
            'debug'           => false,
            'silent'          => false,
            'errorLimit'      => 0,
            'groupStore'      => new ErrorGroupStore(),
            'breadcrumbs'     => null,
            'notifMatrix'     => new NotificationMatrix(),
            'defaultRenderer' => new JsonRenderer(),
            'renderers'       => [],
            'responseFactory' => null,
            'responseFactories'=> [],
            'handlerMap'      => new ExceptionHandlerMap(),
            'traceIdGen'      => new DefaultTraceIdGenerator(),
            'eventDispatcher' => null,
            'collector'       => null,
            'maskFields'      => [],
            'channelSelector' => null,
        ];
        $merged = array_merge($defaults, $config);

        // Normalisasi object
        foreach ($merged as $key => $val) {
            if (is_array($val) && isset($val['class'])) {
                $class = $val['class'];
                $args = $val['args'] ?? [];
                $merged[$key] = new $class(...$args);
            } elseif (is_callable($val) && !is_object($val)) {
                $merged[$key] = $val();
            }
        }
        foreach (['renderers','responseFactories'] as $multi) {
            if (!empty($merged[$multi]) && is_array($merged[$multi])) {
                foreach ($merged[$multi] as $k => $v) {
                    if (is_array($v) && isset($v['class'])) {
                        $class = $v['class'];
                        $args = $v['args'] ?? [];
                        $merged[$multi][$k] = new $class(...$args);
                    }
                }
            }
        }
        return $merged;
    }

    /**
     * Config validator: throw InvalidArgumentException jika config tidak valid
     */
    protected function validateConfig(array $cfg): void {
        // Logger: null ATAU implements LoggerInterface
        if ($cfg['logger'] !== null && !($cfg['logger'] instanceof LoggerInterface)) {
            throw new \InvalidArgumentException("Config: 'logger' harus null atau instance of Psr\\Log\\LoggerInterface");
        }
        if (!($cfg['handlerMap'] instanceof ExceptionHandlerMap)) {
            throw new \InvalidArgumentException("Config: 'handlerMap' harus instance of ExceptionHandlerMap");
        }
        if (!($cfg['defaultRenderer'] instanceof ErrorRendererInterface)) {
            throw new \InvalidArgumentException("Config: 'defaultRenderer' harus instance of ErrorRendererInterface");
        }
        if (!is_array($cfg['renderers'])) {
            throw new \InvalidArgumentException("Config: 'renderers' harus array");
        }
        foreach ($cfg['renderers'] as $k => $renderer) {
            if (!($renderer instanceof ErrorRendererInterface)) {
                throw new \InvalidArgumentException("Config: 'renderers[$k]' harus instance of ErrorRendererInterface");
            }
        }
        if (!is_array($cfg['responseFactories'])) {
            throw new \InvalidArgumentException("Config: 'responseFactories' harus array");
        }
        foreach ($cfg['responseFactories'] as $k => $rf) {
            if (!is_null($rf) && !is_callable($rf)) {
                throw new \InvalidArgumentException("Config: 'responseFactories[$k]' harus callable");
            }
        }
        if ($cfg['responseFactory'] !== null && !is_callable($cfg['responseFactory'])) {
            throw new \InvalidArgumentException("Config: 'responseFactory' harus null atau callable");
        }
        if (!($cfg['groupStore'] instanceof GroupStoreInterface)) {
            throw new \InvalidArgumentException("Config: 'groupStore' harus instance of GroupStoreInterface");
        }
        if (!($cfg['notifMatrix'] instanceof NotificationMatrix)) {
            throw new \InvalidArgumentException("Config: 'notifMatrix' harus instance of NotificationMatrix");
        }
        if ($cfg['breadcrumbs'] !== null && !($cfg['breadcrumbs'] instanceof Breadcrumbs)) {
            throw new \InvalidArgumentException("Config: 'breadcrumbs' harus null atau instance of Breadcrumbs");
        }
        if (!($cfg['traceIdGen'] instanceof TraceIdGeneratorInterface)) {
            throw new \InvalidArgumentException("Config: 'traceIdGen' harus instance of TraceIdGeneratorInterface");
        }
        if ($cfg['collector'] !== null && !($cfg['collector'] instanceof ErrorCollector)) {
            throw new \InvalidArgumentException("Config: 'collector' harus null atau instance of ErrorCollector");
        }
        if ($cfg['eventDispatcher'] !== null && !($cfg['eventDispatcher'] instanceof ErrorEventDispatcher)) {
            throw new \InvalidArgumentException("Config: 'eventDispatcher' harus null atau instance of ErrorEventDispatcher");
        }
        if (!is_array($cfg['maskFields'])) {
            throw new \InvalidArgumentException("Config: 'maskFields' harus array");
        }
        if (!is_int($cfg['errorLimit']) || $cfg['errorLimit'] < 0) {
            throw new \InvalidArgumentException("Config: 'errorLimit' harus integer >= 0");
        }
        if (!is_bool($cfg['silent'])) {
            throw new \InvalidArgumentException("Config: 'silent' harus boolean");
        }
        if (!is_bool($cfg['debug'])) {
            throw new \InvalidArgumentException("Config: 'debug' harus boolean");
        }
        if (!is_null($cfg['channelSelector']) && !is_callable($cfg['channelSelector'])) {
            throw new \InvalidArgumentException("Config: 'channelSelector' harus null atau callable");
        }
    }

    public function getConfig(string $key, $default = null) {
        return $this->config[$key] ?? $default;
    }
    public function setConfig(string $key, $value): void {
        $this->config[$key] = $value;
        if(property_exists($this, $key)) $this->$key = $value;
    }

    public static function reset(): void {
        if (PHP_SAPI !== 'cli') trigger_error("ErrorManager::reset() sebaiknya hanya dipakai untuk testing!", E_USER_WARNING);
        self::$instance = null;
    }

    public static function buildReport(\Throwable $e, ErrorContext|array $ctx = [], TraceIdGeneratorInterface $traceIdGen = null, ?GroupStoreInterface $groupStore = null, ?Breadcrumbs $breadcrumbs = null): ErrorReport {
        $context = $ctx instanceof ErrorContext ? $ctx : new ErrorContext($ctx);
        $type = ThrowableClassifier::classify($e);
        $code = ThrowableClassifier::httpCode($e);
        $prev = $e->getPrevious() ? self::buildReport($e->getPrevious(), $context, $traceIdGen, $groupStore, $breadcrumbs) : null;
        $groupStore = $groupStore ?? new ErrorGroupStore();
        $hash = $groupStore->groupHash($e);
        $stat = $groupStore->updateOccurrence($hash);
        $data = $context->all();
        if ($breadcrumbs) $data['breadcrumbs'] = $breadcrumbs->all();
        return new ErrorReport(
            $e->getMessage(),
            $code,
            $type,
            $data,
            $e,
            $prev,
            $traceIdGen,
            $hash,
            $stat['count'],
            $stat['first'],
            $stat['last']
        );
    }
    public static function instance(): self { return self::$instance ?? throw new \RuntimeException("ErrorManager not initialized"); }
    public function enableDebug(): void { $this->debug = true; }
    public function setChannelSelector(callable $selector): void { $this->channelSelector = $selector; }
    public static function registerHandler(string $exceptionClass, callable|string $handler): void { self::instance()->handlerMap->register($exceptionClass, $handler); }
    public static function registerRenderer(string $channel, ErrorRendererInterface $renderer): void { self::instance()->renderers[$channel] = $renderer; }
    public static function registerResponseFactory(string $channel, callable $factory): void { self::instance()->responseFactories[$channel] = $factory; }
    public static function last(): ?ErrorReport { return self::instance()->lastReport; }
    public static function handle(\Throwable $e, ErrorContext|array $context = []): mixed {
        $self = self::instance();
        if ($self->errorLimit > 0 && $self->errorCount >= $self->errorLimit) return null;
        $self->errorCount++;

        $ctxObj = $context instanceof ErrorContext ? $context : new ErrorContext($context);
        $debug = $ctxObj->get('debug') ?? $self->debug;
        $channel = $self->channelSelector ? ($self->channelSelector)($ctxObj) : (php_sapi_name() === 'cli' ? 'cli' : 'default');

        $hash = $self->groupStore->groupHash($e);
        if (!$self->groupStore->allowLog($hash)) return null;
        $report = self::buildReport($e, $ctxObj, $self->traceIdGen, $self->groupStore, $self->breadcrumbs);
        $self->lastReport = $report;

        $sampling = $ctxObj->get('error_sample', 1.0);
        if ($sampling < 1.0 && mt_rand() / mt_getrandmax() > $sampling) return null;
        if ($self->collector) $self->collector->add($report);
        if ($self->logger) $self->logger->log($report);
        if ($self->eventDispatcher) $self->eventDispatcher->dispatch($report);

        foreach ($self->notifMatrix->getHandlers($report) as $notifHandler) $notifHandler($report, $ctxObj);

        $handler = $self->handlerMap->getHandler($e);
        if ($handler) return $handler($report, $ctxObj);
        $factory = $self->responseFactories[$channel] ?? $self->responseFactory;
        if ($factory) return $factory($report);

        $renderer = $self->renderers[$channel] ?? ($channel === 'cli' ? new CliRenderer() : null) ?? $self->defaultRenderer;
        try { $output = $renderer->render($report); }
        catch (\Throwable $ex) {
            $output = $debug ? "Error rendering failed: {$ex->getMessage()}\n---\n{$ex->getTraceAsString()}" : "Error rendering failed: {$ex->getMessage()}";
        }
        if ($self->silent && !$debug) return null;
        if ($debug && $report->exception) $output .= "\n\nDEBUG TRACE:\n" . $report->exception->getTraceAsString();
        return $output;
    }
    public static function handleAsResponse(\Throwable $e, array $context = []): Response|ResponseInterface {
        $out = self::handle($e, $context);
        if ($out instanceof ResponseInterface || $out instanceof Response) return $out;
        return new Response((string)$out, 500);
    }
    public static function registerShutdownHandler(): void {
        register_shutdown_function(function () {
            $err = error_get_last();
            if ($err && in_array($err['type'], [E_ERROR, E_PARSE, E_CORE_ERROR, E_COMPILE_ERROR])) {
                self::handle(
                    new \ErrorException($err['message'], 0, $err['type'], $err['file'], $err['line']),
                    ['source' => 'shutdown']
                );
            }
        });
    }
}