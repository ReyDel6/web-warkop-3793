Tentu, sebagai Antigravity Lead System Architect, saya akan menyusun dokumen Technical Specification (TECH_SPEC.md) untuk proyek "web warkop" ini dengan fokus pada arsitektur web modern yang efisien, performa tinggi, dan mudah dikelola.

---

# TECH_SPEC.md: Arsitektur Web "web warkop"

## 1. Pendahuluan

Dokumen ini menjelaskan arsitektur teknis dan strategi implementasi untuk proyek "web warkop". Tujuannya adalah membangun sebuah website warkop yang lengkap, informatif, interaktif, dan memiliki performa optimal, dengan memanfaatkan teknologi web modern dan praktik terbaik.

**Nama Proyek:** web warkop
**Kebutuhan:** Website warkop lengkap (menampilkan menu, lokasi, informasi kontak, galeri, event, dan kemungkinan fitur pemesanan sederhana).

Arsitektur yang diusulkan berpusat pada kesederhanaan, kecepatan, dan kendali penuh, dengan menghindari *framework* JavaScript yang berat demi performa dan ukuran *bundle* yang minimal.

## 2. Tech Stack

Kami akan mengadopsi tumpukan teknologi *frontend* yang ringan namun powerful, berfokus pada standar web dan praktik terbaik.

### 2.1. Frontend

*   **HTML5 Semantik:**
    *   **Fungsi:** Menyediakan struktur konten yang jelas dan bermakna untuk website.
    *   **Penerapan:** Penggunaan elemen HTML5 yang tepat seperti `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<figure>`, `<figcaption>`, `<time>`, dll. Ini krusial untuk SEO (Search Engine Optimization) dan aksesibilitas (Screen Readers).
    *   **Manfaat:** Meningkatkan pemahaman konten oleh *search engine*, memudahkan navigasi bagi pengguna dengan disabilitas, dan membuat kode lebih mudah dipelihara.

*   **Tailwind CSS (CDN):**
    *   **Fungsi:** Kerangka kerja CSS *utility-first* untuk membangun antarmuka pengguna dengan cepat dan konsisten.
    *   **Penerapan:** Menggunakan versi CDN Tailwind CSS untuk meminimalkan waktu *setup* dan memastikan *bundle size* yang kecil tanpa perlu proses *build* CSS yang kompleks.
    *   **Manfaat:**
        *   **Rapid UI Development:** Membangun desain kustom langsung di markup HTML.
        *   **Konsistensi:** Memastikan konsistensi desain di seluruh aplikasi dengan sistem desain berbasis *utility*.
        *   **Optimasi Ukuran:** Meskipun menggunakan CDN, Tailwind secara *default* memiliki ukuran yang relatif kecil dan dapat di-*purge* jika menggunakan versi *build* (namun untuk CDN, kita akan mengandalkan efisiensi *cache* browser).
        *   **Tanpa CSS Kustom Berlebihan:** Mengurangi kebutuhan untuk menulis CSS kustom yang banyak, meminimalkan konflik dan meningkatkan pemeliharaan.

*   **Vanilla ES6+ Modular JavaScript:**
    *   **Fungsi:** Mengimplementasikan seluruh logika interaktif dan manajemen data menggunakan JavaScript murni (tanpa *framework* seperti React, Vue, Angular).
    *   **Penerapan:**
        *   **ES Modules (`import`/`export`):** Mengorganisir kode JavaScript ke dalam modul-modul yang terpisah dan dapat di-*reuse*. Ini memungkinkan *dependency management* yang bersih dan *lazy loading* modul jika diperlukan.
        *   **Fitur ES6+:** Memanfaatkan fitur modern JavaScript seperti `const`/`let`, *arrow functions*, *template literals*, *destructuring*, `async`/`await` untuk penanganan *asynchronous operations* (misalnya, mengambil data menu dari API), dan `class` untuk struktur komponen atau utilitas.
        *   **Manfaat:**
            *   **Performa Optimal:** Tidak ada *overhead* runtime *framework*, menghasilkan *bundle size* JavaScript yang sangat kecil dan waktu *loading* yang cepat.
            *   **Kendali Penuh:** Memberikan kontrol penuh atas setiap aspek aplikasi, memungkinkan optimasi yang sangat spesifik.
            *   **Pembelajaran Standar Web:** Mendorong pemahaman mendalam tentang cara kerja browser dan JavaScript.
            *   **Long-term Maintainability:** Bergantung pada standar web yang stabil, bukan pada siklus rilis *framework* yang cepat.

### 2.2. Backend (Opsional/Sederhana)

Untuk kebutuhan "website warkop lengkap", *backend* mungkin diperlukan untuk:
*   Menyimpan data menu, lokasi, event, galeri.
*   Menangani pengiriman formulir kontak.
*   (Opsional) Sistem pemesanan sederhana.

**Pendekatan:**
*   **API Sederhana:** Jika diperlukan, *backend* dapat berupa API RESTful sederhana yang dibangun dengan Node.js (Express), Python (Flask/Django), atau PHP (Laravel).
*   **Data Statis/JSON:** Untuk website yang sangat sederhana, data menu atau event bisa disimpan dalam file JSON statis yang di-*fetch* oleh JavaScript *frontend*.
*   **Serverless Functions:** Untuk formulir kontak, dapat menggunakan *serverless functions* (AWS Lambda, Netlify Functions, Vercel Functions) untuk memproses pengiriman tanpa perlu mengelola server penuh.

**Catatan:** Fokus dokumen ini adalah pada arsitektur *frontend*. Detail *backend* akan dijelaskan dalam dokumen terpisah jika lingkupnya meluas.

### 2.3. Development Tools

*   **Vite / Parcel:** Untuk *development server* yang cepat, *hot module replacement (HMR)*, dan *bundling* modul ES6+ untuk produksi (jika diperlukan untuk mengoptimalkan *loading* modul).
*   **ESLint:** Untuk *linting* kode JavaScript, memastikan konsistensi dan mencegah *bug*.
*   **Prettier:** Untuk *code formatting* otomatis.

## 3. Arsitektur Komponen UI

Arsitektur UI akan mengikuti prinsip *Component-Based Architecture* dengan inspirasi dari Atomic Design untuk memastikan modularitas, *reusability*, dan kemudahan pemeliharaan.

### 3.1. Filosofi Desain Komponen

*   **Atomic Design:** Memecah antarmuka menjadi komponen-komponen yang kecil dan independen, kemudian menggabungkannya menjadi bagian yang lebih besar.
    *   **Atoms:** Elemen HTML dasar (misal: `<button>`, `<input>`, `<h1>`, `<img>`).
    *   **Molecules:** Kelompok *atoms* yang berfungsi bersama sebagai satu unit (misal: *search form* yang terdiri dari `<input>` dan `<button>`).
    *   **Organisms:** Kelompok *molecules* dan/atau *atoms* yang membentuk bagian UI yang lebih kompleks dan fungsional (misal: *header* yang terdiri dari logo, navigasi, dan *search form*).
    *   **Templates:** Mengatur *organisms* ke dalam tata letak halaman tanpa konten nyata.
    *   **Pages:** Instansi spesifik dari *templates* dengan konten nyata, mewakili halaman yang akan dilihat pengguna.

### 3.2. Struktur Direktori

```
web-warkop/
├── public/
│   ├── index.html
│   ├── assets/
│   │   ├── images/
│   │   ├── favicons/
│   │   └── ...
│   └── sitemap.xml
├── src/
│   ├── main.js                 // Entry point aplikasi
│   ├── components/             // Direktori untuk komponen UI yang dapat digunakan kembali
│   │   ├── atoms/
│   │   │   ├── Button/
│   │   │   │   ├── Button.js
│   │   │   │   └── Button.html (template string atau plain HTML)
│   │   │   ├── Icon/
│   │   │   └── ...
│   │   ├── molecules/
│   │   │   ├── MenuItemCard/
│   │   │   │   ├── MenuItemCard.js
│   │   │   │   └── MenuItemCard.html
│   │   │   ├── ContactForm/
│   │   │   └── ...
│   │   ├── organisms/
│   │   │   ├── Header/
│   │   │   │   ├── Header.js
│   │   │   │   └── Header.html
│   │   │   ├── Footer/
│   │   │   ├── MenuList/
│   │   │   ├── GalleryGrid/
│   │   │   └── ...
│   │   └── templates/           // Tata letak halaman abstrak
│   │       ├── DefaultLayout/
│   │       └── ...
│   ├── pages/                   // Direktori untuk halaman-halaman spesifik
│   │   ├── home.js
│   │   ├── menu.js
│   │   ├── locations.js
│   │   ├── contact.js
│   │   └── ...
│   ├── services/                // Logika bisnis, interaksi API
│   │   ├── api.js
│   │   ├── menuService.js
│   │   └── ...
│   ├── store/                   // Manajemen state global
│   │   ├── index.js             // Global state store
│   │   └── actions.js
│   ├── utils/                   // Fungsi-fungsi utilitas
│   │   ├── helpers.js
│   │   ├── dom.js
│   │   └── ...
│   └── styles/                  // Custom CSS (jika ada, selain Tailwind)
│       └── global.css           // CSS untuk reset atau kustomisasi khusus
└── package.json
```

### 3.3. Contoh Komponen Utama

*   **`Header` (Organism):** Navigasi utama, logo warkop, tombol (misal: "Pesan Sekarang").
*   **`Footer` (Organism):** Informasi kontak, jam buka, tautan media sosial, *copyright*.
*   **`HeroSection` (Organism):** Bagian utama halaman depan dengan gambar latar, judul menarik, dan *call-to-action*.
*   **`MenuItemCard` (Molecule):** Menampilkan satu item menu (gambar, nama, deskripsi, harga).
*   **`MenuList` (Organism):** Mengelompokkan `MenuItemCard` berdasarkan kategori.
*   **`LocationMap` (Organism):** Integrasi peta (misal: Google Maps) dengan penanda lokasi warkop.
*   **`ContactForm` (Molecule):** Formulir untuk mengirim pesan ke warkop.
*   **`GalleryGrid` (Organism):** Menampilkan kumpulan gambar (foto suasana warkop, makanan, dll).
*   **`EventCard` (Molecule):** Menampilkan detail satu acara (nama, tanggal, deskripsi).

Setiap komponen akan diimplementasikan sebagai modul ES6, bertanggung jawab atas *markup* HTML-nya (menggunakan *template literals* atau memuat file `.html` terpisah), logika JavaScript, dan interaksi dengan *state* lokal atau global.

## 4. State & Event Handling

Pendekatan untuk manajemen *state* dan *event* akan berpusat pada JavaScript murni, memastikan performa dan kendali penuh.

### 4.1. Manajemen State

*   **State Lokal Komponen:**
    *   Setiap komponen akan mengelola *state* internalnya sendiri (misalnya, nilai input formulir, status *toggle* UI) menggunakan variabel JavaScript biasa atau objek.
    *   Perubahan *state* lokal akan memicu *re-render* atau pembaruan elemen DOM yang relevan secara langsung oleh komponen tersebut.

*   **State Global (Custom Store):**
    *   Untuk *state* yang perlu diakses atau dibagikan oleh banyak komponen yang tidak memiliki hubungan *parent-child* langsung (misalnya, *keranjang belanja*, *status autentikasi pengguna*, *tema aplikasi*), kami akan membuat *custom store* sederhana.
    *   **Implementasi:** Sebuah objek JavaScript polos yang bertindak sebagai *store*. Komponen dapat berlangganan (subscribe) ke perubahan *state* ini.
    *   **Mekanisme:**
        1.  **`store.js`:** Sebuah modul yang mengekspor objek *state* (misal, menggunakan `Proxy` untuk reaktivitas atau metode *setter* untuk memicu pembaruan) dan fungsi untuk memodifikasi *state* tersebut (`dispatch` atau `commit`).
        2.  **`subscribe` / `publish`:** Komponen dapat mendaftarkan *callback function* untuk dipanggil setiap kali *state* global berubah. `store` akan memanggil semua *callback* yang terdaftar setelah *state* dimodifikasi.
        3.  **Contoh:**
            ```javascript
            // store/index.js
            let state = {
                cart: [],
                user: null,
                theme: 'light'
            };
            const subscribers = [];

            export const store = new Proxy(state, {
                set(target, property, value) {
                    target[property] = value;
                    subscribers.forEach(callback => callback(property, value));
                    return true;
                }
            });

            export const subscribe = (callback) => {
                subscribers.push(callback);
                // Return an unsubscribe function
                return () => subscribers.filter(cb => cb !== callback);
            };

            export const actions = {
                addToCart(item) {
                    store.cart = [...store.cart, item];
                },
                setTheme(newTheme) {
                    store.theme = newTheme;
                }
            };
            ```
            ```javascript
            // Dalam komponen yang membutuhkan cart
            import { store, subscribe, actions } from '../store';

            class CartDisplay {
                constructor() {
                    this.element = document.createElement('div');
                    subscribe(() => this.render()); // Subscribe to any state change
                    this.render();
                }

                render() {
                    this.element.innerHTML = `Keranjang Anda: ${store.cart.length} item`;
                }
            }
            ```

### 4.2. Penanganan Event

*   **Standard DOM Events:**
    *   Menggunakan `element.addEventListener()` dan `element.removeEventListener()` untuk menangani interaksi pengguna (klik, input, submit, dll.).
    *   **Event Delegation:** Untuk elemen yang dibuat secara dinamis atau daftar yang panjang, akan diterapkan *event delegation* pada elemen *parent* untuk efisiensi performa dan manajemen memori.

*   **Custom Events untuk Komunikasi Antar Komponen:**
    *   Untuk komunikasi *child-to-parent* atau *sibling-to-sibling* tanpa melalui *global store*, `CustomEvent` API akan digunakan.
    *   **Mekanisme:** Komponen *child* akan `dispatchEvent` sebuah `CustomEvent` dengan detail data yang relevan. Komponen *parent* (atau komponen lain yang relevan) akan mendengarkan *event* tersebut.
    *   **Contoh:**
        ```javascript
        // Dalam MenuItemCard.js (child component)
        class MenuItemCard {
            // ...
            handleAddToCart() {
                const event = new CustomEvent('itemAddedToCart', {
                    bubbles: true, // Event bubbles up the DOM tree
                    detail: { item: this.itemData, quantity: 1 }
                });
                this.element.dispatchEvent(event);
            }
            // ...
        }

        // Dalam MenuList.js (parent component)
        class MenuList {
            // ...
            constructor() {
                // ...
                this.element.addEventListener('itemAddedToCart', this.handleItemAdded);
            }

            handleItemAdded(event) {
                console.log('Item ditambahkan:', event.detail.item.name);
                // Update global cart state using actions.addToCart(event.detail.item);
            }
            // ...
        }
        ```

### 4.3. Aliran Data (Data Flow)

*   **Unidirectional Data Flow:** Data akan mengalir dalam satu arah yang dapat diprediksi:
    *   **Parent to Child:** Data diteruskan dari komponen *parent* ke *child* melalui argumen saat membuat atau memperbarui komponen *child*.
    *   **Child to Parent:** Komponen *child* berkomunikasi kembali ke *parent* melalui *custom events* atau *callbacks* yang diteruskan dari *parent*.
    *   **Global State:** Perubahan *state* global dipicu oleh *actions*, dan komponen yang berlangganan akan memperbarui diri mereka sendiri.

## 5. SEO & Performance Target

Sebagai Antigravity Lead System Architect, prioritas utama adalah memastikan website "web warkop" memiliki performa dan SEO yang luar biasa untuk pengalaman pengguna terbaik dan visibilitas tinggi di mesin pencari.

### 5.1. SEO (Search Engine Optimization)

*   **HTML5 Semantik:** Penggunaan elemen HTML yang tepat (`<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`, dll.) akan secara intrinsik meningkatkan pemahaman konten oleh *search engine*.
*   **Meta Tags:** Optimasi `title`, `description`, `keywords`, `viewport` pada setiap halaman. Implementasi *Open Graph* (`og:title`, `og:description`, `og:image`, `og:url`) untuk *sharing* di media sosial dan *Twitter Cards*.
*   **Struktur URL Bersih:** URL yang *human-readable* dan deskriptif (misal: `/menu`, `/lokasi`, `/kontak`).
*   **Konten Berkualitas & Relevan:** Konten yang informatif dan relevan dengan kata kunci "warkop", "kopi", "makanan", "tempat nongkrong", "event" di setiap halaman.
*   **Mobile-First Design:** Website akan dirancang responsif dan dioptimalkan untuk perangkat seluler terlebih dahulu, yang merupakan faktor peringkat penting bagi Google.
*   **Schema.org Markup (Structured Data):** Implementasi *JSON-LD* untuk memberikan informasi kontekstual kepada *search engine*.
    *   **`LocalBusiness`:** Untuk informasi warkop (nama, alamat, telepon, jam buka, rating).
    *   **`Menu`, `MenuItem`:** Untuk daftar menu dan item-itemnya.
    *   **`Event`:** Untuk acara-acara yang diselenggarakan warkop.
*   **Sitemap XML:** File `sitemap.xml` yang selalu diperbarui untuk membantu *search engine* mengindeks semua halaman dengan efisien.
*   **Robots.txt:** Mengatur akses *search engine crawler* ke halaman-halaman tertentu.
*   **Kecepatan Halaman:** (Lihat bagian Performance) Kecepatan *loading* halaman adalah faktor peringkat SEO yang krusial.

### 5.2. Performance (Kinerja)

Target kami adalah mencapai skor tinggi pada *Core Web Vitals* dan metrik performa lainnya.

*   **Core Web Vitals:**
    *   **Largest Contentful Paint (LCP):** Target di bawah 2.5 detik. Dicapai dengan:
        *   Optimasi gambar utama (hero image) dan konten di atas lipatan.
        *   Meminimalkan blokir render CSS (Tailwind CDN membantu).
        *   Pre-rendering / SSG (lihat di bawah).
    *   **First Input Delay (FID):** Target di bawah 100 milidetik. Dicapai dengan:
        *   Meminimalkan JavaScript yang dieksekusi saat *initial load*.
        *   Memecah tugas JavaScript yang panjang menjadi tugas-tugas kecil.
    *   **Cumulative Layout Shift (CLS):** Target di bawah 0.1. Dicapai dengan:
        *   Menentukan dimensi eksplisit untuk gambar dan video.
        *   Menghindari injeksi konten dinamis di atas konten yang sudah ada.
        *   Memuat font secara efisien (menggunakan `font-display: swap`).

*   **Optimasi Gambar dan Media:**
    *   **Kompresi Gambar:** Menggunakan alat kompresi gambar (misal: TinyPNG, Squoosh) dan format modern (WebP, AVIF) untuk ukuran file yang lebih kecil.
    *   **Gambar Responsif:** Menggunakan `srcset` dan `sizes` pada elemen `<img>` untuk menyajikan gambar dengan ukuran yang tepat sesuai perangkat pengguna.
    *   **Lazy Loading:** Menggunakan atribut `loading="lazy"` pada gambar dan `<iframe>` yang berada di bawah lipatan (*below the fold*) untuk menunda pemuatan hingga dibutuhkan.

*   **Minimalisir JavaScript & CSS:**
    *   **Vanilla JS:** Dengan tidak menggunakan *framework* besar, ukuran *bundle* JavaScript akan sangat minimal.
    *   **Tailwind CSS CDN:** CDN menyediakan aset CSS secara global dengan *caching* yang efisien.

*   **Caching Strategi:**
    *   **Browser Caching:** Mengatur *header HTTP caching* yang tepat untuk aset statis (gambar, CSS, JS) agar browser dapat menyimpannya dan tidak perlu mengunduhnya ulang.
    *   **CDN (Content Delivery Network):** Menggunakan CDN untuk mendistribusikan aset statis secara global, mengurangi latensi bagi pengguna di berbagai lokasi.

*   **Pre-rendering / Static Site Generation (SSG):**
    *   Meskipun menggunakan Vanilla JS, kita dapat *pre-render* halaman-halaman penting (Home, Menu, Lokasi) menjadi file HTML statis pada saat *build*.
    *   **Manfaat:** Halaman pertama akan dimuat sangat cepat karena browser hanya perlu mem-parsing HTML dan CSS, tanpa menunggu JavaScript dieksekusi. Ini sangat baik untuk LCP dan SEO.
    *   **Implementasi:** Menggunakan *tools* seperti `Puppeteer` atau *script* kustom pada *build process* untuk menjelajahi halaman dan menyimpan *output* HTML yang sudah di-*render*. JavaScript kemudian akan "menghidrasi" halaman tersebut setelah dimuat.

*   **Asset Bundling & Minification (via Build Tools):**
    *   Jika menggunakan Vite/Parcel, JavaScript modul akan di-*bundle* dan di-*minify* untuk produksi, mengurangi jumlah permintaan HTTP dan ukuran file.

## 6. Kesimpulan

Arsitektur yang diuraikan dalam dokumen ini untuk proyek "web warkop" mengadopsi pendekatan modern yang memprioritaskan performa, SEO, dan kemudahan pemeliharaan. Dengan memanfaatkan kekuatan HTML5 Semantik, Tailwind CSS CDN, dan Vanilla ES6+ Modular JavaScript, kami akan membangun sebuah website yang:

*   **Sangat Cepat:** Dengan *bundle size* minimal dan optimasi *loading* aset.
*   **Ramah SEO:** Memastikan visibilitas tinggi di mesin pencari melalui struktur semantik dan *structured data*.
*   **Mudah Dikembangkan:** Struktur komponen yang jelas dan modular.
*   **Skalabel:** Meskipun sederhana, fondasi yang kuat ini memungkinkan penambahan fitur di masa depan tanpa harus merombak ulang seluruh sistem.

Sebagai Antigravity Lead System Architect, saya yakin pendekatan ini akan memberikan solusi optimal yang memenuhi semua kebutuhan "web warkop" dan memberikan pengalaman terbaik bagi penggunanya.

---