---
layout: model
title: Named Entity Recognition Profiling (Clinical)
author: John Snow Labs
name: ner_profiling_clinical
date: 2023-05-04
tags: [licensed, en, clinical, profiling, ner_profiling, ner]
task: [Named Entity Recognition, Pipeline Healthcare]
language: en
edition: Healthcare NLP 4.4.0
spark_version: 3.2
supported: true
annotator: PipelineModel
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This pipeline can be used to explore all the available pretrained NER models at once. When you run this pipeline over your text, you will end up with the predictions coming out of each pretrained clinical NER model trained with `embeddings_clinical`. It has been updated by adding new clinical NER models and NER model outputs to the previous version. 

Here are the NER models that this pretrained pipeline includes:

`jsl_ner_wip_clinical`,`jsl_ner_wip_greedy_clinical`,`jsl_ner_wip_modifier_clinical`, `jsl_rd_ner_wip_greedy_clinical`, `ner_abbreviation_clinical`, `ner_ade_binary`, `ner_ade_clinical`, `ner_anatomy`, `ner_anatomy_coarse`, `ner_bacterial_species`, `ner_biomarker`, `ner_biomedical_bc2gm`, `ner_bionlp`, `ner_cancer_genetics`, `ner_cellular`, `ner_chemd_clinical`, `ner_chemicals`, `ner_chemprot_clinical`, `ner_chexpert`, `ner_clinical`, `ner_clinical_large`, `ner_clinical_trials_abstracts`, `ner_covid_trials`, `ner_deid_augmented`, `ner_deid_enriched`, `ner_deid_generic_augmented`,`ner_deid_large`, `ner_deid_sd`,`ner_deid_sd_large`,`ner_deid_subentity_augmented`,`ner_deid_subentity_augmented_i2b2`, `ner_deid_synthetic`, `ner_diseases`, `ner_diseases_large`, `ner_drugprot_clinical`, `ner_drugs`, `ner_drugs_greedy`, `ner_drugs_large`, `ner_eu_clinical_case`, `ner_eu_clinical_condition`, `ner_events_admission_clinical`, `ner_events_clinical`, `ner_financial_contract`, `ner_genetic_variants`, `ner_healthcare`, `ner_human_phenotype_gene_clinical`, `ner_human_phenotype_go_clinical`, `ner_jsl`, `ner_jsl_enriched`, `ner_jsl_slim`, `ner_living_species`, `ner_measurements_clinical`, `ner_medmentions_coarse`, `ner_nature_nero_clinical`, `ner_nihss`, `ner_oncology`, `ner_oncology_anatomy_general`, `ner_oncology_anatomy_granular`, `ner_oncology_biomarker`, `ner_oncology_demographics`, `ner_oncology_diagnosis`, `ner_oncology_posology`, `ner_oncology_response_to_treatment`, `ner_oncology_test`, `ner_oncology_therapy`, `ner_oncology_tnm`, `ner_oncology_unspecific_posology`, `ner_oncology_wip`, `ner_pathogen`, `ner_posology`, `ner_posology_experimental`, `ner_posology_greedy`, `ner_posology_large`, `ner_posology_small`, `ner_radiology`, `ner_radiology_wip_clinical`, `ner_risk_factors`, `ner_sdoh_access_to_healthcare_wip`, `ner_sdoh_community_condition_wip`, `ner_sdoh_demographics_wip`, `ner_sdoh_health_behaviours_problems_wip`, `ner_sdoh_income_social_status_wip`, `ner_sdoh_mentions`, `ner_sdoh_slim_wip`, `ner_sdoh_social_environment_wip`, `ner_sdoh_substance_usage_wip`, `ner_sdoh_wip`, `ner_supplement_clinical`, `ner_vop_anatomy_wip`, `ner_vop_clinical_dept_wip`, `ner_vop_demographic_wip`, `ner_vop_problem_reduced_wip`, `ner_vop_problem_wip`, `ner_vop_slim_wip`, `ner_vop_temporal_wip`, `ner_vop_test_wip`, `ner_vop_treatment_wip`, `ner_vop_wip`, `nerdl_tumour_demo`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/ner_profiling_clinical_en_4.4.0_3.2_1683225723531.zip){:.button.button-orange}
[Copy S3 URI](s3://auxdata.johnsnowlabs.com/clinical/models/ner_profiling_clinical_en_4.4.0_3.2_1683225723531.zip){:.button.button-orange.button-orange-trans.button-icon.button-copy-s3}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}

```python

from sparknlp.pretrained import PretrainedPipeline

ner_profiling_pipeline = PretrainedPipeline("ner_profiling_clinical", "en", "clinical/models")

result = ner_profiling_pipeline.annotate("""A 28-year-old female with a history of gestational diabetes mellitus diagnosed eight years prior to presentation and subsequent type two diabetes mellitus ( T2DM ), one prior episode of HTG-induced pancreatitis three years prior to presentation , associated with an acute hepatitis , and obesity with a body mass index ( BMI ) of 33.5 kg/m2 , presented with a one-week history of polyuria , polydipsia , poor appetite , and vomiting.""")

```
```scala

import com.johnsnowlabs.nlp.pretrained.PretrainedPipeline

val ner_profiling_pipeline = PretrainedPipeline("ner_profiling_clinical", "en", "clinical/models")

val result = ner_profiling_pipeline.annotate("""A 28-year-old female with a history of gestational diabetes mellitus diagnosed eight years prior to presentation and subsequent type two diabetes mellitus ( T2DM ), one prior episode of HTG-induced pancreatitis three years prior to presentation , associated with an acute hepatitis , and obesity with a body mass index ( BMI ) of 33.5 kg/m2 , presented with a one-week history of polyuria , polydipsia , poor appetite , and vomiting.""")

```

{:.nlu-block}
```python

import nlu

nlu.load("en.med_ner.profiling_clinical").predict("""A 28-year-old female with a history of gestational diabetes mellitus diagnosed eight years prior to presentation and subsequent type two diabetes mellitus ( T2DM ), one prior episode of HTG-induced pancreatitis three years prior to presentation , associated with an acute hepatitis , and obesity with a body mass index ( BMI ) of 33.5 kg/m2 , presented with a one-week history of polyuria , polydipsia , poor appetite , and vomiting.""")

```
</div>

## Results

```bash
******************** ner_jsl Model Results ******************** 

[('28-year-old', 'Age'), ('female', 'Gender'), ('gestational diabetes mellitus', 'Diabetes'), ('eight years prior', 'RelativeDate'), ('subsequent', 'Modifier'), ('type two diabetes mellitus', 'Diabetes'), ('T2DM', 'Diabetes'), ('HTG-induced pancreatitis', 'Disease_Syndrome_Disorder'), ('three years prior', 'RelativeDate'), ('acute', 'Modifier'), ('hepatitis', 'Communicable_Disease'), ('obesity', 'Obesity'), ('body mass index', 'Symptom'), ('33.5 kg/m2', 'Weight'), ('one-week', 'Duration'), ('polyuria', 'Symptom'), ('polydipsia', 'Symptom'), ('poor appetite', 'Symptom'), ('vomiting', 'Symptom')]


******************** ner_diseases_large Model Results ******************** 

[('gestational diabetes mellitus', 'Disease'), ('diabetes mellitus', 'Disease'), ('T2DM', 'Disease'), ('pancreatitis', 'Disease'), ('hepatitis', 'Disease'), ('obesity', 'Disease'), ('polyuria', 'Disease'), ('polydipsia', 'Disease'), ('vomiting', 'Disease')]


******************** ner_radiology Model Results ******************** 

[('gestational diabetes mellitus', 'Disease_Syndrome_Disorder'), ('type two diabetes mellitus', 'Disease_Syndrome_Disorder'), ('T2DM', 'Disease_Syndrome_Disorder'), ('HTG-induced pancreatitis', 'Disease_Syndrome_Disorder'), ('acute hepatitis', 'Disease_Syndrome_Disorder'), ('obesity', 'Disease_Syndrome_Disorder'), ('body', 'BodyPart'), ('mass index', 'Symptom'), ('BMI', 'Test'), ('33.5', 'Measurements'), ('kg/m2', 'Units'), ('polyuria', 'Symptom'), ('polydipsia', 'Symptom'), ('poor appetite', 'Symptom'), ('vomiting', 'Symptom')]


******************** ner_clinical Model Results ******************** 

[('gestational diabetes mellitus', 'PROBLEM'), ('subsequent type two diabetes mellitus', 'PROBLEM'), ('T2DM', 'PROBLEM'), ('HTG-induced pancreatitis', 'PROBLEM'), ('an acute hepatitis', 'PROBLEM'), ('obesity', 'PROBLEM'), ('a body mass index', 'PROBLEM'), ('BMI', 'TEST'), ('polyuria', 'PROBLEM'), ('polydipsia', 'PROBLEM'), ('poor appetite', 'PROBLEM'), ('vomiting', 'PROBLEM')]


******************** ner_medmentions_coarse Model Results ******************** 

[('female', 'Organism_Attribute'), ('diabetes mellitus', 'Disease_or_Syndrome'), ('diabetes mellitus', 'Disease_or_Syndrome'), ('T2DM', 'Disease_or_Syndrome'), ('HTG-induced pancreatitis', 'Disease_or_Syndrome'), ('associated with', 'Qualitative_Concept'), ('acute hepatitis', 'Disease_or_Syndrome'), ('obesity', 'Disease_or_Syndrome'), ('body mass index', 'Clinical_Attribute'), ('BMI', 'Clinical_Attribute'), ('polyuria', 'Sign_or_Symptom'), ('polydipsia', 'Sign_or_Symptom'), ('poor appetite', 'Sign_or_Symptom'), ('vomiting', 'Sign_or_Symptom')]

...
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|ner_profiling_clinical|
|Type:|pipeline|
|Compatibility:|Healthcare NLP 4.4.0+|
|License:|Licensed|
|Edition:|Official|
|Language:|en|
|Size:|3.1 GB|

## Included Models

- DocumentAssembler
- SentenceDetectorDLModel
- TokenizerModel
- WordEmbeddingsModel
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- MedicalNerModel
- NerConverter
- Finisher