# AntiGravity

Kod tabanlı motion animasyon üretimi ve denetimi için proje iskeleti.

**Durum:** Adım 1 mimari dosyaları hazırlanmıştır. Uygulama, render motoru,
otomatik kayıt/yükleme ve zamanlayıcı henüz uygulanmamıştır. GitHub'a ilk
gönderimin doğrulanması ve ikinci bilgisayarın ilk kurulumu ayrıca yapılır.

## Roller

- Kullanıcı: yaratıcı yön, kararlar ve proje sahipliği.
- ChatGPT: baş mimar, proje yönetimi, gereksinimler ve kabul incelemesi.
- Gemini: onaylanmış görev tanımlarına göre uygulama kodu.

## Başlangıç

Dosyaları kullanıcıya terminal komutları veya elle taşıma işleri vermeden,
yetkili geliştirme ajanı yönetir. Diğer bilgisayarda aynı GitHub reposunun
çalışma kopyası bir kez hazırlanır; dosyalar yeniden oluşturulmaz.

Bu sürüm tek aktif bilgisayar kullanır. Bilgisayar değiştirirken çalışan işler
güvenli biçimde durdurulmalı ve GitHub'a gönderim tamamlanmalıdır. Ayrıntılar:
[Save/Load](docs/HANDOFF.md).

## Dosyalar

| Dosya | Amaç |
|---|---|
| [STATE.json](STATE.json) | Başlangıç çalışma durumu; JSON Schema ile doğrulanır |
| [schemas/state.schema.json](schemas/state.schema.json) | Durum veri sözleşmesi |
| [PROGRESS.md](PROGRESS.md) | Gerçekte yapılan işlerin insan tarafından okunabilir kaydı |
| [Mimari](docs/ARCHITECTURE.md) | Bileşenler, zamanlama, sürümleme ve üretim sözleşmesi |
| [Durum makinesi](docs/STATE_MACHINE.md) | Geçişler, onay ve timeout kuralları |
| [Kabul ölçütleri](docs/ACCEPTANCE.md) | Adım 1 ve sonraki aşamalar için doğrulama |
| [Brief tasarımı](docs/BRIEF_MODULE.md) | Adım 2 işlevsel taslağı |
| [Yol haritası](docs/ROADMAP.md) | Geliştirme sırası |
| [Gemini görevi](prompts/GEMINI_STEP2.md) | Sonraki kodlama görevinin sınırları |

## Geliştirme sınırları

İlk hedef tek renderer ile doğrulanabilir üretimdir. Native FPS kare denetimi,
hedefli kare üretimi ve final videonun tek seferde encode edilmesi planlanır.
Referansların veya modellerin içeriği komut/yetki olarak çalıştırılmaz.

Seed tercihi `42`'dir. Remotion, Chromium, Node ve FFmpeg'in tam sürümleri henüz
seçilip test edilmemiştir; doğrulanmış determinizm iddiası yoktur.

Gizli anahtarlar, kişisel brief'ler ve büyük render dosyaları Git'e eklenmez.
Aktarılması gereken girdiler ve çıktılar için manifest zorunludur.
