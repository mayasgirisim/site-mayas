# MAYAS · www.mayasgida.com — Yayın Rehberi (Netlify + GoDaddy)

Bu klasör, hiçbir derleme (build) gerektirmeyen **statik bir web sitesidir**.
Olduğu gibi Netlify'a yükleyip yayınlayabilir, ardından GoDaddy alan adınızı
(www.mayasgida.com) ücretsiz olarak bağlayabilirsiniz.

---

## 0) Klasör içeriği

```
mayas-site/
├── index.html          → Ana sayfa (tüm bölümler + animasyonlar + sosyal akış)
├── 404.html            → Özel "sayfa bulunamadı" sayfası
├── netlify.toml        → Güvenlik başlıkları, cache, yönlendirmeler
├── _redirects          → apex→www yönlendirmesi (yedek)
├── robots.txt          → Arama motoru izinleri
├── sitemap.xml         → Site haritası
└── images/             → Logo + hero + 5 poster görseli
```

İçerik tamamen hazır. Aşağıdaki 3 yöntemden **birini** seçin.

---

## 1) EN HIZLI: Sürükle-bırak ile yayın (5 dakika, hesap bile gerektirmez)

1. https://app.netlify.com/drop adresine gidin.
2. Bu `mayas-site` klasörünü (içindeki dosyalarla birlikte) tarayıcıya
   **sürükleyip bırakın**. (Klasörü değil, isterseniz tüm dosyaları seçip de bırakabilirsiniz.)
3. Birkaç saniyede `https://rastgele-isim.netlify.app` adresinde yayında olur.
4. Netlify hesabınıza giriş yaparsanız bu site kalıcı olur ve alan adı bağlayabilirsiniz.

> İlk denemeyi bu yöntemle yapıp siteyi canlı görmenizi öneririm.

---

## 2) ÖNERİLEN: GitHub + Netlify (otomatik güncelleme)

1. GitHub'da yeni bir repo açın (ör. `mayas-site`), bu klasördeki tüm dosyaları yükleyin.
2. https://app.netlify.com → **Add new site → Import an existing project → GitHub**.
3. Repo'yu seçin. Build ayarları:
   - **Build command:** boş bırakın
   - **Publish directory:** `.` (nokta)
4. **Deploy** deyin. Her `git push` sonrası site otomatik güncellenir.

---

## 3) Netlify CLI ile (terminal sevenler için)

```bash
npm install -g netlify-cli
cd mayas-site
netlify deploy --prod
```

---

## 4) GoDaddy alan adını (www.mayasgida.com) bağlama

Netlify sitesi yayında olduktan sonra:

### A. Netlify tarafı
1. Site panelinde **Domain management → Add a domain** → `mayasgida.com` yazın.
2. Netlify size iki seçenek sunar; **"Use Netlify DNS"** veya
   **"Point to Netlify with external DNS"**. GoDaddy'de kalmak isterseniz
   ikinci seçeneği (external DNS) kullanın.
3. Netlify, eklemeniz gereken **DNS kayıtlarını** gösterir. Genelde:
   - `www`  →  **CNAME**  →  `SIZIN-SITE.netlify.app`
   - `@` (apex/mayasgida.com)  →  **A** kaydı →  `75.2.60.5`
     *(veya Netlify'ın gösterdiği güncel A/ALIAS değeri — panelde yazan değeri esas alın)*

### B. GoDaddy tarafı
1. GoDaddy → **My Products → Domains → mayasgida.com → DNS / Manage DNS**.
2. Mevcut çakışan kayıtları düzenleyip Netlify'ın verdiği değerleri girin:
   - **CNAME** kaydı: Name `www`, Value `SIZIN-SITE.netlify.app`, TTL 1 saat
   - **A** kaydı: Name `@`, Value `75.2.60.5` (Netlify'ın gösterdiği IP), TTL 1 saat
   - Forwarding (yönlendirme) kullanıyorsanız kapatın; çakışır.
3. Kaydedin. DNS yayılımı genelde 10 dakika–birkaç saat sürer (en fazla 48 saat).

### C. HTTPS (ücretsiz SSL)
- DNS doğrulandıktan sonra Netlify **Let's Encrypt** sertifikasını otomatik üretir.
- **Domain management → HTTPS → Verify / Provision certificate** ile kontrol edin.
- Primary domain'i `www.mayasgida.com` yapın; `netlify.toml` ve `_redirects`
  apex'i (mayasgida.com) otomatik www'ye yönlendirir.

---

## 5) Yayın sonrası kontrol listesi

- [ ] Ana sayfa açılıyor, hero görseli ve animasyon çalışıyor
- [ ] Üst şerit (sosyal akış) kayıyor; X/Tumblr kaynakları tıklanınca açılıyor
- [ ] "Bize ulaşın" → `iletisim@mayasgida.com` mail uygulamasını açıyor
- [ ] İki anket linki yeni sekmede açılıyor
- [ ] Ekip kartlarındaki LinkedIn bağlantıları doğru profillere gidiyor
- [ ] Poster galerisi büyüyor, görseller yükleniyor
- [ ] `https://www.mayasgida.com/404-test` → özel 404 sayfası geliyor
- [ ] HTTPS kilidi (yeşil/güvenli) görünüyor

---

## Notlar
- Site tamamen istemci taraflıdır; sunucu/veritabanı gerektirmez.
- Sosyal akış X'in `widgets.js` betiğini kullanır. Bazı reklam engelleyiciler
  bunu kısıtlarsa, panel otomatik olarak "kaynakta aç" düğmesine düşer (fallback).
- Görseller `images/` altında; yeni varyant eklerseniz `index.html` galerisine
  aynı kalıpta bir `<a><img></a>` satırı eklemeniz yeterli.
