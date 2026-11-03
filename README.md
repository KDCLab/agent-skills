# agent-skills

AI asistanlarında kullanılmak üzere hazırlanmış, paylaşılabilir skill koleksiyonu.

Her skill bir klasörden oluşur: skill'in akışını ve kurallarını anlatan bir `SKILL.md` dosyası ve gerekirse yanında şablonlar ya da yardımcı dosyalar. Repo, Claude Code plugin marketplace formatında düzenlendi. Bu sayede skill'ler tek komutla kurulup güncellenebilir. Diğer ortamlarda da klasör kopyalanarak kullanılabilir.

## Skill'ler

| Skill | Ne yapar |
|---|---|
| [jira-analiz](plugins/jira-analiz/skills/jira-analiz/SKILL.md) | Bir Jira görevini açıklaması, yorumları, ekleri, bağlantılı görevleri ve referans dokümanlarıyla (PRD, Confluence, prototip) birlikte inceler. Eksik bilgileri sorar, çıktı formatını sana seçtirir, Türkçe ve sade bir analiz yazar, sonunda işi alt görevlere bölmeyi önerir. |

---

## Kurulum

### 1. Claude Code (önerilen)

Marketplace'i bir kez ekle:

```
/plugin marketplace add KDCLab/agent-skills
```

İstediğin skill'i kur:

```
/plugin install jira-analiz@kdclab-skills
```

Güncellemeleri çekmek için:

```
/plugin marketplace update kdclab-skills
```

Kurulu plugin'leri görmek veya kaldırmak için `/plugin` menüsünü kullan.

> `/plugin` komutları VS Code eklentisinin sohbetinde çalışmaz. Terminalde `claude` CLI'ını kullan. Komutların CLI karşılıkları: `claude plugin marketplace add KDCLab/agent-skills` ve `claude plugin install jira-analiz@kdclab-skills`.
>
> Marketplace adı (`kdclab-skills`) repo adından (`agent-skills`) farklı, çünkü `agent-skills` adı Anthropic'in resmi marketplace'leri için ayrılmış.

> Repo private ise, makinende GitHub erişiminin tanımlı olması gerekir (`git clone https://github.com/KDCLab/agent-skills.git` komutu çalışabilmeli).

### 2. Claude Code, elle kopyalama

```bash
git clone https://github.com/KDCLab/agent-skills.git

# Tüm projelerde kullanmak için
cp -r agent-skills/plugins/jira-analiz/skills/jira-analiz ~/.claude/skills/

# Sadece bir projede kullanmak için
cp -r agent-skills/plugins/jira-analiz/skills/jira-analiz <proje>/.claude/skills/
```

### 3. claude.ai ve Claude Desktop

1. [`dist/`](dist/) klasöründen ilgili zip dosyasını indir (ör. `jira-analiz.zip`).
2. **Settings → Capabilities → Skills → Upload skill** adımlarıyla yükle.

### 4. Diğer agent'lar

`SKILL.md` düz Markdown dosyasıdır. Skill formatını destekleyen başka bir araç kullanıyorsan skill klasörünü o aracın skill dizinine kopyala. Desteklemiyorsa `SKILL.md` içeriğini system prompt veya proje talimatı olarak verebilirsin.

---

## jira-analiz

### Ön koşul

Görevleri doğrudan okuyabilmesi için Jira ve Confluence bağlantısı gerekir. Önerilen yol **Atlassian (Rovo) MCP** bağlantısıdır:

- claude.ai / Desktop: **Settings → Connectors → Atlassian**
- Claude Code: claude.ai'deki connector otomatik gelir. Alternatif olarak `claude mcp add` ile Atlassian MCP sunucusunu ekleyebilirsin.

Bağlantı yoksa skill yine çalışır, görev içeriğini sohbete yapıştırmanı ister.

### Kullanım

Görev anahtarını veya linkini verip analiz iste:

```
PROJ-123'ü analiz et
```
```
https://firma.atlassian.net/browse/PROJ-123 için kabul kriterli analiz hazırla
```

### Akış

1. **İnceleme:** Görevi, yorumlarını, eklerini, bağlantılı görevlerini ve referans verilen dokümanları okur. Erişemediği kaynakları ve kaynaklar arasındaki çelişkileri sana bildirir.
2. **Netleştirme:** Kaynaklarda cevabı olmayan soruları gruplayarak, mümkün olduğunda ekranda seçmeli olarak sorar. İş kararlarını kendisi vermez, seçenekleri sunar.
3. **Format seçimi:** Ekranda seçmeli olarak sorulur: Jira açıklamasına uygun analiz, kısa iş analizi ya da kabul kriterli analiz arasından seçim yaparsın. Kendi şablonunu da verebilirsin.
4. **Analiz:** Türkçe ve sade yazar. Kesinleşmiş bilgiyi, varsayımları ve açık soruları ayrı başlıklarda tutar.
5. **Tasklandırma:** İstersen işi kapsam, bağımlılık ve tamamlanma kriterleriyle birlikte alt görevlere böler.

Jira'da görev oluşturmak veya güncellemek **yalnızca sen açıkça istediğinde** yapılır.

### Özelleştirme

Analiz ve alt görev şablonları [`references/formatlar.md`](plugins/jira-analiz/skills/jira-analiz/references/formatlar.md) dosyasındadır. Ekibinin formatına göre düzenleyebilirsin.

---

## Repo yapısı

```
agent-skills/
├── .claude-plugin/
│   └── marketplace.json          # marketplace tanımı ve plugin listesi
├── plugins/
│   └── <plugin-adı>/
│       ├── .claude-plugin/
│       │   └── plugin.json       # plugin bilgileri (ad, versiyon, açıklama)
│       └── skills/
│           └── <skill-adı>/
│               ├── SKILL.md      # skill talimatları (zorunlu)
│               └── references/   # şablonlar, ek dokümanlar (isteğe bağlı)
└── dist/
    └── <skill-adı>.zip           # claude.ai yüklemesi için paket
```

## Yeni skill ekleme

1. `plugins/<yeni-skill>/skills/<yeni-skill>/SKILL.md` dosyasını oluştur. Dosya şu başlıkla başlamalı:
   ```markdown
   ---
   name: yeni-skill
   description: Ne yaptığı ve hangi durumlarda kullanılacağı (tetiklenme bu metne göre olur).
   ---
   ```
2. `plugins/<yeni-skill>/.claude-plugin/plugin.json` dosyasını ekle (mevcut örneği kopyalayabilirsin).
3. `.claude-plugin/marketplace.json` içindeki `plugins` listesine yeni kaydı ekle.
4. claude.ai paketini üret:
   ```bash
   cd plugins/<yeni-skill>/skills && zip -r ../../../dist/<yeni-skill>.zip <yeni-skill> -x '*.DS_Store'
   ```
5. Bu tabloya bir satır ekle, commit'le ve push'la.

Bir skill'i değiştirdiğinde `plugin.json` ve `marketplace.json` içindeki `version` alanını artır ve `dist/` altındaki zip'i yeniden üret.
