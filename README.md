# TemporalCal README

Bu belge, aynı proje için **Türkçe** ve **English** README içeriklerini tek dosyada birleştirir. Üstteki bağlantılara tıklayarak ilgili bölüme kaydırabilirsiniz.

**Dil seçimi / Language switch:** [Türkçe](#turkce) · [English](#english)

***

<a id="turkce"></a>

# Türkçe

[↑ Üste dön](#temporalcal-readme) · [English bölümüne git](#english)

## 📅 TemporalCal

> **TC39 Temporal API** kullanılarak sıfırdan yazılmış, Türkçe arayüzlü modern takvim uygulaması.  
> Eski `Date` nesnesine ihtiyaç duymadan tarih hesaplama, etkinlik yönetimi ve zengin takvim görünümleri.

***

## 🚀 Özellikler

### Temporal API Entegrasyonu
- Tüm tarih işlemleri `Temporal.PlainDate` ile yapılır — eski `Date` nesnesi hiç kullanılmaz
- `Temporal.Now.plainDateISO()` ile anlık tarih alımı
- `.add()` / `.subtract()` ile güvenli tarih aritmetiği (ay sonu / artık yıl kenar durumları otomatik yönetilir)
- `.until()` ile iki tarih arası `Duration` hesabı
- `.weekOfYear`, `.dayOfYear`, `.dayOfWeek`, `.daysInMonth`, `.inLeapYear` özellikleri arayüzde gösterilir
- Yıl/ay değişiminde `daysInMonth` sorgulanarak geçersiz tarih üretimi engellenir

### Takvim Görünümleri
- **Ay Görünümü** — 6×7 ızgara, Pazartesi-başlangıçlı ISO hafta düzeni
- **Hafta Görünümü** — Saatlik (08:00–18:00) dikey ızgara, her kolonda 7 gün

### Navigasyon
- **İleri / Geri** butonları ile ay veya hafta atla
- **Yıl Seçici** dropdown — 100 yıllık aralıkta (±50 yıl) seçim
- **Bugün** butonu ile anında bugüne dön
- **Klavye kısayolları**: `←/→/↑/↓` ile gün/hafta atlama, `N` ile yeni etkinlik, `Esc` ile modal kapatma

### Etkinlik Yönetimi
- Tarih, saat ve renk seçimiyle etkinlik ekleme (modal form)
- Etkinlikleri silebilme
- Ay ızgarasında etkinlik noktaları (max 3)
- Hafta görünümünde saat bazlı etkinlik çipleri
- Etkinlik verisi in-memory tutulur (localStorage yok — sandboxed iframe uyumlu)

### Yan Panel
- Mini takvim (bağımsız ay navigasyonu)
- **Temporal Bilgi Paneli**: seçili günün haftası, yılın günü, artık yıl durumu, yıl sonuna kalan gün, bugünden fark
- Seçili güne ait etkinlik listesi

### Tasarım
- Işık / Karanlık mod (sistem tercihi + manuel toggle)
- Nexus Design System renk paleti (OKLCH renk uzayı)
- Satoshi + Cabinet Grotesk font çifti (Fontshare CDN)
- Tam responsive (375px mobil → 1280px+ masaüstü)
- CSS `clamp()` ile akışkan tipografi ve boşluk
- `@starting-style` modal giriş animasyonu
- `prefers-reduced-motion` desteği

***

## 🧩 Teknoloji Yığını

| Katman | Teknoloji |
|---|---|
| **Tarih / Zaman** | [TC39 Temporal API](https://tc39.es/proposal-temporal/) |
| **Polyfill** | [temporal-polyfill v0.2.5](https://github.com/nicolo-ribaudo/tc39-proposal-temporal-polyfill) (Chrome 144 / Firefox 139 öncesi) |
| **İkonlar** | [Lucide Icons](https://lucide.dev) (UMD CDN) |
| **Fontlar** | [Fontshare](https://www.fontshare.com) — Satoshi + Cabinet Grotesk |
| **Altyapı** | Saf HTML5 + CSS3 + Vanilla JS (derleme adımı yok) |

***

## 📦 Kurulum

Herhangi bir bağımlılık veya derleme adımı **gerekmez**.

```bash
# Repoyu klonla
git clone https://github.com/kullanici-adi/temporal-cal.git

# Klasöre gir
cd temporal-cal

# temporal-calendar.html dosyasını tarayıcıda aç
open temporal-calendar.html
# veya
npx serve .
```

***

## 🗂 Dosya Yapısı

```
temporal-cal/
├── temporal-calendar.html   ← Tek dosya, tüm uygulama (HTML + CSS + JS)
├── README.md                ← İngilizce açıklama
└── README-TR.md             ← Türkçe açıklama (bu dosya)
```

***

## 🌐 Tarayıcı Uyumluluğu

| Tarayıcı | Native Temporal | Polyfill ile |
|---|---|---|
| Chrome ≥ 144 | ✅ | — |
| Firefox ≥ 139 | ✅ | — |
| Safari (henüz) | ❌ | ✅ |
| Edge ≥ 144 | ✅ | — |
| Eski tarayıcılar | ❌ | ✅ |

Polyfill CDN üzerinden otomatik yüklenir; ek yapılandırma gerekmez.

***

## ⌨️ Klavye Kısayolları

| Tuş | Eylem |
|---|---|
| `←` | Önceki gün |
| `→` | Sonraki gün |
| `↑` | Önceki hafta |
| `↓` | Sonraki hafta |
| `N` | Yeni etkinlik ekle |
| `Esc` | Modalı kapat |

***

## 🔍 Temporal API — Kullanılan Özellikler

```js
// Bugünün tarihini al
const today = Temporal.Now.plainDateISO();

// String'den PlainDate oluştur
const d = Temporal.PlainDate.from('2026-04-30');

// Tarih aritmetiği
const nextWeek  = d.add({ weeks: 1 });
const lastMonth = d.subtract({ months: 1 });

// Metadata
d.weekOfYear   // → 18
d.dayOfYear    // → 120
d.dayOfWeek    // → 4 (Perşembe, ISO: 1=Pzt)
d.daysInMonth  // → 30
d.inLeapYear   // → false

// İki tarih arası süre
const duration = today.until(d);
duration.days  // → 0

// Yıl değişiminde güvenli normalizasyon
const safeDay = Math.min(day, Temporal.PlainDate.from({ year, month, day: 1 }).daysInMonth);
```

***

## 🎨 Tasarım Kararları

- **Tek dosya mimarisi**: Dağıtım kolaylığı için tüm CSS, HTML ve JS tek `temporal-calendar.html` dosyasındadır.
- **Yok: localStorage/sessionStorage**: Sandboxed iframe ortamlarında depolama erişimi engellendiğinden state in-memory tutulur.
- **Temporal öncelikli**: Tarihe dokunan her satır Temporal API kullanır; `Date` nesnesi hiçbir yerde çağrılmaz.
- **Renk sistemi (OKLCH)**: Tüm renkler `oklch()` renk uzayında tanımlanır, ışık/karanlık geçişleri perceptually uniform kalır.

***

## 📄 Lisans

MIT © 2026

***

<a id="english"></a>

# English

[↑ Back to top](#temporalcal-readme) · [Türkçe bölüme git](#turkce)

## 📅 TemporalCal

> A modern calendar application built from scratch with the **TC39 Temporal API** — zero legacy `Date` usage.  
> Full date arithmetic, event management, and rich calendar views powered entirely by `Temporal.PlainDate`.

***

## 🚀 Features

### Temporal API Integration
- All date logic uses `Temporal.PlainDate` — the legacy `Date` object is never instantiated
- `Temporal.Now.plainDateISO()` for current-date retrieval
- Safe date arithmetic via `.add()` / `.subtract()` (month-end overflow and leap-year edge cases handled automatically)
- Duration computation between two dates using `.until()`
- `weekOfYear`, `dayOfYear`, `dayOfWeek`, `daysInMonth`, `inLeapYear` properties surfaced in the UI
- Year/month navigation queries `daysInMonth` before committing to prevent invalid dates

### Calendar Views
- **Month View** — 6×7 grid, ISO week layout starting Monday
- **Week View** — Hourly vertical grid (08:00–18:00) across 7 columns

### Navigation
- **Prev / Next** buttons to move by month or week
- **Year Picker** dropdown — select any year in a ±50-year range around the current view
- **Today** button to jump back instantly
- **Keyboard navigation**: `←/→/↑/↓` to move by day/week, `N` for a new event, `Esc` to close the modal

### Event Management
- Add events with title, date, time, and color (modal form)
- Delete events from the side panel
- Event dots on month grid (up to 3 visible per day)
- Hour-slot event chips in week view
- All event state is kept in-memory (no localStorage — sandboxed iframe compatible)

### Side Panel
- Mini calendar with independent month navigation
- **Temporal Info Panel**: week of year, day of year, leap year status, days remaining until year-end, difference from today
- Event list for the selected date

### Design
- Light / Dark mode (respects system preference + manual toggle)
- Nexus Design System color palette (OKLCH color space)
- Satoshi + Cabinet Grotesk font pairing (Fontshare CDN)
- Fully responsive (375px mobile → 1280px+ desktop)
- Fluid typography and spacing via CSS `clamp()`
- `@starting-style` modal enter animation
- `prefers-reduced-motion` support

***

## 🧩 Tech Stack

| Layer | Technology |
|---|---|
| **Date / Time** | [TC39 Temporal API](https://tc39.es/proposal-temporal/) |
| **Polyfill** | [temporal-polyfill v0.2.5](https://github.com/nicolo-ribaudo/tc39-proposal-temporal-polyfill) (pre-Chrome 144 / pre-Firefox 139) |
| **Icons** | [Lucide Icons](https://lucide.dev) (UMD CDN) |
| **Fonts** | [Fontshare](https://www.fontshare.com) — Satoshi + Cabinet Grotesk |
| **Runtime** | Pure HTML5 + CSS3 + Vanilla JS (no build step) |

***

## 📦 Getting Started

No dependencies or build tooling required.

```bash
# Clone the repo
git clone https://github.com/your-username/temporal-cal.git

# Enter the folder
cd temporal-cal

# Open directly in browser
open temporal-calendar.html
# or serve locally
npx serve .
```

***

## 🗂 File Structure

```
temporal-cal/
├── temporal-calendar.html   ← Single file — complete app (HTML + CSS + JS)
├── README.md                ← English documentation (this file)
└── README-TR.md             ← Turkish documentation
```

***

## 🌐 Browser Compatibility

| Browser | Native Temporal | With Polyfill |
|---|---|---|
| Chrome ≥ 144 | ✅ | — |
| Firefox ≥ 139 | ✅ | — |
| Safari (pending) | ❌ | ✅ |
| Edge ≥ 144 | ✅ | — |
| Older browsers | ❌ | ✅ |

The polyfill loads automatically from CDN; no additional configuration needed.

***

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `←` | Previous day |
| `→` | Next day |
| `↑` | Previous week |
| `↓` | Next week |
| `N` | Add new event |
| `Esc` | Close modal |

***

## 🔍 Temporal API — Features Used

```js
// Get today's date
const today = Temporal.Now.plainDateISO();

// Parse a PlainDate from string
const d = Temporal.PlainDate.from('2026-04-30');

// Date arithmetic
const nextWeek  = d.add({ weeks: 1 });
const lastMonth = d.subtract({ months: 1 });

// Rich metadata
d.weekOfYear   // → 18
d.dayOfYear    // → 120
d.dayOfWeek    // → 4 (Thursday; ISO: 1=Monday)
d.daysInMonth  // → 30
d.inLeapYear   // → false

// Duration between two dates
const duration = today.until(d);
duration.days  // → 0

// Safe normalization on year change
const safeDay = Math.min(
  day,
  Temporal.PlainDate.from({ year, month, day: 1 }).daysInMonth
);
```

***

## 🎨 Design Decisions

- **Single-file architecture**: All CSS, HTML, and JS live in one `temporal-calendar.html` for frictionless deployment and sharing.
- **No localStorage/sessionStorage**: Storage APIs are blocked in sandboxed iframes; all state is kept in-memory.
- **Temporal-first**: Every line that touches a date goes through the Temporal API. The legacy `Date` object is never called.
- **OKLCH color system**: All color tokens are defined in `oklch()`, ensuring perceptually uniform transitions between light and dark themes.

***

## 📄 License

MIT © 2026
