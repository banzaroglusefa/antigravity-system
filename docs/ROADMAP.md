# Geliştirme yol haritası

1. **Mimari ve repo:** Belgeler, STATE şeması, ilk GitHub kaydı ve kabul kontrolü.
   İkinci bilgisayarın tek seferlik klonu ayrı ortam kurulumudur.
2. **Brief modülü:** Dokuz bölüm, referanslar, taslak ve sürümlü kesinleştirme.
   Önce gerçek kullanıcı protokolüyle taslağı eşleştir.
3. **Controller ve kapılar:** Yerel işlemsel kayıt, tek controller, olaylar,
   deadline, idempotency, güvenli Save/Load ve sürüme bağlı kararlar.
4. **Render ve FFmpeg:** Tek Remotion örneği, tam araç sürümleri, deterministik
   kare erişimi, manifest, native FPS denetimi ve gerçek çıktı doğrulaması.
5. **Rubric ve onarım:** Yapılandırılmış hakem bulguları, bağımlılık haritası,
   hedefli kare üretimi, tam final encode ve sınırlı iyileştirme döngüsü.
6. **E2E ve ikinci bilgisayar:** Gerçek devir, crash recovery, gecikmiş onay,
   timeout yarışı, eksik asset ve final regresyon testleri.

Her görevde: küçük kapsam → kod → anlamlı doğrulama → mimari inceleme → commit.
Tamamlanmamış özellikler hazır gibi belgelenmez. Yeni servisler, framework'ler
ve mimari değişiklikler görev kapsamına sessizce eklenmez.
