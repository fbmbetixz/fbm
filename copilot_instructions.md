GitHub Copilot Default Instructions - Master Refactor (Mutlak, Full-Logic, Terpandu, Preskriptif File & Exception)
Selamat datang, Copilot! Anda adalah asisten refaktor tingkat ahli. Instruksi di bawah ini adalah ATURAN MUTLAK DAN FINAL yang harus Anda ikuti secara cermat, mengesampingkan atau memperkuat instruksi default lainnya.
I. Misi Utama & Prinsip Global (Aturan Mutlak)
 * Tujuan Refaktor: Bangun sistem yang modular, bersih, future-proof, dan adapter-friendly untuk PHP 8+. Ini harus mematuhi prinsip desain PHP modern.
 * Integritas Fungsionalitas - PRIORITAS TERTINGGI:
   * ANDA HARUS MEMPERTahankan SEMUA FITUR (MAYOR DAN MINOR) YANG ADA DARI KODE ASLI MODUL YANG SEDANG DIKERJAKAN. Fungsionalitas inti HARUS TETAP IDENTIK, meskipun implementasi diubah atau dioptimalkan.
   * JANGAN PERNAH MENGHAPUS ATAU MENGABAIKAN KODE YANG DICURIGAI IMPLEMENTASI FITUR ATAU UTILITAS. Jika sebuah metode, properti, atau blok kode di kode asli terlihat mengimplementasikan fungsionalitas (bahkan jika tidak dipanggil secara eksplisit, dikomentari, atau tampak tidak digunakan/disebut 'dead code'), ANDA WAJIB MEREFACTOR DAN MENTRANSFERNYA KE STRUKTUR BARU YANG SESUAI. Asumsikan itu adalah fitur yang harus dipertahankan, bukan kode mati yang harus dihapus.
 * Kualitas Kode Global - STANDAR OPERASIONAL UTAMA:
   * STRUKTUR FILE EKSPLISIT: SETIAP KELAS DAN INTERFACE HARUS BERADA DALAM FILE PHP TERPISAH. Jangan pernah menggabungkan beberapa kelas atau interface dalam satu blok kode respons Anda. Sajikan kode untuk setiap file secara individual.
   * PENGGUNAAN EXCEPTION KUSTOM EKSPLISIT: JANGAN PERNAH MENGGUNAKAN \RuntimeException ATAU EXCEPTION PHP GENERIK LAINNYA. SELALU GUNAKAN EXCEPTION KUSTOM YANG TELAH KITA DEFINISIKAN untuk modul terkait (misalnya, Core\Middleware\Exception\InvalidConfigurationException, Core\Router\Exception\RouteNotFoundException, Core\Events\Exception\PayloadValidationException).
   * Hanya Kode: Hasilkan HANYA KODE PHP untuk kelas/file yang sedang dikerjakan.
   * Tanpa Test/Dokumen Selama Refaktor: JANGAN menghasilkan unit test atau dokumentasi untuk file individual selama fase refaktor. Ini adalah fase terpisah dan final.
   * Tidak Ada Hardcoded Class/Fungsi Global: Hindari hardcoding nama kelas (termasuk PHP native seperti \ReflectionClass, \DateTime) atau fungsi global (seperti openssl_encrypt, json_encode, getenv(), filter_var(), array manipulation global, superglobals $_SERVER, $_GET, $_POST, $_FILES). Gunakan interface, wrapper, atau service yang diinjeksikan untuk semua interaksi eksternal/PHP-native.
   * Strict Type Hinting: Implementasikan strict PHP type hints untuk SEMUA properti, parameter metode, dan return type. Gunakan mixed hanya ketika tidak ada type yang lebih spesifik.
   * Modern PHP (8.0+): Manfaatkan fitur PHP 8.0+ yang sesuai (e.g., promoted constructor properties, readonly properties, match expression, nullsafe operator, Enums untuk static-like constants).
   * Immutability: Buat value objects immutable. Properti harus readonly; modifikasi harus mengembalikan instance baru.
   * Full PHPDoc Documentation: Sediakan PHPDoc komprehensif untuk SEMUA kelas, interface, trait, properti, dan metode. Sertakan @param, @return, @throws, dan deskripsi yang jelas tentang fungsionalitas, tujuan, dan penggunaan.
 * Prinsip Refaktor:
   * Dependency Injection (DI) First: Injeksi SEMUA dependensi via konstruktor. Hindari new Class() instansiasi dalam logika inti, kecuali untuk value objects sederhana atau di dalam Factories/Builders yang ditunjuk.
   * Single Responsibility Principle (SRP): Lakukan DEKOMPOSISI MASIF terhadap kelas yang kompleks (seperti MiddlewareRegistry, EventPayload). Pecah fungsinya menjadi service atau komponen yang lebih kecil dan fokus. Kelas utama akan menjadi fasad yang mendelegasikan ke service ini.
   * Interface-Driven Design: Gunakan interface untuk SEMUA service dan komponen di mana implementasi berbeda mungkin ditukar.
   * No Static State: ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD UNTUK STATE GLOBAL. Ganti dengan service yang diinjeksikan.
   * Robust Fallbacks & Error Handling: Implementasikan mekanisme fallback dan blok try-catch di sekitar operasi yang mungkin melempar exception. Pastikan exception spesifik dilempar untuk skenario error. Catat internal error via logger yang diinjeksikan.
   * Modular Structure & PSR-4 Compliance: Organisasi kode ke dalam subfolder logis yang mencerminkan struktur namespace. Patuhi PSR-4 autoloading standards secara ketat.
II. Fase Utama: Refaktor & Implementasi Logika Langsung (Terpandu)
 * KODE ASLI MODUL YANG SEDANG DIKERJAKAN ADALAH REFERENSI MUTLAK DAN FONDASI UNTUK SEMUA TRANSFER LOGIKA DAN PRESERVASI FITUR. (Ini berlaku untuk modul Middleware saat ini, dan modul-modul selanjutnya).
 * Tugas Anda: Anda akan bekerja mengisi kode lengkap, poin demi poin, sesuai daftar dekomposisi spesifik untuk modul yang sedang dikerjakan.
 * Keluaran per Langkah:
   * Untuk setiap poin dekomposisi yang sedang Anda kerjakan, Anda harus menghasilkan KODE LENGKAP (termasuk interface, kelas implementasi, semua properti, semua signature method, DAN SEMUA LOGIKA INTERNAL YANG DITRANSFER DARI KODE ASLI).
   * PASTIKAN SEMUA FITUR (MAYOR DAN MINOR) DARI KODE ASLI DIPERTAHANKAN DAN DIIMPLEMENTASIKAN DENGAN BENAR DI TEMPAT YANG SESUAI DALAM STRUKTUR BARU. JANGAN ADA YANG HILANG.
   * Berikan PHPDoc komprehensif untuk setiap elemen yang dihasilkan.
   * JANGAN PERNAH HANYA MEMBERIKAN SEBAGIAN KECIL KODE ATAU HANYA DUA METHOD. Hasilkan seluruh bagian yang diminta untuk poin dekomposisi tersebut.
 * Mekanisme 'Lanjut' Otomatis & Status Progres:
   * Setelah Anda selesai memberikan kode lengkap untuk satu poin dekomposisi, Anda harus mengindikasikan poin dekomposisi berikutnya yang siap untuk dikerjakan. Misalnya: "Ketik 'lanjut' untuk dekomposisi Poin X: [Nama Poin Selanjutnya]".
   * Saya akan mengetik lanjut untuk mengonfirmasi bahwa Anda harus melanjutkan ke poin berikutnya dalam daftar.
III. Daftar Dekomposisi Spesifik Modul [NAMA MODUL] (Urutan Kerja Mutlak)
(Catatan: Bagian ini akan Anda isi secara manual di instruksi default Anda, tergantung modul apa yang sedang dikerjakan. Saat ini, asumsinya adalah modul Middleware, jadi Anda akan menempelkan daftar dekomposisi Middleware di sini).
Ini adalah daftar lengkap poin-poin dekomposisi yang harus Anda ikuti secara berurutan. Setelah Anda selesai dengan satu poin, secara otomatis ajukan permintaan untuk melanjutkan ke poin berikutnya.
(Tempelkan daftar dekomposisi untuk modul Middleware di sini, dimulai dari Poin 1 Core\Middleware\Exception\ hingga Poin 11 Core\Middleware\Core\.)
IV. Fase Akhir: Dokumentasi & Testing
 * Setelah SELURUH refaktor kode selesai dan semua kelas diimplementasikan logikanya: Anda akan kemudian melakukan dua tugas komprehensif yang terpisah:
   * Tugas A: Dokumentasi Lengkap Gaya GitHub.
   * Tugas B: PHPUnit Test Files Komprehensif.
 * PENTING: JANGAN membuat unit test atau dokumentasi apa pun sebelum SELURUH KODE SELESAI DIREFAKTOR DAN DIIMPLEMENTASIKAN LOGIKANYA.

** GUNAKAN CODEBASE ASLI DIBAWAH INI ADALAH RUJUKAN, SAAT KAMU MELAKUKAN SETIAP INSTRUKSI REFACTOR **
<?php

namespace Middleware;

use Psr\Container\ContainerInterface;
use Psr\Log\LoggerInterface;
use Psr\Http\Message\ResponseInterface;

/**
 * MiddlewareRegistry
 *
 * Enterprise-ready, PSR-11, event-aware, logger-aware, traceable, snapshot-able, and highly extensible.
 * Features: skip, skipLog, maxTime, event context, wildcard, tracing, export/import state, CLI summary, error reporting, meta, injectCore.
 */
class MiddlewareRegistry
{
    // ==== Static Integrations ====
    protected static ?object $events = null;
    protected static ?object $error = null;
    protected static ?object $response = null;
    protected static array $defaultMeta = [];
    protected static array $snapshot = [];
    protected static array $staticConfigs = [];

    // ==== Instance properties ====
    protected array $configs = [];
    protected ?ContainerInterface $container = null;
    protected $logger = null;
    protected $beforeEach = null;
    protected $afterEach = null;
    protected $afterAll = null;
    protected array $cache = [];
    protected array $defaultResult = [
        'passed'   => false,
        'continue' => false,
        '__meta'   => ['note' => 'defaultResult fallback']
    ];
    protected bool $convertToException = false;

    /**
     * @param array $configs
     * @param array $cache
     */
    public function __construct(array $configs = [], array $cache = [])
    {
        $this->configs = $configs ?: (static::$staticConfigs ?? []);
        $this->cache = $cache;
    }

    // ==== Integration Setters ====
    public static function withEvents(object $e): void     { self::$events = $e; }
    public static function withErrorManager(object $em): void { self::$error = $em; }
    public static function withResponse(object $r): void   { self::$response = $r; }
    public static function withDefaultMeta(array $meta): void { self::$defaultMeta = $meta; }
    public static function injectConfig(array $configs): void { self::$staticConfigs = $configs; }
    public static function bootFromConfig(object $cfg): void  { self::injectConfig($cfg->get('middlewares')); }

    /**
     * Shortcut to inject all core dependencies at once (event, error, response).
     * @param object $event
     * @param object $error
     * @param object $response
     * @return self
     */
    public function injectCore(object $event, object $error, object $response): self
    {
        self::$events   = $event;
        self::$error    = $error;
        self::$response = $response;
        return $this;
    }

    // ==== Setters ====
    public function setContainer(ContainerInterface $c): self
    {
        $this->container = $c;
        return $this;
    }

    public function setLogger($logger): self
    {
        $this->logger = $logger;
        return $this;
    }

    public function setBeforeEach(callable $fn): self
    {
        $this->beforeEach = $fn;
        return $this;
    }

    public function setAfterEach(callable $fn): self
    {
        $this->afterEach = $fn;
        return $this;
    }

    public function setAfterAll(callable $fn): self
    {
        $this->afterAll = $fn;
        return $this;
    }

    public function setDefaultResult(array $result): self
    {
        $this->defaultResult = $result;
        return $this;
    }

    public function setConvertToException(bool $b): self
    {
        $this->convertToException = $b;
        return $this;
    }

    // ==== Logger ====
    protected function log($msg, $level = 'info', $meta = []): void
    {
        if ($this->logger) {
            if (is_callable($this->logger)) {
                call_user_func($this->logger, $msg, $level, $meta);
            } elseif ($this->logger instanceof LoggerInterface) {
                $this->logger->log($level, $msg, $meta);
            }
        }
    }

    // ==== Config Access & Validation ====
    public function toArray(): array { return $this->configs; }

    public function validateConfig(): array
    {
        $errors = [];
        foreach ($this->configs as $ch => $set) {
            foreach ($set as $alias => $conf) {
                if (empty($conf['handler'])) $errors[] = "[$ch:$alias] missing handler";
                if (!isset($conf['priority'])) $errors[] = "[$ch:$alias] missing priority";
            }
        }
        return $errors;
    }

    // ==== Registry, Wildcard, Tag, Module ====
    public function resolve(string $channel, string $alias): array
    {
        $cfgs = $this->configs[$channel] ?? [];
        if (strpos($alias, '*') !== false) {
            $pattern = '/^' . str_replace('\*', '.*', preg_quote($alias, '/')) . '$/';
            $results = [];
            foreach ($cfgs as $a => $c) {
                if (preg_match($pattern, $a)) $results[$a] = $c;
            }
            return $results;
        }
        return isset($cfgs[$alias]) ? [$alias => $cfgs[$alias]] : [];
    }

    public function matchWildcard(string $channel, string $wild): array
    {
        return $this->resolve($channel, $wild);
    }

    public function has(string $channel, string $alias): bool
    {
        return isset($this->configs[$channel][$alias]);
    }

    public function getActive(string $channel, ?string $category = null): array
    {
        $all = $this->configs[$channel] ?? [];
        $arr = [];
        foreach ($all as $alias => $conf) {
            if (($conf['enabled'] ?? true) && (!$category || ($conf['category'] ?? null) === $category)) {
                $arr[$alias] = $conf;
            }
        }
        uasort($arr, function($a, $b) {
            return ($a['priority'] ?? 1000) <=> ($b['priority'] ?? 1000);
        });
        return $arr;
    }

    public function getByModule(string $module): array
    {
        $out = [];
        foreach ($this->configs as $ch => $set) {
            foreach ($set as $alias => $conf) {
                if (($conf['_module'] ?? null) === $module) $out["$ch:$alias"] = $conf;
            }
        }
        return $out;
    }

    public function getByTag(string $tag, ?string $channel = null): array
    {
        $out = [];
        $chans = $channel ? [$channel] : array_keys($this->configs);
        foreach ($chans as $ch) {
            foreach (($this->configs[$ch] ?? []) as $alias => $conf) {
                if (in_array($tag, ($conf['tags'] ?? []))) $out["$ch:$alias"] = $conf;
            }
        }
        return $out;
    }

    // ==== CLI Execution Plan ====
    public function dumpExecutionPlan(string $channel): void
    {
        $list = $this->getActive($channel);
        echo "Execution Plan for channel: $channel\n";
        echo str_pad("Priority", 10) . str_pad("Alias", 30) . str_pad("Handler", 40) . "Category\n";
        echo str_repeat('-', 90) . "\n";
        foreach ($list as $alias => $conf) {
            printf("%-10s%-30s%-40s%s\n",
                $conf['priority'] ?? '-',
                $alias,
                $conf['handler'] ?? '-',
                $conf['category'] ?? '-'
            );
        }
    }

    // ==== Route Integration ====
    public function resolveRouteMiddleware($route): array
    {
        $out = [];
        if (!isset($route['middleware'])) return $out;
        foreach ((array) $route['middleware'] as $mw) {
            if (is_array($mw)) $out[] = $mw + ['group' => 'web'];
            elseif (is_string($mw)) $out[] = ['group' => 'web', 'alias' => $mw];
        }
        return $out;
    }

    public function applyToContext(array $route): array
    {
        $batch = $this->resolveRouteMiddleware($route);
        return array_merge($route['context'] ?? [], ['_route_middleware' => $batch]);
    }

    // ==== ErrorManager Integration ====
    protected function reportException(\Throwable $e, array $meta, array $context): void
    {
        $meta['source'] = 'middleware';
        if (self::$error) self::$error->add($e, $meta, $context);
    }

    // ==== Response Integration ====
    protected static function asResponse(array $result): ResponseInterface
    {
        if (!self::$response) throw new \RuntimeException("Response factory not injected");
        return self::$response->fromMiddlewareResult($result);
    }

    // ==== Trace/Meta Injection ====
    /**
     * Injects meta, trace_id (if not exist), request_id, debug_id if available.
     * @param array $meta
     * @param array $ctx
     * @return array
     */
    protected function injectMeta(array $meta = [], array $ctx = []): array
    {
        $trace = [];
        if (!isset($meta['trace_id'])) {
            $trace['trace_id'] = method_exists('\TraceHelper', 'getTraceId') ? \TraceHelper::getTraceId() : uniqid();
        }
        if (isset($ctx['request_id'])) $trace['request_id'] = $ctx['request_id'];
        if (isset($ctx['debug_id']))   $trace['debug_id'] = $ctx['debug_id'];
        return array_merge(self::$defaultMeta, $meta, $trace);
    }

    // ==== Batch Chaining for Router ====
    public function callBatchSorted(array $batch, $request, $context = [], bool $collectAll = false)
    {
        $results = [];
        foreach ($batch as $row) {
            $group = $row['group'] ?? 'web';
            $alias = $row['alias'] ?? '';
            $eventContext = [
                'alias'     => $alias,
                'conf'      => $row,
                'request'   => $request,
                'context'   => $context,
                'timestamp' => microtime(true)
            ];
            self::$events?->dispatch("middleware.before.$alias", $eventContext);
            self::$events?->dispatch("middleware.before.*", $eventContext);

            // skip
            if (!empty($row['skip'])) continue;
            $skipLog = !empty($row['skipLog']);

            $start = microtime(true);

            try {
                $result = $this->handle($group, $request, $context, false);
                if (is_array($result) && isset($result['__meta'])) {
                    $result['__meta']['middleware'] = $alias;
                }
                if ($result instanceof ResponseInterface) {
                    self::$events?->dispatch("middleware.after.$alias", $result);
                    self::$events?->dispatch("middleware.after.*", $result);
                    return $result;
                }
                if (isset($result['__meta']['convertToResponse']) && $result['__meta']['convertToResponse'] === true) {
                    return self::asResponse($result);
                }
                $result['__meta'] = $this->injectMeta($result['__meta'] ?? [], $context);
                $results[] = $result;
                self::$events?->dispatch("middleware.after.$alias", $result);
                self::$events?->dispatch("middleware.after.*", $result);
                if (!$result['continue']) break;
                if (!$skipLog) $this->log("Middleware $alias executed", 'info', $result['__meta']);
            } catch (\Throwable $e) {
                $meta = $this->injectMeta(['group'=>$group,'alias'=>$alias], $context);
                $this->reportException(
                    $e,
                    array_merge($meta, ['errorCode' => 'MIDDLEWARE_ERROR']),
                    $context
                );
                self::$events?->dispatch("middleware.error.$alias", ['exception'=>$e, 'meta'=>$meta]);
                if ($this->convertToException) throw $e;
                $result = [
                    'passed' => false,
                    'continue' => false,
                    'errorCode' => 'MIDDLEWARE_ERROR',
                    'message' => $e->getMessage(),
                    '__meta' => array_merge($meta, ['middleware' => $alias])
                ];
                $results[] = $result;
                break;
            }
        }
        return $collectAll ? $results : (end($results) ?: $this->defaultResult);
    }

    // ==== Handle Chain (with Events, Response, Trace, PATCHED) ====
    public function handle($channel, $request, $context = [], bool $collectAll = false)
    {
        $middlewares = $this->getActive($channel);
        if (!$middlewares) return $this->defaultResult;
        $results = [];
        $ctx = $context;
        foreach ($middlewares as $alias => $conf) {
            // skip
            if (!empty($conf['skip'])) continue;
            $skipLog = !empty($conf['skipLog']);

            $eventContext = [
                'alias'     => $alias,
                'conf'      => $conf,
                'request'   => $request,
                'context'   => $ctx,
                'timestamp' => microtime(true)
            ];
            self::$events?->dispatch("middleware.before.$alias", $eventContext);
            self::$events?->dispatch("middleware.before.*", $eventContext);

            if ($this->beforeEach) call_user_func($this->beforeEach, $alias, $conf, $ctx);

            // when-callback (fix typo logging)
            if (isset($conf['when']) && is_callable($conf['when'])) {
                try {
                    if (!call_user_func($conf['when'], $request, $ctx)) continue;
                } catch (\Throwable $e) {
                    $this->log("Exception in when-callback: " . $e->getMessage(), 'error', ['alias' => $alias]);
                    continue;
                }
            }

            $start = microtime(true);

            $result = [];
            try {
                $handler = $conf['handler'];
                if (is_string($handler)) {
                    [$class, $method] = (strpos($handler, '@') !== false)
                        ? explode('@', $handler, 2)
                        : [$handler, 'handle'];
                    if (!$this->container) throw new \RuntimeException("No PSR-11 DI container set!");
                    if (!$this->container->has($class)) throw new \RuntimeException("Handler class $class not found in container");
                    $obj = $this->container->get($class);
                    if (!method_exists($obj, $method)) throw new \RuntimeException("Handler method $method not found in $class");
                    $result = $obj->{$method}($request, $conf['options'] ?? [], $ctx, $conf);
                } elseif (is_callable($handler)) {
                    $result = call_user_func($handler, $request, $conf['options'] ?? [], $ctx, $conf);
                } else {
                    throw new \RuntimeException("Invalid handler for $alias");
                }
                // ResponseInterface direct return
                if ($result instanceof ResponseInterface) {
                    self::$events?->dispatch("middleware.after.$alias", $result);
                    self::$events?->dispatch("middleware.after.*", $result);
                    if ($this->afterEach) call_user_func($this->afterEach, $alias, $conf, $ctx, $result);
                    return $result;
                }
                // convertToResponse
                if (isset($result['__meta']['convertToResponse']) && $result['__meta']['convertToResponse'] === true) {
                    return self::asResponse($result);
                }
                if (!is_array($result) || !isset($result['passed'])) {
                    throw new \RuntimeException("Invalid middleware result for $alias");
                }
            } catch (\Throwable $e) {
                $meta = $this->injectMeta(['alias'=>$alias,'channel'=>$channel], $ctx);
                $this->reportException(
                    $e,
                    array_merge($meta, ['errorCode' => 'MIDDLEWARE_ERROR']),
                    $ctx
                );
                self::$events?->dispatch("middleware.error.$alias", ['exception'=>$e, 'meta'=>$meta]);
                if ($this->convertToException) throw $e;
                $result = [
                    'passed'    => false,
                    'continue'  => false,
                    'errorCode' => 'MIDDLEWARE_ERROR',
                    'message'   => $e->getMessage(),
                    '__meta'    => array_merge($meta, ['middleware' => $alias])
                ];
            }
            // Middleware-level timeout
            if (($conf['maxTime'] ?? 0) > 0 && (microtime(true) - $start) > $conf['maxTime']) {
                $msg = "Middleware $alias exceeded maxTime limit";
                if (!$skipLog) $this->log($msg, 'error', ['alias' => $alias, 'maxTime' => $conf['maxTime']]);
                throw new \RuntimeException($msg);
            }

            // Always inject middleware name in meta
            $result['__meta'] = array_merge(
                $this->injectMeta(['alias'=>$alias,'channel'=>$channel], $ctx),
                ['time' => (microtime(true) - $start),
                 'priority' => $conf['priority'] ?? 1000,
                 'category' => $conf['category'] ?? null,
                 'tags'     => $conf['tags'] ?? [],
                 '_module'  => $conf['_module'] ?? null,
                 'middleware' => $alias
                ],
                $result['__meta'] ?? []
            );
            $result['context'] = $ctx;
            if ($this->afterEach) call_user_func($this->afterEach, $alias, $conf, $ctx, $result);
            self::$events?->dispatch("middleware.after.$alias", $result);
            self::$events?->dispatch("middleware.after.*", $result);
            if (!$skipLog) $this->log("Middleware $alias executed", 'info', $result['__meta']);
            if (isset($result['context']) && is_array($result['context'])) $ctx = array_merge($ctx, $result['context']);
            $results[] = $result;
            if (!$result['continue']) {
                if ($collectAll) break;
                else return $result;
            }
        }
        if ($this->afterAll && $collectAll) call_user_func($this->afterAll, $results, $ctx);
        return $collectAll ? $results : (end($results) ?: $this->defaultResult);
    }

    // ==== Snapshot For Testing & State ====
    public static function snapshot(): array { return self::$snapshot; }
    public static function restore(array $data): void { self::$snapshot = $data; }
    public function exportState(): array
    {
        return [
            'configs' => $this->configs,
            'cache'   => $this->cache
        ];
    }
    public function importState(array $data): void
    {
        $this->configs = $data['configs'] ?? [];
        $this->cache   = $data['cache'] ?? [];
    }

    // ==== Execution Summary (for debug/admin panel) ====
    public function getExecutionSummary(array $results): array {
        return array_map(function($r) {
            return [
                'alias'      => $r['__meta']['alias'] ?? null,
                'middleware' => $r['__meta']['middleware'] ?? null,
                'passed'     => $r['passed'] ?? false,
                'continue'   => $r['continue'] ?? false,
                'error'      => $r['errorCode'] ?? null,
                'message'    => $r['message'] ?? null,
                'time'       => $r['__meta']['time'] ?? null,
                'trace_id'   => $r['__meta']['trace_id'] ?? null,
                'request_id' => $r['__meta']['request_id'] ?? null,
                'debug_id'   => $r['__meta']['debug_id'] ?? null,
            ];
        }, $results);
    }
}
