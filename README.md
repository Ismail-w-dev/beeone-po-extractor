# BeeOne PO Extractor

AI-powered extraction of structured data from French purchase orders (bons de commande).

Uses **Qwen 2.5 VL** — a Vision-Language Model that reads document images directly and outputs structured JSON. No separate OCR needed.

## How to Use

### Option 1: Google Colab (Recommended)

1. Open [`notebooks/beeone_colab_VLM.ipynb`](notebooks/beeone_colab_VLM.ipynb) in Google Colab
2. Go to **Runtime > Change runtime type > T4 GPU**
3. Run all cells (**Runtime > Run all**)
4. Upload your purchase order in the Gradio interface
5. Get structured JSON output

### Option 2: Local Machine

```bash
# Clone the repo
git clone https://github.com/Ismail-w-dev/beeone-po-extractor.git
cd beeone-po-extractor

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate      # Linux/Mac
# .venv\Scripts\activate       # Windows

# Install dependencies
pip install -r requirements.txt

# Open the notebook
jupyter notebooks/beeone_colab_VLM_v8.ipynb
```

> Requires Python 3.10+, NVIDIA GPU with 6GB+ VRAM, and CUDA installed.

## What It Extracts

| Category | Fields |
|----------|--------|
| **Seller** | Name, address, SIRET, VAT, phone, email, IBAN |
| **Buyer** | Company name, address, contact, phone, email |
| **Order** | Order number, date, ferme, payment conditions, delivery |
| **Items** | Reference, designation, quantity, unit price, line total, VAT rate |
| **Totals** | Gross HT, discount, net HT, VAT, TTC, amount in words |
| **Other** | Additional information (notes, observations) |

## Architecture

```
Old approach: PDF → Image → EasyOCR → Flat Text → LLM → JSON
Our approach:  PDF → Image → Qwen 2.5 VL → JSON
```

Instead of using a separate OCR engine + text LLM, we use a single Vision-Language Model that sees the document image directly. This eliminates OCR errors and preserves layout understanding.

## Key Features

- **Bilingual** — French and English documents auto-detected
- **Smart validation** — Cross-checks totals, fixes unit swaps, validates amount-in-words
- **Configurable** — Edit `output_schema.json` and `extraction_rules.yaml` to customize extraction
- **Batch processing** — Upload multiple documents at once
- **4-bit quantization** — Runs on Colab free T4 GPU (~6GB VRAM)

## Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| GPU VRAM | 6 GB | 8+ GB |
| RAM | 8 GB | 16 GB |
| Disk | 15 GB | 20 GB |

Works on T4 (Colab free), V100, RTX 3060+, or CPU (slower).

## Technologies

| Component | Technology |
|-----------|-----------|
| AI Model | Qwen 2.5 VL 7B Instruct (4-bit quantization) |
| UI | Gradio |
| PDF Processing | PyMuPDF |
| Language | Python 3.10+ |

## License

Proprietary — BeeOne SAAS. All rights reserved.

## Third-Party Licenses

- **Qwen 2.5 VL** — Apache 2.0 License (Alibaba Cloud)
  https://huggingface.co/Qwen/Qwen2.5-VL-7B-Instruct
