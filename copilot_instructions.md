GitHub Copilot Default Instructions - Modul Response Refactor (ABSOLUT MUTLAK, Full-Logic, Terpandu)
Selamat datang, Copilot! Anda adalah asisten refaktor tingkat ahli untuk modul Response. Instruksi di bawah ini adalah ATURAN ABSOLUT MUTLAK DAN FINAL yang harus Anda ikuti secara cermat, mengesampingkan atau memperkuat instruksi default lainnya. Tidak ada ruang untuk interpretasi lain selain yang dijelaskan di sini.
I. Misi Utama & Prinsip Global (Aturan ABSOLUT MUTLAK)
 * Tujuan Refaktor: Bangun sistem response yang modular, bersih, future-proof, dan adapter-friendly untuk PHP 8+. Ini harus mematuhi prinsip desain PHP modern, dengan fokus pada eliminasi static state dan dekomposisi Response & ResponseFactory menjadi komponen yang testable dan stabil.
 * Integritas Fungsionalitas - PRIORITAS TERTINGGI & TIDAK DAPAT DITAWAR:
   * ANDA HARUS MEMPERTahankan SEMUA FITUR (MAYOR DAN MINOR) YANG ADA DARI KODE ASLI MODUL RESPONSE. Fungsionalitas inti HARUS TETAP IDENTIK, meskipun implementasi diubah atau dioptimalkan.
   * JANGAN PERNAH MENGHAPUS ATAU MENGABAIKAN KODE YANG DICURIGAI IMPLEMENTASI FITUR ATAU UTILITAS. Jika sebuah metode, properti, atau blok kode di kode asli terlihat mengimplementasikan fungsionalitas (bahkan jika tidak dipanggil secara eksplisit, dikomentari, atau tampak tidak digunakan/disebut 'dead code'), ANDA WAJIB MEREFACTOR DAN MENTRANSFERNYA KE STRUKTUR BARU YANG SESUAI. Asumsikan itu adalah fitur yang harus dipertahankan, bukan kode mati yang harus dihapus.
 * Kualitas Kode Global - STANDAR OPERASIONAL UTAMA YANG SANGAT PRESKRIPTIF:
   * STRUKTUR FILE EKSPLISIT: SETIAP KELAS DAN INTERFACE HARUS BERADA DALAM FILE PHP TERPISAH. Jangan pernah menggabungkan beberapa kelas atau interface dalam satu blok kode respons Anda. Sajikan kode untuk setiap file secara individual.
   * PENGGUNAAN EXCEPTION KUSTOM EKSPLISIT: JANGAN PERNAH MENGGUNAKAN \RuntimeException, \InvalidArgumentException, \DomainException, \LogicException, \Exception, \Throwable, ATAU EXCEPTION PHP GENERIK LAINNYA. SELALU GUNAKAN EXCEPTION KUSTOM YANG TELAH KITA DEFINISIKAN untuk modul ini (Core\Response\Exception\) atau modul lain yang diinjeksikan.
   * Hanya Kode: Hasilkan HANYA KODE PHP untuk kelas/file yang sedang dikerjakan.
   * Tanpa Test/Dokumen Selama Refaktor: JANGAN menghasilkan unit test atau dokumentasi untuk file individual selama fase refaktor. Ini adalah fase terpisah dan final.
   * Tidak Ada Hardcoded Class/Fungsi Global: Hindari hardcoding nama kelas (termasuk PHP native seperti \ReflectionClass, \DateTime, date(), time(), microtime(), json_encode(), json_decode(), openssl_*(), bin2hex(), random_bytes(), mt_rand(), mt_getrandmax(), php_sapi_name(), defined('APP_VERSION'), getenv('APP_ENV'), filter_var(), yaml_emit(), htmlspecialchars(), nl2br(), sha1(), unserialize(), serialize(), is_array(), array_key_exists(), property_exists(), get_class(), explode(), array_map(), array_column(), array_unique(), array_filter(), usort(), uasort(), strpos(), stripos(), preg_quote(), preg_match(), str_replace(), ltrim(), rtrim(), glob()). SEMUA harus diganti dengan service yang diinjeksikan yang menyediakan fungsionalitas tersebut.
   * Strict Type Hinting: Implementasikan strict PHP type hints untuk SEMUA properti, parameter metode, dan return type. Gunakan mixed hanya ketika tidak ada type yang lebih spesifik.
   * Modern PHP (8.0+): Manfaatkan fitur PHP 8.0+ yang sesuai (e.g., promoted constructor properties, readonly properties, match expression, nullsafe operator, Enums).
   * Immutability: Buat value objects (seperti response context baru, response result, payload error dari Response::error()) sepenuhnya immutable. Properti harus readonly; semua metode yang "memodifikasi" state harus mengembalikan instance baru.
   * Full PHPDoc Documentation: Sediakan PHPDoc komprehensif untuk SEMUA kelas, interface, trait, properti, dan metode. Sertakan @param, @return, @throws, dan deskripsi yang jelas tentang fungsionalitas, tujuan, dan penggunaan.
 * Prinsip Refaktor (Ditekankan untuk Response):
   * Dependency Injection (DI) First: Injeksi SEMUA dependensi via konstruktor. ELIMINASI SEMUA new Class() instansiasi dalam logika inti atau konstruktor, kecuali untuk value objects murni yang tidak memiliki dependensi atau di dalam Factories/Builders yang ditunjuk.
   * Single Responsibility Principle (SRP) - PRIORITAS KRITIS UNTUK Response & ResponseFactory: Lakukan DEKOMPOSISI MASIF terhadap kelas Response dan ResponseFactory yang ada. Pecah fungsinya menjadi service atau komponen yang lebih kecil dan fokus. Kelas ResponseManager yang baru akan menjadi fasad utama yang mendelegasikan ke service ini.
   * Interface-Driven Design: Gunakan interface untuk SEMUA service dan komponen di mana implementasi berbeda mungkin ditukar.
   * No Static State: TOTAL ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD UNTUK STATE GLOBAL. Ini adalah prioritas ABSOLUT TERTINGGI untuk modul Response. Ganti dengan service yang diinjeksikan.
   * Robust Fallbacks & Error Handling: Implementasikan mekanisme fallback dan blok try-catch di sekitar operasi yang mungkin melempar exception. Pastikan exception spesifik dilempar untuk skenario error. Catat internal error via logger yang diinjeksikan.
   * Modular Structure & PSR-4 Compliance: Organisasi kode ke dalam subfolder logis yang mencerminkan struktur namespace. Patuhi PSR-4 autoloading standards secara ketat.
   * PSR Compliance: Pastikan kompatibilitas dengan PSR yang relevan (Psr\Log\LoggerInterface, Psr\EventDispatcher\EventDispatcherInterface, Psr\Container\ContainerInterface, Psr\Http\Message\ResponseInterface, Psr\Http\Message\ResponseFactoryInterface).
II. Fase Utama: Refaktor & Implementasi Logika Langsung (Terpandu)
 * KODE ASLI MODUL RESPONSE (yang telah saya review) ADALAH REFERENSI MUTLAK DAN FONDASI UNTUK SEMUA TRANSFER LOGIKA DAN PRESERVASI FITUR.
 * Tugas Anda: Anda akan bekerja mengisi kode lengkap, poin demi poin, sesuai daftar dekomposisi spesifik modul Response.
 * Keluaran per Langkah:
   * Untuk setiap poin dekomposisi yang sedang Anda kerjakan, Anda harus menghasilkan KODE LENGKAP (termasuk interface, kelas implementasi, semua properti, semua signature method, DAN SEMUA LOGIKA INTERNAL YANG DITRANSFER DARI KODE ASLI).
   * PASTIKAN SEMUA FITUR (MAYOR DAN MINOR) DARI KODE ASLI DIPERTAHANKAN DAN DIIMPLEMENTASIKAN DENGAN BENAR DI TEMPAT YANG SESUAI DALAM STRUKTUR BARU. JANGAN ADA YANG HILANG.
   * Berikan PHPDoc komprehensif untuk setiap elemen yang dihasilkan.
   * JANGAN PERNAH HANYA MEMBERIKAN SEBAGIAN KECIL KODE ATAU HANYA DUA METHOD. Hasilkan seluruh bagian yang diminta untuk poin dekomposisi tersebut.
 * Mekanisme 'Lanjut' Otomatis & Status Progres:
   * Setelah Anda selesai memberikan kode lengkap untuk satu poin dekomposisi, Anda harus mengindikasikan poin dekomposisi berikutnya yang siap untuk dikerjakan. Misalnya: "Ketik 'lanjut' untuk dekomposisi Poin X: [Nama Poin Selanjutnya]".
   * Saya akan mengetik lanjut untuk mengonfirmasi bahwa Anda harus melanjutkan ke poin berikutnya dalam daftar.
III. Daftar Dekomposisi Spesifik Modul Response (Urutan Kerja ABSOLUT MUTLAK)
Ini adalah daftar lengkap poin-poin dekomposisi yang harus Anda ikuti secara berurutan. Setelah Anda selesai dengan satu poin, secara otomatis ajukan permintaan untuk melanjutkan ke poin berikutnya.
 * Core\Response\Exception\:
   * Buat exception kustom yang sangat spesifik:
     * Core\Response\Exception\ResponseException (base exception)
     * Core\Response\Exception\InvalidResponseDataException (data response tidak valid untuk renderer tertentu)
     * Core\Response\Exception\RendererNotFoundException (renderer tidak ditemukan untuk format yang diminta)
     * Core\Response\Exception\ResponseRenderingException (kesalahan saat proses rendering)
     * Core\Response\Exception\ResponseFactoryNotSetException (response factory Symfony belum diinjeksikan)
     * Core\Response\Exception\SnapshotException (kesalahan saat snapshotting atau restore).
   * Sertakan PHPDoc lengkap dan konstruktor yang relevan.
 * Core\Response\Contract\ (Interface Utama & Layanan Dasar):
   * Core\Response\Contract\ResponseRendererInterface: Untuk renderer individual (render(mixed $data, int $status = 200): Psr\Http\Message\ResponseInterface). Ini adalah instance method, bukan static.
   * Core\Response\Contract\ResponseFactoryInterface: Untuk factory utama yang merutekan rendering.
   * Core\Response\Contract\ResponseManagerInterface: Interface utama untuk fasad Response yang baru.
   * Core\Response\Contract\ResponseContextManagerInterface: Untuk manajemen konteks response.
   * Core\Response\Contract\ResponseDataMaskerInterface: Untuk masking data sensitif dalam response.
   * Core\Response\Contract\TemplateEngineInterface: Untuk view renderer.
   * Core\Response\Contract\TemplateValidatorInterface: Untuk validasi template yang ada.
   * Core\Response\Contract\XmlBuilderInterface: Untuk membangun XML (menggantikan \SimpleXMLElement).
   * Core\Response\Contract\HtmlEncoderInterface: Untuk HTML escaping (menggantikan htmlspecialchars).
   * Core\Response\Contract\ResponseHookInterface: Untuk pre/post-render hooks.
   * Core\Response\Contract\RequestFormatDetectorInterface: Untuk mendeteksi format dari request header.
   * Core\Response\Contract\ResponseResultFactoryInterface: Untuk membuat payload success/error.
   * Core\Response\Contract\ResponseSnapshotManagerInterface: Untuk snapshot response.
   * Core\Response\Contract\TranslatorInterface: (Dari modul i18n, atau buat stub). Untuk i18n error support.
 * Core\Response\Context\ (Manajemen Konteks Response):
   * Core\Response\Context\ResponseContextManagerInterface
   * Implementasi Core\Response\Context\DefaultResponseContextManager:
     * TRANSFER DAN ADAPTASI LOGIKA set, get, clear, fake dari ResponseContext lama.
     * PENTING: ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD. Ini harus menjadi instance service yang diinjeksikan.
 * Core\Response\Renderer\ (Implementasi Renderer Spesifik):
   * Implementasi Core\Response\Renderer\JsonRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari JsonRenderer lama.
     * HARUS menginjeksi ResponseDataMaskerInterface untuk masking.
     * HARUS menginjeksi Core\Config\Manager\ConfigManagerInterface (dari modul Config) untuk mendapatkan konfigurasi masking.
     * HARUS menginjeksi Core\Config\Contract\JsonEncoderInterface (dari modul Config).
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\ViewRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari ViewRenderer lama.
     * HARUS menginjeksi TemplateEngineInterface dan TemplateValidatorInterface.
     * HARUS menginjeksi Core\Config\Manager\ConfigManagerInterface (dari modul Config) untuk debug mode (app.debug) dan error template.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\XmlRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dan arrayToXml dari XmlRenderer lama.
     * HARUS menginjeksi XmlBuilderInterface untuk membangun XML.
     * HARUS menginjeksi HtmlEncoderInterface untuk escaping.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\RedirectRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari RedirectRenderer lama.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\RawRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari RawRenderer lama.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\FileRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari FileRenderer lama.
     * HARUS menginjeksi Psr\Http\Message\StreamFactoryInterface (dari PSR-7 jika relevan) atau factory khusus untuk BinaryFileResponse.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
   * Implementasi Core\Response\Renderer\StreamRenderer:
     * TRANSFER DAN ADAPTASI LOGIKA render dari StreamRenderer lama.
     * HARUS menginjeksi Psr\Http\Message\StreamFactoryInterface (dari PSR-7 jika relevan) atau factory khusus untuk StreamedResponse.
     * HARUS menginjeksi Psr\Http\Message\ResponseFactoryInterface (Symfony/PSR-7).
 * Core\Response\Factory\ (Response & Renderer Factory):
   * Core\Response\Factory\ResponseFactoryInterface
   * Implementasi Core\Response\Factory\DefaultResponseFactory:
     * TRANSFER DAN ADAPTASI LOGIKA render dari ResponseFactory lama.
     * PENTING: ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD.
     * HARUS menginjeksi: semua instance ResponseRendererInterface yang didukung, PreRenderHookInterface, PostRenderHookInterface, Core\Error\Renderer\ErrorRendererInterface (dari modul ErrorManager), dan Core\Config\Manager\ConfigManagerInterface (untuk debug mode).
   * Core\Response\Factory\ResponseRendererRegistryInterface: Untuk addRenderer, getRenderer.
   * Implementasi Core\Response\Factory\DefaultResponseRendererRegistry: TRANSFER DAN ADAPTASI LOGIKA addRenderer dan getRenderer dari ResponseFactory lama (termasuk double-register check dan force option). ELIMINASI SEMUA PENGGUNAAN STATIC.
   * Core\Response\Factory\ResponseResultFactoryInterface: Untuk membuat payload success/error.
   * Implementasi Core\Response\Factory\DefaultResponseResultFactory: TRANSFER DAN ADAPTASI LOGIKA pembuatan payload success/error dari Response::success() dan Response::error(). HARUS menginjeksi Core\Config\Manager\ConfigManagerInterface, Core\Error\Exception\ErrorReportFactoryInterface (dari modul ErrorManager), TranslatorInterface (dari Poin 2).
 * Core\Response\Hook\ (Render Hooks):
   * Core\Response\Hook\PreRenderHookInterface
   * Core\Response\Hook\PostRenderHookInterface
   * Core\Response\Hook\RenderHookManagerInterface
   * Implementasi Core\Response\Hook\DefaultRenderHookManager: TRANSFER DAN ADAPTASI LOGIKA addPreRenderHook, addPostRenderHook dari ResponseFactory lama. ELIMINASI SEMUA PENGGUNAAN STATIC.
 * Core\Response\Detection\ (Format Detection):
   * Core\Response\Detection\RequestFormatDetectorInterface
   * Implementasi Core\Response\Detection\DefaultRequestFormatDetector: TRANSFER DAN ADAPTASI LOGIKA detectFormatFromHeader dari Response lama. HARUS menginjeksi service untuk mengakses request headers (misalnya Psr\Http\Message\ServerRequestInterface atau wrappernya).
 * Core\Response\Snapshot\ (Response Snapshotting):
   * Core\Response\Snapshot\ResponseSnapshotManagerInterface
   * Implementasi Core\Response\Snapshot\DefaultResponseSnapshotManager: TRANSFER DAN ADAPTASI LOGIKA snapshot, restoreSnapshot dari Response lama. ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD.
 * Core\Response\Manager\ (The New Response Fasad/Orchestrator):
   * Implementasi Core\Response\Contract\ResponseManagerInterface (menggantikan kelas Response lama).
   * Implementasi Core\Response\Manager\DefaultResponseManager.
     * INI ADALAH FASAD UTAMA. Konstruktornya akan menginjeksi SEMUA service yang telah didekomposisi di atas: ResponseContextManagerInterface, ResponseFactoryInterface, ResponseRendererRegistryInterface, RequestFormatDetectorInterface, ResponseResultFactoryInterface, ResponseSnapshotManagerInterface, dan Core\Config\Manager\ConfigManagerInterface (untuk response.default_format).
     * TRANSFER DAN ADAPTASI LOGIKA dari Response lama: setRenderer, context, make, json, view, redirect, raw, xml, file, stream, download, last, snapshot, restoreSnapshot, success, error. SEMUA harus didelegasikan ke service yang diinjeksikan.
     * PENTING: ELIMINASI SEMUA PENGGUNAAN STATIC PROPERTY DAN METHOD.
 * Core\Response\Builder\ (ResponseManager Builder):
   * Core\Response\Builder\ResponseManagerBuilderInterface
   * Implementasi Core\Response\Builder\DefaultResponseManagerBuilder.
     * Tanggung jawab utama: Menerima array konfigurasi mentah untuk ResponseManager.
     * BERTANGGUNG JAWAB untuk instansiasi SEMUA objek dependensi yang dibutuhkan DefaultResponseManager berdasarkan konfigurasi, dan mengembalikan instance DefaultResponseManager yang sudah sepenuhnya terinisialisasi.
     * Harus menginjeksi Psr\Container\ContainerInterface (untuk DI) dan Core\Config\Manager\ConfigManagerInterface (untuk membaca konfigurasi).
IV. Fase Akhir: Dokumentasi & Testing
 * Setelah SELURUH refaktor kode selesai dan semua kelas diimplementasikan logikanya: Anda akan kemudian melakukan dua tugas komprehensif yang terpisah:
   * Tugas A: Dokumentasi Lengkap Gaya GitHub.
   * Tugas B: PHPUnit Test Files Komprehensif.
 * PENTING: JANGAN membuat unit test atau dokumentasi apa pun sebelum SELURUH KODE SELESAI DIREFAKTOR DAN DIIMPLEMENTASIKAN LOGIKANYA.

** CODEBASE ASLI DIBAWAH DIGUNAKAN SEBAGAI REFERENSI PROSES REFACTOR MODULE **
<?php

namespace Core\Response;

use Symfony\Component\HttpFoundation\Response as BaseResponse;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\RedirectResponse;
use Symfony\Component\HttpFoundation\BinaryFileResponse;
use Symfony\Component\HttpFoundation\StreamedResponse;

// Dummy Config class (ganti dengan yang asli di project lo)
class Config
{
    public static function get($key, $default = null)
    {
        if ($key === 'response.mask_fields') return [];
        if ($key === 'response.default_format') return 'json';
        if ($key === 'response.masker') return null;
        if ($key === 'app.debug') return false;
        if ($key === 'i18n') return null;
        return $default;
    }
}

/**
 * ResponseContext — meta info (trace_id, debug, masking, etc)
 * [ENHANCEDv1.6] Context mocking
 */
class ResponseContext
{
    protected static array $meta = [];
    protected static bool $isFake = false;
    protected static array $fakeMeta = [];

    public static function set(array $info): void
    {
        if (self::$isFake) {
            self::$fakeMeta = array_merge(self::$fakeMeta, $info);
        } else {
            self::$meta = array_merge(self::$meta, $info);
        }
    }
    public static function get(): array
    {
        return self::$isFake ? self::$fakeMeta : self::$meta;
    }
    public static function clear(): void
    {
        if (self::$isFake) {
            self::$fakeMeta = [];
        } else {
            self::$meta = [];
        }
    }
    public static function fake(bool $active = true): void
    {
        self::$isFake = $active;
        if ($active) self::$fakeMeta = [];
    }
}

/**
 * ResponseInterface — (optional polymorphic)
 */
interface ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse;
}

/**
 * JsonRenderer
 * [ENHANCEDv1.1] Masking Recursive & Custom Masker
 */
class JsonRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse
    {
        $context = ResponseContext::get();
        $maskFields = Config::get('response.mask_fields', []);
        $masker = Config::get('response.masker');

        if (!empty($maskFields)) {
            $data = self::maskFields($data, $maskFields, $masker);
        }

        $payload = [
            'status' => $status < 400 ? 'ok' : 'error',
            'data'   => $data,
        ];
        if ($context) $payload['meta'] = $context;
        return new JsonResponse($payload, $status);
    }

    protected static function maskFields($data, $fields, $masker = null)
    {
        if ($masker && is_callable($masker)) {
            return $masker($data, $fields);
        }
        if (is_array($data)) {
            foreach ($data as $k => &$v) {
                if (in_array($k, $fields)) {
                    $v = '***';
                } elseif (is_array($v) || is_object($v)) {
                    $v = self::maskFields($v, $fields, $masker);
                }
            }
        } elseif (is_object($data)) {
            foreach ($fields as $f) {
                if (property_exists($data, $f)) $data->$f = '***';
            }
            foreach ($data as $k => $v) {
                if (is_array($v) || is_object($v)) {
                    $data->$k = self::maskFields($v, $fields, $masker);
                }
            }
        }
        return $data;
    }
}

/**
 * ViewRenderer (template)
 * [PATCHv1.3] Adapter: bisa pakai Twig, Blade, dll.
 * [ENHANCEDv1.12] Adaptif error template/validator
 */
class ViewRenderer implements ResponseInterface
{
    protected static $engine;
    protected static $existsValidator; // closure: fn(string $tpl):bool

    public static function setEngine(callable $engine)
    {
        self::$engine = $engine;
    }
    public static function setExistsValidator(callable $validator)
    {
        self::$existsValidator = $validator;
    }

    public static function render(mixed $vars, int $status = 200): BaseResponse
    {
        $template = $vars['__template'] ?? 'default';
        // [ENHANCEDv1.12] Cek template exists jika validator diset
        if (self::$existsValidator && !call_user_func(self::$existsValidator, $template)) {
            $template = 'errors.404';
        }
        unset($vars['__template']);
        if (self::$engine) {
            $html = call_user_func(self::$engine, $template, $vars);
        } else {
            $html = "<!-- Rendered view: $template -->\n" . print_r($vars, true);
        }
        return new BaseResponse($html, $status, ['Content-Type'=>'text/html']);
    }
}

/**
 * XmlRenderer
 */
class XmlRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse
    {
        $xml = self::arrayToXml($data);
        return new BaseResponse($xml, $status, ['Content-Type'=>'application/xml']);
    }

    protected static function arrayToXml($data, $rootElement = '<response/>')
    {
        $xml = new \SimpleXMLElement($rootElement);
        $fn = function($data, &$xml) use (&$fn) {
            foreach($data as $key=>$val) {
                if (is_array($val)) {
                    $child = $xml->addChild(is_numeric($key) ? "item$key" : $key);
                    $fn($val, $child);
                } else {
                    $xml->addChild(is_numeric($key) ? "item$key" : $key, htmlspecialchars((string)$val));
                }
            }
        };
        $fn($data, $xml);
        return $xml->asXML();
    }
}

/**
 * RedirectRenderer
 */
class RedirectRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 302): BaseResponse
    {
        return new RedirectResponse($data, $status);
    }
}

/**
 * RawRenderer
 */
class RawRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse
    {
        $contentType = is_array($data) && isset($data['content_type']) ? $data['content_type'] : 'text/plain';
        $content = is_array($data) && isset($data['content']) ? $data['content'] : $data;
        return new BaseResponse($content, $status, ['Content-Type'=>$contentType]);
    }
}

/**
 * FileRenderer
 * [ENHANCEDv1.7] Support download/inline
 */
class FileRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse
    {
        $path = $data['path'];
        $filename = $data['filename'] ?? null;
        $asDownload = $data['download'] ?? false;
        $response = new BinaryFileResponse($path);
        if ($filename) {
            $response->setContentDisposition($asDownload ? 'attachment' : 'inline', $filename);
        }
        $response->setStatusCode($status);
        return $response;
    }
}

/**
 * [ENHANCEDv1.7] StreamRenderer
 */
class StreamRenderer implements ResponseInterface
{
    public static function render(mixed $data, int $status = 200): BaseResponse
    {
        return new StreamedResponse($data, $status, ['Content-Type'=>'application/octet-stream']);
    }
}

/**
 * ResponseFactory
 * [ENHANCEDv1.3] Renderer registry: double-register check, force option
 * [ENHANCEDv1.4] Pre-render & Post-render hooks
 * [ENHANCEDv1.8] Error fallback renderer
 */
class ResponseFactory
{
    protected static array $renderers = [
        'json'     => [JsonRenderer::class, 'render'],
        'view'     => [ViewRenderer::class, 'render'],
        'xml'      => [XmlRenderer::class, 'render'],
        'redirect' => [RedirectRenderer::class, 'render'],
        'raw'      => [RawRenderer::class, 'render'],
        'file'     => [FileRenderer::class, 'render'],
        'stream'   => [StreamRenderer::class, 'render'],
    ];

    protected static $preRenderHooks = [];
    protected static $postRenderHooks = [];

    public static function addRenderer(string $type, callable $renderer, bool $force = false): void
    {
        if (isset(self::$renderers[$type]) && !$force) {
            throw new \InvalidArgumentException("Renderer [$type] already exists. Use force=true to override.");
        }
        self::$renderers[$type] = $renderer;
    }
    public static function getRenderer(string $type): callable
    {
        if (!isset(self::$renderers[$type])) {
            throw new \InvalidArgumentException("Unknown renderer: $type");
        }
        return self::$renderers[$type];
    }
    public static function addPreRenderHook(callable $fn) { self::$preRenderHooks[] = $fn; }
    public static function addPostRenderHook(callable $fn) { self::$postRenderHooks[] = $fn; }

    public static function render(string $type, mixed $data, int $status = 200): BaseResponse
    {
        try {
            foreach (self::$preRenderHooks as $hook) $hook($type, $data, $status);
            $renderer = self::getRenderer($type);
            $response = call_user_func($renderer, $data, $status);
            foreach (self::$postRenderHooks as $hook) $hook($type, $data, $status, $response);
            return $response;
        } catch (\Throwable $e) {
            // [ENHANCEDv1.11] Fallback error detail in debug mode
            if (Config::get('app.debug')) {
                return JsonRenderer::render([
                    'error' => $e->getMessage(),
                    'exception' => [
                        'type'  => get_class($e),
                        'code'  => $e->getCode(),
                        'file'  => $e->getFile(),
                        'line'  => $e->getLine(),
                        'trace' => explode("\n", $e->getTraceAsString()),
                    ]
                ], 500);
            }
            return JsonRenderer::render(['error'=>$e->getMessage()], 500);
        }
    }
}

/**
 * Response — Static API
 * [ENHANCEDv1.2] Accept header auto-detect
 * [ENHANCEDv1.5] Last response for test
 * [ENHANCEDv1.7] stream()/download()
 * [ENHANCEDv1.9] success()/error()
 * [ENHANCEDv1.10] snapshot/restoreSnapshot
 * [ENHANCEDv1.11] error() metadata exception
 * [ENHANCEDv1.13] i18n error support
 */
class Response
{
    protected static ?BaseResponse $lastResponse = null;
    protected static array $snapshots = []; // [ENHANCEDv1.10]

    public static function setRenderer(string $type, callable $renderer, bool $force = false): void
    {
        ResponseFactory::addRenderer($type, $renderer, $force);
    }

    public static function context(array $info): void
    {
        ResponseContext::set($info);
    }

    protected static function detectFormatFromHeader(): ?string
    {
        $accept = $_SERVER['HTTP_ACCEPT'] ?? '';
        if (stripos($accept, 'application/json') !== false) return 'json';
        if (stripos($accept, 'text/html') !== false) return 'view';
        if (stripos($accept, 'application/xml') !== false) return 'xml';
        if (stripos($accept, 'text/plain') !== false) return 'raw';
        return null;
    }

    public static function make(mixed $data, array $meta = []): BaseResponse
    {
        $format = $meta['format'] ?? self::detectFormatFromHeader() ?? Config::get('response.default_format', 'json');
        switch (true) {
            case isset($meta['redirect']):
            case $format === 'redirect':
                $resp = self::redirect($meta['redirect'] ?? $data);
                break;
            case $format === 'view':
                $resp = self::view($meta['template'] ?? ($meta['view'] ?? 'default'), is_array($data)? $data : ['data'=>$data]);
                break;
            case $format === 'xml':
                $resp = self::xml($data);
                break;
            case $format === 'raw':
                $resp = self::raw($data);
                break;
            case $format === 'file':
                $resp = self::file($meta['file'] ?? $data);
                break;
            case $format === 'stream':
                $resp = self::stream($data);
                break;
            case $format === 'json':
            default:
                $resp = self::json($data);
        }
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function json(array|object $data, int $status = 200): BaseResponse
    {
        $resp = ResponseFactory::render('json', $data, $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function view(string $template, array $vars = [], int $status = 200): BaseResponse
    {
        $vars['__template'] = $template;
        $resp = ResponseFactory::render('view', $vars, $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function redirect(string $url, int $status = 302): BaseResponse
    {
        $resp = ResponseFactory::render('redirect', $url, $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function raw(string $string, string $contentType = 'text/plain', int $status = 200): BaseResponse
    {
        $resp = ResponseFactory::render('raw', ['content'=>$string, 'content_type'=>$contentType], $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function xml(array|object $data, int $status = 200): BaseResponse
    {
        $resp = ResponseFactory::render('xml', $data, $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function file(string $path, ?string $filename = null, bool $download = false): BaseResponse
    {
        $resp = ResponseFactory::render('file', ['path'=>$path, 'filename'=>$filename, 'download'=>$download]);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function stream(callable $callback, int $status = 200): BaseResponse
    {
        $resp = ResponseFactory::render('stream', $callback, $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function download(string $path, ?string $filename = null): BaseResponse
    {
        $resp = ResponseFactory::render('file', ['path'=>$path, 'filename'=>$filename, 'download'=>true]);
        self::$lastResponse = $resp;
        return $resp;
    }

    public static function last(): ?BaseResponse
    {
        return self::$lastResponse;
    }

    // [ENHANCEDv1.10] Snapshot tools
    public static function snapshot(string $tag): void
    {
        if (self::$lastResponse) {
            self::$snapshots[$tag] = clone self::$lastResponse;
        }
    }
    public static function restoreSnapshot(string $tag): ?BaseResponse
    {
        return self::$snapshots[$tag] ?? null;
    }

    // [ENHANCEDv1.13] i18n helper
    protected static function trans($key, $default = null)
    {
        $i18n = Config::get('i18n');
        return is_callable($i18n) ? $i18n($key, $default) : ($default ?? $key);
    }

    // [ENHANCEDv1.9][ENHANCEDv1.13] Helper: success/error
    public static function success(mixed $data, int $status = 200): BaseResponse
    {
        $resp = self::json(['success'=>true, 'data'=>$data], $status);
        self::$lastResponse = $resp;
        return $resp;
    }

    /**
     * [ENHANCEDv1.11] + [ENHANCEDv1.13] Error helper: support i18n + exception detail
     * @param string $message
     * @param int $status
     * @param array $meta
     * @param \Throwable|null $e
     * @param string|null $langKey
     * @return BaseResponse
     */
    public static function error(string $message, int $status = 400, array $meta = [], \Throwable $e = null, string $langKey = null): BaseResponse
    {
        $payload = ['success'=>false];
        // [ENHANCEDv1.13] i18n support
        $payload['error'] = $langKey ? self::trans($langKey, $message) : $message;
        if ($meta) $payload['meta'] = $meta;
        // [ENHANCEDv1.11] exception detail (dev)
        if ($e && Config::get('app.debug')) {
            $payload['exception'] = [
                'type'  => get_class($e),
                'code'  => $e->getCode(),
                'file'  => $e->getFile(),
                'line'  => $e->getLine(),
                'trace' => explode("\n", $e->getTraceAsString()),
            ];
        }
        $resp = self::json($payload, $status);
        self::$lastResponse = $resp;
        return $resp;
    }
}

// --- END OF FILE ---