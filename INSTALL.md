# INSTALL.md

## Installation Goal

This file explains how to install and enable the `summarize-ZJE-slides` skill for OpenCode, Claude Code, or any other LLM agent that supports skills.

This skill converts course PDFs, lecture slides, or teaching materials into Chinese-first revision notes. By default, it generates both a LaTeX source file and a compiled PDF. It is especially suitable for ZJE biomedical science courses, including exam-focused summaries, formula cheat sheets, concept cheat sheets, and revision handouts with slide page references.

## Required Configuration

Because this project involves image processing, the API used by the agent must support image recognition, image understanding, or image processing. Suitable options include multimodal GPT models, Kimi vision-capable models, and similar APIs. Pure text-only models cannot reliably parse or process image content, which may cause incorrect image matching, random image insertion, or outputs that do not correspond to the actual slide images.

Recommended configuration:

- Image reading and image-content analysis: a multimodal GPT model or `moonshot/kimi-k2.6`
- Text merging, summarisation, LaTeX generation, and verification: GPT, DeepSeek, or another long-context text model
- PDF reading and page extraction: must be used together with the `pdf` skill
- Local PDF compilation: `xelatex` must be installed
- PDF-to-image conversion: `pdftoppm` must be installed

If the current agent only has access to a text-only model, it cannot use the full image-analysis workflow of this skill. In that case, switch to a vision-capable model first, or ask the user to provide reliable pre-extracted image-page analysis results.



## Environment Requirements

### 1. Required Skill Dependency

This skill must be used together with the `pdf` skill.

`summarize-ZJE-slides` handles the slide-summary workflow, including global reading, sectioning, image selection, merged summarisation, LaTeX writing, and final verification.  
The `pdf` skill handles the PDF input side, including PDF reading, page extraction, OCR, screenshots, and page-content confirmation.

Do not replace the `pdf` skill entirely with temporary scripts.  
Do not load unrelated skills for distribution, planning, verification, or writing.

 Companion `pdf` skill references:

- Repository path: `https://github.com/anthropics/skills/tree/main/skills/pdf`
- Reference raw files for policy-permitted local installs:
  - `https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/SKILL.md`
  - `https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/LICENSE.txt`
- Recommended extra reference files:
  - `https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/forms.md`
  - `https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/reference.md`
 

### 2. System Commands

Before installation, check whether the following commands are available:

```bash
which xelatex
which pdftoppm
```

For Windows PowerShell:

```powershell
where xelatex
where pdftoppm
```

`xelatex` is used to compile Chinese LaTeX PDFs.  
`pdftoppm` is used to convert PDF pages into PNG images for visual analysis.

### 3. Recommended System Dependencies

#### Ubuntu / Debian

```bash
sudo apt update
sudo apt install -y texlive-xetex texlive-latex-recommended texlive-latex-extra texlive-lang-chinese poppler-utils
```

#### macOS

```bash
brew install --cask mactex
brew install poppler
```

#### Windows

Recommended installations:

- TeX Live or MiKTeX
- Poppler for Windows

After installation, make sure both `xelatex` and `pdftoppm` are available in the system `PATH`.

## Installation Methods

### Method 1: Ask the Agent to Fetch the Installation Instructions

Copy the following text into OpenCode, Claude Code, or another LLM agent that supports skills:

```text
Fetch and follow instructions from:
https://raw.githubusercontent.com/JoeChen1122/ZJE-Slides-Skill/asset/INSTALL.md
```

### Method 2: Install the Skill Manually

1. Open the repository:

```text
https://github.com/JoeChen1122/ZJE-Slides-Skill
```

2. Download or copy `SKILL.md`.

3. Place `SKILL.md` into your agent's skills directory, keeping the filename as:

```text
SKILL.md
```

4. Confirm that the skill name is:

```text
summarize-ZJE-slides
```

5. Restart or refresh your agent so that it reloads the available skills.

## Post-Installation Check

After installation, ask the agent to run the following check:

```text
Please check whether the summarize-ZJE-slides skill is loaded, and confirm that the pdf skill, xelatex, pdftoppm, and a vision-capable model are available.
```

The expected result should confirm that:

- The `summarize-ZJE-slides` skill is loaded
- The `pdf` skill is available
- The current model or a callable model supports image understanding
- `xelatex` is available
- `pdftoppm` is available
- The agent can create a `Summary - <pdf-stem>` output directory
- The agent can convert PDF pages into the `pages/` subdirectory
- The agent can generate `.tex` and `.pdf` deliverables

## Quick Start

After installation, provide a course PDF in the conversation and describe the task directly, for example:

```text
Summarise this lecture PDF into exam revision notes and generate a PDF file.
```

You can also specify the scope and style:

```text
Only summarise Lecture 3 to Lecture 5. Use Chinese as the main language, keep important English terms, and turn the output into formula and concept cheat sheets.
```

Default behaviour:

- Input: PDF lecture slides or course materials
- Output: `Summary - <pdf-stem>.tex` and `Summary - <pdf-stem>.pdf`
- Output directory: `Summary - <pdf-stem>/` under the same directory as the source PDF
- Language: Chinese-first, with important English terms preserved in brackets
- Images: preserve useful mechanism diagrams, structural diagrams, experimental figures, plots, and important schematics
- Page references: keep slide page numbers or compact page ranges for important points by default

## Output Directory Structure

After a normal run, the output directory should look like this:

```text
Summary - <pdf-stem>/
├── pages/
│   ├── slide-01.png
│   ├── slide-02.png
│   └── ...
├── kimi_batch_1.md
├── kimi_batch_2.md
├── Summary - <pdf-stem>.tex
└── Summary - <pdf-stem>.pdf
```

Explanation:

- `pages/` stores images converted from the PDF pages
- `kimi_batch_*.md` stores batched visual-model analysis results for the slide images
- `.tex` is the final LaTeX source file
- `.pdf` is the final revision handout

All intermediate and final files must be saved inside `Summary - <pdf-stem>/`.  
Do not scatter OCR text, temporary notes, screenshots, or LaTeX auxiliary files in the source PDF directory.

## Troubleshooting

### 1. `xelatex` Cannot Be Found

This means the local LaTeX environment is not installed or is not available in `PATH`.

Solutions:

- Ubuntu / Debian: install `texlive-xetex`
- macOS: install MacTeX
- Windows: install TeX Live or MiKTeX and configure `PATH`

If `xelatex` cannot be installed, the skill can only generate `.tex`; it cannot guarantee final PDF generation.

### 2. `pdftoppm` Cannot Be Found

This means Poppler is not installed or is not available in `PATH`.

Solutions:

- Ubuntu / Debian: install `poppler-utils`
- macOS: install `poppler`
- Windows: install Poppler for Windows and configure `PATH`

Without `pdftoppm`, PDF pages cannot be reliably converted into PNG images, so image selection and image insertion may be affected.

### 3. The Current Model Has No Image-Recognition Capability

Switch to a vision-capable model, such as a multimodal GPT model or a Kimi vision model.

Do not ask a text-only model to directly analyse images. A text-only model can only process text that has already been reliably transcribed; it cannot judge image content, mechanism diagrams, experimental figures, or screenshots for inclusion in the final PDF.

### 4. The Generated PDF Has Missing Chinese Text or Garbled Characters

Check that:

- The file is compiled with `xelatex`
- The LaTeX source is saved with UTF-8 encoding
- A usable Chinese font exists on the system
- The LaTeX template enables `fontspec` and `xeCJK`

### 5. Images Are Inserted Randomly or Do Not Match the Text

This usually happens because:

- No vision-capable model was used
- The PDF pages were not converted into PNG images first
- The vision model did not inspect each page image
- The verifier did not check whether each inserted image matches the surrounding text

To fix this, rerun the visual analysis and make sure every inserted image is directly related to the adjacent written content.

## Uninstallation

Delete the `summarize-ZJE-slides` folder or its corresponding `SKILL.md` file from the agent skills directory, then restart or refresh the agent.

If you also installed the `pdf` skill, it is not recommended to remove it, because this skill depends on the `pdf` skill for PDF reading and page processing.

## Verification Prompt

After installation, you can test the skill with the following prompt:

```text
Use the summarize-ZJE-slides skill to summarise this PDF lecture into Chinese-first exam revision notes, and output both tex and pdf files. Please keep important English terms, formulas, page references, and necessary mechanism diagrams.
```

If the environment is configured correctly, the agent should first check the PDF, the `pdf` skill, the vision model, `xelatex`, and `pdftoppm`, then create the `Summary - <pdf-stem>/` output directory and start processing.
