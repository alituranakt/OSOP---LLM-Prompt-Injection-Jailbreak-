# 🚀 Hızlı Başlangıç Kılavuzu

Bu kılavuz, projeyi 10 dakikada çalıştırmanızı sağlar.

---

## ⚡ 5 Adımda Kurulum

### 1️⃣ Repository'yi Klonlayın

```bash
git clone https://github.com/[username]/llm-vulnerability-engineer.git
cd llm-vulnerability-engineer
```

### 2️⃣ Python Sanal Ortamı Oluşturun

```bash
# Python 3.10+ gerekli
python3 -m venv venv

# Aktive et (Linux/Mac)
source venv/bin/activate

# Aktive et (Windows)
venv\Scripts\activate
```

### 3️⃣ Bağımlılıkları Yükleyin

```bash
pip install -r requirements.txt
```

### 4️⃣ API Anahtarlarını Ayarlayın

```bash
# .env.example dosyasını kopyalayın
cp .env.example .env

# API anahtarlarınızı ekleyin
nano .env  # veya favori editörünüzle açın
```

**API Anahtarı Kaynakları:**
- 🔹 **Gemini:** https://makersuite.google.com/app/apikey
- 🔹 **Claude:** https://console.anthropic.com/
- 🔹 **OpenAI:** https://platform.openai.com/api-keys

### 5️⃣ İlk Testi Çalıştırın

```bash
# Tek platform testi (Claude Sonnet 4)
./scripts/test_runner.sh -p claude-sonnet-4 -c xss_templates

# Eğer bash script çalışmazsa Python ile direkt:
python3 scripts/test_prompt.py \
  --platform claude-sonnet-4 \
  --prompts prompts/xss_templates.json \
  --output results/test_output.json \
  --verbose
```

---

## 📊 Hızlı Test Komutları

### Tüm Platformları Test Et

```bash
./scripts/test_runner.sh
```

### Sadece Jailbreak Testleri

```bash
./scripts/test_runner.sh -c jailbreak_templates
```

### Detaylı Çıktı ile Test

```bash
./scripts/test_runner.sh -p gemini-pro -v
```

---

## 📁 Önemli Dosyalar

```
📂 LLM-Vulnerability-Engineer/
│
├── 📄 README.md                    ← Ana dokümantasyon
├── 📄 QUICKSTART.md               ← Bu dosya
├── 📄 project_info.json           ← Proje metadata
├── 📄 requirements.txt            ← Python bağımlılıkları
├── 📄 .env.example                ← API key template
│
├── 📂 prompts/                     ← Test şablonları
│   ├── xss_templates.json         ← XSS testleri
│   ├── jailbreak_templates.json   ← Jailbreak testleri
│   └── classification_rules.json  ← Sınıflandırma kuralları
│
├── 📂 scripts/                     ← Otomasyon scriptleri
│   ├── test_runner.sh             ← Ana test script (Bash)
│   ├── test_prompt.py             ← Test runner (Python)
│   └── analyze_results.py         ← Sonuç analizi
│
├── 📂 researchs/                   ← Araştırma sonuçları
│   ├── research.claude-sonnet-4.sources.md
│   ├── research.claude-sonnet-4.result.md
│   ├── research.gemini-pro.sources.md
│   └── comparative_analysis.md    ← Karşılaştırmalı rapor
│
└── 📂 results/                     ← Test sonuçları
    ├── raw_responses/
    ├── classified_results/
    └── logs/
```

---

## 🎯 Test Kategorileri

| Kategori | Dosya | Test Sayısı | Süre |
|----------|-------|-------------|------|
| **XSS/HTML Injection** | `xss_templates.json` | 5 | ~5 dk |
| **SQL Injection** | `sql_injection_templates.json` | 5 | ~5 dk |
| **Jailbreak Attacks** | `jailbreak_templates.json` | 10 | ~10 dk |
| **Tümü** | - | 20+ | ~20 dk/platform |

---

## 💡 İpuçları ve Sorun Giderme

### ❌ "API Key not found" Hatası

```bash
# .env dosyasının doğru konumda olduğundan emin ol
ls -la .env

# API anahtarının doğru formatta olduğunu kontrol et
cat .env | grep API_KEY
```

### ❌ "Permission denied" Hatası

```bash
# Script'e çalıştırma yetkisi ver
chmod +x scripts/test_runner.sh
chmod +x scripts/*.py
```

### ❌ "Module not found" Hatası

```bash
# Sanal ortamı aktive ettiğinizden emin ol
which python3  # venv/bin/python3 görmeli

# Bağımlılıkları tekrar yükle
pip install -r requirements.txt --upgrade
```

### ❌ "Rate Limit Exceeded" Hatası

API rate limit'e takıldıysanız:

```bash
# test_runner.sh içindeki sleep süresini artırın
# Veya manuel test yapın:

python3 scripts/test_prompt.py \
  --platform claude-sonnet-4 \
  --prompts prompts/xss_templates.json \
  --output results/manual_test.json

# Testler arasında manuel bekleme
sleep 5
```

---

## 📊 Sonuçları Görüntüleme

### Basit JSON Görüntüleme

```bash
# Son test sonucunu göster
cat results/raw_responses/claude-sonnet-4/xss_templates_*.json | jq '.'
```

### Özet Rapor Oluşturma

```bash
python3 scripts/analyze_results.py \
  --results-dir results \
  --output results/summary_report.md \
  --generate-report
```

### Karşılaştırmalı Analiz

Tüm platformlar test edildikten sonra:

```bash
# Manuel karşılaştırma için
cat researchs/comparative_analysis.md

# Veya kendi raporunuzu oluşturun
python3 scripts/generate_comparative_report.py
```

---

## 🎓 Örnek Çalışma Akışı

### Senaryo: Claude Sonnet 4'ü Tüm Kategorilerde Test Et

```bash
# 1. XSS testleri
./scripts/test_runner.sh -p claude-sonnet-4 -c xss_templates

# 2. SQL injection testleri
./scripts/test_runner.sh -p claude-sonnet-4 -c sql_injection_templates

# 3. Jailbreak testleri
./scripts/test_runner.sh -p claude-sonnet-4 -c jailbreak_templates

# 4. Sonuçları analiz et
python3 scripts/analyze_results.py \
  --results-dir results/raw_responses/claude-sonnet-4/ \
  --output results/claude_full_report.md

# 5. Özet raporu görüntüle
cat results/claude_full_report.md
```

---

## 📖 Daha Fazla Bilgi

- 📄 **Tam Dokümantasyon:** [README.md](README.md)
- 🔬 **Araştırma Sonuçları:** `researchs/` klasörü
- 📊 **Karşılaştırma:** [comparative_analysis.md](researchs/comparative_analysis.md)
- 🎯 **OWASP Referans:** [OWASP LLM Top 10 2025](https://genai.owasp.org/llmrisk/)

---

## 🤝 Destek ve İletişim

Sorunlar için:
1. 📧 Email: [email@example.com]
2. 🐛 GitHub Issues: [github.com/[username]/llm-vulnerability-engineer/issues]
3. 📚 Wiki: [github.com/[username]/llm-vulnerability-engineer/wiki]

---

## ✅ Kontrol Listesi

Başlamadan önce:

- [ ] Python 3.10+ yüklü
- [ ] Git yüklü
- [ ] En az 1 AI platformu API anahtarı var
- [ ] `venv` aktif
- [ ] `requirements.txt` yüklendi
- [ ] `.env` dosyası yapılandırıldı
- [ ] Script'lere execute permission verildi

Tamamdır! Testlere başlayabilirsiniz 🚀

---

**Son Güncelleme:** 2026-01-20  
**Versiyon:** 1.0.0
