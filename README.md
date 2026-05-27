# ⛽ FUELWATCH

> Global fuel price tracker — 15 countries with live FX conversion

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://html.spec.whatwg.org/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chart.js&logoColor=white)](https://www.chartjs.org/)
[![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-222?style=for-the-badge&logo=github&logoColor=white)](https://pages.github.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> ⚠️ **Class Project — Prototype Stage**
> โปรเจคนี้เป็นส่วนหนึ่งของรายวิชา ENGINEERING APPLICATIONS OF ARTIFICIAL INTELLIGENCE
> อยู่ในระดับ Prototype ใช้ดูข้อมูลได้จริง แต่ราคา pump ขึ้นกับประกาศของแต่ละประเทศ
> ไม่เหมาะสำหรับการตัดสินใจซื้อขายในเชิงพาณิชย์

---

## 📌 ปัญหาที่แก้

คนทั่วไปอยากรู้ว่าราคาน้ำมันบ้านเราถูกหรือแพงเมื่อเทียบกับประเทศอื่น แต่ข้อมูลกระจัดกระจาย — แต่ละประเทศใช้สกุลเงิน หน่วยวัด และเกรดน้ำมันต่างกัน — **FuelWatch** รวมราคา pump 15 ประเทศไว้ที่เดียว แปลงเป็นสกุลเงินที่ต้องการด้วย FX rate live และมี forecast 30 วันแบบ statistical extrapolation

---

## ✨ Features

| Feature | รายละเอียด |
| --- | --- |
| 🌍 **15-Country Coverage** | ราคา petrol & diesel จาก 15 ประเทศหลัก รวม TH / US / UK / JP / DE / SG / MY |
| 💱 **Live FX Conversion** | แปลงค่าสกุลเงิน real-time ผ่าน ExchangeRate-API (refresh ทุก 60 วินาที) |
| 🇹🇭 **Thailand Live Pump Prices** | ดึงราคาตรงจาก thai-oil-api (PTT / Bangchak / ESSO / Shell) |
| 📊 **Compare Countries** | เปรียบเทียบราคา side-by-side ในสกุลเงินเดียวกัน |
| 🔮 **30-Day Forecast** | extrapolation ทางสถิติจาก historical 30 วัน — ไม่ใช่ AI prediction |
| 📰 **Energy News Feed** | RSS aggregator จาก OilPrice.com / Reuters / OGJ / WorldOil |
| 🧮 **Fill-up Calculator** | คำนวณค่าใช้จ่ายตามขนาดถังและระยะทาง |
| 🎨 **Dark UI** | Refined dark palette + Space Grotesk / Outfit / JetBrains Mono |

---

## 🏗️ Architecture

```
User (Browser)
    │
    ▼
Static HTML + Vanilla JS (single-file)
    │
    ├── Chart.js (CDN)                  → ราคา historical + forecast charts
    │
    ├── ExchangeRate-API               → FX rates (USD base, 60s refresh)
    ├── thai-oil-api (chnwt.dev)        → ราคา pump ไทยจริงจาก PTT/Bangchak
    ├── Stooq.com                      → historical crude price (CSV)
    └── RSS Feeds (via CORS proxy)
            ├── OilPrice.com
            ├── Reuters Agency
            ├── Oil & Gas Journal (OGJ)
            └── WorldOil

CORS Proxy Fallback Chain:
  api.allorigins.win → api.codetabs.com → corsfix.com → cors.x2u.in
```

### Why single-file HTML?

ทุกอย่างอยู่ใน `index.html` ไฟล์เดียว — CSS, JS, data tables — ทำให้ deploy ง่ายมาก:
- ไม่ต้องมี build step (no webpack, no npm)
- โหลดบน GitHub Pages ฟรี ไม่ต้องมี server
- copy ไฟล์เดียวก็เปิดทำงานได้บน localhost / USB drive / S3

ข้อแลกเปลี่ยน: ไฟล์ใหญ่ (~170 KB) และ refactor ยากขึ้นเมื่อโต

---

## 🛠️ Tech Stack

| Layer | เทคโนโลยี | เหตุผล |
| --- | --- | --- |
| Frontend | Vanilla HTML + CSS + JS | ไม่ต้อง build, deploy ฟรีได้ทุกที่ |
| Charts | Chart.js 4.4.1 (CDN) | lightweight, ใช้กับ canvas เร็ว |
| FX Rates | ExchangeRate-API | ฟรี ไม่ต้อง API Key (160+ currencies) |
| TH Pump Prices | [thai-oil-api](https://github.com/max180643/thai-oil-api) | ดึงตรงจากเว็บ PTT/Bangchak |
| Crude History | Stooq.com CSV | ฟรี historical OHLC |
| News | RSS via CORS proxy | aggregate energy news แบบ free |
| Fonts | Space Grotesk + Outfit + JetBrains Mono | Google Fonts |
| Hosting | GitHub Pages | ฟรี HTTPS, CDN, custom domain ได้ |

**ค่าใช้จ่ายต่อเดือน: $0** — ไม่มี API key, ไม่มี server, ไม่มี database

---

## 🚀 วิธีติดตั้งและรัน

### Live Demo (GitHub Pages)

หลัง deploy แล้วจะใช้ได้ที่: `https://kmuttstudent7.github.io/FUELWATCH/`

### รันในเครื่อง (Local)

```bash
# 1. Clone repo
git clone https://github.com/KMUTTstudent7/FUELWATCH.git
cd FUELWATCH

# 2. เปิดด้วย browser โดยตรง
# Windows
start index.html
# Mac
open index.html
# Linux
xdg-open index.html

# หรือใช้ local server (แนะนำ เพราะ CORS proxy บางตัวต้องการ origin จริง)
python -m http.server 8000
# เปิด http://localhost:8000
```

### Deploy ของตัวเอง

1. Fork repo นี้
2. Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` → Save
3. รอ 1–2 นาที ได้ URL ของตัวเอง

---

## 📁 โครงสร้างไฟล์

```
FUELWATCH/
├── index.html              ← ทั้งหมดอยู่ในไฟล์นี้ไฟล์เดียว (HTML + CSS + JS)
├── README.md               ← ไฟล์นี้
├── LICENSE                 ← MIT License
└── .gitignore              ← ignore patterns
```

> 💡 หากในอนาคตอยาก refactor แยกไฟล์ แนะนำ split เป็น:
> `index.html` / `styles.css` / `app.js` / `data/countries.js` / `data/fx.js`

---

## 🗂️ Sections ในแอป

| Tab | หน้าที่ |
| --- | --- |
| **Price Dashboard** | overview ราคา 15 ประเทศ + FX live |
| **🇹🇭 Thailand** | ราคา pump ไทยแยกตามบริษัทและเกรด (Gasohol 95/91, E20, Diesel) |
| **Compare Countries** | side-by-side comparison ในสกุลเงินเดียว |
| **📊 Forecasts & News** | กราฟ 30 วัน + extrapolation + energy news feed |
| **Calculate Fill-up** | เครื่องคิดเลขค่าน้ำมันตามถังและระยะทาง |

---

## ⚠️ ข้อจำกัด (Prototype Stage)

| จุดจำกัด | สาเหตุ | แนวทางแก้ใน Production |
| --- | --- | --- |
| **Static Price Tables** | ราคานอกประเทศไทยไม่ live, อัพเดทตามประกาศ | scraper job ดึง globalpetrolprices.com ทุก 6 ชม. |
| **CORS Proxy ไม่เสถียร** | proxy ฟรีอาจล่ม → fallback chain ยาว | self-host proxy บน Cloudflare Worker |
| **Forecast = Extrapolation** | ใช้ linear/MA จาก 30 วัน — ไม่ใช่ ML | เพิ่ม ARIMA / Prophet / LSTM |
| **No Persistence** | refresh แล้ว state หาย | localStorage หรือ backend DB |
| **Single File** | refactor ยากเมื่อโต | split modules + bundler (Vite) |
| **No Tests** | prototype | เพิ่ม Vitest / Playwright |

---

## 🔮 แนวทางพัฒนาต่อ

```
Prototype (ปัจจุบัน)
    │
    └── Production-Ready
            ├── Backend scraper (Python + cron) → DB → REST API
            ├── PostgreSQL เก็บ historical prices ย้อนหลัง 5 ปี
            ├── Real forecasting model (Prophet / LSTM)
            ├── Mobile-first PWA + offline cache
            ├── User accounts + price alerts
            ├── Telegram / LINE bot แจ้งเตือนเมื่อราคาเปลี่ยน
            └── i18n (Thai / English / Japanese / Chinese)
```

---

## 📚 Data Sources

| Source | Type | License |
| --- | --- | --- |
| [thai-oil-api](https://github.com/max180643/thai-oil-api) | TH pump prices | MIT |
| [ExchangeRate-API](https://www.exchangerate-api.com) | FX rates | Free tier |
| [Stooq.com](https://stooq.com) | Crude OHLC | Free |
| [GlobalPetrolPrices.com](https://www.globalpetrolprices.com) | Reference data | Public |
| [OilPrice.com RSS](https://oilprice.com) | News | Free RSS |
| [Reuters Agency RSS](https://www.reutersagency.com/feed/) | News | Free RSS |

---

## 👨‍💻 ผู้พัฒนา

โปรเจคนี้จัดทำขึ้นเป็นส่วนหนึ่งของรายวิชา **ENGINEERING APPLICATIONS OF ARTIFICIAL INTELLIGENCE**, KMUTT

> *"จากคำถามง่ายๆ ว่า น้ำมันบ้านเราถูกหรือแพงเมื่อเทียบกับโลก สู่ dashboard 15 ประเทศที่ใช้งานได้จริงในไฟล์ HTML ไฟล์เดียว"*

---

## 📄 License

MIT License — ใช้และพัฒนาต่อได้อย่างอิสระ ดู [LICENSE](LICENSE)

---

Built with ⛽ using Vanilla JS + Chart.js + free public APIs
