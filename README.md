# KampusGuide Access

Asisten navigasi suara untuk mahasiswa/pengunjung tunanetra & *low vision* di Kampus ITS.
Web app (PWA) satu file, tanpa build. Implementasi langsung dari `docs/prd_imk.md`.

## Jalankan

Mikrofon (Web Speech) & Service Worker butuh **secure context** → pakai `localhost` atau HTTPS.
`file://` tidak bisa.

```bash
# dari folder ini
python -m http.server 5500
# buka http://localhost:5500
```

Alternatif: ekstensi **Live Server** (VS Code), atau `npx serve`.

Uji penuh di **Chrome/Edge desktop** (localhost) atau **Android Chrome** (lewat HTTPS,
mis. di-deploy ke Vercel/Netlify/GitHub Pages) untuk haptic + voice.

## Fitur (peta ke PRD)

| PRD | Implementasi |
|-----|--------------|
| F1 Voice-first search | `webkitSpeechRecognition` (id-ID); ucapan dicocokkan ke gedung |
| F2 Audio turn-by-turn | `speechSynthesis` (TTS id-ID) tiap langkah |
| F3 Haptic spasial | `navigator.vibrate`: lurus `[200]`, kanan `[200,100,200]`, kiri `[600]`, tiba pola berulang |
| F4 Landmark/rintangan | Peringatan kuning + bingkai kuning + suara otomatis |
| F5 SOS one-tap | Overlay panggilan + `tel:` SKK ITS + kirim koordinat `geolocation` |
| §6 Token kontras tinggi | CSS variables: canvas/ink/warn/action/sos/tile |
| §6 Tipografi | SF Pro Display/Text (Apple) → Inter (fallback) |
| §5 Navigasi dangkal | Pager 3 halaman (scroll-snap) + overlay state |
| §9.2 Luring | Service worker cache; Favorit jalan offline |

## Interaksi

- **Geser kiri/kanan** (sentuh atau 2 jari trackpad) pindah halaman: `Favorit ‹ Input Suara › SOS`.
- **Ketuk dua kali** mengaktifkan fitur (ketuk sekali = dengar label via TTS; cegah salah-tekan).
- Pembaca layar (TalkBack/VoiceOver): tiap kontrol `<button>` ber-`aria-label`; gestur dobel-ketuk OS = aktivasi.
- `aria-live` mengumumkan instruksi navigasi.
- `prefers-reduced-motion` dihormati.

## Dukungan browser

- Pengenalan suara: Chrome / Edge (`webkit`). Safari/Firefox → fallback ke Favorit.
- TTS & getaran: terbaik di Android Chrome.

## Struktur

```
index.html              aplikasi (HTML + CSS + JS inline)
manifest.webmanifest    metadata PWA
sw.js                   service worker (offline)
icon.svg                ikon aplikasi
docs/prd_imk.md         dokumen acuan
```
