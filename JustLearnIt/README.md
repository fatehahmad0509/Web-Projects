# JustLearnIt

Öğrencilerin soru çözerek XP ve altın kazandığı, öğretmenlerin ödev takibi yaptığı, okul yönetiminin platform genelini yönettiği bir LMS (Öğrenim Yönetim Sistemi) web uygulaması. FastAPI backend + vanilla HTML/JS frontend.

---

## Özellikler

### Öğrenci
- Konu bazlı soru çözme (pratik maçlar)
- Puan ve altın kazanma
- Günlük seri (streak) sistemi
- Rozet ve başarım sistemi
- Market: kazanılan altınla puan boost'u, streak koruması, kozmetik rozetler ve loot chest satın alma
- Ödev takibi ve bildirimler
- Konuya göre başarı istatistikleri

### Öğretmen
- Sınıf bazlı öğrenci listesi ve istatistikleri
- Ödev atama ve güncelleme
- Ders ve konu görüntüleme
- Okul kapsamında filtrelenmiş analitik raporlar

### Admin
- Okul, kullanıcı, öğrenci, öğretmen CRUD
- Ders, konu, soru yönetimi
- Platform geneli istatistikler
- Denetim logu (tüm anlamlı kullanıcı aksiyonları `system_logs` tablosuna yazılır)

---

## Teknoloji

- **Backend:** Python 3.10+ · FastAPI 0.109 · Uvicorn
- **Veritabanı:** PostgreSQL (psycopg2)
- **Kimlik Doğrulama:** JWT (python-jose) · SHA-256 şifre hash
- **Frontend:** Vanilla HTML/CSS/JS (tek dosya: `frontend/index.html`)

---

## Kurulum

### 1. Repoyu klonla

```bash
git clone https://github.com/kullaniciadiniz/JustLearnIt.git
cd JustLearnIt
```

### 2. Sanal ortam oluştur ve bağımlılıkları yükle

```bash
python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. `.env` dosyasını oluştur

```env
DB_URL=postgresql://kullanici:sifre@localhost:5432/justlearnit
JWT_SECRET=production-icin-degistir
PASSWORD_SALT=bu-salti-de-degistir
```

### 4. Uygulamayı başlat

```bash
uvicorn main:app --reload
```

Uygulama ilk başlatmada tüm tabloları ve başlangıç verilerini (dersler, market öğeleri, admin kullanıcısı) otomatik olarak oluşturur.

Frontend için ayrı bir sunucuya gerek yok. `http://localhost:8000` adresini aç — backend, `frontend/index.html`'i doğrudan sunmaktadır.

---

## Proje Yapısı

```
JustLearnIt/
├── main.py               # Uygulama giriş noktası
├── requirements.txt
├── core/
│   ├── db.py             # PostgreSQL bağlantısı, tablo oluşturma, seed
│   ├── security.py       # JWT, şifre hash, rol tabanlı dependency'ler
│   ├── achievements.py   # Rozet verme, puan kontrolü, istatistik güncelleme
│   └── logs.py           # Denetim logu (log_action)
├── modules/
│   ├── auth.py           # Giriş ve JWT üretimi
│   ├── students.py       # Öğrenci profili, rozetler, ödevler, bildirimler
│   ├── teacher.py        # Öğretmen paneli ve ödev yönetimi
│   ├── questions.py      # Ders, konu ve soru listeleme
│   ├── match.py          # Pratik maç başlatma ve cevap gönderme
│   ├── market.py         # Market öğeleri ve satın alma
│   ├── analytics.py      # Sınıf ve okul bazlı istatistikler
│   └── admin.py          # Admin CRUD ve raporlama
└── frontend/
    └── index.html        # Tek sayfalık frontend
```

---

## API

Swagger arayüzü: `http://localhost:8000/docs`

| Prefix | Kapsam |
|---|---|
| `POST /auth/login` | Giriş, JWT alma |
| `GET /students/...` | Öğrenci profili, rozetler, ödevler |
| `GET /questions/subjects` | Ders ve konu listesi |
| `POST /match/start` | Yeni pratik maç başlat |
| `POST /match/submit` | Cevap gönder |
| `GET /market/items` | Market listesi |
| `POST /market/buy` | Ürün satın al |
| `GET /teacher/...` | Öğretmen paneli |
| `GET /analytics/...` | Analitik raporlar |
| `GET /admin/...` | Admin yönetimi |
| `GET /health` | Sağlık kontrolü |

---

## Lisans

MIT
