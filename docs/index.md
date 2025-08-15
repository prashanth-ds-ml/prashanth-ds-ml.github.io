# Katakam Prashanth

AI engineer building practical systems in **RAG, CV, and data apps**.  
I like shipping demos fast, writing clear notes, and keeping things reproducible.

[GitHub](https://github.com/prashanth-ds-ml) • [LinkedIn](https://www.linkedin.com/in/<your-handle>/) • [Email](mailto:<your-email@example.com>) • [Resume (PDF)](assets/resume.pdf)

---

## What I’m focused on
- **RAG chatbots** with MongoDB vector search, robust prompting, and guardrails  
- **Computer vision PoCs** (e.g., blood cell detection) with lightweight models  
- **Neat demos** on Streamlit / Hugging Face Spaces that others can reuse

---

## Highlights
- Built **MSME RAG Chatbot** end-to-end (profile → retrieval → explain)  
- Prototyped **SmartCBC** for RBC/WBC count using YOLOv8-nano  
- Streamlit **Stock Planner** with profit rotation & MMI overlays

> Want details? See **Projects** below — each item includes code & live demo links.

---

## Projects

### MSME RAG Chatbot
Retrieval-augmented assistant that recommends govt schemes from profile or Udyam ID.
- **Stack:** Python, Hugging Face, MongoDB (vector search), Streamlit  
- **Code:** <https://github.com/…> **Demo:** <https://huggingface.co/spaces/…>

??? info "More"
    - Pre-filters on category/status metadata  
    - Clean prompt templates & “next step” suggestions  
    - Numeric selection UX to drill into a scheme

---

### SmartCBC — Blood Cell Detection
Lightweight CV pipeline to detect/count RBCs/WBCs; designed for on-device feasibility.
- **Stack:** YOLOv8-nano, Python, OpenCV  
- **Code:** <https://github.com/…> **Demo:** <https://huggingface.co/spaces/…>

??? info "More"
    - Trained on TXL-PBC + BCCD  
    - Future: differential WBC & morphology flags

---

### Stock Planner
Daily planner that generates sell plans and rotates booked profits intelligently.
- **Stack:** Streamlit, yFinance, MongoDB  
- **Code:** <https://github.com/…> **Demo:** <https://huggingface.co/spaces/…>

??? info "More"
    - Goal-based sell ladders  
    - MMI context overlay

---

## Writing (pinned)
- **MkDocs + GitHub Pages in 10 minutes** — zero-drama deploy notes *(coming up)*  
- **Designing RAG prompts that don’t break** — checklists & examples *(coming up)*

> Long posts will live on this page for now. If I move to a blog later, I’ll link them here.

---

## About
I work hands-on across data prep, modeling, retrieval, and UX.  
Tooling I’m comfy with: **PyTorch, Hugging Face, YOLO, Streamlit, MongoDB, MkDocs**.

---

## Contact
- Email: <your-email@example.com>  
- LinkedIn: <https://www.linkedin.com/in/<your-handle>/>  
- GitHub: <https://github.com/prashanth-ds-ml>

*Last updated: {{ git_revision_date_localized or "2025-08-15" }}*
