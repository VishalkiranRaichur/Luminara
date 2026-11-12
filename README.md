Luminara
Making academic reading more accessible without losing the meaning.
Luminara is a team-built, AI-assisted reading application developed through the Responsible Computing Club at San José State University (Spring 2026). It helps students work through dense academic material by identifying difficult language, simplifying selected passages, and supporting vocabulary learning—all while aiming to preserve the document's original meaning and structure.

Rather than sending an entire document to a generative model, Luminara uses traditional NLP to identify what needs attention first, then a locally running Llama 3 model through Ollama to rewrite the selected text. The result is a more targeted approach to simplification, designed with accessibility, privacy, and resource efficiency in mind.

Project: Original team repository · Status: Spring 2026 team project
Why we built it
A lot of academic material is harder to read than it needs to be. Technical vocabulary, long sentences, and dense formatting can make studying exhausting—particularly for students learning in a second language or readers who benefit from dyslexia-friendly presentation.

Our first approach was to make complicated text shorter. Feedback from our demo made us rethink that goal: simpler isn't necessarily better if important details disappear. We shifted toward a system that helps readers understand difficult language without unnecessarily rewriting everything around it, and added a personal dictionary so the experience supports learning over time.
What Luminara does
- Document input and output: Processes PDFs, slide decks, Word documents, and images of pages; supports text extraction, OCR where necessary, and converting processed content back into a downloadable file.
- Targeted simplification: Uses readability and word-frequency signals to flag difficult words or phrases before asking the language model to generate replacements.
- Local AI: Uses Ollama with Llama 3 for rewriting, rather than requiring a cloud-hosted model to process document text.
- Personal vocabulary dictionary: Provides authenticated endpoints to save, retrieve, and delete words with definitions and parts of speech; saved vocabulary can guide subsequent simplification.
- Reading accessibility options: The application interface includes dyslexia-friendly and text-to-speech options, alongside format, language, and page-range choices.
- Containerized development: Uses Docker Compose to coordinate the frontend, backend, and local AI service.
Architecture
                 User / React frontend
                          |
                   Flask REST API
                          |
                   Document upload
                          |
          +---------------+---------------+
          |                               |
   Parse PDF / DOCX / PPTX         OCR for page images
          |                               |
          +---------------+---------------+
                          |
                 Extract and normalize
                          |
            spaCy + textstat + wordfreq
               Identify difficult spans
                          |
              Ollama / local Llama 3
             Rewrite flagged spans only
                          |
                Reconstruct document
                          |
                  Download output

      JWT authentication + per-user dictionary API
The central design decision was flag first, generate second. NLP handles identifying potentially difficult text, while the generative model focuses on replacements. This reduces unnecessary rewriting and gives us more control over the information that stays unchanged.
Document processing flow
1. Accept a document and check its file type.
2. Extract the content using a format-specific parser or OCR.
3. Analyze readability and flag difficult spans.
4. Simplify selected spans using the local model, taking saved vocabulary into account.
5. Reconstruct the content with the targeted replacements.
6. Convert it back toward the source document's file type and structure.
7. Return the downloadable result and clean up temporary uploaded files.
Tech stack
Area	Technology
Frontend	React
Backend and APIs	Python, Flask, Flask-CORS
NLP and readability	spaCy, textstat, wordfreq
Local language model	Ollama, Llama 3
File processing	PyMuPDF, python-docx, python-pptx, pytesseract, Pillow
Authentication	JWT, PyJWT
Development and orchestration	Docker, Docker Compose


My contributions — Lead Backend Developer
I led backend development for Luminara, from the original processing flow to the Flask services that connect document handling, NLP, and local AI. My main contributions included:
- Designed and implemented the document-processing pipeline, integrating text extraction and format handling for PDFs, presentations, Word documents, and image-based content.
- Built the difficulty-detection stage with readability scoring and word-rarity checks, so the AI could focus on flagged sections instead of rewriting an entire document.
- Integrated Ollama and Llama 3 to generate replacements for selected text, with support for the reader's saved vocabulary.
- Implemented backend authentication safeguards, including JWT-based login, rate limiting, disposable-email checks, and password-length validation.
- Developed the personal-dictionary API for user-specific word storage, retrieval, and deletion.
- Helped connect the React frontend to Flask, resolving CORS, port, and API-integration issues, and contributed to teammates' code and Figma design work.
I also sketched the original seven-step backend pipeline. One lesson that stayed with me: AI isn't always more useful when it generates more. For this project, the challenge was deciding what not to change.
Engineering challenges and lessons
Preserving meaning. Early simplification could remove useful details. We responded by narrowing the scope of rewriting and building vocabulary support, rather than optimizing only for shorter output.

Readability scores aren't consistent across every document. Flesch–Kincaid measurements varied with text length, which sometimes led to too much or too little text being flagged. Testing on course textbooks surfaced those problems more clearly than small examples.

Document formatting is harder than plain text. Extracting words is one problem; putting them back without disturbing headings, titles, and structure is another. Our pipeline had to account for the source format and reconstruction step.

Integration matters. Connecting the React interface, Flask endpoints, dictionary operations, and local inference environment involved practical debugging of CORS and service ports.
Testing and project feedback
We evaluated Luminara with textbook material from our own classes across different subjects, looking at readability and whether simplified passages retained their meaning. The team also used feedback from a project demo to adjust the product direction. This was a semester team project, not a production-scale or clinically validated accessibility tool; no numerical accuracy or performance claims are made here.
Team and credit
Luminara was developed collaboratively through SJSU's Responsible Computing Club in Spring 2026.
- Vishalkiran Raichur — Lead backend developer
- Shivaji Ganesh
- Ryan Deng
- Shubh Rawal
- Naysa Ratra
- Suhas Kappala
