# Components included in the Windows portable bundle

This is an academic, non-commercial image-processing demonstration. The project's own scientific code and the following third-party components remain separate works.

- **Python 3.12.10:** bundled application-local, with its original `runtime/LICENSE.txt`. Its search paths are restricted by `runtime/python312._pth`; it does not use a recipient's Python installation. Packaging approach: https://docs.python.org/3.12/using/windows.html#finding-modules
- **Installed Python libraries:** exact versions are listed in `runtime_packages.json`. Their original distribution metadata, license files and third-party notices are retained under `runtime/Lib/site-packages/`.
- **CLIP:** original `openai/clip-vit-base-patch32` checkpoint, revision `3d74acf9a28c67741b2f4f2ea7635f0aaf6f0268`. The official OpenAI CLIP MIT license is included in `licenses/CLIP_LICENSE.txt`. Sources: https://huggingface.co/openai/clip-vit-base-patch32 and https://github.com/openai/CLIP
- **COCO 2017:** the selected original images and four original annotation JSON files are included. Image-specific source URLs and license IDs, and the corresponding license tables, remain in `data/raw/annotations/`. Images retain their respective owners' licenses; the package does not relicense those images. Dataset source: https://cocodataset.org/#download
- **Windows C/C++ runtime libraries:** application-local dependency DLLs retain Microsoft's copyright and runtime terms. No Windows operating-system DLLs are copied. Windows 10/11 x64 supplies its own operating-system runtime.

The embedded model and vector indexes are used for retrieval of existing image/caption evidence only. No final RAG caption-generation system is included.
