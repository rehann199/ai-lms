# Engineering Challenges

## Grounding generation in course material

Generic prompts produce generic quizzes. The system rewrites the request into a retrieval query, runs hybrid search over only the selected documents and passes that context into the generation prompt. Scoping retrieval to the chosen files keeps outputs tied to what the instructor actually selected.

## Checking model output

Generated quizzes and assignments can be off-topic or poorly formed. A verification stage scores each draft, and the workflow can loop back with feedback. A fine-tuned verifier model was trained for this role, with a fallback to the base model when it returns no output.

## Keeping a human in the loop

Instructors can preview a generated assignment, give feedback and regenerate, or confirm it. The workflow accepts previous version and feedback as inputs so regeneration refines the draft instead of starting over.

## Long-running AI calls from a web UI

Generation takes several seconds and involves several model calls. The UI shows a progress bar and a blocking loading dialog and disables conflicting actions while a tool is running; only one AI tool is active at a time for students.

## Role-dependent experience

One app serves three audiences. Role is stored per enrolment, so the same account can be instructor in one course and student in another, and the course page renders the matching view.

## Making it demonstrable without hosted services

The original system depends on hosted Supabase and hosted LLM endpoints. For the showcase, local stand-ins mimic the auth, table, storage and vector-search APIs and the chat-completion endpoints, so the unmodified application code can run end to end. Only model text is scripted.

## Known limits

- Static placeholder content in Overview, Announcements and Assignments.
- Pinned backend dependencies target an older Python version.
- Backend CORS is configured for a single local origin.
