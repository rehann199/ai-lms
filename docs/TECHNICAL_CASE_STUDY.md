# Technical Case Study

## Goal

Reduce two common course pain points: students lacking condensed material for revision, and instructors spending time authoring and grading quizzes and assignments. The approach is to ground generation in the course's own documents rather than in a general prompt.

## Approach

1. **Ingest.** Course files are chunked and embedded once on upload; document-level and chunk-level vectors are stored in Supabase.
2. **Retrieve.** For each AI request the user's prompt is rewritten into a retrieval query, embedded, and matched against chosen documents with hybrid keyword and vector search.
3. **Generate.** A role-specific prompt template (quiz, assignment, summary, flashcards, practice questions) is filled with retrieved context and sent to the LLM.
4. **Verify.** For quizzes and assignments, a second pass scores the output against a rubric prompt. A fine-tuned Llama-3-8B verifier is used for this step, falling back to the base model.
5. **Deliver.** Output is converted to a notebook or PDF; students can also download summaries, flashcards and practice questions.

## Orchestration

LangGraph models the quiz and assignment pipeline as a state graph (retrieval, metaprompt, generate, verify, convert). Conditional edges decide between accepting, retrying with feedback, or finishing, and the instructor can add feedback and regenerate through the UI.

## Fine-tuning

A small self-generated dataset of draft-and-feedback examples (about 550 lines of JSON) was used with notebooks to fine-tune a Llama-3-8B model with LoRA, which was then served serverlessly for verification.

## Frontend

A single Next.js application serves admin, instructor and student views. Course tabs, tool panels, a flip-card flashcard modal and an in-page chat are built with Material UI and Joy. For this showcase a CSS-only theme was added on top of the existing components.

## Results and limits

- The end-to-end flows (upload, ingest, retrieve, generate, export) run locally against seeded fictional courses.
- Output quality in the demo is not representative, because model responses are scripted.
- Pinned Python dependencies target Python 3.10.
