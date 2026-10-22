# Durum makinesi ve onaylar

## Kapılar

SHOT_LIST → STYLE_SHEET → ANIMATIC → FULL_TRANSITIONS → POLISH → AUDIO_SYNC
→ ERROR_CHECK → COMPLETED.

COMPLETED bir çalışma durumudur; sekizinci kapı değildir. ERROR_CHECK kararı
başarılı olduğunda gate aynı kalır ve phase COMPLETED olur. Sonuç projenin
tamamlanmasıdır, dışarıya yayınlanması değildir.

## Temel geçişler

| Başlangıç | Olay / koşul | Sonuç |
|---|---|---|
| IDLE | Brief CONFIRMED ve başlangıç koşulları sağlandı | GENERATING |
| GENERATING | Çıktı kaydedildi | CHECKING |
| CHECKING | Teknik/rubric koşulları başarılı | WAITING_USER |
| CHECKING | Düzeltilebilir kalite sorunu | NEEDS_REVISION |
| NEEDS_REVISION | Bütçe var | GENERATING |
| NEEDS_REVISION | Deneme/bütçe tükendi | BLOCKED |
| WAITING_USER | Geçerli kullanıcı onayı | Sonraki kapıda GENERATING |
| WAITING_USER | Geçerli timeout ve politika izinli | Sonraki kapıda GENERATING |
| WAITING_USER | Geçerli revizyon isteği | NEEDS_REVISION |
| Aktif durum | Güvenli durdurma/checkpoint | PAUSED |
| Aktif durum | Çalıştırma hatası | FAILED |
| Aktif durum | Kullanıcı iptali | CANCELLED |

FAILED için açık retry kararı, BLOCKED için engelin çözülmesi gerekir.
PAUSED sadece doğrulanmış resume_phase üzerinden devam eder. CANCELLED ve
COMPLETED aynı run üzerinde otomatik üretim başlatmaz; yeni iş yeni run alır.
Kullanıcı reddi olayı REJECTED olarak saklanır ve NEEDS_REVISION'a yönlenir;
iptal ayrı kullanıcı kararıdır.

## İnceleme ve timeout

1. Çıktı hazır, kalıcı ve erişilebilir olmalıdır. Kullanıcıya inceleme isteği
   görünür hale gelmeden deadline başlatılmaz. Bildirim hatası kaydedilir;
   inceleme sunulamıyorsa WAITING_USER açılmaz.
2. request_id, gate, artifact_hash, policy_version, opened_at ve deadline_at
   birlikte kaydedilir. deadline_at = opened_at + 600 saniye.
3. Zamanlar UTC'dir. Süre kaydetme, yükleme veya uygulama açılışıyla sıfırlanmaz.
4. Timeout öncesi mevcut artifact, teknik kalite, rubric ve politika tekrar
   doğrulanır. Otomatik karar AUTO_ADVANCED, kullanıcı kararı APPROVED'dır.
5. Onay ve timeout aynı yerel transaction'da beklenen revision/request_id
   üzerinden yarışır; ilk geçerli karar kazanır. Sonraki çağrı etkisizdir.
6. Deadline'a eşit/sonraki kabul zamanındaki onay geç kalmış sayılır. Olaylar
   controller'ın kabul zamanıyla sıralanır; istemci saatine güvenilmez.
7. Geç kalan revizyon isteği kaybolmaz; uygulanmış kararı silmek yerine yeni
   revizyon açar. Eski artifact onayı yeni çıktıya taşınmaz.
8. Onay tüketilince aktif review null yapılır; karar geçmişi olaylarda kalır.
9. Saat geriye/ileri sıçraması veya makineler arasında güvenilir olmayan zaman
   tespitinde otomatik karar durur, CLOCK_UNCERTAIN engeli gösterilir. İşlem
   içi beklemelerde monotonik saat; kalıcı deadline'da UTC kullanılır.

## Save/Load etkisi

İnceleme beklerken checkpoint PAUSED olur, resume_phase WAITING_USER saklanır
ve review korunur. Load incelemeyi doğruladıktan sonra eski deadline'ı değerlendirir.
Birden çok kapı atlanmaz: her sonraki kapının kendi çıktısı ve yeni incelemesi
gerekir. Uygulama kapalıyken iş yapılmış gibi olay üretilmez.

## Geçersizleşme

Brief, kaynak veya asset değişimi bağımlılık grafiğine göre değerlendirilir.
Kapsam bilinmiyorsa etkilenen kapıdan sonraki tüm çıktılar geçersiz sayılır.
Eski çıktılar silinmek yerine tarihçede tutulur. Aktif review ve quality
temizlenir; yeni artifact yeni istek alır. Başka kapının puanı devralınmaz.

## Şemanın sınırları

JSON Schema şekil ve temel koşulları doğrular. Hash eşitliği, kapı/review
uyumu, deadline aritmetiği, puan politikası, sürüm artışı ve geçmişe göre geçiş
yasallığı controller tarafından doğrulanmalıdır. date-time format denetimi
validator'da açıkça etkinleştirilir; kalıcı yazımlarda UTC Z zorunludur.
