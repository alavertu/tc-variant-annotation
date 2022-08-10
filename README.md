# Variant Annotation Technical Challenge
### Original Author: Reece Hart @ MyOme
# Variant Annotation Technical Challenge

## Introduction

The goal for this technical interview is to understand how a candidate approaches a software development task.  We will discuss a project to annotate biological sequence variants using a single remote REST service.  No prior knowledge of bioinformatics or sequence variation is necessary.  This challenge provides an opportunity to discuss the candidate's experience with variant annotation (not required), code architecture, coding skills and style.

There is no right answer to this challenge. Given the breadth of
relevant topics and skills, it is unlikely that candidates will be
able to address all discussion areas and implementation
considerations.  That's okay!

## Overall project goal

Our goal is to annotate variants using Ensembl, a public annotation service.  We will write a command line interface that accepts a file with a list of
variants, makes the REST call to Ensembl, receives the JSON reply, and outputs a TSV file with selected annotations.

An example curl command line and reply are at the bottom of this README, and a  file of variants is included with this repo.

Your code should be executed with  a command line like `annotate-variants <variants.txt> annotations.tsv`.  The result should be a TSV file with these columns: `input variant`, `assembly name`,  `seq_region_name`, `start`, `end`, `most_severe_consequnece`,   `strand`, and `genes`. The `genes` column should be a list of the unique `gene_symbol` field from `transcript_consequences`.

## In-Person Interview Discussion

### Sequences and sequence variation

* How are sequences and sequence variants represented?
* What kinds of variant annotation and sources is the candidate aware
  of? How are they distributed?
* How can we search for annotations at these? What constitutes a
  matching variant? What challenges exist with matching variants?

### Code planning and architecture

* How would you structure this code?  What questions occur to you when
  thinking about code structure?  What Python packages or modules would you use? 
* The proposed annotation system uses data from a remote REST
  service.  What factors or concerns occur to you when using an external service
  in a production clinical setting?
* Assume that the input is messy: variants might be duplicated, semantically
  invalid, syntactically invalid, or perhaps not variants at all.  How would you handle erroneous variants?
* Ensembl is one of many annotation sources. Suppose we now want to write an internal microservice that aggregates annotations from multiple sources. What
  standards and tools might we use to implement it? How does this change your code structure?  

## Take-home challenge

* You will be invited to <https://github.com/reece/tc-variant-annotation/>.  This is a template repository.  Make a new repo under your own username using this repo as a template.
* Invite me as a collaborator. 
* Create a new branch for your work.
* Write a Python package that implements the functionality discussed above. Restructure the repo consistent with Python coding conventions.
* Demonstrate your interface using the variants.txt file included in
  this repo.  Please include the full output, errors, warnings and
  all.  The `variants.txt` is exactly as intended.
* Write tests using pytest. Your tests do not need to be exhaustive.
* Use conventional Python code structure and create a package from which you can build a Python wheel.  You may restructure the repo files in any way you see fit.
* Create a new file, `CHALLENGE-RESPONSE.md`, that describes how to set up your code, demonstrates the command line use, and provides any additional discussion or explanation that you think is appropriate.
* Create a PR for your branch. Invite me (github user `reece`) to
  review it. Thanks!

## Questions

* Suppose we now want to create a web microservice that accepts a GET
  (or POST) request with the variant in HGVS format and returns the
  annotation as JSON.  What tools or standards would you implement
  this?  How does your code structure change?
* Start-ups and start-up employees must balance quick and satisfactory
  (i.e., good enough) results with more deliberate and reliable
  results. The same is true for code.  You do not have enough time to
  write the ultimate answer to variant annotation. What is important
  to get "right" now and what can be deferred.
* What's the simplest method you can think of to handle cases of
  duplicated variants in the input?
* What optimizations would you pursue in your code if you had time?
  How would you prioritize your effort?

## Ensembl Annotation Sources

An example Ensembl query and edited response is below.  **N.B. Ensembl
has two certs, one of which is old and leads to random failures. Use
http (not https) for now.** The response is also in
`ensembl-sample-response.json`.
  
    $ curl -H "Content-type:application/json" 'http://rest.ensembl.org/vep/human/hgvs/NC_000006.12:g.152387156G>A'
    [
      {
          "allele_string": "G/A",
          "assembly_name": "GRCh38",
          "end": 152387156,
          "most_severe_consequence": "synonymous_variant",
          "seq_region_name": "6",
          "start": 152387156,
          "strand": 1,
          "transcript_consequences": [
            {
                "amino_acids": "Y",
                "biotype": "protein_coding",
                "cdna_end": 8900,
                "cdna_start": 8900,
                "cds_end": 8424,
                "cds_start": 8424,
                "codons": "taC/taT",
                "consequence_terms": [
                  "synonymous_variant"
                ],
                "gene_id": "ENSG00000131018",
                "gene_symbol": "SYNE1",
                "gene_symbol_source": "HGNC",
                "hgnc_id": "HGNC:17089",
                "impact": "LOW",
                "protein_end": 2808,
                "protein_start": 2808,
                "strand": -1,
                "transcript_id": "ENST00000423061",
                "variant_allele": "A"
            },
            {
                "biotype": "retained_intron",
                "cdna_end": 8621,
                "cdna_start": 8621,
                "consequence_terms": [
                  "non_coding_transcript_exon_variant"
                ],
                "gene_id": "ENSG00000131018",
                "gene_symbol": "SYNE1",
                "gene_symbol_source": "HGNC",
                "hgnc_id": "HGNC:17089",
                "impact": "MODIFIER",
                "strand": -1,
                "transcript_id": "ENST00000461872",
                "variant_allele": "A"
            }
          ]
      }
    ]
