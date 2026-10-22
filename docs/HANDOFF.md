# İki bilgisayar arasında Save / Load

## Bir kerelik kurulum

İkinci bilgisayarda aynı GitHub hesabı veya repo erişimi, projenin klonu ve
ileride seçilecek tam geliştirme bağımlılıkları gerekir. Dosyalar veya mimari
yeniden yazılmaz. Yetkili ajan kurulumu yapar; oturum açma gerekiyorsa kullanıcı
yalnızca hesabını doğrular. Giriş bilgileri sohbetten istenmez.

Bu oturumda hazırlanmış çalışma kopyası masaüstündeki eski boş klasör olmak
zorunda değildir. Tek bir çalışma kopyası seçilir; kullanıcıya iki klasör
arasında kopyalama işi bırakılmaz. İkinci bilgisayar kurulumu orada açılan
oturumda ayrıca doğrulanır.

## Save — kaynak bilgisayar

1. Yeni iş alımını durdur; alt süreçleri bitir veya güvenli iptal et.
2. Yerel transaction'da durumu dondur. phase PAUSED ve resume_phase sakla.
3. Benzersiz checkpoint_id üret; handoff PREPARED ve kaynak makineyi kaydet.
4. Snapshot, olaylar ve manifest'i tutarlı dışa aktar; şema/hash doğrula.
5. Gerekli büyük/özel dosyaları onaylı depoya koy; erişim ve hash doğrula.
6. Commit ve push yap; uzak commit'i doğrula. Başarısızsa devir tamamlanmadı.
7. Kaynak makine duraklatılmış kalır. Başka makineye geçmeden gönderim bitmeli.

GitHub'a gönderim başarısı STATE.json içindeki önceden yazılmış bir boolean
ile ispatlanamaz. Uzak commit ve checkpoint kimliği kontrol edilir.

## Load — hedef bilgisayar

1. Kaynakta controller'ın durduğu doğrulanır. Doğrulanamıyorsa devralma yok.
2. Hedefte aktif controller ve kirli çalışma ağacı olmamalı. Varsa değişiklikler
   korunur; ajan üzerine yazmadan uyuşmazlığı çözer.
3. Uzak repo güncellenir; sadece doğrulanmış checkpoint devralınır.
4. Şema, olay sırası, manifest, araç sürümleri ve gerekli asset'ler doğrulanır.
5. Yerel işlemsel depo yeniden kurulur. Yerel süreç PID'leri taşınmaz.
6. Checkpoint CLAIMED olarak kaydedilip uzak repoya gönderilir. Gönderim
   başarısızsa üretim başlamaz. Bu işlem dağıtık kilit garantisi sağlamaz;
   tek aktif makine kuralı operasyonel olarak zorunludur.
7. resume_phase'e göre devam edilir. Yarım GENERATING işi yeniden başlatılır;
   tamamlanmamış dosyalar geçerli artifact olarak kabul edilmez.
8. WAITING_USER için eski deadline değerlendirilir; süre sıfırlanmaz.

## Çatışma ve arıza

- STATE.json otomatik anlamsal merge edilmez; checkpoint/olay geçmişi incelenir.
- Force push ve değişiklikleri atan reset rutin devir işlemi değildir.
- Ağ yoksa yeni makine devralamaz. Mevcut makinenin yerel taslağı korunabilir.
- OneDrive gibi dosya senkronizasyonu, iki bilgisayarın aynı .git veya SQLite
  çalışma dosyasını eşzamanlı paylaşmak için kullanılmaz; her makine ayrı klon alır.
- Eksik büyük dosya BLOCKED nedenidir. Sadece STATE.json aktarımı yeterli değildir.

## Kullanıcı deneyimi hedefi

İleride uygulamada iki anlaşılır işlem: "Kaydet ve bırak" ve "Yükle ve devam et".
Bu belgeler bu düğmelerin bugün çalıştığını iddia etmez. Uygulanana kadar teknik
adımları yetkili geliştirme ajanı yürütür.
