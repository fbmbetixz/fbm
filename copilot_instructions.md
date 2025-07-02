Baik, saya paham. Saya sangat menghargai kepercayaan Anda sepenuhnya. Untuk modul ErrorManager yang kompleks ini, kita akan memastikan tidak ada ruang untuk keraguan dalam instruksi Copilot.
Saya akan merumuskan instruksi "absolute mutlak" yang super lengkap, mencakup semua poin peningkatan yang kita diskusikan. Instruksi ini akan menjadi yang paling detail dan preskriptif yang pernah kita buat, dirancang untuk menghasilkan kode yang sangat stabil, testable, dan enterprise-ready.
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
SIAP UNTUK MEMULAI:
Setelah Anda menempatkan instruksi ini ke pengaturan default Copilot Anda, dan menempelkan daftar dekomposisi spesifik untuk modul ErrorManager ini, mulai chat baru dengan Copilot.
Anda TIDAK perlu memberi tahu saya nomor dekomposisi awal lagi. Cukup ketik:
"Oke Copilot, mari kita mulai refaktor modul ErrorManager sekarang."
Copilot akan secara otomatis mulai menghasilkan kode lengkap untuk Poin 1: Core\Error\Exception\.
Setelah itu, untuk setiap poin berikutnya, Anda cukup mengetik:
"lanjut"
Copilot akan secara otomatis melanjutkan ke poin dekomposisi berikutnya dalam daftar ini, menghasilkan kode lengkap dengan logika.
Dengan instruksi "absolute mutlak" ini, saya yakin Copilot akan dapat merefaktor modul ErrorManager Anda dengan presisi tertinggi, memecahnya menjadi komponen-komponen yang dapat diuji, dan menghasilkan kode yang sangat stabil, testable, dan enterprise-ready. Ini akan menjadi transformasi yang luar biasa.
Apakah instruksi ini sudah sesuai dengan yang Anda inginkan?
