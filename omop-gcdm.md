```mermaid
---
title: Genomic-CDM Specification for OMOP Common Data Model 5.4
---
%%{init: {
 "theme": "default",
 "themeCSS": [
  ".er.relationshipLabel { fill: black; }", 
  ".er.relationshipLabelBox { fill: white; }", 
  ".er.entityBox { fill: lightgray; }",
  "[id^=entity-genomic_test] .er.entityBox { fill: lightgreen; }",
  "[id^=entity-target_gene] .er.entityBox { fill: powderblue; }",
  "[id^=entity-variant_occurrence] .er.entityBox { fill: pink; }",
  "[id^=entity-variant_annotation] .er.entityBox { fill: pink; }"
  ]
}}%%
erDiagram

note FROM OMOP CDM
person {
  integer person_id PK "NOT NULL, person_id"
  integer gender_concept_id FK "NOT NULL, gender_concept_id"
  integer year_of_birth "NOT NULL, year_of_birth"
  integer month_of_birth "NULL, month_of_birth"
  integer day_of_birth "NULL, day_of_birth"
  timestamp birth_datetime "NULL, birth_datetime"
  integer race_concept_id FK "NOT NULL, race_concept_id"
  integer ethnicity_concept_id FK "NOT NULL, ethnicity_concept_id"
  integer location_id FK "NULL, location_id"
  integer provider_id FK "NULL, provider_id"
  integer care_site_id FK "NULL, care_site_id"
  varchar(50) person_source_value "NULL, person_source_value"
  varchar(50) gender_source_value "NULL, gender_source_value"
  integer gender_source_concept_id FK "NULL, gender_source_concept_id"
  varchar(50) race_source_value "NULL, race_source_value"
  integer race_source_concept_id FK "NULL, race_source_concept_id"
  varchar(50) ethnicity_source_value "NULL, ethnicity_source_value"
  integer ethnicity_source_concept_id FK "NULL, ethnicity_source_concept_id"
}

note FROM OMOP CDM
care_site {
  integer care_site_id PK "NOT NULL, care_site_id"
  varchar(255) care_site_name "NULL, care_site_name"
  integer place_of_service_concept_id "NULL, place_of_service_concept_id"
  integer location_id FK "NULL, location_id"
  varchar(50) care_site_source_value "NULL, care_site_source_value"
  varchar(50) place_of_service_source_value "NULL, place_of_service_source_value"
}

note FROM OMOP CDM
fact_relationship {
  integer domain_concept_id_1 "NOT NULL, domain_concept_id_1"
  integer fact_id_1 "NOT NULL, fact_id_1"
  integer domain_concept_id_2 "NOT NULL, domain_concept_id_2"
  integer fact_id_2 "NOT NULL, fact_id_2"
  integer relationship_concept_id "NOT NULL, relationship_concept_id"
}

note FROM OMOP CDM
specimen {
  integer specimen_id PK "NOT NULL, specimen_id"
  integer person_id FK "NOT NULL, person_id"
  integer specimen_concept_id FK "NOT NULL, specimen_concept_id"
  integer specimen_type_concept_id FK "NOT NULL, specimen_type_concept_id"
  date specimen_date "NOT NULL, specimen_date"
  timestamp specimen_datetime "NULL, specimen_datetime"
  numeric quantity "NULL, quantity"
  integer unit_concept_id FK "NULL, unit_concept_id"
  integer anatomic_site_concept_id FK "NULL, anatomic_site_concept_id"
  integer disease_status_concept_id FK "NULL, disease_status_concept_id"
  varchar(50) specimen_source_id FK "NULL, specimen_source_id"
  varchar(50) specimen_source_value "NULL, specimen_source_value"
  varchar(50) unit_source_value "NULL, unit_source_value"
  varchar(50) anatomic_site_source_value "NULL, anatomic_site_source_value"
  varchar(50) disease_status_source_value "NULL, disease_status_source_value"
}

note FROM OMOP CDM
procedure_occurrence {
  integer procedure_occurrence_id PK "NOT NULL, procedure_occurrence_id"
  integer person_id FK "NOT NULL, person_id"
  integer procedure_concept_id FK "NOT NULL, procedure_concept_id"
  date procedure_date "NOT NULL, procedure_date"
  timestamp procedure_datetime "NULL, procedure_datetime"
  date procedure_end_date "NULL, procedure_end_date"
  timestamp procedure_end_datetime "NULL, procedure_end_datetime"
  integer procedure_type_concept_id FK "NOT NULL, procedure_type_concept_id"
  integer modifier_concept_id FK "NULL, modifier_concept_id"
  integer quantity "NULL, quantity"
  integer provider_id FK "NULL, provider_id"
  integer visit_occurrence_id FK "NULL, visit_occurrence_id"
  integer visit_detail_id FK "NULL, visit_detail_id"
  varchar(50) procedure_source_value "NULL, procedure_source_value"
  integer procedure_source_concept_id FK "NULL, procedure_source_concept_id"
  varchar(50) modifier_source_value "NULL, modifier_source_value"
}

note FROM OMOP CDM
condition_occurrence {
  integer condition_occurrence_id PK "NOT NULL, condition_occurrence_id"
  integer person_id FK "NOT NULL, person_id"
  integer condition_concept_id FK "NOT NULL, condition_concept_id"
  date condition_start_date "NOT NULL, condition_start_date"
  timestamp condition_start_datetime "NULL, condition_start_datetime"
  date condition_end_date "NULL, condition_end_date"
  timestamp condition_end_datetime "NULL, condition_end_datetime"
  integer condition_type_concept_id FK "NOT NULL, condition_type_concept_id"
  integer condition_status_concept_id FK "NULL, condition_status_concept_id"
  varchar(20) stop_reason "NULL, stop_reason"
  integer provider_id FK "NULL, provider_id"
  integer visit_occurrence_id FK "NULL, visit_occurrence_id"
  integer visit_detail_id FK "NULL, visit_detail_id"
  varchar(50) condition_source_value "NULL, condition_source_value"
  integer condition_source_concept_id FK "NULL, condition_source_concept_id"
  varchar(50) condition_status_source_value "NULL, condition_status_source_value"
}

genomic_test{
  integer genomic_test_id PK "NOT NULL, A unique identifier for each platform"
  integer care_site_id  FK "NOT NULL, A foreign key to the site of primary care in the care_site table, where the details of the care site are stored"
  varchar(255) genomic_test_name "NULL, Information about the name of platform using sequencing assigned by institution"
  varchar(50) genomic_test_version "NULL, Information about the name of platform using sequencing maintained by institution"
  varchar(50) reference_genome "NULL, Information about the Reference genome used to sequencing analysis"
  varchar(50) sequencing_device "NULL, Sequencer machine information"
  varchar(50) library_preparation "NULL, Information about the preparation method for the sequencing library"
  varchar(50) target_capture "NULL, Information about the capture method of examined and targeted region"
  varchar(50) read_type "NULL, Information about the method of sequence reading"
  integer read_length "NULL, Information about the length of read"
  varchar(255) quality_control_tools "NULL, Information about the tool used to control quality"
  integer total_reads "NULL, Total count of reads involved in assembly"
  float mean_target_coverage "NULL, Mean target coverage"
  float per_target_base_cover_100x "NULL Percentage of selected bases"
  varchar(255) alignment_tools "NULL, Information about the name and version of the alignment tool"
  varchar(255) variant_calling_tools "NULL, Information about the name and version of variant calling tool"
  varchar(255) chromosome_corrdinate "NULL, Coordinated system for numbering the chromosomes"
  varchar(255) annotation_tools "NULL, Information about the tool used for annotation"
  varchar(255) annotation_databases "NULL, Information about the database for annotation"
}

target_gene{
  integer target_gene_id PK "NOT NULL, A system-generated unique identifier for each target region"
  integer genomic_test_id FK "NOT NULL, A foreign key identifier to the platform containing the target region. The details of that platform are stored in the Platform_info table"
  varchar(50) hgnc_id "NOT NULL, Gene ID based on HGNC nomenclature"
  varchar(50) hgnc_symbol	"NOT NULL, Gene Symbol given by HGNC nomenclature"
  %%varchar(50) chromosome_id "NOT NULL, Chromosome ID"
  %%integer start_position "NOT NULL, start position"
  %%integer end_position "NOT NULL, end position"
}

variant_occurrence{
  integer variant_occurrence_id PK "NOT NULL, A unique identifier for each variant occurrence event"
  integer procedure_occurrence_id FK "NOT NULL, A foreign key identifier to the Procedure_occurrence table for the procedure used to obtain the specimen"
  integer specimen_id FK "NOT NULL, Tumor specimen ID"
  integer reference_specimen_id FK "NULL, ID of the normal specimen related to the tumor specimen"
  varchar(50) target_gene1_id FK "NULL, A foreign key identifier to the Target_gene table for which the variant information is recorded"
  varchar(255) target_gene1_symbol "NULL, A symbol of gene where the variant information is recorded"
  varchar(50) target_gene2_id FK "NULL, A foreign key identifier to the Target_gene table for which the variant information is recorded when a translocation variant occurs"
  varchar(255) target_gene2_symbol "NULL, A symbol of gene where the variant information is recorded"
  varchar(50) reference_sequence "NULL, Transcript ID based on a protein-coding RNA (mRNA) made up of the accession number and version number"
  varchar(50) rs_id "NULL, dbSNP reference ID (rsID) maintained by NCBI"
  varchar(255) reference_allele "NULL, Reference allele sequence (e.g., A)"
  varchar(255) alternate_allele "NULL, Variant allele sequence (e.g., C)"
  varchar(MAX) hgvs_c "NULL, Nomenclature for the sequence variant at the DNA level"
  varchar(MAX) hgvs_p "NULL, Nomenclature for the sequence variant at the protein level"
  integer variant_read_depth "NULL, Variant depth divided by read depth"
  integer variant_exon_number "NULL, Exon number in which the variant occurred"
  float copy_number "NULL, Copy number value for CNV data"
  varchar(MAX) cnv_locus "NULL, Locus information for CNV"
  varchar(MAX) fusion_breakpoint "NULL, sequential position that fusion variant occurred"
  integer fusion_supporting_reads "NULL, Supporting read count of the fusion"
  varchar(MAX) sequence_alteration "NULL, Structural variant type"
  varchar(MAX) variant_feature "NULL, Functional variant type"
  varchar(50) genetic_origin "NULL, Somatic or germline origin or the variant"
  varchar(50) genotype "NULL, Allele state"
}

variant_annotation{
  integer variant_annotation_id PK "NOT NULL, A unique identifier for each variant annotation event"
  integer variant_occurrence_id FK "NOT NULL, A foreign key identifier to the variant_occurrence table for which the variant annotation is recorded"
  varchar(MAX) annotation_field "NOT NULL, Categories or database name of annotation"
  varchar(MAX) value_as_string "NULL, Annotation value as a data type of string"
  float value_as_number "NULL, ANnotation value as a data type of number"
  %% varchar(MAX) annotation_database "NOT NULL, Categories or database name of annotation"
  %% varchar(MAX) variant_origin "NULL, Annotation value as a data type of string"
  %% varchar(MAX) variant_pathogeny "NULL, ANnotation value as a data type of number"
  %% varchar(MAX) variant_class_level "NULL, ANnotation value as a data type of number"
  %% varchar(MAX) variant_tier_level "NULL, ANnotation value as a data type of number"
  %% float allele_frequency "NULL, ANnotation value as a data type of number"
  %% varchar(MAX) medication "NULL, ANnotation value as a data type of number"
  %% varchar(MAX) clinical_trial_information "NULL, Annotation value as a data type of number"
}


person ||--o{ condition_occurrence : "person.person_id condition_occurrence.person_id" 
person ||--o{ procedure_occurrence : "person.person_id procedure_occurrence.person_id" 
person ||--o{ specimen : "person.person_id specimen.person_id" 
genomic_test ||--o{ care_site : "genomic_test.care_site_id care_site.care_site_id" 
variant_occurrence ||--o{ procedure_occurrence : "variant_occurrence.procedure_occurrence_id procedure_occurrence.procedure_occurrence_id" 
variant_occurrence ||--o{ specimen : "variant_occurrence.specimen_id specimen.specimen_id" 
variant_occurrence ||--o{ specimen : "variant_occurrence.reference_specimen_id specimen.specimen_id" 

fact_relationship ||--o{ procedure_occurrence : "fact_relationship.fact_id_1 procedure_occurrence.procedure_occurrence_id"
fact_relationship ||--o{ specimen : "fact_relationship.fact_id_2 specimen.specimen_id"

genomic_test ||--o{ target_gene : "genomic_test.genomic_test_id target_gene.genomic_test_id"
target_gene ||--o{ variant_occurrence : "target_gene.target_gene_id variant_occurrence.target_gene1_id"
target_gene ||--o{ variant_occurrence : "target_gene.target_gene_id variant_occurrence.target_gene2_id"
variant_occurrence ||--o{ variant_annotation : "variant_occurrence.variant_occurrence_id variant_annotation.variant_occurrence_id"
```