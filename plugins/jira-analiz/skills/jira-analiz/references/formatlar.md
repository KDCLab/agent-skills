# Analiz Formatları

Kullanıcının kendi şablonu yoksa aşağıdaki iskeletlerden seçtiği formatı kullan. Başlıklar rehberdir: işe katkısı olmayan başlığı çıkar, boş başlık bırakma.

İçindekiler:
1. Jira açıklamasına uygun analiz
2. Kısa iş analizi
3. Kabul kriterli analiz
4. Alt görev şablonu

---

## 1. Jira açıklamasına uygun analiz

Doğrudan görevin açıklama alanına yapıştırılacak. Kısa başlıklar, madde işaretleri, gereksiz giriş yok.

```
## Amaç
<1-3 cümle: neden yapılıyor, kimin hangi problemi çözülüyor>

## Kapsam
- <dahil olan>
**Kapsam dışı:**
- <dahil olmayan>

## İş Kuralları
1. <kural>
2. <kural>

## Beklenen Davranış
- <normal akış>
- <istisna / hata durumu>

## Bağımlılıklar
- <görev anahtarı / ekip / servis — neden bağımlı>

## Referanslar
- <PRD / Confluence / Figma linkleri>

## Varsayımlar
- <onaylanmamış varsayım>

## Açık Sorular
- <soru> — (cevaplayacak: <kişi/rol>)
```

## 2. Kısa iş analizi

Yarım-bir sayfa. Hızlı hizalanma için.

```
**Özet:** <2-3 cümle>

**Kapsam:** <dahil> / **Kapsam dışı:** <hariç>

**İş Kuralları:**
- …

**Bağımlılıklar:** …

**Varsayımlar:** …

**Açık Sorular:** …
```

## 3. Kabul kriterli analiz

Format 1'in tamamı + kabul kriterleri. Kabul kriterleri test edilebilir olmalı: her biri tek bir davranışı doğrular ve "doğru çalışır" gibi ölçülemez ifadeler içermez.

```
<Format 1 başlıkları>

## Kabul Kriterleri

**AC1 — <kısa ad>**
- Verilen: <ön koşul>
- Ne zaman: <eylem>
- O zaman: <beklenen sonuç>

**AC2 — <kısa ad>**
…
```

Normal akışın yanında en az önemli istisna ve hata durumları için de kriter yaz. Varsayıma dayanan bir kriter varsa yanına "(varsayım)" ekle.

## 4. Alt görev şablonu

```
### <Sıra>. <Başlık>
**Kapsam:** <bu görevde yapılacaklar; gerekiyorsa yapılmayacaklar>
**Bağımlılık:** <önce bitmesi gereken görev(ler)> | Yok
**Tamamlanma Kriterleri:**
- [ ] <doğrulanabilir kriter>
- [ ] <doğrulanabilir kriter>
```

Listenin sonunda kısa bir sıralama notu ver: hangi görevler paralel yürütülebilir, kritik yol hangisi.
