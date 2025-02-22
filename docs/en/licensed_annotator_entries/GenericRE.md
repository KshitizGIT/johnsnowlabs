{%- capture title -%}
GenericREModel
{%- endcapture -%}


{%- capture model -%}
model
{%- endcapture -%}

{%- capture model_description -%}
Instantiated RelationExtractionModel for extracting relationships between any entities.
This class is not intended to be directly used, please use the RelationExtractionModel instead.
Pairs of entities should be specified using setRelationPairs.


{%- endcapture -%}

{%- capture model_input_anno -%}
WORD_EMBEDDINGS, POS, CHUNK, DEPENDENCY
{%- endcapture -%}

{%- capture model_output_anno -%}
CATEGORY
{%- endcapture -%}

{%- capture model_python_medical -%}
from johnsnowlabs import nlp, medical

documenter = nlp.DocumentAssembler()\
    .setInputCol("text")\
    .setOutputCol("document")

sentencer = nlp.SentenceDetector()\
    .setInputCols(["document"])\
    .setOutputCol("sentences")

tokenizer = nlp.Tokenizer()\
    .setInputCols(["sentences"])\
    .setOutputCol("tokens")

words_embedder = nlp.WordEmbeddingsModel()\
    .pretrained("embeddings_clinical", "en", "clinical/models")\
    .setInputCols(["sentences", "tokens"])\
    .setOutputCol("embeddings")

pos_tagger = nlp.PerceptronModel()\
    .pretrained("pos_clinical", "en", "clinical/models") \
    .setInputCols(["sentences", "tokens"])\
    .setOutputCol("pos_tags")

ner_tagger = medical.NerModel()\
    .pretrained("ner_posology", "en", "clinical/models")\
    .setInputCols("sentences", "tokens", "embeddings")\
    .setOutputCol("ner_tags")

ner_chunker = medical.NerConverterInternal()\
    .setInputCols(["sentences", "tokens", "ner_tags"])\
    .setOutputCol("ner_chunks")

dependency_parser = nlp.DependencyParserModel()\
    .pretrained("dependency_conllu", "en")\
    .setInputCols(["sentences", "pos_tags", "tokens"])\
    .setOutputCol("dependencies")

reModel = medical.RelationExtractionModel()\
    .pretrained("generic_re")\
    .setInputCols(["embeddings", "pos_tags", "ner_chunks", "dependencies"])\
    .setOutputCol("relations")\
    .setRelationPairs(["problem-test",
                       "problem-treatment"])\
    .setMaxSyntacticDistance(4)

pipeline = nlp.Pipeline(stages=[
    documenter,
    sentencer,
    tokenizer,
    words_embedder,
    pos_tagger,
    ner_tagger,
    ner_chunker,
    dependency_parser,
    reModel
])

text = """
A 28-year-old female with a history of gestational diabetes mellitus diagnosed eight years prior to
presentation and subsequent type two diabetes mellitus ( T2DM ), one prior episode of HTG-induced pancreatitis
three years prior to presentation , associated with an acute hepatitis , and obesity with a body mass index
( BMI ) of 33.5 kg/m2 , presented with a one-week history of polyuria , polydipsia , poor appetite , and
vomiting . Two weeks prior to presentation , she was treated with a five-day course of amoxicillin for a
respiratory tract infection . She was on metformin , glipizide , and dapagliflozin for T2DM and atorvastatin
and gemfibrozil for HTG . She had been on dapagliflozin for six months at the time of presentation . Physical
examination on presentation was significant for dry oral mucosa ; significantly , her abdominal examination was
benign with no tenderness , guarding , or rigidity .
"""
df = spark.createDataFrame([[text]]).toDF("text")
result = pipeline.fit(df).transform(df)

# Show results
result.select(F.explode(F.arrays_zip(
                              result.relations.result,
                              result.relations.metadata)).alias("cols"))\
.select(
    F.expr("cols['1']['chunk1']").alias("chunk1"),
    F.expr("cols['1']['chunk2']").alias("chunk2"),
    F.expr("cols['1']['entity1']").alias("entity1"),
    F.expr("cols['1']['entity2']").alias("entity2"),
    F.expr("cols['0']").alias("relations"),
    F.expr("cols['1']['confidence']").alias("confidence")).show(5, truncate=False)

+-----------------+-------------+-------+-------+------------+----------+
|chunk1           |chunk2       |entity1|entity2|relations   |confidence|
+-----------------+-------------+-------+-------+------------+----------+
|obesity          |BMI          |PROBLEM|TEST   |PROBLEM-TEST|1.0       |
|a body mass index|BMI          |PROBLEM|TEST   |PROBLEM-TEST|1.0       |
|BMI              |polyuria     |TEST   |PROBLEM|TEST-PROBLEM|1.0       |
|BMI              |polydipsia   |TEST   |PROBLEM|TEST-PROBLEM|1.0       |
|BMI              |poor appetite|TEST   |PROBLEM|TEST-PROBLEM|1.0       |
+-----------------+-------------+-------+-------+------------+----------+

{%- endcapture -%}

{%- capture model_scala_medical -%}
import spark.implicits._

val documenter = new DocumentAssembler()
    .setInputCol("text") 
    .setOutputCol("document") 

val sentencer = new SentenceDetector()
    .setInputCols("document")
    .setOutputCol("sentences") 

val tokenizer = new Tokenizer()
    .setInputCols("sentences") 
    .setOutputCol("tokens") 

val words_embedder = WordEmbeddingsModel.pretrained("embeddings_clinical","en","clinical/models") 
    .setInputCols(Array("sentences","tokens")) 
    .setOutputCol("embeddings") 

val pos_tagger = PerceptronModel.pretrained("pos_clinical","en","clinical/models") 
    .setInputCols(Array("sentences","tokens")) 
    .setOutputCol("pos_tags") 

val ner_tagger = MedicalNerModel.pretrained("ner_posology","en","clinical/models") 
    .setInputCols("sentences","tokens","embeddings") 
    .setOutputCol("ner_tags") 

val ner_chunker = new NerConverterInternal()
    .setInputCols(Array("sentences","tokens","ner_tags")) 
    .setOutputCol("ner_chunks") 

val dependency_parser = DependencyParserModel.pretrained("dependency_conllu","en") 
    .setInputCols(Array("sentences","pos_tags","tokens")) 
    .setOutputCol("dependencies") 

val reModel = RelationExtractionModel.pretrained("generic_re") 
    .setInputCols(Array("embeddings","pos_tags","ner_chunks","dependencies")) 
    .setOutputCol("relations")
    .setRelationPairs(Array("problem-test","problem-treatment"))
    .setMaxSyntacticDistance(4)
    

val pipeline = new Pipeline().setStages(Array(
                                             documenter, 
                                             sentencer, 
                                             tokenizer,
                                             words_embedder, 
                                             pos_tagger, 
                                             ner_tagger, 
                                             ner_chunker, 
                                             dependency_parser, 
                                             reModel )) 

val text = "A 28-year-old female with a history of gestational diabetes mellitus diagnosed eight years prior to " +
"presentation and subsequent type two diabetes mellitus ( T2DM ), one prior episode of HTG-induced pancreatitis " +
"three years prior to presentation , associated with an acute hepatitis , and obesity with a body mass index " +
"( BMI ) of 33.5 kg/m2 , presented with a one-week history of polyuria , polydipsia , poor appetite , and " +
"vomiting . Two weeks prior to presentation , she was treated with a five-day course of amoxicillin for a " +
"respiratory tract infection . She was on metformin , glipizide , and dapagliflozin for T2DM and atorvastatin " +
"and gemfibrozil for HTG . She had been on dapagliflozin for six months at the time of presentation . Physical " +
"examination on presentation was significant for dry oral mucosa ; significantly , her abdominal examination was " +
"benign with no tenderness , guarding , or rigidity."

val df = Seq(text) .toDF("text") 
val result = pipeline.fit(df) .transform(df) 

// Show results

+-----------------+-------------+-------+-------+------------+----------+
|chunk1           |chunk2       |entity1|entity2|relations   |confidence|
+-----------------+-------------+-------+-------+------------+----------+
|obesity          |BMI          |PROBLEM|TEST   |PROBLEM-TEST|1.0       |
|a body mass index|BMI          |PROBLEM|TEST   |PROBLEM-TEST|1.0       |
|BMI              |polyuria     |TEST   |PROBLEM|TEST-PROBLEM|1.0       |
|BMI              |polydipsia   |TEST   |PROBLEM|TEST-PROBLEM|1.0       |
|BMI              |poor appetite|TEST   |PROBLEM|TEST-PROBLEM|1.0       |
+-----------------+-------------+-------+-------+------------+----------+

{%- endcapture -%}


{%- capture model_api_link -%}
[RelationExtractionModel](https://nlp.johnsnowlabs.com/licensed/api/com/johnsnowlabs/nlp/annotators/re/RelationExtractionModel.html)
{%- endcapture -%}

{%- capture model_python_api_link -%}
[RelationExtractionModel](https://nlp.johnsnowlabs.com/licensed/api/python/reference/autosummary/sparknlp_jsl/annotator/re/relation_extraction/index.html#sparknlp_jsl.annotator.re.relation_extraction.RelationExtractionModel)
{%- endcapture -%}

{%- capture model_notebook_link -%}
[RelationExtractionModelNotebook](https://github.com/JohnSnowLabs/spark-nlp-workshop/blob/Healthcare_MOOC/Spark_NLP_Udemy_MOOC/Healthcare_NLP/RelationExtractionModel.ipynb)
{%- endcapture -%}


{% include templates/licensed_approach_model_medical_fin_leg_template.md
title=title
model=model
model_description=model_description
model_input_anno=model_input_anno
model_output_anno=model_output_anno
model_python_medical=model_python_medical
model_scala_medical=model_scala_medical
model_api_link=model_api_link
model_python_api_link=model_python_api_link
model_notebook_link=model_notebook_link
%}
