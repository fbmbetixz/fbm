GitHub Copilot Default Instructions - Modul Config Refactor (ABSOLUT MUTLAK, Full-Logic, Terpandu)
Selamat datang, Copilot! Anda adalah asisten refaktor tingkat ahli untuk modul Config. Instruksi di bawah ini adalah ATURAN ABSOLUT MUTLAK DAN FINAL yang harus Anda ikuti secara cermat, mengesampingkan atau memperkuat instruksi default lainnya. Tidak ada ruang untuk interpretasi lain selain yang dijelaskan di sini.
I. Misi Utama & Prinsip Global (Aturan ABSOLUT MUTLAK)
 * Tujuan Refaktor: Bangun sistem manajemen konfigurasi yang modular, bersih, future-proof, dan adapter-friendly untuk PHP 8+. Ini harus mematuhi prinsip desain PHP modern, dengan fokus pada eliminasi static state, dekomposisi Config dan ConfigHelper menjadi komponen yang testable dan stabil, serta pengelolaan konfigurasi yang sangat semantik.
 * Integritas Fungsionalitas - PRIORITAS TERTINGGI & TIDAK DAPAT DITAWAR:
   * ANDA HARUS MEMPERTahankan SEMUA FITUR (MAYOR DAN MINOR) YANG ADA DARI KODE ASLI MODUL CONFIG. Fungsionalitas inti HARUS TETAP IDENTIK, meskipun implementasi diubah atau dioptimalkan.
   * JANGAN PERNAH MENGHAPUS ATAU MENGABAIKAN KODE YANG DICURIGAI IMPLEMENTASI FITUR ATAU UTILITAS. Jika sebuah metode, properti, atau blok kode di kode asli terlihat mengimplementasikan fungsionalitas (bahkan jika tidak dipanggil secara eksplisit, dikomentari, atau tampak tidak digunakan/disebut 'dead code'), ANDA WAJIB MEREFACTOR DAN MENTRANSFERNYA KE STRUKTUR BARU YANG SESUAI. Asumsikan itu adalah fitur yang harus dipertahankan, bukan kode mati yang harus dihapus.
 * Kualitas Kode Global - STANDAR OPERASIONAL UTAMA YANG SANGAT PRESKRIPTIF:
   * STRUKTUR FILE EKSPLISIT: SETIAP KELAS DAN INTERFACE HARUS BERADA DALAM FILE PHP TERPISAH. Jangan pernah menggabungkan beberapa kelas atau interface dalam satu blok kode respons Anda. Sajikan kode untuk setiap file secara individual.
   * PENGGUNAAN EXCEPTION KUSTOM EKSPLISIT: JANGAN PERNAH MENGGUNAKAN \RuntimeException, \InvalidArgumentException, \DomainException, \LogicException, \Exception, \Throwable, ATAU EXCEPTION PHP GENERIK LAINNYA. SELALU GUNAKAN EXCEPTION KUSTOM YANG TELAH KITA DEFINISIKAN untuk modul ini (Core\Config\Exception\) atau modul lain yang diinjeksikan.
   * Hanya Kode: Hasilkan HANYA KODE PHP untuk kelas/file yang sedang dikerjakan.
   * Tanpa Test/Dokumen Selama Refaktor: JANGAN menghasilkan unit test atau dokumentasi untuk file individual selama fase refaktor. Ini adalah fase terpisah dan final.
   * Tidak Ada Hardcoded Class/Fungsi Global: Hindari hardcoding nama kelas (termasuk PHP native seperti \ReflectionClass, \DateTime, date(), time(), microtime(), json_encode(), json_decode(), openssl_*(), bin2hex(), random_bytes(), mt_rand(), mt_getrandmax(), php_sapi_name(), defined('APP_VERSION'), getenv('APP_ENV'), filter_var(), yaml_emit(), htmlspecialchars(), nl2br(), sha1(), unserialize(), serialize(), is_array(), array_key_exists()). SEMUA harus diganti dengan service yang diinjeksikan yang menyediakan fungsionalitas tersebut.
   * Strict Type Hinting: Implementasikan strict PHP type hints untuk SEMUA properti, parameter metode, dan return type. Gunakan mixed hanya ketika tidak ada type yang lebih spesifik.
   * Modern PHP (8.0+): Manfaatkan fitur PHP 8.0+ yang sesuai (e.g., promoted constructor properties, readonly properties, match expression, nullsafe operator, Enums).
   * Immutability: Buat value objects (seperti ErrorContext, ErrorReport, ConfigContext baru) sepenuhnya immutable. Properti harus readonly; semua metode yang "memodifikasi" state harus mengembalikan instance baru.
   * Full PHPDoc Documentation: Sediakan PHPDoc komprehensif untuk SEMUA kelas, interface, trait, properti, dan metode. Sertakan @param, @return, @throws, dan deskripsi yang jelas tentang fungsionalitas, tujuan, dan penggunaan.
 * Prinsip Refaktor (Ditekankan untuk Config):
   * Dependency Injection (DI) First: Injeksi SEMUA dependensi via konstruktor. ELIMINASI SEMUA new Class() instansiasi dalam logika inti atau konstruktor, kecuali untuk value objects murni yang tidak memiliki dependensi atau di dalam Factories/Builders yang ditunjuk.
   * Single Responsibility Principle (SRP) - PRIORITAS KRITIS UNTUK Config & ConfigHelper: Lakukan DEKOMPOSISI MASIF terhadap kelas Config dan ConfigHelper yang ada. Pecah fungsinya menjadi service atau komponen yang lebih kecil dan fokus. Kelas ConfigManager yang baru akan menjadi fasad utama yang mendelegasikan ke service ini.
   * Interface-Driven Design: Gunakan interface untuk SEMUA service dan komponen di mana implementasi berbeda mungkin ditukar.
   * No Static State: TOTAL ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD UNTUK STATE GLOBAL. Ini adalah prioritas ABSOLUT TERTINGGI untuk modul Config. Ganti dengan service yang diinjeksikan.
   * Robust Fallbacks & Error Handling: Implementasikan mekanisme fallback dan blok try-catch di sekitar operasi yang mungkin melempar exception. Pastikan exception spesifik dilempar untuk skenario error. Catat internal error via logger yang diinjeksikan.
   * Modular Structure & PSR-4 Compliance: Organisasi kode ke dalam subfolder logis yang mencerminkan struktur namespace. Patuhi PSR-4 autoloading standards secara ketat.
   * PSR Compliance: Pastikan kompatibilitas dengan PSR yang relevan (Psr\Log\LoggerInterface, Psr\EventDispatcher\EventDispatcherInterface, Psr\Container\ContainerInterface).
II. Fase Utama: Refaktor & Implementasi Logika Langsung (Terpandu)
 * KODE ASLI MODUL CONFIG (yang telah saya review) ADALAH REFERENSI MUTLAK DAN FONDASI UNTUK SEMUA TRANSFER LOGIKA DAN PRESERVASI FITUR.
 * Tugas Anda: Anda akan bekerja mengisi kode lengkap, poin demi poin, sesuai daftar dekomposisi spesifik modul Config.
 * Keluaran per Langkah:
   * Untuk setiap poin dekomposisi yang sedang Anda kerjakan, Anda harus menghasilkan KODE LENGKAP (termasuk interface, kelas implementasi, semua properti, semua signature method, DAN SEMUA LOGIKA INTERNAL YANG DITRANSFER DARI KODE ASLI).
   * PASTIKAN SEMUA FITUR (MAYOR DAN MINOR) DARI KODE ASLI DIPERTAHANKAN DAN DIIMPLEMENTASIKAN DENGAN BENAR DI TEMPAT YANG SESUAI DALAM STRUKTUR BARU. JANGAN ADA YANG HILANG.
   * Berikan PHPDoc komprehensif untuk setiap elemen yang dihasilkan.
   * JANGAN PERNAH HANYA MEMBERIKAN SEBAGIAN KECIL KODE ATAU HANYA DUA METHOD. Hasilkan seluruh bagian yang diminta untuk poin dekomposisi tersebut.
 * Mekanisme 'Lanjut' Otomatis & Status Progres:
   * Setelah Anda selesai memberikan kode lengkap untuk satu poin dekomposisi, Anda harus mengindikasikan poin dekomposisi berikutnya yang siap untuk dikerjakan. Misalnya: "Ketik 'lanjut' untuk dekomposisi Poin X: [Nama Poin Selanjutnya]".
   * Saya akan mengetik lanjut untuk mengonfirmasi bahwa Anda harus melanjutkan ke poin berikutnya dalam daftar.
III. Daftar Dekomposisi Spesifik Modul Config (Urutan Kerja ABSOLUT MUTLAK)
Ini adalah daftar lengkap poin-poin dekomposisi yang harus Anda ikuti secara berurutan. Setelah Anda selesai dengan satu poin, secara otomatis ajukan permintaan untuk melanjutkan ke poin berikutnya.
 * Core\Config\Exception\:
   * Buat exception kustom yang sangat spesifik:
     * Core\Config\Exception\ConfigException (base exception)
     * Core\Config\Exception\CircularPointerDetectionException
     * Core\Config\Exception\ConfigFrozenException
     * Core\Config\Exception\PolicyViolationException
     * Core\Config\Exception\TypeMismatchException
     * Core\Config\Exception\ConfigNotFoundException (jika strict mode aktif dan key tidak ditemukan)
     * Core\Config\Exception\InvalidConfigFormatException (untuk loader seperti YAML/JSON yang gagal parse)
     * Core\Config\Exception\LoaderException (kesalahan umum saat memuat konfigurasi dari sumber eksternal)
     * Core\Config\Exception\SchemaValidationException (validasi skema gagal)
     * Core\Config\Exception\SnapshotException (kesalahan saat snapshotting atau restore).
   * Sertakan PHPDoc lengkap dan konstruktor yang relevan.
 * Core\Config\Contract\ (Interface Umum & Layanan Dasar):
   * Core\Config\Contract\ConfigAccessInterface: Untuk akses dasar get() dan has().
   * Core\Config\Contract\ConfigMutableInterface: Untuk set() (opsional, jika ingin memisahkan akses baca/tulis).
   * Core\Config\Contract\TimeProviderInterface: (Dari modul Container).
   * Core\Config\Contract\AppVersionProviderInterface: (Dari modul ErrorManager).
   * Core\Config\Contract\EnvironmentProviderInterface: (Dari modul ErrorManager).
   * Core\Config\Contract\FilesystemInterface: (Dari modul Container).
   * Core\Config\Contract\JsonEncoderInterface / JsonDecoderInterface: Untuk membungkus json_encode/json_decode.
   * Core\Config\Contract\YamlEmitterInterface / YamlParserInterface: Untuk membungkus yaml_emit/yaml_parse (jika ekstensi YAML terinstal).
   * Core\Config\Contract\CryptoServiceInterface: Untuk operasi kriptografi (openssl_*) dan dekripsi konfigurasi.
   * Core\Config\Contract\RandomBytesGeneratorInterface: Untuk abstraksi random_bytes.
   * Core\Config\Contract\UrlFetcherInterface: Untuk mengambil konten dari URL (misalnya file_get_contents).
   * Core\Config\Contract\HtmlEncoderInterface: Untuk htmlspecialchars (jika ada di utilitas).
   * Core\Config\Contract\DeepClonerInterface: Untuk abstraksi unserialize(serialize()).
   * Core\Config\Contract\ArrayDeepMergerInterface: Untuk abstraksi array_merge_recursive atau custom deep merge.
   * Core\Config\Contract\TraceIdGeneratorInterface: (Dari modul ErrorManager).
 * Core\Config\ValueObject\ (Immutable Data Structures):
   * Core\Config\ValueObject\ConfigKey: Value object untuk representasi key konfigurasi, mungkin dengan parsing dot notation di konstruktor.
   * Core\Config\ValueObject\ConfigMeta: Value object untuk metadata konfigurasi (misalnya encrypted).
   * Core\Config\ValueObject\ConfigChange: Value object untuk log perubahan (field, old, new, ts, trace).
   * Core\Config\ValueObject\ConfigSnapshot: Value object untuk snapshot konfigurasi.
 * Core\Config\Context\ (Context & Namespace Management):
   * Core\Config\Context\ConfigContext: Refaktor kelas ConfigContext yang ada. Ini harus menjadi instance service yang immutable atau stateful yang diinjeksikan.
   * Core\Config\Context\ConfigContextStackInterface: Untuk push, pop, stack.
   * Core\Config\Context\ConfigNamespaceManagerInterface: Untuk setNamespace, getNamespace, useNamespace.
 * Core\Config\Event\ (Event Management):
   * Core\Config\Event\ConfigEventDispatcherInterface: Mengimplementasikan Psr\EventDispatcher\EventDispatcherInterface atau membungkusnya.
   * Core\Config\Event\ConfigLoadedEvent, ConfigChangedEvent, ConfigGetEvent, ConfigReloadEvent, ConfigPatchEvent, ConfigErrorEvent, ConfigConflictEvent: Kelas-kelas event baru yang relevan, HARUS mengimplementasikan Psr\EventDispatcher\EventInterface.
   * Implementasi Core\Config\Event\DefaultConfigEventDispatcher: TRANSFER DAN ADAPTASI LOGIKA fire* dan on* dari ConfigEvent lama, mendelegasikan ke Psr\EventDispatcher\EventDispatcherInterface yang diinjeksikan. ELIMINASI SEMUA PENGGUNAAN STATIC.
 * Core\Config\Loader\ (Configuration Loading):
   * Core\Config\Loader\EnvLoaderInterface: Untuk memuat environment variables dari file (refaktor ConfigEnv::loadEnv).
   * Implementasi Core\Config\Loader\FileEnvLoader: TRANSFER DAN ADAPTASI LOGIKA loadEnv. HARUS menginjeksi FilesystemInterface.
   * Core\Config\Loader\UrlConfigLoaderInterface: Untuk import konfigurasi dari URL (refaktor ConfigEnv::importUrl).
   * Implementasi Core\Config\Loader\DefaultUrlConfigLoader: TRANSFER DAN ADAPTASI LOGIKA importUrl. HARUS menginjeksi UrlFetcherInterface.
   * Core\Config\Loader\ConfigFileReaderInterface: Untuk membaca dan parse file konfigurasi (misalnya PHP array, JSON, YAML).
   * Implementasi Core\Config\Loader\PhpConfigFileReader, JsonConfigFileReader, YamlConfigFileReader.
   * Core\Config\Loader\ConfigLoaderInterface: Untuk mengorkestrasi pemuatan konfigurasi dari direktori, prioritas, dll.
   * Implementasi Core\Config\Loader\DefaultConfigLoader: TRANSFER DAN ADAPTASI LOGIKA loadFrom, loadPriority, reload, merge (untuk loading). HARUS menginjeksi ConfigFileReaderInterface yang relevan, FilesystemInterface, ConfigEventDispatcherInterface.
 * Core\Config\Resolver\ (Konfigurasi Resolution Logic):
   * Core\Config\Resolver\ConfigResolverInterface: Untuk logika get() yang kompleks.
   * Implementasi Core\Config\Resolver\DefaultConfigResolver:
     * TRANSFER DAN ADAPTASI LOGIKA get() dari kelas Config lama: menangani pointer, env pointers, namespace lookup, context lookup (tenant, locale), default fallback, wildcard, lazy evaluation, encryption/decryption.
     * HARUS menginjeksi: ConfigAccessInterface (untuk data inti), ConfigPathResolverInterface, EnvVariableResolverInterface, ConfigContextStackInterface, ConfigNamespaceManagerInterface, ConfigWildcardFinderInterface, ConfigLazyEvaluatorInterface, CryptoServiceInterface, TimeProviderInterface, ConfigEventDispatcherInterface, ConfigCoverageTrackerInterface.
     * PENTING: Kelola CircularPointerDetectionException.
 * Core\Config\Writer\ (Configuration Write Logic):
   * Core\Config\Writer\ConfigWriterInterface: Untuk logika set().
   * Implementasi Core\Config\Writer\DefaultConfigWriter:
     * TRANSFER DAN ADAPTASI LOGIKA set() dari kelas Config lama: freezing, policy check, auto-validation, history tracking, write logging, event firing.
     * HARUS menginjeksi: ConfigAccessInterface (untuk data inti), ConfigPolicyEnforcerInterface, ConfigValidatorInterface, ConfigHistoryTrackerInterface, ConfigWriteLoggerInterface, ConfigEventDispatcherInterface.
 * Core\Config\Manipulation\ (Utilities & Transformations):
   * Core\Config\Manipulation\ConfigArrayManipulatorInterface: Untuk flatten, importArray, arrayMergeRecursive.
   * Implementasi Core\Config\Manipulation\DefaultConfigArrayManipulator.
   * Core\Config\Manipulation\ConfigTransformerInterface: Untuk dumpTransform.
   * Implementasi Core\Config\Manipulation\DefaultConfigTransformer.
   * Core\Config\Manipulation\ConfigDifferInterface: Untuk diff dan treeDiff.
   * Implementasi Core\Config\Manipulation\DefaultConfigDiffer.
   * Core\Config\Manipulation\ConfigMaskerInterface: Untuk masking data sensitif.
   * Implementasi Core\Config\Manipulation\DefaultConfigMasker.
   * Core\Config\Manipulation\DeepClonerInterface: Untuk abstraksi deep cloning.
   * Implementasi Core\Config\Manipulation\DefaultDeepCloner.
   * Core\Config\Manipulation\ConfigPathResolverInterface: Untuk isPointer, isEnvPointer, resolveEnvPointer, explode('.') untuk key path.
   * Implementasi Core\Config\Manipulation\DefaultConfigPathResolver.
   * Core\Config\Manipulation\ConfigWildcardFinderInterface: Untuk wildcardGet.
   * Implementasi Core\Config\Manipulation\DefaultConfigWildcardFinder.
 * Core\Config\Policy\ (Access Policy Enforcement):
   * Core\Config\Policy\ConfigPolicyEnforcerInterface
   * Implementasi Core\Config\Policy\DefaultConfigPolicyEnforcer: TRANSFER DAN ADAPTASI LOGIKA policy, checkPolicy dari ConfigAdvanced dan Config. Harus menginjeksi resolver policy jika perlu.
 * Core\Config\Validation\ (Schema Validation - Kelas Baru):
   * Core\Config\Validation\ConfigValidatorInterface: (Metode validate(array $config, array $schema): array, validateGroup(array $config, string $prefix, array $schema): array, schema(array $schema): array).
   * Implementasi Core\Config\Validation\DefaultConfigValidator:
     * TRANSFER DAN ADAPTASI LOGIKA validasi skema dari Config::validate, Config::validateGroup, Config::schema.
     * HARUS menginjeksi PayloadTypeCheckerInterface (dari modul Event) atau type checker kustom.
   * Core\Config\Validation\TypeRegistryInterface: Untuk registerType.
   * Implementasi Core\Config\Validation\DefaultTypeRegistry: TRANSFER DAN ADAPTASI LOGIKA registerType dari ConfigValidator lama.
 * Core\Config\Advanced\ (Advanced Features - Kelas Baru):
   * Core\Config\Advanced\ConfigAdvancedInterface: (Metode applyPatch, migrate, generateTemplate, enforceType, policy).
   * Implementasi Core\Config\Advanced\DefaultConfigAdvanced:
     * TRANSFER DAN ADAPTASI LOGIKA applyPatch, migrate, generateTemplate, enforceType, policy dari Config lama.
     * HARUS menginjeksi: ConfigDifferInterface, ConfigMigratorInterface, ConfigTemplateGeneratorInterface, ConfigPolicyEnforcerInterface, TypeEnforcerInterface.
   * Core\Config\Advanced\ConfigMigratorInterface
   * Implementasi Core\Config\Advanced\DefaultConfigMigrator.
   * Core\Config\Advanced\ConfigTemplateGeneratorInterface
   * Implementasi Core\Config\Advanced\DefaultConfigTemplateGenerator.
   * Core\Config\Advanced\TypeEnforcerInterface
   * Implementasi Core\Config\Advanced\DefaultTypeEnforcer.
 * Core\Config\History\ (History & Write Tracking):
   * Core\Config\History\ConfigHistoryTrackerInterface: Untuk history, explain.
   * Implementasi Core\Config\History\DefaultConfigHistoryTracker.
   * Core\Config\History\ConfigWriteLoggerInterface: Untuk writeLog.
   * Implementasi Core\Config\History\DefaultConfigWriteLogger.
   * Core\Config\History\ConfigSnapshotManagerInterface: Untuk snapshot dan restore.
   * Implementasi Core\Config\History\DefaultConfigSnapshotManager: TRANSFER DAN ADAPTASI LOGIKA snapshot, restoreSnapshot, snapshotGroup, restoreGroupSnapshot. HARUS menginjeksi DeepClonerInterface.
 * Core\Config\Monitor\ (Monitoring & Debugging):
   * Core\Config\Monitor\ConfigFileWatcherInterface: Untuk watchFiles.
   * Implementasi Core\Config\Monitor\DefaultConfigFileWatcher: TRANSFER DAN ADAPTASI LOGIKA watchFiles. HARUS menginjeksi FilesystemInterface, TimeProviderInterface.
   * Core\Config\Monitor\ConfigCoverageTrackerInterface: Untuk coverageTrack, coverageReport.
   * Implementasi Core\Config\Monitor\DefaultConfigCoverageTracker.
   * Core\Config\Monitor\ConfigCliRunnerInterface: Untuk CLI commands lint, explain.
   * Implementasi Core\Config\Monitor\DefaultConfigCliRunner: HARUS menginjeksi CliOutputInterface (dari modul Router), ConfigAccessInterface, ConfigSchemaProviderInterface (baru).
 * Core\Config\Manager\ (The New ConfigManager Fasad/Orchestrator):
   * Implementasi Core\Config\Contract\ConfigAccessInterface (Baca).
   * Implementasi Core\Config\Contract\ConfigMutableInterface (Tulis).
   * Implementasi Core\Config\Manager\ConfigManagerInterface (Interface utama, gabungan akses).
   * Implementasi Core\Config\Manager\DefaultConfigManager.
     * INI ADALAH FASAD UTAMA. Konstruktornya akan menginjeksi SEMUA service yang telah didekomposisi di atas: ConfigResolverInterface, ConfigWriterInterface, ConfigLoaderInterface, ConfigMergerInterface, ConfigSnapshotManagerInterface, ConfigHistoryTrackerInterface, ConfigCliRunnerInterface, ConfigFileWatcherInterface, ConfigCoverageTrackerInterface, ConfigEventDispatcherInterface.
     * PENTING: ELIMINASI SEMUA PENGGUNAAN static di kelas Config lama.
     * PENTING: ELIMINASI SEMUA new instansiasi dependensi internal.
     * TRANSFER DAN ADAPTASI LOGIKA get, set, setWithValidation, enableAutoValidate, getAt, has, group, all, flatten, importArray, dump, export, cacheTo, loadCache, validate, validateGroup, schema, strict, enableLazyEval, enableWriteTracking, getWriteLog, overrideTemp, pushContext, popContext, fake, isTestMode, setContext, getContext, setNamespace, getNamespace, onLoad sampai onError, watch, reloadIfChanged, reload, diffWith, treeDiffWith, applyPatch, explain, snapshot sampai restoreGroupSnapshot, resolver, builder, migrate, generateTemplate, type, enforceType, policy, generateDoc, coverageReport, loadFrom, loadPriority, merge, loadEnv, importUrl, useNamespace, freeze, isFrozen, reset, arrayMergeRecursive. SEMUA harus didelegasikan ke service yang diinjeksikan.
 * Core\Config\Builder\ (ConfigManager Builder):
   * Core\Config\Builder\ConfigManagerBuilderInterface
   * Implementasi Core\Config\Builder\DefaultConfigManagerBuilder.
     * Tanggung jawab utama: Menerima array konfigurasi mentah untuk ConfigManager.
     * TRANSFER DAN ADAPTASI LOGIKA normalizeConfig dan validateConfig (jika ada validator di Config lama).
     * BERTANGGUNG JAWAB untuk instansiasi SEMUA objek dependensi yang dibutuhkan DefaultConfigManager berdasarkan konfigurasi, dan mengembalikan instance DefaultConfigManager yang sudah sepenuhnya terinisialisasi.
     * Harus menginjeksi Psr\Container\ContainerInterface untuk menyelesaikan dependensi melalui container.
     * Metode set, merge, build dari ConfigBuilder lama akan ditransfer ke sini.
     * Logika dumpTransform, coverageTrack, coverageReport, cli akan didelegasikan ke service yang diinjeksikan.
IV. Fase Akhir: Dokumentasi & Testing
 * Setelah SELURUH refaktor kode selesai dan semua kelas diimplementasikan logikanya: Anda akan kemudian melakukan dua tugas komprehensif yang terpisah:
   * Tugas A: Dokumentasi Lengkap Gaya GitHub.
   * Tugas B: PHPUnit Test Files Komprehensif.
 * PENTING: JANGAN membuat unit test atau dokumentasi apa pun sebelum SELURUH KODE SELESAI DIREFAKTOR DAN DIIMPLEMENTASIKAN LOGIKANYA.

** CODEBASE ASLI DIBAWAH DIGUNAKAN SEBAGAI SUMBER INFORMASI DALAM REFACTOR MODUL **
<?php

interface ConfigInterface {
    public static function get(string $key, $default = null, $pointerDepth = 0, array $opts = []): mixed;
    public static function set(string $key, $value, $role = 'system'): void;
}

trait ConfigTrait {
    public static function getInt($key, $default = 0, ...$args): int { return (int)static::get($key, $default, ...$args); }
    public static function getBool($key, $default = false, ...$args): bool { return (bool)static::get($key, $default, ...$args); }
    public static function getArray($key, $default = [], ...$args): array { return (array)static::get($key, $default, ...$args); }
}

class ConfigCrypto {
    public static function decrypt($val) { return base64_decode($val); }
}

class ConfigHelper
{
    public static function isPointer($val): bool { return is_string($val) && strlen($val) > 1 && $val[0] === '@'; }
    public static function isEnvPointer($val): bool { return is_string($val) && strpos($val, '@env:') === 0; }
    public static function resolveEnvPointer($val, $default = null) {
        $var = substr($val, 5 + 1);
        return $_ENV[$var] ?? $_SERVER[$var] ?? $default;
    }
    public static function flatten(array $array, string $prefix = ''): array {
        $result = [];
        foreach ($array as $k => $v) {
            $dot = $prefix === '' ? $k : "$prefix.$k";
            if (is_array($v)) $result += self::flatten($v, $dot);
            else $result[$dot] = $v;
        }
        return $result;
    }
    public static function importArray(array $flat): array {
        $result = [];
        foreach ($flat as $key => $value) {
            $ref = &$result;
            $parts = explode('.', $key);
            foreach ($parts as $i => $part) {
                if ($i === count($parts) - 1) $ref[$part] = $value;
                else {
                    if (!isset($ref[$part]) || !is_array($ref[$part])) $ref[$part] = [];
                    $ref = &$ref[$part];
                }
            }
        }
        return $result;
    }
    public static function export(array $data, string $format = 'array') {
        if ($format === 'json') return json_encode($data, JSON_PRETTY_PRINT);
        if ($format === 'yaml' && function_exists('yaml_emit')) return yaml_emit($data);
        return $data;
    }
    public static function diff(array $a, array $b): array {
        $flatA = self::flatten($a); $flatB = self::flatten($b); $diff = [];
        foreach (array_keys($flatA + $flatB) as $k) {
            if (!array_key_exists($k,$flatB)) $diff[$k]=['removed',$flatA[$k]];
            elseif (!array_key_exists($k,$flatA)) $diff[$k]=['added',$flatB[$k]];
            elseif($flatA[$k]!==$flatB[$k]) $diff[$k]=['changed',$flatA[$k],$flatB[$k]];
        }
        return $diff;
    }
    public static function treeDiff(array $a, array $b): array {
        $diff = [];
        foreach ($a as $k => $v) {
            if (!array_key_exists($k, $b)) $diff[$k] = ['removed', $v];
            elseif (is_array($v) && is_array($b[$k])) {
                $d = self::treeDiff($v, $b[$k]);
                if ($d) $diff[$k] = $d;
            } elseif ($v !== $b[$k]) $diff[$k] = ['changed', $v, $b[$k]];
        }
        foreach ($b as $k => $v) {
            if (!array_key_exists($k, $a)) $diff[$k] = ['added', $v];
        }
        return $diff;
    }
    public static function mask(array $data, array $fields = ['password', 'api_key', 'secret'], array $meta = []): array {
        $flat = self::flatten($data);
        foreach ($fields as $f) foreach ($flat as $k => &$v) if (stripos($k, $f)!==false) $v = '****';
        foreach ($meta as $k => $m) if (!empty($m['mask']) && isset($flat[$k])) $flat[$k] = '****';
        return self::importArray($flat);
    }
    public static function watchFiles(array $files, int $lastCheck): bool {
        foreach ($files as $f) if (file_exists($f) && filemtime($f) > $lastCheck) return true;
        return false;
    }
    public static function wildcardGet(array $array, string $pattern): array {
        $result = [];
        $pattern = str_replace('.', '\.', $pattern);
        $pattern = '/^' . str_replace('\*', '[^.]+', $pattern) . '$/';
        foreach (self::flatten($array) as $k => $v) if (preg_match($pattern, $k)) $result[$k] = $v;
        return $result;
    }
    public static function envSync(string $key, $default = null) {
        $envKey = strtoupper(str_replace('.', '_', $key));
        return $_ENV[$envKey] ?? $_SERVER[$envKey] ?? $default;
    }
    public static function deepClone($arr) { return unserialize(serialize($arr)); }
    public static function explain(array $history, string $key): ?array { return $history[$key] ?? null; }
    public static function generateDoc(array $schema, string $format = 'md'): string {
        if ($format === 'md') {
            $md = "| Key | Type | Required | Default | Description |\n|---|---|---|---|---|\n";
            foreach ($schema as $k => $r) {
                $md .= "| `$k` | `".($r['type']??'mixed')."` | ".(!empty($r['required'])?'yes':'no')." | ".($r['default']??'')." | ".($r['desc']??'')." |\n";
            }
            return $md;
        }
        if ($format === 'json') return json_encode($schema, JSON_PRETTY_PRINT);
        return print_r($schema, true);
    }
}

class ConfigEvent
{
    const TYPE_IMPORT_URL = 'importUrl';
    const TYPE_CONFLICT = 'conflict';
    protected static $onLoad = null, $onChange = null, $onGet = null, $onReload = null, $onPatch = null, $onError = null;
    public static function onLoad(callable $cb) { self::$onLoad = $cb; }
    public static function onChange(callable $cb) { self::$onChange = $cb; }
    public static function onGet(callable $cb) { self::$onGet = $cb; }
    public static function onReload(callable $cb) { self::$onReload = $cb; }
    public static function onPatch(callable $cb) { self::$onPatch = $cb; }
    public static function onError(callable $cb) { self::$onError = $cb; }
    public static function fireLoad($key, $val, $path) { if (self::$onLoad) call_user_func(self::$onLoad, $key, $val, $path);}
    public static function fireChange($key, $old, $new) { if (self::$onChange) call_user_func(self::$onChange, $key, $old, $new);}
    public static function fireGet($key, $val) { if (self::$onGet) call_user_func(self::$onGet, $key, $val);}
    public static function fireReload() { if (self::$onReload) call_user_func(self::$onReload);}
    public static function firePatch($patch) { if (self::$onPatch) call_user_func(self::$onPatch, $patch);}
    public static function fireError($type, $detail) { if (self::$onError) call_user_func(self::$onError, $type, $detail);}
    public static function fireConflict($key, $old, $new) { if (self::$onError) call_user_func(self::$onError, self::TYPE_CONFLICT, ['key'=>$key,'old'=>$old,'new'=>$new]); }
}

class ConfigEnv
{
    public static function loadEnv(string $path = '.env'): array {
        if (!file_exists($path)) return [];
        $lines = file($path, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
        $env = [];
        foreach ($lines as $line) {
            if (strpos(trim($line), '#') === 0) continue;
            if (!strpos($line, '=')) continue;
            [$k, $v] = explode('=', $line, 2);
            $env[trim($k)] = trim($v, "\"' ");
        }
        return $env;
    }
    public static function importUrl(string $url): array {
        $raw = @file_get_contents($url);
        if (!$raw) {
            ConfigEvent::fireError(ConfigEvent::TYPE_IMPORT_URL, $url);
            trigger_error("ConfigEnv::importUrl failed: $url", E_USER_NOTICE);
            return [];
        }
        $arr = [];
        foreach (explode("\n", $raw) as $line) {
            if (strpos(trim($line), '#') === 0) continue;
            if (!strpos($line, '=')) continue;
            [$k, $v] = explode('=', $line, 2);
            $arr[trim($k)] = trim($v, "\"' ");
        }
        return $arr;
    }
}

class ConfigBuilder
{
    protected array $data = [];
    public static array $coverage = [];
    public static function make(): self { return new self(); }
    public function set(string $key, $val): self {
        $ref = &$this->data;
        $parts = explode('.', $key);
        foreach ($parts as $i => $part) {
            if ($i === count($parts) - 1) $ref[$part] = $val;
            else {
                if (!isset($ref[$part]) || !is_array($ref[$part])) $ref[$part] = [];
                $ref = &$ref[$part];
            }
        }
        return $this;
    }
    public function merge(array $arr): self {
        $this->data = array_merge_recursive($this->data, $arr);
        return $this;
    }
    public function build(): array { return $this->data; }
    public static function dumpTransform(array $data, callable $transform, array $whitelist = null): array {
        $flat = ConfigHelper::flatten($data);
        foreach ($flat as $k => &$v) {
            if (is_array($whitelist) && !in_array($k, $whitelist)) continue;
            $nv = $transform($k, $v);
            if (gettype($nv) === gettype($v)) $v = $nv;
        }
        return ConfigHelper::importArray($flat);
    }
    public static function cli($argv): void {
        if (count($argv) < 2) exit("Usage: config.php <command> [args]\n");
        $cmd = $argv[1];
        if ($cmd === 'lint') echo "[ENHANCED19] Lint: Not implemented in this demo.\n";
        elseif ($cmd === 'explain' && isset($argv[2])) echo "[ENHANCED19] Explain {$argv[2]}: ...\n";
    }
    public static function coverageTrack($key) { self::$coverage[$key] = true; }
    public static function coverageReport(array $schema): array {
        $all = array_keys($schema);
        $hit = array_keys(self::$coverage);
        $miss = array_diff($all, $hit);
        return ['hit'=>$hit,'miss'=>$miss];
    }
}
class Config implements ConfigInterface
{
    use ConfigTrait;

    protected static array $data = [];
    protected static array $meta = [];
    protected static array $history = [];
    protected static array $paths = [];
    protected static array $priority = [];
    protected static bool $loaded = false;
    protected static bool $frozen = false;
    protected static bool $strict = false;
    protected static bool $lazyEval = false;
    protected static bool $trackWrites = false;
    protected static array $writeLog = [];
    protected static array $files = [];
    protected static int $lastCheck = 0;
    protected static array $dataCache = [];
    protected static array $snapshots = [];
    protected static int $maxPointerDepth = 10;
    protected static array $typeMap = [];
    protected static array $policy = [];
    protected static array $schema = [];
    protected static array $namespaceConfig = [];
    protected static bool $autoValidate = false;

    public static function get(string $key, $default = null, $pointerDepth = 0, array $opts = [], array $visited = []): mixed
    {
        if (in_array($key, $visited, true)) throw new \RuntimeException("Circular pointer detected at: $key");
        $ns = $opts['namespace'] ?? ConfigContext::getNamespace() ?? '';
        if ($ns && isset(static::$namespaceConfig[$ns])) {
            $v = static::$namespaceConfig[$ns]->get($key, $default, $pointerDepth, []);
            if ($v !== null) return $v;
        }
        $ctx = ConfigContext::getContext();
        if (isset($ctx['tenant'])) {
            $tenantKey = "tenant:{$ctx['tenant']}.$key";
            if (self::has($tenantKey)) return self::get($tenantKey, $default, $pointerDepth, $opts, $visited);
        }
        if (isset($ctx['locale'])) {
            $localeKey = "locale:{$ctx['locale']}.$key";
            if (self::has($localeKey)) return self::get($localeKey, $default, $pointerDepth, $opts, $visited);
        }
        foreach (array_reverse(ConfigContext::stack()) as $ctx) if (array_key_exists($key, $ctx)) return $ctx[$key];

        // [DEFAULT.* fallback]
        $defaultKey = "default.$key";
        if (self::has($defaultKey)) return self::get($defaultKey, $default, $pointerDepth, $opts, $visited);

        if (str_contains($key, '*')) {
            $set = ConfigHelper::wildcardGet(static::$data, $key);
            return $set ?: $default;
        }
        $ref = &static::$data;
        foreach (explode('.', $key) as $part) {
            if (!is_array($ref) || !array_key_exists($part, $ref)) {
                if (ConfigHelper::isEnvPointer($key)) return ConfigHelper::resolveEnvPointer($key, $default);
                $envVal = ConfigHelper::envSync($key);
                if ($envVal !== null) return $envVal;
                if (static::$strict) throw new \RuntimeException("Config key not found: $key");
                return $default;
            }
            $ref = &$ref[$part];
        }
        if (is_array(static::$meta[$key]??null) && !empty(static::$meta[$key]['encrypted'])) {
            return ConfigCrypto::decrypt($ref);
        }
        if (isset($ref) && ConfigHelper::isPointer($ref)) {
            if ($pointerDepth > static::$maxPointerDepth) throw new \RuntimeException("Pointer recursion detected: $key");
            $target = substr($ref, 1);
            return static::get($target, $default, $pointerDepth + 1, $opts, [...$visited, $key]);
        }
        if (ConfigHelper::isEnvPointer($ref)) return ConfigHelper::resolveEnvPointer($ref, $default);
        if (static::$lazyEval && is_callable($ref)) {
            if (!array_key_exists($key, static::$dataCache)) static::$dataCache[$key] = $ref();
            ConfigEvent::fireGet($key, static::$dataCache[$key]);
            ConfigBuilder::coverageTrack($key);
            return static::$dataCache[$key];
        }
        ConfigEvent::fireGet($key, $ref);
        ConfigBuilder::coverageTrack($key);
        return $ref;
    }

    public static function set(string $key, $value, $role = 'system'): void
    {
        if (static::$frozen) throw new \RuntimeException("Config is frozen. Cannot set $key");
        if (isset(static::$policy[$key]) && !ConfigAdvanced::checkPolicy($key, 'set', $role)) throw new \RuntimeException("Policy denied to set config key: $key");
        if (static::$autoValidate && isset(static::$schema[$key])) {
            $t = gettype($value);
            $type = static::$schema[$key]['type'] ?? null;
            if ($type && $t !== $type) throw new \RuntimeException("Type mismatch on set($key): $t, expected $type");
        }
        $ref = &static::$data;
        $parts = explode('.', $key);
        foreach ($parts as $i => $part) {
            if ($i === count($parts) - 1) {
                $old = $ref[$part] ?? null;
                $ref[$part] = $value;
                static::$history[$key] = ['set', debug_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS)[0]];
                if (static::$trackWrites) static::$writeLog[] = ['key' => $key, 'value' => $value, 'trace' => debug_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS)];
                ConfigEvent::fireChange($key, $old, $value);
            } else {
                if (!isset($ref[$part]) || !is_array($ref[$part])) $ref[$part] = [];
                $ref = &$ref[$part];
            }
        }
    }

    public static function setWithValidation(string $key, $value, $role = 'system'): void { static::set($key, $value, $role); }
    public static function enableAutoValidate(bool $on = true): void { static::$autoValidate = $on; }
    public static function getAt(string $key, $at = null) {
        $ref = &static::$data;
        foreach (explode('.', $key) as $part) {
            if (!isset($ref[$part])) return null;
            $ref = &$ref[$part];
        }
        if ($at && isset($ref['versions'][$at])) return $ref['versions'][$at];
        return $ref['value'] ?? $ref ?? null;
    }
    public static function has(string $key): bool {
        $ref = &static::$data;
        foreach (explode('.', $key) as $part) {
            if (!is_array($ref) || !array_key_exists($part, $ref)) return false;
            $ref = &$ref[$part];
        }
        return true;
    }
    public static function group(string $prefix): array {
        $ref = &static::$data;
        foreach (explode('.', $prefix) as $part) {
            if (!is_array($ref) || !array_key_exists($part, $ref)) return [];
            $ref = &$ref[$part];
        }
        return $ref;
    }
    public static function all(): array { return static::$data; }
    public static function flatten(): array { return ConfigHelper::flatten(static::$data); }
    public static function importArray(array $flat): void { static::merge(ConfigHelper::importArray($flat)); }
    public static function dump(bool $asJson = false, array $maskFields = [], callable $transform = null): array|string {
        $data = $maskFields ? ConfigHelper::mask(static::$data, $maskFields, static::$meta) : static::$data;
        if ($transform) $data = ConfigBuilder::dumpTransform($data, $transform);
        return $asJson ? json_encode($data, JSON_PRETTY_PRINT) : $data;
    }
    public static function export(string $format = 'array') { return ConfigHelper::export(static::$data, $format); }
    public static function cacheTo(string $file, string $format = 'php'): void { file_put_contents($file, $format==='php' ? "<?php return " . var_export(static::$data,true) . ";" : json_encode(static::$data, JSON_PRETTY_PRINT)); }
    public static function loadCache(string $file): void { static::$data = (substr($file, -4)==='.php' ? require $file : json_decode(file_get_contents($file),true)); }
    public static function validate(array $schema): array { static::$schema = $schema; return ConfigValidator::validate(static::$data, $schema); }
    public static function validateGroup(string $prefix, array $schema): array { return ConfigValidator::validateGroup(static::$data, $prefix, $schema); }
    public static function schema(array $schema): array { return ConfigValidator::schema($schema); }
    public static function strict(bool $on = true): void { static::$strict = $on; }
    public static function enableLazyEval(bool $on = true): void { static::$lazyEval = $on; }
    public static function enableWriteTracking(bool $on = true): void { static::$trackWrites = $on; }
    public static function getWriteLog(): array { return static::$writeLog; }
    public static function overrideTemp(array $override, callable $fn){ return ConfigContext::overrideTemp($override, $fn); }
    public static function pushContext(array $override): void { ConfigContext::push($override); }
    public static function popContext(): void { ConfigContext::pop(); }
    public static function fake(array $data): void { ConfigContext::fake($data); }
    public static function isTestMode(): bool { return ConfigContext::isTestMode(); }
    public static function setContext(array $ctx): void { ConfigContext::setContext($ctx); }
    public static function getContext(): array { return ConfigContext::getContext(); }
    public static function setNamespace(string $ns):void { ConfigContext::setNamespace($ns);}
    public static function getNamespace():string { return ConfigContext::getNamespace();}
    public static function onLoad(callable $cb): void { ConfigEvent::onLoad($cb); }
    public static function onChange(callable $cb): void { ConfigEvent::onChange($cb); }
    public static function onGet(callable $cb): void { ConfigEvent::onGet($cb); }
    public static function onReload(callable $cb): void { ConfigEvent::onReload($cb); }
    public static function onPatch(callable $cb): void { ConfigEvent::onPatch($cb); }
    public static function onError(callable $cb): void { ConfigEvent::onError($cb); }
    public static function watch(int $interval = 3){
        if (ConfigHelper::watchFiles(static::$files, static::$lastCheck)) static::reload();
        static::$lastCheck = time();
    }
    public static function reloadIfChanged(){ if (ConfigHelper::watchFiles(static::$files, static::$lastCheck)) static::reload(); static::$lastCheck = time(); }
    public static function diffWith(array $other): array { return ConfigHelper::diff(static::$data, $other); }
    public static function treeDiffWith(array $other): array { return ConfigHelper::treeDiff(static::$data, $other); }
    public static function applyPatch(array $patch, bool $validate = false): void { ConfigAdvanced::applyPatch(static::$data, $patch, $validate, static::$schema); }
    public static function explain(string $key): ?array { return ConfigHelper::explain(static::$history, $key); }
    public static function snapshot(?string $tag = null): array { $snap = ConfigHelper::deepClone(static::$data); if ($tag) static::$snapshots[$tag] = $snap; return $snap; }
    public static function restoreSnapshot(string $tag): void { if (!isset(static::$snapshots[$tag])) throw new \RuntimeException("No snapshot: $tag"); static::$data = ConfigHelper::deepClone(static::$snapshots[$tag]); }
    public static function snapshotGroup(string $prefix, ?string $tag = null): array { $group = static::group($prefix); $snap = ConfigHelper::deepClone($group); if ($tag) static::$snapshots["group:$tag"] = $snap; return $snap; }
    public static function restoreGroupSnapshot(string $prefix, string $tag): void {
        if (!isset(static::$snapshots["group:$tag"])) throw new \RuntimeException("No group snapshot: $tag");
        $group = &static::$data;
        foreach (explode('.', $prefix) as $part) { if (!isset($group[$part]) || !is_array($group[$part])) $group[$part] = []; $group = &$group[$part]; }
        $group = ConfigHelper::deepClone(static::$snapshots["group:$tag"]);
    }
    public static function resolver(callable $cb): void { /* implement as needed, eg. ConfigResolver::add($cb); */ }
    public static function builder(): ConfigBuilder { return ConfigBuilder::make(); }
    public static function migrate(callable $cb): void { static::$data = ConfigAdvanced::migrate(static::$data, $cb); }
    public static function generateTemplate(array $schema): string { return ConfigAdvanced::generateTemplate($schema); }
    public static function type(string $key, string $type): void { static::$typeMap[$key] = $type; }
    public static function enforceType($val, $type) { return ConfigAdvanced::enforceType($val, $type); }
    public static function policy(string $key, $cbOrArr): void { static::$policy[$key] = $cbOrArr; ConfigAdvanced::policy($key, $cbOrArr); }
    public static function generateDoc(array $schema, string $format = 'md'):string { return ConfigHelper::generateDoc($schema, $format); }
    public static function coverageReport(): array { return ConfigBuilder::coverageReport(static::$schema); }
    public static function loadFrom(string $dir): void {
        if (!is_dir($dir)) throw new \InvalidArgumentException("Config dir not found: $dir");
        foreach (glob(rtrim($dir, '/').'/*.php') as $file) {
            $cfg = require $file;
            if (!is_array($cfg)) throw new \RuntimeException("Config file invalid: $file");
            foreach (ConfigHelper::flatten($cfg) as $k => $v) {
                if (isset(static::$data[$k])) {
                    ConfigEvent::fireConflict($k, static::$data[$k], $v);
                }
            }
            static::merge($cfg, $file);
            static::$files[] = $file;
        }
        static::$paths[] = $dir;
        static::$loaded = true;
        static::$lastCheck = time();
    }
    public static function loadPriority(array $paths): void { foreach ($paths as $dir) { static::loadFrom($dir); static::$priority[] = $dir; } }
    public static function merge(array $data, string $fromFile = null, $firePatch = false): void {
        if (static::$frozen) throw new \RuntimeException('Config is frozen.');
        if ($fromFile) foreach (ConfigHelper::flatten($data) as $k => $v) { ConfigEvent::fireLoad($k, $v, $fromFile); static::$history[$k] = ['load', $fromFile]; }
        static::$data = static::arrayMergeRecursive(static::$data, $data);
        if ($firePatch) ConfigEvent::firePatch($data);
    }
    public static function loadEnv(string $path = '.env'): void {
        $envArr = ConfigEnv::loadEnv($path);
        foreach ($envArr as $k => $v) static::set($k, $v);
    }
    public static function importUrl(string $url): void {
        $arr = ConfigEnv::importUrl($url);
        static::merge($arr, $url);
    }
    public static function useNamespace(string $ns, ?Config $instance = null): void {
        static::$namespaceConfig[$ns] = $instance ?? new Config();
        ConfigContext::setNamespace($ns);
    }
    public static function freeze(): void { static::$frozen = true; }
    public static function isFrozen(): bool { return static::$frozen; }
    public static function reset(): void {
        static::$data = [];
        static::$meta = [];
        static::$history = [];
        static::$paths = [];
        static::$priority = [];
        static::$loaded = false;
        static::$frozen = false;
        static::$strict = false;
        static::$lazyEval = false;
        static::$trackWrites = false;
        static::$writeLog = [];
        static::$files = [];
        static::$lastCheck = 0;
        static::$dataCache = [];
        static::$namespaceConfig = [];
        ConfigContext::$stack = [];
        ConfigContext::$context = [];
        ConfigContext::$namespace = '';
    }
    public static function reload(): void {
        $priority = static::$priority;
        static::reset();
        if ($priority) static::loadPriority($priority);
        else foreach (static::$paths as $dir) static::loadFrom($dir);
        ConfigEvent::fireReload();
    }
    protected static function arrayMergeRecursive(array $a, array $b): array {
        foreach ($b as $k => $v) {
            if (is_array($v) && isset($a[$k]) && is_array($a[$k])) $a[$k] = static::arrayMergeRecursive($a[$k], $v);
            else $a[$k] = $v;
        }
        return $a;
    }
}

// Register type plugin untuk validator
ConfigValidator::registerType('email', fn($v)=> filter_var($v, FILTER_VALIDATE_EMAIL) !== false);
ConfigValidator::registerType('path', fn($v)=> is_string($v) && strlen($v) > 0 && $v[0] === '/');