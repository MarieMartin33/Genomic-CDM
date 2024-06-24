# Genomic-CDM
 
This repository houses table specifications and related documents of Genomic Common Data Model (G-CDM).
This is a fork of [OHDSI G-CDM](https://github.com/OHDSI/Genomic-CDM), an original proposal from  Seojeong Shin and Christian Reich, not maintained till 5 years to the knowledge of the author of these lines.

This is still a prototype of the Genomic CDM that is on developing, so please use it for reference purpose.

[Original Entity-Relationship Diagram](ERD.png) was not up to date with content below and is not up to date with CDM 5.4 but gives some clues regarding this effort.

Current [G-CDM Entity-Relationship Diagram](omop-gcdm.md) is up to date with the following content and the OMOP CDM 5.4.

Mermaid cardinalites of each links have not beee worked (0..* is kept as default to create a visual link only).

See [FHIR Genomics](https://build.fhir.org/ig/HL7/genomics-reporting/sequencing.html) for consistent parallel modeling.

VARIANT_OCCURRENCE table is flattening some common variant attributes pretty surely to avoid to much jointures.

Vocabularies are listed at the end. Like FHIR Genomics, idea will be to avoid pre-coordination.
Idea here will be to use [Athena tooling](https://athena.ohdsi.org/search-terms/start) and to make an effort to use more concept encoded fields as there are to much free text in the v2.0.

COHORT and COHORT_DEFINTION are reproducted here from CDM 5.4 as multi-patient VCF files (1000Genom, 3000+ patients for exemple) may be modeled using these tables and then [Atlas tooling](https://atlas-demo.ohdsi.org/) could be plugged.

Then [DataQualityDashboard tooling](https://github.com/OHDSI/DataQualityDashboard) may be adapted to genomics.

Browse Git push history to retrieve older versions and check differences.

**_Genomic-CDM (G-CDM) v3.0 Specifications_**

# Contents

## GENOMIC_TEST

| Field                      | Required | Type         | Description                                                                                                     |
| -------------------------- | -------- | ------------ | --------------------------------------------------------------------------------------------------------------- |
| genomic_test_id            | Yes      | integer      | A unique identifier for each platform                                                                           |
| care_site_id               | Yes      | integer      | A foreign key to the site of primary care in the care_site table, where the details of the care site are stored |
| genomic_test_name          | No       | varchar(255) | Information about the name of platform using sequencing assigned by institution                                 |
| genomic_test_version       | No       | varchar(50)  | Information about the name of platform using sequencing maintained by institution                               |
| reference_genome_concept_id           | No       | integer  | Information about the Reference genome used to sequencing analysis                                              |
| sequencing_device          | No       | varchar(50)  | Sequencer machine information                                                                                   |
| library_preparation        | No       | varchar(50)  | Information about the preparation method for the sequencing library                                             |
| target_capture             | No       | varchar(50)  | Information about the capture method of examined and targeted region                                            |
| read_type_concept_id                  | No       | integer  | Information about the method of sequence reading                                                                |
| read_length                | No       | integer      | Information about the length of read                                                                            |
| quality_control_tools      | No       | varchar(255) | Information about the tool used to control quality                                                              |
| total_reads                | No       | integer      | Total count of reads involved in assembly                                                                       |
| mean_target_coverage       | No       | float        | Mean target coverage                                                                                            |
| per_target_base_cover_100x | No       | float        | Percentage of selected bases                                                                                    |
| alignment_tools            | No       | varchar(255) | Information about the name and version of the alignment tool                                                    |
| variant_calling_tools      | No       | varchar(255) | Information about the name and version of variant calling tool                                                  |
| chromosome_coordinate_concept_id      | No       | integer | Coordinated system for numbering the chromosomes                                                                |
| annotation_tools           | No       | varchar(255) | Information about the tool used for annotation                                                                  |
| annotation_databases       | No       | varchar(255) | Information about the database for annotation                                                                   |

## TARGET_GENE

| Field           | Required | Type        | Description                                                                                                                               |
| --------------- | -------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| target_gene_id  | Yes      | integer     | A system-generated unique identifier for each target region                                                                               |
| genomic_test_id | Yes      | integer     | A foreign key identifier to the platform containing the target region. The details of that platform are stored in the Platform_info table |
| gene_concept_id         | Yes      | integer | Gene ID, mainly based on HGNC nomenclature                                                                                                        |

## VARIANT_OCCURRENCE

| Field                   | Required | Type         | Description                                                                                                                         |
| ----------------------- | -------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| variant_occurrence_id   | Yes      | integer      | A unique identifier for each variant occurrence event                                                                               |
| procedure_occurrence_id | Yes      | integer      | A foreign key identifier to the Procedure_occurrence table for the procedure used to obtain the specimen                            |
| specimen_id             | Yes      | integer      | Tumor specimen ID                                                                                                                   |
| reference_specimen_id   | No       | integer      | ID of the normal specimen related to the tumor specimen                                                                             |
| target_gene1_id         | No       | varchar(50)  | A foreign key identifier to the Target_gene table for which the variant information is recorded                                     |
| target_gene1_symbol     | No       | varchar(255) | A symbol of gene where the variant information is recorded                                                                          |
| target_gene2_id         | No       | varchar(50)  | A foreign key identifier to the Target_gene table for which the variant information is recorded when a translocation variant occurs |
| target_gene2_symbol     | No       | varchar(255) | A symbol of gene where the variant information is recorded                                                                          |
| reference_sequence_concept_id      | No       | integer  | Transcript ID based on a protein-coding RNA (mRNA) made up of the accession number and version number                               |
| rs_concept_id                   | No       | integer  | dbSNP reference ID (rsID) maintained by NCBI                                                                                        |
| reference_allele        | No       | varchar(255) | Reference allele sequence (e.g., A)                                                                                                 |
| alternate_allele        | No       | varchar(255) | Variant allele sequence (e.g., C)                                                                                                   |
| dnalevel_concept_id                  | No       | integer | Nomenclature for the sequence variant at the DNA level (HGVS_A)                                                                              |
| proteinlevel_concept_id                  | No       | integer | Nomenclature for the sequence variant at the protein level  (HGVS_P)                                                                           |
| variant_read_depth      | No       | integer      | Variant depth divided by read depth                                                                                                 |
| variant_exon_number     | No       | integer      | Exon number in which the variant occurred                                                                                           |
| copy_number             | No       | float        | Copy number value for CNV data                                                                                                      |
| cnv_locus               | No       | varchar(MAX) | Locus information for CNV                                                                                                           |
| fusion_breakpoint       | No       | varchar(MAX) | sequential position that fusion variant occurred                                                                                    |
| fusion_supporting_reads | No       | integer      | Supporting read count of the fusion                                                                                                 |
| sequence_alteration     | No       | varchar(MAX) | Structural variant type                                                                                                             |
| variant_feature         | No       | varchar(MAX) | Functional variant type                                                                                                             |
| genetic_origin_concept_id          | No       | integer  | Somatic or germline origin or the variant                                                                                           |
| genotype_concept_id                | No       | integer  | Allele state                                                                                                                        |

## VARIANT_ANNOTATION

| Field                 | Required | Type         | Description                                                                                           |
| --------------------- | -------- | ------------ | ----------------------------------------------------------------------------------------------------- |
| variant_annotation_id | Yes      | integer      | A unique identifier for each variant annotation event                                                 |
| variant_occurrence_id | Yes      | integer      | A foreign key identifier to the variant_occurrence table for which the variant annotation is recorded |
| annotation_concept_id       | Yes      | integer | Categories or database name of annotation                                                             |
| value_as_string       | No       | varchar(MAX) | Annotation value as a data type of string                                                             |
| value_as_number       | No       | float        | ANnotation value as a data type of number 



# Concept and Concept_class

Concept table should be provisionned with concepts linked to these concept_class when appropriate (see FHIR Genomics):


| Defining Component | Note |
| ------------------ | ---- |
| genomic-hgvs (LOINC 81290-9) OR representative-coding-hgvs (LOINC 48004-6) | [Proper usage of HGVS contains the reference sequence identifier followed by ‘:g.’ for genomic or ‘:c.’ for a coding sequence. In HGVS notation, the “=” (equals) is used to indicate a sequence was tested but found unchanged [ref].](https://varnomen.hgvs.org/recommendations/general/) |
| cyogenomic-nomenclature (LOINC 81291-7) | more information on formatting structural variations below.|



| Defining Component | Note |
| ------------------ | ---- |
| genomic-ref-seq (LOINC 48013-7)                   | must send at least this or representative transcript refseq |
| representative-transcript-ref-seq (LOINC 51958-7) | must send at least this or genomic refseq                                                                    |
| coordinate-system (LOINC 92822-6)                 | Common coordinate systems are described by LOINC.                                                            |
| exact-start-end (LOINC 81254-5)                   | Interpretation of this number requires the reference sequence and coordinate system.                         |
| ref-allele (LOINC 69547-8)                        | This string should be normalized per the VCF standard.                                                       |
| alt-allele (LOINC 69551-0)                        | If the reference allele is tested and found unchanged, this string should be equal to the REF allele string. |


# Vocabularies

Vocabularies to be created / imported / injected in Concepts' tables:

| Vocabulary | Note |
| ------------------ | ---- |
| HGNC | ---- |
| HGVS_A | ---- |
| HGVS_P | ---- |
| dbSNP  | reference ID (rsID) maintained by NCBI |
| genetic_origin  | ---- |
| genotype  | ---- |
| reference_genome  | ---- |
| read_type  | ---- |
| chromosome_coordinate  | ---- |
| variant_annotation  | ---- |
| reference_sequence  | ---- |
