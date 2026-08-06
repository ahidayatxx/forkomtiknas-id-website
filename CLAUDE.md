# Forkomtiknas.id Website — CLAUDE.md

## Project Overview

Static frontend portal for **Forum Komunikasi Teknologi Informasi Kesehatan Nasional (Forkomtiknas)** — Indonesia's national health IT communication forum. Built as Phase 2 of the PRD roadmap: complete 7-page static frontend, no backend.

**PRD Reference**: `~/Downloads/PRD Website Forkomtiknas.id - Forum Komunikasi Teknologi Informasi Kesehatan Nasional.md`

## Technology Stack

| Category  | Technology                    | Notes                               |
|-----------|-------------------------------|-------------------------------------|
| Framework | React 18 (CDN)                | `unpkg.com/react@18/umd/react.production.min.js` |
| JSX       | Babel Standalone (CDN)        | In-browser transform, `type="text/babel"` |
| Styling   | Tailwind CSS 3 (CDN)          | Custom config with `primary` + `teal` palette |
| Fonts     | Inter + Plus Jakarta Sans     | Google Fonts |
| Runtime   | Static HTML (no build step)   | Open directly in browser |

## File Structure

```
forkomtiknas-id-website/
├── index.html          # Beranda — Hero, Stats, Pengumuman, Berita, Agenda, Dokumen, CTA
├── tentang.html        # Tentang Kami — Profil, Visi Misi, Struktur Org, AD/ART, Mitra
├── keanggotaan.html    # Keanggotaan — Manfaat, Jenis, Formulir, Direktori, Portal Login
├── publikasi.html      # Publikasi & Dokumen — Filterable/searchable document repository
├── kegiatan.html       # Kegiatan & Acara — Event list, Galeri
├── berita.html         # Berita & Opini — Featured article, filterable article grid
├── kontak.html         # Kontak & Layanan — Formulir, Info kontak, Sosmed, FAQ accordion
└── CLAUDE.md
```

## Essential Commands

```bash
# Open any page directly — no server needed
open index.html

# Optional: live-reload dev server
cd /Users/ahmadhidayat/claude-code/projects/forkomtiknas-id-website
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Design System

### Color Palette

```js
primary: { 50, 100, 200, 600, 700, 800, 900 }  // Navy Blue — authority, trust
teal:    { 400, 500, 600, 700 }                  // Teal — health, digital
```

- **Background**: `bg-slate-50` (page), `bg-white` (cards)
- **Page hero banners**: `bg-primary-900 text-white`
- **CTAs**: `bg-primary-800` / `bg-primary-900` buttons
- **Accent**: Amber (`bg-amber-50/200`) for announcements

### Fonts

- Body/UI: **Inter** → `font-sans`
- Headings: **Plus Jakarta Sans** → `font-display`

### Animations

```js
'fade-in': 'fadeIn 0.6s cubic-bezier(0.16,1,0.3,1) forwards'
'slide-up': 'slideUp 0.5s cubic-bezier(0.16,1,0.3,1) forwards'
```

### Global CSS

All pages include these rules inside `<style>`:

```css
* { touch-action: manipulation; }
button, a { min-height: 44px; display: inline-flex; align-items: center; }
body { -webkit-font-smoothing: antialiased; }
.line-clamp-2 / .line-clamp-3  /* webkit multi-line clamp */
```

## Shared Components (identical in all files)

Every file contains verbatim copies of:
- `IconBase` helper + `MenuIcon` / `XIcon`
- `NAV_LINKS` array (7 entries)
- `Header` component — sticky navy bar, active page highlight, hamburger for mobile
- `Footer` component — 4-column navy footer with nav, contact, social badges

**activePage prop**: Pass the current filename (e.g. `"publikasi.html"`) to `<Header>` to highlight the active nav item.

## Responsive Breakpoints

| Breakpoint | Width  | Layout change                              |
|------------|--------|--------------------------------------------|
| `sm:`      | 640px  | 2-column grids, inline hero buttons        |
| `md:`      | 768px  | Desktop nav appears, hamburger hidden      |
| `lg:`      | 1024px | 3/4-column grids, two-column content areas |

## Adding New Content

### New Page

1. Copy any existing `.html` file as a template
2. Change `<title>` and `<meta description>`
3. Update `activePage` prop to match the new filename
4. Add the new filename to `NAV_LINKS` in **all 7 existing files**
5. Add footer nav entry in **all 7 existing files**

### New Section in an Existing Page

Each page's sections are standalone React components assembled in `<App>`. Add a new component before `<Footer />`:

```jsx
const NewSection = () => (
  <section className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-16">
    <h2 className="font-display font-bold text-2xl text-slate-900 mb-8">Section Title</h2>
    {/* content */}
  </section>
);
```

### Interactive Features Pattern

Pages use `useState` for client-side filtering:

```jsx
const [activeKategori, setActiveKategori] = useState('Semua');
const filtered = DATA.filter(item =>
  activeKategori === 'Semua' || item.kategori === activeKategori
);
```

## Notes on Placeholder Interactivity

All form submissions and download buttons fire `alert()` — they are frontend-only placeholders awaiting backend integration (Phase 3 of the PRD roadmap):
- Formulir Pendaftaran → `alert('Terima kasih! Tim kami akan menghubungi Anda...')`
- Unduh PDF → `alert('Mengunduh... Fitur ini akan diaktifkan setelah integrasi backend.')`
- Portal Login → `alert('Fitur login akan tersedia setelah akun Anda diverifikasi.')`

## Attribution

- **Organization**: Forum Komunikasi Teknologi Informasi Kesehatan Nasional (Forkomtiknas)
- **Developer**: Ahmad Hidayat
- **PRD Version**: 1.0 (6 Agustus 2026)
