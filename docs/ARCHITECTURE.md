# Mimari sözleşme — v1

## Kapsam ve sorumluluk

Bu repo mimari başlangıçtır. ChatGPT sözleşmeleri ve kabulü yönetir; Gemini
sınırlandırılmış görevlerde kod üretir. Proje sahibi kullanıcıdır. İkinci
bilgisayar aynı projeyi klonlar; mimariyi yeniden üretmez.

## Roaming Node

Aynı run için yalnızca bir aktif bilgisayar vardır. Bilgisayarlar otomatik
devralma yapmaz. GitHub kalıcı kod ve checkpoint aktarımı içindir; Git dağıtık
kilit veya süreç taşıma sistemi değildir. Ağ yokken yeni bilgisayar devralamaz.
Eski bilgisayarın durduğu doğrulanamıyorsa devir durur.

STATE.json güncel snapshot'tır. Yerel işlemsel kayıt (uygulama aşamasında
yerel SQLite önerilir) durum değişimi, olay kaydı ve yeni iş kaydını aynı
transaction'da tutar. SQLite çalışma dosyaları .runtime altında kalır ve Git'e
konmaz. Checkpoint sırasında tutarlı STATE.json, events/<event_id>.json
olayları ve manifest dışa aktarılır. Load bunları doğrular, yerel depoyu yeniden
kurar. Tek JSON dosyasına yazmak atomik iş yürütme garantisi sayılmaz.

## Planlanan bileşenler

- Controller: tek yerel sahip, geçiş doğrulama, kararlar ve kalıcı deadline.
- Brief modülü: Türkçe girdi, eksik alanlar, referans açıklamaları ve sürümleme.
- Renderer adaptörü: önce Remotion; Canvas/SVG daha sonra ihtiyaçla eklenir.
- Inspector: teknik kontroller, native FPS inceleme ve kanıt üretimi.
- Reviewer adaptörü: sürümlü rubric ve yapılandırılmış AI bulguları.
- Repair planner: etkilenen kareleri ve bağımlılıkları hesaplar.
- Checkpoint yöneticisi: durdurma, doğrulama, Git aktarımı ve yeniden kurma.

Controller render alt sürecinden bağımsızdır. Uzun model/FFmpeg çağrısı karar
döngüsünü bloklamaz. Uygulama kapanınca timer çalışmaz; Load gecikmiş deadline'ı
değerlendirir. Bu sürüm 24/7 servis garantisi vermez.

## Sürüm ve artifact kimliği

Her run benzersiz kimlik alır. Her mantıksal değişimde revision artar. Kararlar
run_id, request_id, gate, revision ve artifact_hash üzerinden doğrulanır.
Artifact ve brief SHA-256 ile, tanımlanmış canonical JSON/byte kurallarıyla
hash'lenir. Şifrelenmiş erişim URL'si yerine içerik kimliği kullanılır.

Manifest şunları taşır: kaynak commit, brief sürümü/hash'i, asset/font hash'leri,
tam araç sürümleri, render ayarları, kare sayısı, göreli dosya yolları veya
kalıcı depo anahtarları, dosya boyutları ve hash'leri. Makineye özgü mutlak yol
taşınabilir kimlik olamaz. source_commit kaynak kod commit'idir; kendisini
içeren checkpoint commit hash'i değildir.

Manifest snapshots/manifests/ altında, olaylar events/ altında tutulabilir.
Bu klasörler ihtiyaç oluşunca oluşturulur. Büyük ve özel girdiler Git'e
otomatik eklenmez. Depo çözümü seçilene kadar aktarım eksikse Load BLOCKED olur.
İlk boş iskeletin harici artifact bağımlılığı yoktur.

## Deterministik render

Hedef: frame = render(global_frame, brief, assets, seed, config).
Seed tercihi 42. Tam Node, renderer, browser ve FFmpeg sürümleri test edilip
lock dosyasına alınana kadar environment_verified false kalır.
chromium-120 ve remotion-4.0 doğrulanmış tam sürüm sayılmaz.
Font ve ağ asset'leri render öncesi hazır olmalıdır. Önceki kareye bağlı
simülasyon başlangıçtan deterministik ilerletilmeli veya checkpoint kullanmalıdır.
GPU/işletim sistemi farklarında byte eşitliği garanti edilmez; tolerans raporlanır.

## Denetim ve onarım

- Contact sheet/strip önizlemedir; native FPS hata kontrolünün yerine geçmez.
- Kare aralıkları [start, end); FPS pay/payda olarak saklanır.
- Renderer API'sinin uç kare davranışı adaptörde dönüştürülür.
- Geçiş, blur, simülasyon, ortak bileşen ve ses bağımlılıkları hesaba katılır.
- Bağımlılık bilinmiyorsa onarım kapsamı sahneye veya tüm kompozisyona genişler.
- Değişen kareler üretilir, final video baştan uca bir kez encode edilir.
- Tam decode, toplam kare/süre, ses, birleşim sınırları ve regresyon kontrolü yapılır.
- MP4 hash'i bütünlük kontrolüdür. Determinizm için decode kareleri ve ses
  örnekleri karşılaştırılır; uygun tolerans açıkça belirlenir.

Teknik kontroller başarılı, kritik bulgu yok ve zorunlu rubric boyutlarının
her biri en az 8 ise inceleme açılabilir. Rubric kapıya göre sürümlenir; metin
çıktılarında ilgili semantik kontroller uygulanır. Varsayılan onarım sınırı 3
denemedir; süre/maliyet sınırları uygulama öncesi ayrıca belirlenir.
İyileşme yoksa en iyi aday korunur ve iş BLOCKED olur.

## Girdi ve işlem sınırları

Brief, referans ve hakem çıktıları veri olarak ele alınır. Bunlardan shell
komutu türetilmez. Uzak URL'ler indirilecekse boyut/tür sınırı, zaman aşımı ve
yerel/özel ağ adreslerine erişim denetimi uygulanır. Kod üretim/çalıştırma,
paylaşım ve yayınlama ayrı yetkilerdir. Bu sürümde otomatik yayın yoktur.
