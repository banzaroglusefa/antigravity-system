# Kabul ölçütleri

## Adım 1 — bu teslim

- README, STATE, şema, PROGRESS ve mimari/devir/durum/brief belgeleri mevcut.
- Başlangıç STATE.json, Draft 2020-12 validator ile doğrulanır.
- WAITING_USER, review/kalite/manifest olmadan şemadan geçmez.
- PREPARED checkpoint PAUSED ve gerekli devir kimliği olmadan geçmez.
- Bilinmeyen durum, yanlış hash ve negatif revision reddedilir.
- Null deadline, run ve araç sürümleriyle başlangıç gerçekçi biçimde IDLE'dır.
- İkinci bilgisayar, çevrimdışı süre ve artifact aktarım sınırları açıklıdır.
- Repo gizli anahtar, bağımlılık ve üretilmiş video içermez.
- İlk commit doğru GitHub reposuna gönderilip uzak commit doğrulanır.

Bu kabul, render/timer/Save düğmelerinin çalıştığı anlamına gelmez.

## Uygulama aşamasında zorunlu senaryolar

- Tam ve parça render aynı global kareyi üretir; tolerans varsa raporlanır.
- Onay ile timeout aynı anda gelince yalnızca bir geçiş olur.
- Aynı olay tekrar teslim edilince ek iş/karar oluşmaz.
- Çökme sonrası onay ve iş kaydı tutarlı geri yüklenir.
- Deadline kapanış/açılış ve bilgisayar değişiminde sıfırlanmaz.
- Eski artifact onayı, değiştirilmiş çıktıda reddedilir.
- Eski makine aktifken hedef makine üretime başlamaz.
- Eksik/bozuk asset veya doğrulanmamış araç ortamı uygun engel üretir.
- Ortak font ve süre değişimi bütün bağımlı aralıkları geçersizleştirir.
- Tek karelik hata, kısa video, son eksik panel ve rasyonel FPS test edilir.
- Final encode ses, süre, kare sayısı ve decode kontrolünden geçer.
- Onarım sınırı dolunca en iyi aday korunur; sonsuz döngü oluşmaz.
- Yayınlama otomatik tamamlanma kararından bağımsız kalır.
