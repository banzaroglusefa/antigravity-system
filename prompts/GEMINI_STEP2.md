# Gemini kodlama görevi — Adım 2 / ilk dilim

Baş mimar ve proje yöneticisi ChatGPT'dir. Sen bu görev kapsamındaki uygulama
kodunu yaz. Kullanıcıya terminal komutları veya dosya taşıma işi verme;
erişebildiğin çalışma kopyasında işlemleri kendin yap. Erişimin yoksa açıkça
bildir; dosya oluşturmuş, test etmiş veya GitHub'a göndermiş gibi davranma.

## Önce oku

README.md, docs/ARCHITECTURE.md, docs/STATE_MACHINE.md,
docs/BRIEF_MODULE.md, docs/ACCEPTANCE.md ve schemas/state.schema.json.

Dokuz bölüm ve üç referans seviyesi henüz öneri taslağıdır. Kullanıcının eski
protokolüyle eşleştirme yapılmadıysa bunu açıkça raporla; kesinleşmiş protokol
iddiasında bulunma. Bu durumda ilk dilimi mevcut taslağın prototipi olarak sun.

## Uygula

1. Türkçe dokuz bölümlü brief sihirbazı.
2. İleri/geri gezinme ve girilen verilerin korunması.
3. Yerel taslak kaydı ve geri yükleme; bunun cihazlar arası senkronizasyon
   olmadığı görünür olmalı.
4. Bölüm bazlı doğrulama ve çelişki uyarıları; taslak kaydı serbest,
   eksik zorunlu veride kesinleştirme kapalı.
5. Referansların açıklama, tür ve hareket zaman kodu alanları.
6. Düzenlenebilir özet ve açık "Brief'i kesinleştir" işlemi.
7. Sürümlü brief veri modeli ve JSON Schema; kesinleştirmede canonical
   içerik hash'i; düzenlemede önceki sürümü koruyan yeni taslak.
8. Modül testleri: geçersiz girdi, taslak geri yükleme, sürüm/hash davranışı.

Teknoloji: TypeScript + React + Vite başlangıç önerisidir. Mevcut proje farklı
stack içeriyorsa üzerine ikinci uygulama kurmadan uyumu değerlendir ve bildir.
Bağımlılıkları tam sürüm ve lock dosyasıyla kaydet. Kullanıcının çift tıklama
veya uygulama içi başlatma beklentisi için mevcut ortamı değerlendir; çalışmayan
bir başlatıcı verme. Başlatma/önizlemeyi yetkili ajan yürütür.

## Kapsam dışı

Renderer, AI sağlayıcı bağlantısı, FFmpeg, kapı motoru, 600 saniyelik timer,
GitHub senkronizasyonu ve dışarı yayınlama. Dosya/URL referansını kaydetmek
inceleme yapmak anlamına gelmez. Sahte üretim veya sahte başarı göstermeme.
STATE.json runtime entegrasyonunu controller gelmeden taklit etme.

## Teslim

Değişen dosyalar, çalışan davranış, test sonucu, başlatma/önizleme sonucu ve
eksikler. Başarısız veya çalıştırılmamış kontrolü açıkça belirt. PROGRESS.md'yi
yalnızca tamamlanan işlerle güncelle. Mimari sözleşmeyi değiştirmen gerekiyorsa
ayrı öneri olarak bildir; sessizce değiştirip tamamlandı deme.
