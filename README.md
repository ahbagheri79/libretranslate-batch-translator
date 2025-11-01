# LibreTranslate Batch Translator 🗣️➡️📝

A simple yet powerful **Python utility** that batch-translates `.properties` files into another language using the **[LibreTranslate](https://libretranslate.com)** API — producing a clean Excel (`.xlsx`) output with original, translated, and UTF-16-escaped text.

---

## 🚀 Features

- **Batch translation** of `.properties` key–value pairs  
- **Automatic placeholder protection** (`{0}`, `%s`, `${name}`, etc.)  
- **LibreTranslate API** integration (local or cloud)  
- **Retry & rate-limit handling** for `HTTP 429`  
- **Live progress bar** with ETA and speed  
- **Excel or CSV output** (`pandas + xlsxwriter/openpyxl`)  
- **UTF-16 column generation** for Java-compatible resource files  
- Works fully **offline** when used with a local LibreTranslate server

---

## 🧩 Example Workflow

### 1️⃣ Input – `messages.properties`
```properties
welcome.title=Welcome to our platform!
button.save=Save changes
error.network=Network error, please try again later.
greeting.user=Hello {username}!
```

### 2️⃣ Run the Script
```bash
python3 translate_props.py
```

### 3️⃣ Output – `translations.xlsx`

| key            | english                                | fa (translated)                        | utf16                               |
|----------------|----------------------------------------|----------------------------------------|------------------------------------|
| welcome.title  | Welcome to our platform!               | به پلتفرم ما خوش آمدید!                | \u0628\u0647 ... |
| button.save    | Save changes                           | ذخیره تغییرات                          | \u0630\u062E... |
| ...            | ...                                    | ...                                    | ...                                |

---

## ⚙️ Configuration

All configuration values are defined at the top of the script:

| Variable | Description | Default |
|-----------|--------------|----------|
| `LT_URL` | LibreTranslate API endpoint | `"http://[::1]:5000/translate"` |
| `LT_API_KEY` | API key (optional) | `""` |
| `SRC_LANG` | Source language code | `"en"` |
| `DST_LANG` | Target language code | `"fa"` |
| `MIN_INTERVAL_SEC` | Minimum delay between requests | `0.1` |
| `DEFAULT_RETRY_AFTER` | Default wait time when rate-limited | `150` |

You can also override them using environment variables, for example:
```bash
export LT_URL="https://libretranslate.de/translate"
export SRC_LANG="en"
export DST_LANG="tr"
```

---

## 🧠 Requirements

Create a virtual environment and install dependencies:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**requirements.txt**
```txt
pandas
requests
openpyxl
xlsxwriter
```

---

## 🖥️ Running a Local LibreTranslate Server

You can self-host LibreTranslate easily using Docker:
```bash
docker run -it -p 5000:5000 libretranslate/libretranslate
```

Then update the script endpoint:
```python
LT_URL = "http://127.0.0.1:5000/translate"
```

---

## 📦 Output Structure

The script automatically detects installed Excel engines and saves either:

- `translations.xlsx` (preferred, if `xlsxwriter` or `openpyxl` available), or  
- `translations.csv` (fallback if Excel dependencies missing)

Columns:
- **key** — Property key  
- **english** — Original text  
- **fa** — Translated text  
- **utf16** — Java-escaped string (`\u0628\u0647...`)

---

## 🧱 Code Highlights

- Resilient against API timeouts and rate-limits (`HTTP 429`)  
- Smart placeholder masking to preserve `{variables}`  
- Clean modular functions (`translate_text`, `_mask_placeholders`, `_unmask_placeholders`, etc.)  
- Unicode-safe output using UTF-16 conversion helpers  

---

## 🧪 Example Command-Line Use

```bash
python3 translate_props.py --file messages.properties --src en --dst fr
```

*(Future feature idea: add `argparse` or `click` for CLI arguments)*

---

## 🧾 License

This project is licensed under the **MIT License** — free to use, modify, and distribute.

---

## ✨ Author

**Amir Hossein Bagheri**  
Founder & Developer at [Roxcode](https://roxcode.ir)  
📍 Based in Turkey  
💼 Focused on PHP, Laravel, WordPress, and automation tools.  
💡 Exploring AI-powered localization & workflow optimization.

---

## ⭐️ Contributions

Pull requests, feature ideas, and improvements are welcome!  
If you find this tool useful, please give it a **⭐️ on GitHub** — it helps others discover it too.

---
