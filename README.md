# Boyozcum Dijital Menü

Selçuk / İzmir'deki Boyozcum Unlu Mamülleri için 5 dilli (TR, EN, DE, RU, FR) QR menü.
Fiyatlar ve ürünler Google E-Tablo'dan okunur; kod değiştirmeden telefondan güncellenir.

## Dosya yapısı

```
boyozcum.github.io/
├── index.html        → ana sayfa, menüye yönlendirir
├── menu/
│   ├── index.html    → menü sayfası (QR buraya gider)
│   └── img/          → (isteğe bağlı) ürün fotoğrafları
├── urunler.csv       → Google E-Tablo'ya aktarılacak şablon
└── README.md
```

## 1. GitHub'a yükleme

1. `boyozcum` organizasyonunda **`boyozcum.github.io`** adında public bir repo açın.
2. Bu dosyaları klasör yapısını koruyarak yükleyin.
3. **Settings → Pages → Source: Deploy from a branch → main / (root) → Save**
4. 1–2 dakika sonra menü şu adreste yayında olur: `https://boyozcum.github.io/menu/`

## 2. Google E-Tablo'yu bağlama

1. Fırının Google hesabıyla yeni bir E-Tablo açın, adını `Boyozcum Menü` koyun.
2. **Dosya → İçe aktar → Yükle → `urunler.csv`** → "Mevcut sayfayı değiştir".
3. Alttaki sayfa sekmesinin adını `urunler` yapın.
4. **Dosya → Paylaş → Web'de yayınla**
   - Bağlantı: `urunler` sayfası
   - Biçim: **Virgülle ayrılmış değerler (.csv)**
   - "Değişiklik yapıldığında otomatik olarak yeniden yayınla" seçeneği açık olsun
   - **Yayınla** → çıkan linki kopyalayın
5. `menu/index.html` dosyasında en üstteki `CONFIG` bölümüne bu linki yapıştırın:
   ```js
   SHEET_CSV_URL: "https://docs.google.com/spreadsheets/d/e/....../pub?gid=0&single=true&output=csv",
   ```

E-Tablo'da yapılan değişiklikler menüye birkaç dakika içinde yansır (Google yayını önbelleğe alır).

> **Güvenlik:** "Web'de yayınla" linki sadece okunabilir. E-Tablo'nun *düzenleme* yetkisini yalnızca
> işletme sahibine ve size verin; düzenleme linkini kimseyle paylaşmayın.
> E-Tablo'ya müşteri bilgisi veya kişisel veri yazmayın; yayınlanan veriler herkese açıktır.

## 3. Ayarlar (`menu/index.html` → `CONFIG`)

| Ayar | Açıklama | Örnek |
|---|---|---|
| `SHEET_CSV_URL` | Yayınlanan E-Tablo CSV linki. Boşsa örnek menü görünür. | yukarıdaki link |
| `WHATSAPP` | Sipariş numarası: ülke koduyla, boşluksuz, `+` olmadan. Boşsa sepet gizlenir. | `"905321234567"` |
| `PHONE` | "Ara" butonu. Boşsa gizlenir. | `"+905321234567"` |
| `GOOGLE_REVIEW_URL` | Google İşletme Profili → "Yorum iste" linki. | `"https://g.page/r/.../review"` |
| `MAPS_URL` | Google Haritalar → Paylaş linki. Boşsa isimle arama yapılır. | `"https://maps.app.goo.gl/..."` |
| `INSTAGRAM` | Kullanıcı adı, `@` olmadan. | `"boyozcum_s"` |
| `HOURS` | Açılış/kapanış (İstanbul saati). `null` ise "açık/kapalı" bilgisi gizlenir. | `{ open: "04:00", close: "21:00" }` |

## 4. E-Tablo sütunları

| Sütun | Ne yazılır |
|---|---|
| `kategori` | `boyoz`, `kahvalti`, `gevrek`, `borek`, `tatli`, `icecek`. Yeni bir kelime yazılırsa yeni kategori oluşur (sadece Türkçe adıyla görünür). |
| `sira` | Kategori içindeki sıra: 1, 2, 3… |
| `durum` | `aktif` (normal), `tukendi` (üstü çizili, "Tükendi" yazar), `gizli` (menüde görünmez) |
| `ad_tr` … `ad_fr` | Ürün adı. Bir dil boş bırakılırsa Türkçe ad gösterilir. |
| `aciklama_tr` … `aciklama_fr` | Kısa açıklama. Boşsa Türkçe gösterilir. |
| `fiyat` | Sadece sayı: `25` veya `27,50`. Boşsa fiyat gösterilmez, sepete eklenemez. |
| `alerjenler` | Boşlukla ayrılmış: `gluten susam sut yumurta findik ceviz fistik soya` |
| `etiketler` | Boşlukla ayrılmış: `populer yeni vejetaryen vegan` |
| `foto` | İsteğe bağlı: `img/tahinli.jpg` (dosyayı `menu/img/` klasörüne yükleyin) |

**Günlük kullanım için** genelde sadece `fiyat` ve `durum` sütunlarına dokunmak yeterli.
İpucu: `durum` sütununa **Veri → Veri doğrulama → Açılır liste** (`aktif, tukendi, gizli`) ekleyin, yazım hatası olmaz.

> ⚠️ Şablondaki ürünler ve fiyatlar **örnektir**. Yayına almadan önce gerçek ürün ve fiyatlarla değiştirin.
> Çeviri eklemek veya değiştirmek isterseniz ilgili `ad_xx` / `aciklama_xx` hücresini düzenlemeniz yeterli.

## 5. Fotoğraflar

- Kare (1:1), en az 600×600 px, JPG, dosya başına 150 KB altı.
- Aynı ışık ve arka planla çekilmiş olması menüyü çok daha profesyonel gösterir.
- Dosya adlarında Türkçe karakter ve boşluk kullanmayın: `tahinli-boyoz.jpg`

## 6. Menü özellikleri

- 5 dil: telefon diline göre otomatik seçilir, sağ üstteki butondan değiştirilir, seçim hatırlanır.
  Belirli bir dilde açmak için: `.../menu/?lang=de`
- Arama (tüm dillerde), Popüler / Vejetaryen / Vegan filtreleri, kaydırırken aktif kategori.
- WhatsApp ön sipariş: müşteri ürünleri seçer, sipariş dükkâna **Türkçe** mesaj olarak gelir
  (müşterinin dili de not edilir).
- İnternet kesilirse son yüklenen menü gösterilir.
- Açık/koyu tema, telefon çentiği ve erişilebilirlik desteği.

## 7. Yayına almadan önce kontrol listesi

- [ ] Gerçek ürünler ve fiyatlar E-Tablo'da
- [ ] `SHEET_CSV_URL`, `WHATSAPP`, `GOOGLE_REVIEW_URL`, `MAPS_URL`, `HOURS` dolduruldu
- [ ] iPhone ve Android'de açıldı, 5 dil kontrol edildi
- [ ] Test siparişi WhatsApp'a doğru geldi
- [ ] QR kod basılmadan önce birkaç telefonla okutuldu
