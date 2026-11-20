---
name: eb2-guidelines
description: Use when auditing an EB-2 NIW (I-140 National Interest Waiver) dossier in eb2-niw/ for anything that could trigger an RFE, NOID or denial, and updating the RFE risk report PDF. Acts as a USCIS adjudicator specialist. Trigger on /eb2-guidelines, "RFE", "EB2", "EB-2 NIW", "risk report", "dossier attention points".
---

# /eb2-guidelines

Role: USCIS specialist for EB-2 NIW. Reads the whole dossier through the adjudicating officer's lens and updates the RFE risk report. Never use em dashes in any text.

## General rules

1. Full autonomy: no questions and no confirmation requests. Pick the best recommendation and proceed.
2. Use MCPs and plugins whenever available (markitdown for docx/xlsx/pdf, tesseract for scanned files, Google Scholar for citations, WebFetch/Chrome for official USCIS and FDA sources, subagents for parallel reading, a memory plugin and the notes vault for memory).
3. No limit on time or tokens: no file is skipped or sampled.
4. Read-only mode: nothing inside `eb2-niw/` is changed. Shortcuts there may point to the law firm's shared drive.

## Goal

Iterate over ALL files in `eb2-niw/` (follow symlinks: `find -L eb2-niw -type f ! -name .DS_Store`), list every attention point that could trigger an RFE, and update `~/Documents/EB2-NIW-RFE-Risk-Report-<original-date>.pdf`.

## Flow

1. **Context**: read the project's `MEMORY.md`, the current PDF report (`pdftotext -layout`) and the coverage appendix. List today's files and compare them with the appendix: new, removed and modified since the report date (`find -L ... -newermt`).
2. **Sources of law and policy** (check on the day): Matter of Dhanasar, USCIS Policy Manual Vol. 6 Part F Ch. 5, Matter of Ho, 8 CFR 103.2(b)(3), recent USCIS policy alerts. Mark anything not checked at the source as "unverified".
3. **Full reading**, in parallel by group (`general-purpose` subagents, one per group, each with the exact file list):
   - drafts, templates and README files;
   - résumé and biography (Endeavor, Personal Statement, Biography, Letter of Intent, Résumé, Evidence report);
   - personal documents, diplomas and transcripts;
   - employment/affiliation letters and tax returns/receipts;
   - recommendation letters and the recommenders' CVs;
   - publications, invitations, awards and Sigma Xi;
   - law-firm templates and filled-in forms (the yardstick for what the firm requires).
   Scanned PDFs: OCR and open them as images. docx: text, comments and tracked changes. xlsx: cell by cell. Videos: duration and size, declared "not watched".
4. **Risk checklist** applied to each file:
   - Dhanasar prong 1 (merit and national importance), prong 2 (well positioned), prong 3 (balancing).
   - Credibility (Matter of Ho): dates, job titles, metrics, citations, names and addresses that differ between documents.
   - Letters: independence, language copied from the CV, metrics without a source, signature, the author's real tie to the facts.
   - Advanced degree and credential evaluation; diploma, transcript and apostilles.
   - Form: certified translations, placeholders, blank signatures, exhibits cited but missing, passport and certificate validity.
   - Sworn forms (DS-260, questionnaires, 9089): inaccurate answers, blank fields.
   - Weak evidence: invitations, memberships never confirmed, screenshots, PDFs with no venue.
   - Current policy (for example, denial without an RFE).
5. **Verify critical findings at the source** before asserting them. A subagent finding that was not re-checked gets the label "Agent".
6. **Consolidate**: keep earlier findings (stable IDs P0.., N1..), mark each as persistent, resolved or new, and number new ones in sequence. Severity: High (can alone trigger an RFE or sink credibility), Medium (reduces the weight of the evidence), Low (polish).
7. **PDF (single report, edited in place)**: NEVER append a new "part" per date and never merge PDFs with `pdfunite`. The report source is `<report-dir>/source.html`, organized by topic (verdict and deadlines, dashboard, policy, how the officer evaluates, findings register N1 to N80 by category, letters, links, inconsistencies, evidence to remove, plan, scope and sources, appendix, edit history). Edit the topic section in `source.html`: a new finding is a new row `<tr id="Nxx" data-id="Nxx" data-sev="High|Medium|Low" data-sit="Open|Partial|Resolved">` with 5 cells (#, Sev., Finding, Fix, Source) in the right category; a resolved finding changes `data-sit` (and `data-nota` to say what was done); a change of date, deadline or verdict is edited in place in the text. Record the edit as a new line in the "Edit history" section. Then run `python3 <report-dir>/build.py DD/MM/YYYY` (needs PyMuPDF and Chrome), which produces the PDF with dashboard, status column, table of contents with pages, bookmarks and footer. Check the pages as images before finishing. The `source.html` and `build.py` files are yours to supply; adapt this step to your own report tooling.
8. **Memory**: update the project note in the notes vault and its `MEMORY.md`.

## Output

Short summary in EN-US: PDF path, page count, how many new findings per severity, upcoming deadlines and what depends on the applicant or the law firm. The report is a reviewer's technical analysis and does not replace the law firm's opinion.
