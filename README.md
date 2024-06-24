# Genomic-CDM
Welcome to Genomic Common Data Model (G-CDM) ! 
This repository houses table specifications and related documents of G-CDM for the latest version as well as changes.
This is a prototype of the Genomic CDM that is on developing, so please use it for reference purpose.

[Original Entity-Relationship Diagram](ERD.png) was not up to date with content below and is not up to date with CDM 5.4 but gives some clues regarding this effort.

Current [G-CDM Entity-Relationship Diagram](omop-gcdm.md) is up to date with the original G-CDM wiki content and the OMOP CDM 5.4.

**_Genomic-CDM (G-CDM) v2.0 Specifications_**

# Contents

## GENOMIC_TEST

| Field                      | Required | Type         | Description                                                                                                     |
| -------------------------- | -------- | ------------ | --------------------------------------------------------------------------------------------------------------- |
| genomic_test_id            | Yes      | integer      | A unique identifier for each platform                                                                           |
| care_site_id               | Yes      | integer      | A foreign key to the site of primary care in the care_site table, where the details of the care site are stored |
| genomic_test_name          | No       | varchar(255) | Information about the name of platform using sequencing assigned by institution                                 |
| genomic_test_version       | No       | varchar(50)  | Information about the name of platform using sequencing maintained by institution                               |
| reference_genome           | No       | varchar(50)  | Information about the Reference genome used to sequencing analysis                                              |
| sequencing_device          | No       | varchar(50)  | Sequencer machine information                                                                                   |
| library_preparation        | No       | varchar(50)  | Information about the preparation method for the sequencing library                                             |
| target_capture             | No       | varchar(50)  | Information about the capture method of examined and targeted region                                            |
| read_type                  | No       | varchar(50)  | Information about the method of sequence reading                                                                |
| read_length                | No       | integer      | Information about the length of read                                                                            |
| quality_control_tools      | No       | varchar(255) | Information about the tool used to control quality                                                              |
| total_reads                | No       | integer      | Total count of reads involved in assembly                                                                       |
| mean_target_coverage       | No       | float        | Mean target coverage                                                                                            |
| per_target_base_cover_100x | No       | float        | Percentage of selected bases                                                                                    |
| alignment_tools            | No       | varchar(255) | Information about the name and version of the alignment tool                                                    |
| variant_calling_tools      | No       | varchar(255) | Information about the name and version of variant calling tool                                                  |
| chromosome_corrdinate      | No       | varchar(255) | Coordinated system for numbering the chromosomes                                                                |
| annotation_tools           | No       | varchar(255) | Information about the tool used for annotation                                                                  |
| annotation_databases       | No       | varchar(255) | Information about the database for annotation                                                                   |

## TARGET_GENE

| Field           | Required | Type        | Description                                                                                                                               |
| --------------- | -------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| target_gene_id  | Yes      | integer     | A system-generated unique identifier for each target region                                                                               |
| genomic_test_id | Yes      | integer     | A foreign key identifier to the platform containing the target region. The details of that platform are stored in the Platform_info table |
| hgnc_id         | Yes      | varchar(50) | Gene ID based on HGNC nomenclature                                                                                                        |
| hgnc_symbol     | Yes      | varchar(50) | Gene Symbol given by HGNC nomenclature                                                                                                    |

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
| reference_sequence      | No       | varchar(50)  | Transcript ID based on a protein-coding RNA (mRNA) made up of the accession number and version number                               |
| rs_id                   | No       | varchar(50)  | dbSNP reference ID (rsID) maintained by NCBI                                                                                        |
| reference_allele        | No       | varchar(255) | Reference allele sequence (e.g., A)                                                                                                 |
| alternate_allele        | No       | varchar(255) | Variant allele sequence (e.g., C)                                                                                                   |
| hgvs_c                  | No       | varchar(MAX) | Nomenclature for the sequence variant at the DNA level                                                                              |
| hgvs_p                  | No       | varchar(MAX) | Nomenclature for the sequence variant at the protein level                                                                          |
| variant_read_depth      | No       | integer      | Variant depth divided by read depth                                                                                                 |
| variant_exon_number     | No       | integer      | Exon number in which the variant occurred                                                                                           |
| copy_number             | No       | float        | Copy number value for CNV data                                                                                                      |
| cnv_locus               | No       | varchar(MAX) | Locus information for CNV                                                                                                           |
| fusion_breakpoint       | No       | varchar(MAX) | sequential position that fusion variant occurred                                                                                    |
| fusion_supporting_reads | No       | integer      | Supporting read count of the fusion                                                                                                 |
| sequence_alteration     | No       | varchar(MAX) | Structural variant type                                                                                                             |
| variant_feature         | No       | varchar(MAX) | Functional variant type                                                                                                             |
| genetic origin          | No       | varchar(50)  | Somatic or germline origin or the variant                                                                                           |
| genotype                | No       | varchar(50)  | Allele state                                                                                                                        |

## VARIANT_ANNOTATION

| Field                 | Required | Type         | Description                                                                                           |
| --------------------- | -------- | ------------ | ----------------------------------------------------------------------------------------------------- |
| variant_annotation_id | Yes      | integer      | A unique identifier for each variant annotation event                                                 |
| variant_occurrence_id | Yes      | integer      | A foreign key identifier to the variant_occurrence table for which the variant annotation is recorded |
| annotation_field       | Yes      | varchar(MAX) | Categories or database name of annotation                                                             |
| value_as_string       | No       | varchar(MAX) | Annotation value as a data type of string                                                             |
| value_as_number       | No       | float        | ANnotation value as a data type of number 
