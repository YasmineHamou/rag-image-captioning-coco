# Phase 1: Data exploration and pre-processing

Deadline: 4 November 2026.

## Execution status

This report only summarizes artifacts present on disk. Synthetic test fixtures are not COCO results.

## Dataset and sampling

MS COCO 2017: knowledge base and tuning from train2017; test from val2017.
Configured sizes (not observed counts): KB=10000, validation=100, test=1000.
Teacher clarification relayed on 7 November 2026: target=1000 test images from val2017; retain the initial 100-image test for a future comparison. Current frozen release: test_size=1000. Extended test prepared; original 100 retained as a nested comparison subset. See [dated clarification](../docs/project/teacher_clarifications_20261107.md) and [requirements review](phase1_review_20261107.md).

Split status: **PROVISIONAL_NOT_TEACHER_SPLIT**.
Sampling: derived instance-count strata, proportional largest-remainder quotas, seeded sampling within strata.
Simple <= configured simple threshold; medium <= configured medium threshold; complex above. These are our proxies, not COCO scene labels.
Teacher-supplied common test IDs override provisional sampling. Complexity alone does not guarantee coverage of interactions or contexts.

## Data audit and anomalies

Audit passed: True. See [audit.json](audit.json) and [issues CSV](tables/audit_issues.csv).
Recorded issues: 2. No silent exclusions or deletions.
The [initial audit](audit_initial_20261028.json) stopped on annotation 918 (COCO image 200365): bbox height=0 and area=0 in the official annotation.
Decision: preserve image, captions and category annotation; set bbox_valid=false and exclude this annotation only from spatial geometry and box drawing. Counts still describe original annotation entries. Raw inputs and split IDs were not changed.
- warning: degenerate_bbox — annotation_id=918; bbox=[296.65, 388.33, 1.03, 0.0]; retained raw/category annotation; excluded only from geometric relations/box drawing
- warning: shared_caption_text — 87 identical normalized caption strings across splits; inspect, not proof of provenance leakage
Inspect [shared caption strings and their distinct source IDs](tables/shared_caption_strings.csv) to distinguish repeated wording from provenance leakage.

## Statistical exploration

| Split | Images | Captions | Objects/image (mean) | Caption words (mean) | Vocabulary |
|---|---:|---:|---:|---:|---:|
| knowledge_base | 10000 | 50000 | 7.251 | 10.453 | 9972 |
| validation | 100 | 500 | 7.340 | 10.290 | 945 |
| test | 1000 | 5000 | 7.388 | 10.449 | 3372 |
| test_100 | 100 | 500 | 7.050 | 10.272 | 904 |

Tokenization: NLTK TreebankWordTokenizer; keep tokens containing alphabetic characters; lowercase for vocabulary; POS tags from original case/full tokens, count alphabetic tokens only.

Figures:
- [knowledge_base_top50_objects.png](figures/knowledge_base_top50_objects.png)
- [knowledge_base_word_frequencies.png](figures/knowledge_base_word_frequencies.png)
- [knowledge_base_pos.png](figures/knowledge_base_pos.png)
- [knowledge_base_complexity.png](figures/knowledge_base_complexity.png)
- [knowledge_base_derived_attributes.png](figures/knowledge_base_derived_attributes.png)
- [knowledge_base_spatial_relations.png](figures/knowledge_base_spatial_relations.png)
- [knowledge_base_objects_per_image.png](figures/knowledge_base_objects_per_image.png)
- [knowledge_base_caption_length.png](figures/knowledge_base_caption_length.png)
- [knowledge_base_cooccurrence.png](figures/knowledge_base_cooccurrence.png)
- [knowledge_base_complexity_caption_scatter.png](figures/knowledge_base_complexity_caption_scatter.png)
- [knowledge_base_examples.png](figures/knowledge_base_examples.png)
- [validation_top50_objects.png](figures/validation_top50_objects.png)
- [validation_word_frequencies.png](figures/validation_word_frequencies.png)
- [validation_pos.png](figures/validation_pos.png)
- [validation_complexity.png](figures/validation_complexity.png)
- [validation_derived_attributes.png](figures/validation_derived_attributes.png)
- [validation_spatial_relations.png](figures/validation_spatial_relations.png)
- [validation_objects_per_image.png](figures/validation_objects_per_image.png)
- [validation_caption_length.png](figures/validation_caption_length.png)
- [validation_cooccurrence.png](figures/validation_cooccurrence.png)
- [validation_complexity_caption_scatter.png](figures/validation_complexity_caption_scatter.png)
- [validation_examples.png](figures/validation_examples.png)
- [test_top50_objects.png](figures/test_top50_objects.png)
- [test_word_frequencies.png](figures/test_word_frequencies.png)
- [test_pos.png](figures/test_pos.png)
- [test_complexity.png](figures/test_complexity.png)
- [test_derived_attributes.png](figures/test_derived_attributes.png)
- [test_spatial_relations.png](figures/test_spatial_relations.png)
- [test_objects_per_image.png](figures/test_objects_per_image.png)
- [test_caption_length.png](figures/test_caption_length.png)
- [test_cooccurrence.png](figures/test_cooccurrence.png)
- [test_complexity_caption_scatter.png](figures/test_complexity_caption_scatter.png)
- [test_examples.png](figures/test_examples.png)
- [test_100_top50_objects.png](figures/test_100_top50_objects.png)
- [test_100_word_frequencies.png](figures/test_100_word_frequencies.png)
- [test_100_pos.png](figures/test_100_pos.png)
- [test_100_complexity.png](figures/test_100_complexity.png)
- [test_100_derived_attributes.png](figures/test_100_derived_attributes.png)
- [test_100_spatial_relations.png](figures/test_100_spatial_relations.png)
- [test_100_objects_per_image.png](figures/test_100_objects_per_image.png)
- [test_100_caption_length.png](figures/test_100_caption_length.png)
- [test_100_cooccurrence.png](figures/test_100_cooccurrence.png)
- [test_100_complexity_caption_scatter.png](figures/test_100_complexity_caption_scatter.png)
- [test_100_examples.png](figures/test_100_examples.png)

### Observations on the actual knowledge-base subset

Object-count mean=7.2507, median=4.0, maximum=59; inspect the histogram to see the uneven scene-complexity distribution.
Zero object annotations do not imply an empty image: only the annotated COCO categories are counted. Vocabulary sizes depend on corpus size and cannot be compared directly across the differently sized splits.
Object-class frequencies describe annotation instances; co-occurrence describes image-level presence. These count different units and should not be interpreted interchangeably.

## Preprocessing and provenance

Original images remain unchanged; load with EXIF transpose and RGB conversion. The official model processor handles resizing, cropping and normalization.
Raw captions are preserved exactly; whitespace normalization is applied only to a separate retrieval field; lowercase is configurable.
Annotations use source=coco_annotation; keyword metadata uses source=caption_heuristic; spatial proxies use source=bbox_heuristic.
Ordered box pairs: left/right and above/below compare box centres; overlap means IoU >= configured threshold. Opposite directions are symmetric in aggregate; overlap counts both orientations. Boxes remain in original COCO coordinates.

## Embeddings and FAISS

Model: openai/clip-vit-base-patch32; immutable revision: 3d74acf9a28c67741b2f4f2ea7635f0aaf6f0268; device: cpu.
Image vectors: 10000 x 512; text vectors: 50000 x 512.
Float32; L2 normalization; batch size 32; truncated captions: 0.
image: IndexFlatIP, 10000 vectors, dimension 512; build 0.0075 s; disk 20480045 bytes.
text: IndexFlatIP, 50000 vectors, dimension 512; build 0.0340 s; disk 102400045 bytes.

## Validation image-query retrieval

Actual image queries encoded: 100; knowledge-base images searched: 10000; top-k: 5.
Status: passed; encoding batches: 7.982 seconds; mean per-query search: 4.167 ms.
Results: [JSON](retrieval/validation/results.json), [CSV](retrieval/validation/results.csv), [visual gallery](retrieval/validation/gallery.html).
These are functional retrieval checks. No ground-truth relevance labels, Recall@k claim, generated captions or final caption evaluation are provided.

## Anti-leakage and quality control

Only knowledge-base metadata feeds embedding/index creation. Validation/test references remain outside retrieval; all rows for the query image are excluded from results.
Frozen ID hashes, annotation hashes, metadata hashes, embedding row maps and reconstructed FAISS vectors are checked.
Quality control passed: True.

## Limitations and decisions

- The 1000-image test is prepared and the initial 100 is retained with separate statistics. Common teacher IDs remain unavailable; the local split cannot be called the official common test.
- No visual object detector was run (optional recommendation). Object counts come from COCO annotations, attributes from caption heuristics. Human-reference confidence=1 and threshold=1 are explicit in metadata; derived attributes do not inherit this convention.
- No direct ground truth is claimed for colour, shape, day/night, indoor/outdoor or semantic interactions. Absent keywords mean unknown.
- COCO crowd annotations count as annotation instances, not individual people/objects.
- Exact file/pixel duplication is checked; perceptual near-duplicates remain a limitation.
- Identical caption strings across unrelated images are flagged for review, not automatically treated as provenance leakage.
- Processor centre crops may remove peripheral content; no additional augmentations are applied.
- No preprocessing optimum is claimed without a validation comparison approved for this phase.
- Python 3.11 clean installation remains unverified; the actual execution environment is recorded in manifests.
- Caption generation, final metrics, model statistical comparisons and ablations belong to later phases.
