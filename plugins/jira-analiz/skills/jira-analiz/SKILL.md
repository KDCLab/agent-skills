---
name: jira-analiz
description: Bir Jira görevini (açıklama, ekler, yorumlar, bağlantılı görevler, PRD, prototip, Confluence sayfaları) inceleyip Türkçe, sade ve uygulanabilir bir iş analizi hazırlar; eksik bilgileri sorar, çıktı formatını kullanıcıya seçtirir ve sonunda alt görevlere bölmeyi önerir. Kullanıcı bir Jira anahtarı (ör. ABC-123) veya Jira linki verip "analiz et", "analizini çıkar", "iş analizi yaz", "kabul kriterlerini hazırla", "bu görevi incele", "taskları çıkar" gibi bir şey istediğinde, ya da bir Jira görevinin kapsamını/iş kurallarını netleştirmek istediğinde bu skill'i mutlaka kullan — "analiz" kelimesi geçmese bile.
---

# Jira Görev Analizi

Bu skill, bir Jira görevinden yola çıkarak ekibin doğrudan üzerinde çalışabileceği bir analiz üretir. Amaç uzun bir doküman değil; geliştiricinin, testçinin ve ürün sahibinin aynı şeyi anlamasını sağlayan, neyin kesin neyin açık olduğunu dürüstçe gösteren bir metin.

Akış beş adımdan oluşur. Adımları sırayla izle; çünkü her adım bir sonrakinin kalitesini belirler (kaynak okunmadan sorulan sorular gereksiz olur, format sorulmadan yazılan analiz çoğu zaman baştan yazılır).

Kullanıcıyla her zaman Türkçe konuş ve analizi Türkçe yaz.

## Soru sorma yöntemi

Bu skill boyunca kullanıcıya birçok soru sorulur: çelişkiler, eksik bilgiler, iş kararları, format seçimi, tasklandırma ve Jira'ya yazma onayı. Ortamda seçmeli soru aracı varsa bu soruları onunla sor. Claude Code'da bu araç `AskUserQuestion`. Kullanıcı ekranda seçenek işaretleyerek uzun cevap yazmadan hızla ilerler; verilen cevaplar da birbirine karışmaz.

Aracı şöyle kullan:
- **Bir çağrıda en fazla 4 soru** sor. Her soruya 2-4 seçenek koy. Daha fazla soru varsa birbiriyle ilişkili olanları aynı çağrıda topla ve birkaç tura böl.
- **Her soruya kısa bir başlık** ver (ör. "Kapsam", "Yetki", "Format").
- **Seçenekleri birbirini dışlayacak şekilde** yaz. Her seçeneğin açıklamasında etkisini tek cümleyle belirt. Kullanıcı her zaman "Diğer" seçip serbest metin yazabilir, o yüzden "Diğer" seçeneğini sen ekleme.
- **Önerin varsa** o seçeneği ilk sıraya koy ve etiketin sonuna "(Önerilen)" ekle. Öneri, karar değildir; son karar kullanıcıya aittir.
- **Birden fazla cevabın doğru olabildiği** sorularda (ör. "Hangi kullanıcı tipleri etkilenir?") çoklu seçimi aç.
- **Format sorusunda** her formatın kısa iskeletini önizleme olarak göster, böylece kullanıcı seçmeden önce çıktının nasıl görüneceğini görür.
- **Gerçekten açık uçlu sorularda** (ör. "Mevcut limit değeri nedir?") sabit seçenek uydurma. Bu soruları düz metinle sor ya da kaynaklardan çıkan olası değerleri seçenek yap.

Seçmeli soru aracı olmayan ortamlarda soruları numaralı listeyle sor, seçenekleri harflendir (a/b/c). Böylece kullanıcı "1a, 2c" gibi kısa cevap verebilir.

---

## 1. Görevi ve ilgili kaynakları incele

Kullanıcının verdiği görevi eksiksiz oku:

- **Görevin kendisi:** başlık, açıklama, tip, durum, öncelik, etiketler, bileşenler, özel alanlar.
- **Yorumlar:** hepsini kronolojik sırayla oku. Yorumlar genellikle açıklamadan daha güncel bilgi taşır — kapsam değişiklikleri, verilen kararlar, eklenen kısıtlar çoğunlukla buradadır. Çakışma varsa en son ve yetkili kişiden geleni öne al, ama bunu sessizce yapma (aşağıya bak).
- **Ekler:** okunabilen ekleri (görsel, PDF, doküman) incele.
- **Bağlantılı görevler:** parent/epic, alt görevler, "blocks / is blocked by / relates to" bağlantıları. Bağımlılıkları ve daha önce yapılmış işleri anlamak için en azından başlık, durum ve açıklamalarına bak.
- **Referans verilen dokümanlar:** açıklamada veya yorumlarda geçen PRD, Confluence sayfası, Figma/prototip, Google Doc vb. linkleri takip et ve oku.

Jira/Confluence için ortamda hangi araç varsa onu kullan (ör. Atlassian MCP araçları: görev getirme, JQL araması, Confluence sayfası okuma, uzak link listeleme; Figma linkleri için Figma araçları). Hiçbir Jira aracı yoksa kullanıcıdan görev içeriğini yapıştırmasını iste.

**Erişilemeyen kaynaklar:** Açamadığın her linki/eki not et ve kullanıcıya listele ("Şu PRD linkine erişemedim: …"). İçeriği hakkında tahmin yürütme — başlığından ya da bağlamdan ne yazdığını varsaymak, analize doğrulanmamış bilgi sokar ve sonradan fark edilmesi zordur. Kullanıcı içeriği paylaşabilir ya da o kaynak olmadan devam etmeye karar verebilir.

**Çelişkiler:** Kaynaklar birbiriyle çelişiyorsa (ör. PRD "tüm kullanıcılar" diyor, yorumda "sadece premium" denmiş; prototip ile açıklama farklı akış gösteriyor) hangisinin geçerli olduğuna kendin karar verme. Çelişkiyi kaynaklarıyla birlikte kısaca göster ve kullanıcıya seçmeli soruyla sor. Seçenekler çelişen kaynaklar olsun, ör. "PRD'deki hali geçerli" / "Yorumdaki karar geçerli".

İnceleme bittiğinde kullanıcıya kısa bir özet ver: neyi okudun, neye erişemedin, hangi çelişkileri buldun. Bu özet birkaç maddeyi geçmemeli.

## 2. Eksik bilgileri netleştir

Okuduklarından sonra iş kuralları, mevcut sistem davranışı veya bağımlılıklar konusunda analizi yazmaya yetmeyen noktalar kalabilir. Bunları kullanıcıya sor.

- **Önce kaynaklara bak.** Cevabı görevde, yorumlarda veya dokümanlarda olan bir şeyi sorma; bu kullanıcıya dokümanı okumadığın izlenimini verir.
- **Soruları grupla.** İlişkili soruları tek başlık altında topla (ör. "Yetkilendirme", "Hata durumları", "Mevcut ekranla ilişki"). Tek seferde makul sayıda, kısa ve net soru sor; her soruyu tek cümlede tut.
- **İş kararlarını kendin verme.** İlerlemeyi engelleyen bir karar varsa (ör. eski kayıtlar migrate edilecek mi, limit aşılınca ne olacak) seçenekleri kısaca sun, her birinin etkisini bir satırla belirt ve kararın kullanıcıya/ürün sahibine ait olduğunu açıkça söyle. Önerin varsa "öneri" olarak işaretle, karar gibi sunma.
- **Kritik olmayan boşluklar analizi bekletmesin.** Kullanıcı bazı sorulara cevap veremiyorsa veya "şimdilik geç" derse, o noktaları analize "Açık soru" veya "Varsayım" olarak taşı ve devam et.

Soruları "Soru sorma yöntemi" bölümündeki kurallarla seçmeli olarak sor. İş kararlarında her seçenek bir alternatif olsun; açıklamasında etkisini tek cümleyle yaz.

## 3. Çıktı formatını sor

Analizi yazmaya başlamadan önce kullanıcıya hangi formatı istediğini sor. Kullanıcının kendi şablonu varsa (mesajda, bir dosyada veya Confluence'ta) ona birebir uy; hazır formatları sadece şablon yoksa öner.

Önerilebilecek formatlar (ayrıntılı iskeletler `references/formatlar.md` dosyasında):

1. **Jira açıklamasına uygun analiz** — doğrudan görevin açıklama alanına yapıştırılabilecek, başlıklı ve kısa.
2. **Kısa iş analizi** — amaç, kapsam, iş kuralları ve açık sorulardan oluşan yarım-bir sayfalık özet.
3. **Kabul kriterli analiz** — iş analizine ek olarak test edilebilir kabul kriterleri (Given/When/Then veya madde listesi).

Formatı tek bir seçmeli soruyla sor. Bu üç formatı seçenek olarak koy ve her birinin kısa iskeletini önizlemede göster. Görevin niteliğine göre bir formatı "(Önerilen)" olarak işaretle; ör. geliştirmeye hazırlanan bir story için kabul kriterli analizi öner. Kullanıcı "Diğer" ile farklı bir şey isterse (ör. sadece kabul kriterleri, Confluence sayfası formatı) ona göre uyarla. Formatı seçtikten sonra `references/formatlar.md` dosyasından ilgili iskeleti oku.

## 4. Analizi yaz

**Dil ve ton:** Türkçe, sade, doğrudan. Okuyucu ekip arkadaşın; onu etkilemeye değil, işi anlatmaya çalış.

- Gereksiz teknik detaya, tekrar eden açıklamalara ve dolgu cümlelere yer verme. Bir bilgi bir kez, en uygun başlıkta geçsin.
- Teknik terimleri sadece iş gerektiriyorsa kullan; uygulama detayına (sınıf adı, tablo yapısı) kullanıcı istemedikçe girme.
- İşin büyüklüğüne göre uzunluğu ayarla: küçük bir değişiklik için beş başlık açma.

**İçerik** — işin gerektirdiği ölçüde:
- İhtiyacın amacı (neden yapılıyor, kimin problemi çözülüyor)
- Kapsam ve kapsam dışı
- İş kuralları
- Beklenen davranışlar (normal akış, istisnalar, hata durumları)
- Bağımlılıklar (diğer görevler, ekipler, servisler, üçüncü taraflar)

**Kesin bilgi ile belirsizliği ayır.** Bu, analizin en önemli özelliğidir: okuyucu bir cümleye bakıp onun onaylanmış mı yoksa tahmin mi olduğunu anlayabilmeli.
- Kaynaklarda yazan veya kullanıcının onayladığı bilgileri normal metinde yaz.
- Kullanıcının onaylamadığı varsayımları ayrı bir **Varsayımlar** başlığında topla.
- Cevaplanmamış soruları ayrı bir **Açık Sorular** başlığında, mümkünse kimin cevaplaması gerektiğiyle birlikte listele.
- Erişilemeyen kaynakları belirt.

Analizi doğrudan sohbet içinde sun. Kullanıcı istemedikçe Jira'ya yazma (bkz. 5. adım).

## 5. Tasklandırmayı sor

Analizi sunduktan sonra kullanıcıya işi alt görevlere bölmeni isteyip istemediğini seçmeli bir soruyla sor. Örnek seçenekler: "Evet, alt görevlere böl" / "Hayır, analiz yeterli" / "Önce analizde düzeltme yapalım".

İsterse görevleri `references/formatlar.md` içindeki "Alt Görev Şablonu"na göre hazırla. Her görevde şunlar anlaşılır olmalı:
- **Başlık** (kısa, eylem odaklı)
- **Kapsam** (bu görevde ne yapılacak, ne yapılmayacak)
- **Bağımlılıklar** (hangi görev bitmeden başlanamaz)
- **Tamamlanma kriterleri** (görevin bittiğini nasıl anlarız)

Görevleri mantıklı bir sırada ver; paralel yürütülebilecekleri belirt. Görev bölmeyi teknik katmanlara göre değil, mümkün olduğunca test edilebilir/teslim edilebilir parçalara göre yap.

**Jira'ya yazma:** Görev oluşturmak, mevcut görevin açıklamasını güncellemek, yorum eklemek veya durum değiştirmek başkalarının da gördüğü kalıcı değişikliklerdir. Bunları yalnızca kullanıcı açıkça istediğinde yap. İstediğinde de önce neyi nereye yazacağını (proje, görev tipi, parent, alanlar) seçmeli bir soruyla teyit et. Örnek seçenekler: "Alt görevleri oluştur ve analizi açıklamaya yaz" / "Sadece alt görevleri oluştur" / "Hiçbir şey yazma"; sonra işlemi yap ve oluşturulan/güncellenen görevlerin anahtarlarını ve linklerini paylaş.
