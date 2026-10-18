# Crypto + FX Widget for HYTE Y70 Touch

หน้าเว็บไฟล์เดียวสำหรับแสดงราคาคริปโตแบบเรียลไทม์และอัตราแลกเปลี่ยนเงินบาท
บนจอสัมผัสของเคส HYTE Y70 Touch ผ่าน Web View widget ของ Nexus

> 🇬🇧 **English version is at the bottom of this page** — scroll down or [jump to it](#english).

---

## นี่คืออะไร

ไฟล์ `index.html` ไฟล์เดียวจบ ไม่มี dependency ไม่ต้องติดตั้งอะไร ไม่ต้องใช้ API key

เอาไปวางบน static host ที่ไหนก็ได้ (GitHub Pages ฟรีและพอ) แล้วเอา URL ไปใส่ใน
**Web View widget** ของ Nexus จอเคสก็จะแสดงราคาให้ทันที

เลย์เอาต์ปรับขนาดตัวอักษรเองตามขนาดช่อง widget — วางช่องเล็กช่องใหญ่ก็อ่านได้

---

## สิ่งที่แสดง

### เหรียญคริปโต — หมุนอัตโนมัติทุก 8 วินาที

| คู่เหรียญ | แหล่งข้อมูล | ทศนิยม |
|---|---|---|
| BTC/USDT | Binance | 0 |
| ETH/USDT | Binance | 2 |
| SOL/USDT | Binance | 2 |
| BNB/USDT | Binance | 2 |
| MNT/USDT | Bybit | 4 |
| BAT/USDT | Binance | 4 |

แต่ละเหรียญแสดง: ราคาล่าสุด · %เปลี่ยนแปลง 24 ชม. · ราคาสูง/ต่ำ 24 ชม. ·
กราฟ sparkline ย้อนหลัง 24 ชม. (แท่ง 15 นาที × 96 แท่ง)

สีทั้งหน้าสลับตาม %24 ชม. — เขียวเมื่อขึ้น แดงเมื่อลง และตัวเลขราคาจะ flash
ทุกครั้งที่ขยับ

**MNT ดึงจาก Bybit** เพราะ Binance Spot ไม่มีคู่นี้ ตัวที่ดึงจาก Bybit ใช้
REST polling ทุก 10 วินาที ส่วนที่เหลือใช้ WebSocket แบบ push เรียลไทม์

หากคู่เหรียญไหนไม่มีอยู่จริงบน exchange ที่ระบุ ระบบจะ**ข้ามออกจากรอบหมุน
อัตโนมัติ** ไม่พัง

### ค่าเงิน — ฐานเป็นเงินบาท (THB)

| สกุล | หน่วยที่แสดง |
|---|---|
| USD | บาท ต่อ 1 ดอลลาร์สหรัฐ |
| JPY | บาท ต่อ **100 เยน** |
| SGD | บาท ต่อ 1 ดอลลาร์สิงคโปร์ |
| CNY | บาท ต่อ 1 หยวน |
| HKD | บาท ต่อ 1 ดอลลาร์ฮ่องกง |
| TWD | บาท ต่อ 1 ดอลลาร์ไต้หวัน |

เยนคิดต่อ 100 หน่วยเพื่อให้ตรงกับบอร์ดร้านแลกเงินในไทย จะได้เทียบกันได้ทันที

สกุลที่เรตมีวันที่เก่ากว่าสกุลอื่น (TWD มักช้ากว่าเพราะมาจาก provider คนละชุด)
จะถูกหรี่สีลงให้สังเกตได้ และบรรทัดล่างสุดกำกับวันที่ของเรตชุดล่าสุดไว้เสมอ

---

## เริ่มใช้งาน

1. **Fork** repo นี้ หรือดาวน์โหลด `index.html` ไปไว้ใน repo ของตัวเอง
2. เปิด GitHub Pages: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**
3. รอราว 1 นาที จะได้ URL หน้าตาแบบ `https://<username>.github.io/<repo>/`
4. ใน Nexus: **Edit Widgets → เพิ่ม Web View widget → ใส่ URL นั้น**

### เคล็ดลับเรื่อง cache

Nexus และ GitHub Pages ต่างก็แคชไฟล์ ถ้าแก้โค้ดแล้วจอไม่เปลี่ยน ให้ต่อเลขเวอร์ชัน
ท้าย URL แล้วเปลี่ยนเลขทุกครั้งที่อัปเดต:

```
https://<username>.github.io/<repo>/?v=2
```

---

## พารามิเตอร์ URL

ต่อท้าย URL ได้เลย ไม่ต้องแก้โค้ด

### `?mode=` — เลือกว่าจะแสดงอะไร

| ค่า | ได้อะไร |
|---|---|
| `all` | ครบทุกอย่าง เรียงบนลงล่าง |
| `compact` | **(ค่าเริ่มต้น)** ตัด sparkline / สูง-ต่ำ / บรรทัดวันที่ออก เพื่อให้ตัวอักษรใหญ่ที่สุด |
| `split` | กล่องเดียวแบ่งซ้าย-ขวา — คริปโตซ้าย ค่าเงินขวาเรียงคอลัมน์เดียว |
| `crypto` | เฉพาะคริปโต |
| `fx` | เฉพาะค่าเงิน (ไม่ยิง exchange เลย ประหยัดทรัพยากร) |

### `?bg=` — สีพื้นหลัง

| ค่า | ได้อะไร |
|---|---|
| `dark` | **(ค่าเริ่มต้น)** `#0A0E0C` ดำอมเขียว ทึบ |
| `black` | ดำสนิท |
| `tint` | ดำ 92% |
| `glass` | ดำ 35% |
| `clear` | โปร่งใส |
| hex เอง | เช่น `?bg=0A1420` หรือ `?bg=000000e6` (8 หลัก = มี alpha) ไม่ต้องพิมพ์ `#` |

เงาตัวอักษรเปิด/ปิดเองตามความทึบของพื้น

ใช้รวมกันได้: `?mode=split&bg=black&v=3`

---

## แหล่งข้อมูล

| แหล่ง | ใช้ทำอะไร | API key |
|---|---|---|
| Binance Spot WebSocket | ราคาเรียลไทม์ (BTC/ETH/SOL/BNB/BAT) | ไม่ต้อง |
| Binance Spot REST | กราฟ sparkline และตรวจคู่เหรียญตอนเริ่ม | ไม่ต้อง |
| Bybit Spot REST | ราคา MNT (polling 10 วินาที) | ไม่ต้อง |
| Frankfurter | อัตราแลกเปลี่ยน (รีเฟรชทุก 30 นาที) | ไม่ต้อง |

ทุกแหล่งเปิด CORS ยิงตรงจากเบราว์เซอร์ได้ ไม่ต้องมี proxy หรือ backend

Frankfurter เรียก API v2 เป็นหลัก และมี v1 เป็นตัวสำรองอัตโนมัติถ้า v2 ล้ม
(แต่ v1 ไม่มี TWD)

---

## การปรับแต่ง

แก้ได้ใน block `CONFIG` ภายใน `index.html` — มีคอมเมนต์กำกับทุกบรรทัด

```js
coins: [ ... ]        // เพิ่ม/ลบเหรียญ · เลือก source เป็น "binance" หรือ "bybit"
fiat:  [ ... ]        // เพิ่ม/ลบสกุลเงิน · unit คือ "บาทต่อกี่หน่วย"
rotationMs:    8000   // ความเร็วหมุนเหรียญ
fiatRefreshMs: 1800000 // ความถี่รีเฟรชค่าเงิน
splitRatio:    0.60   // สัดส่วนซ้าย:ขวา ในโหมด split
fill:          0.90   // เนื้อหากินพื้นที่กี่ % ของกล่อง
defaultContent: "compact"  // โหมดเริ่มต้นเมื่อไม่ใส่ ?mode=
```

สีทั้งหมดอยู่ใน `:root` ด้านบนสุดของไฟล์

> **หากคุณไม่ได้อยู่ประเทศไทย** ค่าเริ่มต้นตั้งฐานเป็นเงินบาทและคิดเยนต่อ
> 100 หน่วยตามธรรมเนียมร้านแลกเงินไทย หากต้องการสกุลอื่นให้แก้
> `CONFIG.fiat` และเปลี่ยน `base=THB` ในฟังก์ชัน `loadFx()`

---

## ข้อกำหนด

- **HYTE Nexus 3.1.3 ขึ้นไป** — Web View widget เพิ่งเพิ่มเข้ามาเมื่อ 16 ตุลาคม 2569
  (เวอร์ชันก่อนหน้าใช้ iFrame widget ซึ่งไฟล์นี้ก็ทำงานได้เหมือนกัน)
- จอ HYTE Y70 Touch หรือ Y70 Touch Infinite (ดูหัวข้อข้อจำกัดเรื่องการทดสอบ)
- เครื่องต้องต่ออินเทอร์เน็ต

---

## ข้อจำกัดที่ทราบ

- **ทดสอบบน Y70 Touch (gen 1) กับ Nexus 3.1.x เท่านั้น** — Y70 Touch Infinite
  ใช้จอคนละความละเอียด (ราว 688 × 2560 เทียบกับ 1100 × 3840 ของ gen 1) แต่
  เลย์เอาต์คำนวณขนาดตัวอักษรจากกล่อง widget จริงตอนรันไทม์ ไม่ได้ผูกกับความ
  ละเอียดใดเป็นการเฉพาะ จึงน่าจะใช้ได้ — แต่ผู้เขียนไม่มีเครื่องทดสอบ
- **พื้นหลังโปร่งใสใช้ไม่ได้** Web View ของ Nexus 3 render เป็นหน้าเว็บจริงซึ่งมี
  ฐานสีขาวทึบ ตั้ง `?bg=clear` แล้วจะได้พื้นขาว ไม่ใช่ wallpaper ทะลุ
- **ธีมออกแบบมาสำหรับพื้นมืดเท่านั้น** ตัวเลขสีเขียวอ่อนอ่านไม่ออกบนพื้นสว่าง
- **อัตราแลกเปลี่ยนเป็น mid-rate รายวัน** ไม่ใช่เรตที่แลกได้จริง ร้านแลกเงินมี
  ส่วนต่างของตัวเอง ใช้ดูทิศทางได้ แต่อย่าใช้อ้างอิงในการทำธุรกรรม
- **ตลาด FX ปิดเสาร์-อาทิตย์** วันหยุดจะเห็นเรตของวันทำการล่าสุดค้างอยู่
  ซึ่งเป็นพฤติกรรมที่ถูกต้อง ไม่ใช่ข้อมูลเสีย
- **การแตะจอเพื่อข้ามเหรียญอาจไม่ทำงาน** เพราะ Nexus ดักการแตะไปเปิดเว็บเต็มจอก่อน
- Binance และ Bybit ถูกบล็อกในบางประเทศ ถ้าเข้าไม่ได้ต้องเปลี่ยนไปใช้แหล่งอื่น
- **อย่าเปิด auto-refresh ของ Web View** หน้านี้อัปเดตตัวเองผ่าน WebSocket อยู่แล้ว
  การรีเฟรชจะตัดการเชื่อมต่อทิ้งโดยไม่จำเป็น

---

## License

MIT — เอาไปใช้ แก้ไข แจกต่อได้ตามสบาย ขอแค่เก็บบรรทัดลิขสิทธิ์ไว้
ดูรายละเอียดใน [LICENSE](LICENSE)

---
---

<a name="english"></a>

# English

A single-page web widget showing live cryptocurrency prices and Thai baht
exchange rates on the touchscreen of a HYTE Y70 Touch case, via the Nexus
Web View widget.

## What this is

One `index.html` file. No dependencies, no build step, no API keys.

Host it anywhere static (GitHub Pages is free and sufficient), then point a
**Web View widget** in Nexus at the URL.

The layout scales its own type size to whatever widget slot you drop it in.

## What it shows

### Crypto — rotates every 8 seconds

| Pair | Source | Decimals |
|---|---|---|
| BTC/USDT | Binance | 0 |
| ETH/USDT | Binance | 2 |
| SOL/USDT | Binance | 2 |
| BNB/USDT | Binance | 2 |
| MNT/USDT | Bybit | 4 |
| BAT/USDT | Binance | 4 |

Each shows last price, 24h change, 24h high/low, and a 24-hour sparkline
(96 bars at 15 minutes each). The accent colour follows the 24h direction,
and the price flashes on every tick.

MNT comes from Bybit because Binance Spot does not list that pair. Bybit
pairs poll over REST every 10 seconds; everything else streams over
WebSocket. Any symbol that does not resolve is **skipped automatically**.

### Currencies — Thai baht base

| Code | Displayed as |
|---|---|
| USD | THB per 1 US dollar |
| JPY | THB per **100 yen** |
| SGD | THB per 1 Singapore dollar |
| CNY | THB per 1 yuan |
| HKD | THB per 1 Hong Kong dollar |
| TWD | THB per 1 Taiwan dollar |

Yen is quoted per 100 units to match the boards at Thai exchange counters.
Currencies whose rate carries an older date than the rest (TWD often does,
as it comes from a different provider set) are dimmed, and the footer always
states the date of the newest rate in the set.

## Getting started

1. **Fork** this repo, or drop `index.html` into your own.
2. Enable GitHub Pages: **Settings → Pages → Deploy from a branch → `main` / `(root)`**.
3. Wait about a minute for `https://<username>.github.io/<repo>/`.
4. In Nexus: **Edit Widgets → add a Web View widget → paste that URL**.

Both Nexus and GitHub Pages cache aggressively. Append a version number and
bump it on every change: `?v=2`, `?v=3`, and so on.

## URL parameters

### `?mode=`

| Value | Result |
|---|---|
| `all` | Everything, stacked top to bottom |
| `compact` | **(default)** Drops the sparkline, high/low and date line for the largest possible type |
| `split` | One tile divided left (crypto) and right (currencies, single column) |
| `crypto` | Crypto only |
| `fx` | Currencies only — makes no exchange calls at all |

### `?bg=`

| Value | Result |
|---|---|
| `dark` | **(default)** `#0A0E0C`, opaque |
| `black` | Pure black |
| `tint` | 92% black |
| `glass` | 35% black |
| `clear` | Transparent |
| raw hex | e.g. `?bg=0A1420` or `?bg=000000e6` (8 digits includes alpha); omit the `#` |

Text shadow toggles itself based on background opacity. Parameters combine:
`?mode=split&bg=black&v=3`

## Data sources

| Source | Used for | API key |
|---|---|---|
| Binance Spot WebSocket | Live prices (BTC/ETH/SOL/BNB/BAT) | None |
| Binance Spot REST | Sparklines and startup symbol probe | None |
| Bybit Spot REST | MNT price, polled every 10s | None |
| Frankfurter | Exchange rates, refreshed every 30 min | None |

All are CORS-enabled and callable straight from the browser — no proxy or
backend required. Frankfurter is called on its v2 API with an automatic
fallback to v1 (which does not carry TWD).

## Customising

Everything lives in the `CONFIG` block inside `index.html`, commented
line by line. Colours are the custom properties under `:root`.

```js
coins: [ ... ]             // add/remove coins; source is "binance" or "bybit"
fiat:  [ ... ]             // add/remove currencies; unit = "base per N units"
rotationMs:     8000       // coin rotation speed
fiatRefreshMs:  1800000    // FX refresh interval
splitRatio:     0.60       // left:right ratio in split mode
fill:           0.90       // how much of the tile the content fills
defaultContent: "compact"  // mode used when ?mode= is absent
```

> **If you are not in Thailand:** the defaults use a THB base and quote yen
> per 100 units, following Thai exchange-counter convention. For another
> base currency, edit `CONFIG.fiat` and change `base=THB` in `loadFx()`.

## Requirements

- **HYTE Nexus 3.1.3 or newer** — the Web View widget arrived on 16 October 2026.
  (Earlier builds used an iFrame widget, which this file also works in.)
- A HYTE Y70 Touch or Y70 Touch Infinite display (see the testing caveat below)
- An internet connection

## Known limitations

- **Only tested on a Y70 Touch (gen 1) running Nexus 3.1.x.** The Y70 Touch
  Infinite ships a different panel (roughly 688 × 2560 against gen 1's
  1100 × 3840). The layout derives its type size from the actual widget box at
  runtime rather than assuming any particular resolution, so it ought to be
  fine — but the author has no unit to verify that on.
- **Transparent backgrounds do not work.** Nexus 3's Web View renders a real
  web page over an opaque white base, so `?bg=clear` yields white, not your
  wallpaper.
- **The theme assumes a dark background.** Light accent colours are unreadable
  on light surfaces.
- **Exchange rates are daily mid-market rates**, not dealable prices. Bureaux
  apply their own spread. Fine for direction; do not transact against them.
- **FX markets close at weekends.** Seeing Friday's rates on a Sunday is
  correct behaviour, not stale data.
- **Tap-to-skip may not reach the page**, since Nexus intercepts the tap to
  open the site fullscreen.
- Binance and Bybit are blocked in some jurisdictions.
- **Do not enable the Web View auto-refresh.** The page updates itself over
  WebSocket; refreshing only tears down that connection.

## License

MIT — see [LICENSE](LICENSE).
