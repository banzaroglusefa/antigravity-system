# Adım 2 — İnteraktif brief modülü taslağı

Durum: Uygulama öncesi taslak. Gemini ile daha önce belirlenen dokuz bölüm ve
üç referans seviyesinin tam metni elimizde değildir. Aşağıdaki eşleştirme
kullanıcı tarafından doğrulanana kadar öneridir.

## Kullanıcı akışı

Yeni proje → dokuz bölüm → eksiklerin kontrolü → düzenlenebilir özet →
"Brief'i kesinleştir" → sürümlü brief → Shot List üretimine hazır.

İlk kodlama dilimi taslak, özet ve brief doğrulamasını uygular. Gerçek renderer,
AI API, onay timer'ı ve Save/Load motoru sonraki işlerdir. Çalışmayan bir düğme
başarılı üretim veya GitHub senkronizasyonu yapmış gibi gösterilmez.

## Dokuz bölüm

| Bölüm | Toplanacak veri |
|---|---|
| 1. Amaç | Ana mesaj, beklenen eylem, başarı tanımı |
| 2. Hedef | Hedef kitle, yayın platformu, izleme bağlamı |
| 3. Çıktı | Süre, oran/çözünürlük, FPS tercihi, dil |
| 4. Hikâye | Metin, zorunlu sahneler, içerik sırası |
| 5. Görsel dil | Renk, tipografi, marka ve stil |
| 6. Hareket | Tempo, easing beklentisi, geçiş örnekleri |
| 7. Asset | Logo, görsel, font, lisans/erişim bilgisi |
| 8. Ses | Konuşma, müzik, BPM/işaretler, sessiz çıktı tercihi |
| 9. Sınırlar | Kaçınılacaklar, zorunluluklar, kabul koşulları |

Kullanıcı bilmediği teknik değerde "Öneri istiyorum" seçebilir. Öneriler açıkça
öneri olarak gösterilir ve onaysız kesin veri sayılmaz. Zorunlu eksikler ve
çelişkiler görünürdür; taslak kaydı engellenmez, kesinleştirme engellenir.
Ses kullanımı animatic zamanlamasına erken girdi sağlar.

## Üç seviye referans — önerilen taslak

1. Metin: harici referans olmadan açıklama.
2. Görsel: renk, tipografi veya kompozisyon örneği.
3. Hareket: video + başlangıç/bitiş zaman kodu + alınacak hareket özelliği.

Her referansın kimliği, kaynak türü, açıklaması, örnek alınacak/kaçınılacak
özelliği ve varsa dosya hash'i olur. Seviyeler kalite puanı veya zorunlu
yükseltme sırası değildir. URL yapıştırmak içeriğin incelendiği anlamına gelmez.

## Veri ve sürümleme

- brief_id, schema_version, version, status, dokuz bölüm ve references.
- EMPTY → DRAFT → READY → CONFIRMED. READY bütün zorunlu alanlar geçerli demektir.
- CONFIRMED ancak kullanıcı açıkça kesinleştirdiğinde oluşur; timeout yoktur.
- İlk taslak version 1; kesinleştirme içeriğin canonical hash'ini oluşturur.
- Kesinleşmiş içerik düzenlenirse yeni taslak sürüm açılır; eski sürüm korunur.
- Salt whitespace/anahtar sırası hash kuralı canonical JSON sözleşmesiyle çözülür.
- Brief değişimi etkilediği kapıların onayını düşürür; eski kararlar korunur.
- Kişisel içerik assets/private veya inputs/private altında kalır; Git'e
  otomatik commit edilmez. STATE yalnızca durum/sürüm/hash referansı taşır.

## İlk dilimin kabulü

Türkçe anlaşılır arayüz, ileri/geri, taslak geri yükleme, yanlış süre/oran
uyarısı, referans alanları, düzenlenebilir özet, gerçek kesinleştirme olayı ve
eski sürümün korunması. Yerel tarayıcı taslağının makineler arası Save/Load
olmadığı açıkça gösterilir. Çıktı üretimi olmadığı için 600 saniyelik sayaç yoktur.
