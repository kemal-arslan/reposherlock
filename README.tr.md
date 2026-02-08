# RepoSherlock

[English](README.md) | Türkçe

Bir GitHub repo URL'i veya local path ver; mimari harita, çalıştırma rehberi, riskler ve aksiyon alınabilir issue'ları dakikalar içinde al.

## Canlı CLI Önizleme

![RepoSherlock CLI Preview](docs/assets/cli-reposherlock.gif)

## Neden RepoSherlock?

- Çalıştırmadan önce: mimariyi hızla anla.
- Güvenmeden önce: lisans ve güvenlik risklerini gör.
- Zaman kaybetmeden önce: doğrulanmış komutlarla başla.

## Farkı Nedir?

RepoSherlock sadece komut tahmini yapmaz. `--try-run` ile install/test/build/start adımlarını sandbox'ta dener ve rapora kanıt (sinyal, timeout, not) olarak yazar.

## Ne Üretir?

RepoSherlock, public GitHub reposunu veya local proje dizinini analiz edip aşağıdaki klasöre çıktı yazar:

- `.reposherlock/output/<run-dir>/`

Varsayılan run-dir timestamp tabanlıdır (örnek: `20260208-103104`).

Çıktılar:
- `report.md` / `report.json`
- `architecture.mmd` / `architecture.json`
- `risks.md` / `risks.json`
- `issues.json`
- `issues.good-first.md` / `issues.good-first.json`
- `README_2.0.md`
- `run_attempt.md` / `run_attempt.json` (sadece `--try-run`)
- `pr_draft.md` (sadece `--pr-draft` veya wizard Full Sherlock profili)

LLM polish açıkken ek varyantlar da üretilir:
- `README_2.0.deterministic.md` / `README_2.0.llm.md`
- `issues.deterministic.json` / `issues.llm.json`
- `report.deterministic.md` / `report.llm.md`

## Kısa Örnek Çıktı

```text
Run Plan
Target: https://github.com/octocat/Hello-World
Try-Run: enabled

Sherlock Thinking
✓ Validating repository target and runtime profile
✓ Planning scan strategy and safe execution path
✓ Preparing architecture, risk, and issue synthesis

Summary
Repo type: web
Risks: high=0, med=1, low=0
Output: .reposherlock/output/20260208-103104
```

## Kurulum

Bun (önerilen):

```bash
bun install
bun run build
```

Node alternatifi:

```bash
npm install
npm run build
```

## Hızlı Başlangıç

Minimum yazı (wizard):

```bash
bun run sherlock
```

Repo analizi:

```bash
bun run sherlock -- analyze https://github.com/octocat/Hello-World --try-run
```

Local path analizi:

```bash
bun run sherlock -- analyze . --no-network --try-run
```

Mevcut bir run özetini aç:

```bash
bun run sherlock -- report .reposherlock/output/<run-dir>
```

Toolchain kontrolü:

```bash
bun run sherlock -- doctor
```

UI demo modu:

```bash
bun run sherlock -- ui-demo
```

Node ile çalıştırmak istersen:

```bash
npm run sherlock
```

## Yaygın Kullanım Senaryoları

- Yeni bir codebase'e girerken hızlı mimari + çalıştırma resmi çıkarmak.
- Hızlı audit yapmak: lisans, CI, secret pattern ve bağımlılık risklerini görmek.
- Maintainer triage: aksiyon alınabilir issue ve good-first issue listesi üretmek.
- Bir açık kaynak repo'yu projene eklemeden önce değerlendirmek.

## Konfigürasyon

Mevcut sürümde CLI akışı LLM polish açık şekilde çalışır.
Deterministik analiz yine üretilir ve LLM varyantlarıyla birlikte kaydedilir.

Kimlik gerektiren sağlayıcılar (OpenAI, Gemini, Anthropic, Grok, OpenAI-compatible) için:

```bash
export LLM_API_KEY="..."
```

Opsiyonel:
- `LLM_MODEL`
- `LLM_BASE_URL`

Wizard üzerinden provider/model/key seçip aşağıya kaydedebilirsin:
- `~/.reposherlock/credentials.json` (chmod 600)

## Nasıl Çalışır? (Yüksek Seviye)

RepoSherlock dosya yapısını ve ana dosyaları tarar, modül bağımlılık grafiği çıkarır, runtime/env/risk sinyallerini toplar, opsiyonel sandbox try-run çalıştırır ve rapor paketini üretir. LLM polish açıkken deterministik çıktılar daha okunaklı hale getirilir; bulunan komutlar ve gerçekler korunur.

## Notlar

- CLI örneklerinde tek isim kullanılır: `sherlock`.
- Girdi GitHub repo URL'i veya local dizin path'i olabilir.
- Raporlar heuristik üretimdir; production kararları için doğrulama önerilir.
- İnsan okunur çıktılarda secret benzeri değerler maskelenir.
- Try-run opt-in'dir (`--try-run`) ve timeout/çıktı limiti ile çalışır.

## Geliştirme

```bash
bun test
# veya
npm test
```

## Lisans

MIT
