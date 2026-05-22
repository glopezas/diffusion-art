# Artists vs. Diffusion Models

Technical blog post for a course on Responsible Use of Data and Algorithms.

**Live post:** https://glopezas.github.io/diffusion-art/

---

## Project structure

```
diffusion_art/
├── _quarto.yml           # Quarto website config
├── index.qmd             # Blog post source
├── styles.css            # Custom CSS
├── assets/
│   ├── portfolio/        # Rutkowski reference images (10-15 PNG/JPG)
│   ├── control/          # Charlie Bowater reference images (10-15 PNG/JPG)
│   ├── generated/
│   │   ├── sd15_style/   # SD 1.5 style generations
│   │   ├── sd21_style/   # SD 2.1 style generations
│   │   ├── sd15_caption/ # Caption-conditioned generations
│   │   └── mia/
│   ├── gemini/           # Gemini generations (add manually)
│   ├── laion_matches/    # LAION retrieval results
│   └── figures/          # All output charts (PNG + Plotly JSON)
├── notebook/
│   └── experiments.ipynb # Colab notebook (all experiments)
└── results/
    └── results.json      # Numerical results (populated by notebook)
```

---

## Quickstart

### 1. Add artist portfolio images

Download 10-15 images from each artist's ArtStation:
- Rutkowski: https://www.artstation.com/rutkowski → `assets/portfolio/`
- Bowater: https://www.artstation.com/charliebowater → `assets/control/`

Save as PNG or JPG, any filename.

### 2. Run the notebook

Open `notebook/experiments.ipynb` in Google Colab (T4 GPU, free tier).
Run all cells top-to-bottom. Estimated time: 2-3 hours.

The notebook produces:
- All figures in `assets/figures/` (PNG + Plotly JSON)
- All numerical results in `results/results.json`
- All generated images in `assets/generated/`

### 3. Run frontier model experiments (manual)

Run the three experiment types across Gemini, DALL-E 3, Midjourney, and Adobe Firefly.
See the protocol in **Section 5** of the notebook for exact prompts and naming convention.
Save outputs to:
- `assets/frontier/named/` — named style prompt
- `assets/frontier/descriptive/` — description without artist name
- `assets/frontier/transfer/` — zero-shot style transfer (where supported)

Then run the Section 5 cells in the notebook to compute CSD scores and generate Figure 5d.

### 4. Preview locally

Install Quarto if not already installed: https://quarto.org/docs/get-started/

If Quarto was installed without sudo (WSL/Linux), the binary is at `~/.local/opt/quarto/bin/quarto`.

```bash
# Build the site
quarto render index.qmd
# or if not in PATH:
~/.local/opt/quarto/bin/quarto render index.qmd
```

Then serve the output in a separate terminal:

```bash
python3 -m http.server 4848 --directory _site
```

Open http://localhost:4848 in your browser.

> **Note:** `quarto preview` requires an interactive terminal. Use the two-step render + serve approach above for WSL or headless environments.

### 5. Deploy to GitHub Pages

```bash
quarto publish gh-pages
```

---

## Dependencies

The notebook installs everything automatically on Colab. For local development:

```bash
pip install diffusers transformers accelerate torch torchvision
pip install clip-retrieval ftfy regex open_clip_torch
pip install plotly scikit-learn pillow matplotlib
```

Quarto for the blog post: https://quarto.org

---

## References

- [Andersen v. Stability AI](https://www.courtlistener.com/docket/66732129/) — ongoing litigation
- [Getty v. Stability AI](https://www.bailii.org/ew/cases/EWHC/Ch/2025/2863.html) — UK High Court 2025
- [CSD paper](https://arxiv.org/abs/2404.01292) — style similarity metric
- [CSD code](https://github.com/learn2phoenix/CSD) — model checkpoint
- [Carlini et al. 2023](https://arxiv.org/abs/2301.13188) — training data extraction
- [Dubinski et al. 2024](https://arxiv.org/abs/2306.12983) — MIA at LAION scale
- [Cooper & Grimmelmann 2025](https://arxiv.org/abs/2404.12590) — legal analysis
