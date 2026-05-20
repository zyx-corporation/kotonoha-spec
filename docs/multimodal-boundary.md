# Multimodal semantic-change boundary

Status: **Informative design note**. This document does not add Phase 1 conformance requirements.

## Purpose

This note defines a future-work boundary for multimodal semantic-change evaluation in Kotonoha/SLS.

Phase 1 remains centered on document and text-oriented semantic lineage. It does not claim complete support for image, audio, video, diagram, or mixed-media semantic-change evaluation.

## Boundary

Future multimodal work should not reduce non-text artifacts to caption or transcript diffs alone. Captions, transcripts, OCR, embeddings, regions, time spans, and layers may be useful evidence, but they are not the whole semantic subject.

Future designs should preserve reviewability by making the evidence basis inspectable. When observations depend on generated captions, transcripts, object detection, speech recognition, or feature extraction, uncertainty should be surfaced rather than hidden.

## Candidate future subjects

Future work may consider review subjects such as:

- image revisions;
- diagrams and visual layouts;
- audio recordings and transcripts;
- video segments and scene changes;
- mixed document-media artifacts;
- generated visual or audio outputs.

## Candidate evidence references

Future evidence references may need to point to:

- text spans;
- time spans;
- visual regions;
- layers or objects;
- transcript segments;
- captions or alt text;
- model-derived features.

## RDE boundary

RDE observations for multimodal artifacts should remain review records. They should not become final approval, rejection, publication authorization, or policy enforcement by themselves.

## Non-goals

This note does not define a full multimodal schema, a benchmark, a model requirement, or a conformance profile. It only records the boundary for future design work.