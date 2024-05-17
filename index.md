---
layout: entry
title: Documentation for Physical Protein Interaction Relationship Annotation of the ComplexTome corpus and trigger word annotation
---

## Relationship Annotation

### General guidelines

* Annotations should be made according to the annotator’s best understanding of the __author’s intended meaning in context__. For example, relations expressed using ambiguous verbs such as __"associate"__ that express complex formation in some contexts but not others should be annotated if and only if the annotator interprets the authors as intending to describe complex formation. The annotators should only use the text excerpt they have available to make this judgement.
* Annotators should treat all named entities as being __masked__. __Masked__ is a term adopted from large language model training, and it means that the entities should be treated as if they are not visible, but the annotators knows their place in text (e.g. _mutations in **p53** have been associated with lung cancer_ should be treated as _mutations in **\[MASK\]** have been associated with lung cancer_).  This means that annotators shouldn't annotate relationships between entities just based on their names, when they would be unable to make the same annotations for two other entities. 

### Complex formation definition

Undirected binary relation associating two proteins that form a complex. Annotated for any statement implying the existence of a complex, including statements explicitly discussing the dissociation of a complex.
Relevant gene ontology terms:
* [GO:0065003](http://amigo.geneontology.org/amigo/term/GO:0065003) (__protein-containing complex assembly__): The aggregation, arrangement and bonding together of a set of macromolecules to form a protein-containing complex.
* [GO:0032984](http://amigo.geneontology.org/amigo/term/GO:0032984) (__protein-containing complex disassembly__): The disaggregation of a protein-containing macromolecular complex into its constituent components.
* [GO:0032991](http://amigo.geneontology.org/amigo/term/GO:0032991) (__protein-containing complex__): A stable assembly of two or more macromolecules, i.e. proteins, nucleic acids, carbohydrates or lipids, in which at least one component is a protein and the constituent parts function together.

Note that by contrast to the scope of [GO:0032991](http://amigo.geneontology.org/amigo/term/GO:0032991) (__protein-containing complex__) and related terms, the annotated complex formation relation is restricted to cases where both of the associated constituents are _proteins_, _protein complexes_, _protein families_, _groups of proteins_ or _chemicals_.

#### Detailed guidelines

1. Complex formation relations can be annotated between two different protein mentions, but also between the same mentions, when the masked entities could be viewed as two different entities. However, statements such as “homodimerization of A” __are not annotated__ as _Complex formation_, since self-loops are not annotated in the corpus. 
2. Complexes of more than two proteins are annotated by creating __all binary relations__ between the components.
3. Nominalized expressions (“interaction of A and B”, “A/B interaction”, "A:B complex") and noun phrases with __any surface word__ that can be understood as implying the existence of a complex (“A/B complex”, “A/B heterodimer”) are __annotated__ as expressing complex formation relations. However, __in the absence of any such word__, text such as “A/B” is not annotated. The text A-B will be annotated based on the understanding of the annotator from the entire context (abstract or paragraph) and not based on former biological knowledge. 
~~~ ann
direct inhibition of NFATp/AP-1 complex formation by a nuclear hormone receptor
T1	GGP 21 26	NFATp
T2	Complex 27 31	AP-1
R1	Complex_formation Arg1:T1 Arg2:T2	 
~~~
4. Relations __should not be interpreted as combinations__, on the contrary each annotated relation should be __valid on each own__.
5. __Co-immunoprecipitation__ can be used as an indicator of complex formation between two NE mentions.
6. Post-translational modifications should __not__ receive a binding annotation unless binding is clearly mentioned in context. PTMs imply transient interactions which will not be present in physical interaction databases, so they shouldn't be annotated as such. For an example of a corner case see [Specific examples](#specific-examples-discussed)
7. The following are generally understood as implying _Complex formation_:
  * consitutive association
  * stable association
8. The following are generally understood as __NOT__ implying _Complex formation_:
  * synergize
  * stabilize
9. Incorporation of a small molecule/protein congugate to a Protein (i.e. a Post-translational modification) is __Out-of-scope__ and should not be annotated as __Complex formation__
10. If __part of a protein/complex__ has the ability to __form a complex__, then the ability of the entire protein/complex to do the same can be extrapolated from that. 
11. Subcellular localization is not annotated for _Complex formation_ even if the structure is made of proteins.
12. When an entity is a substrate of another entity then the relation connecting them is __Catalysis of protein modification__ and not __Complex formation__. Thus no annotation is added in such cases.
13. Synthetic lethal interactions are genetic and thus are __NOT__ annotated as Complex formation. 
14. __Chemicals__ COVALENTLY bound to other entities are __NOT__ annotated as __Complex formation__, since complex formation is non-covalent interactions.
15. Orientation of _Protein A_ relatively to _Protein B_ is not enough cue to annotate __Complex formation__
e.g. from [19858358](https://pubmed.ncbi.nlm.nih.gov/19858358/)
~~~ann
Orientation of palmitoylated CaVbeta2a relative to CaV2.2
T1	Protein 29 38	CaVbeta2a
T2	Complex 51 57	CaV2.2
~~~
16. Proteoforms (e.g. proteins with PTMs, or isoforms), should receive annotations as if they were the main isoform/unmodified protein
e.g. from [19015234](https://pubmed.ncbi.nlm.nih.gov/19015234/)
~~~ann
CDK7 binds preferentially to the SUMOylation-deficient form of SF-1
T1	Protein 0 4	CDK7
T2	Protein 63 67	SF-1
R1	Complex_formation Arg1:T1 Arg2:T2
~~~
17. __Complex formation__ should be annotated when a __Chemical__ binds to any other entity (__Protein__, __Family__ or __Complex__) unless it is clearly stated that the bond is covalent (either by the fact that it is a post-translational modification or covalently bound is mentioned in the text). 
18. The interactions between members of __transient intermediate complexes__ as part of catalytic reactions should __NOT__ be annotated neither between Protein-Protein (e.g. kinase-substrate), nor between Protein-Chemical entities.
19. __Chemical A modulates, inhibits, acts as an agonist/antagonist for Protein B__: A __Complex formation__ relationship between A and B should be annotated (this rule applies mostly to drugs.)

#### Negation and speculation

1. Statements explicitly __denying__ the formation of a complex (e.g. “A does not bind B”) are __not annotated__ in any way. However, if the negated statement is qualified with conditions in a way that implies that the proteins would normally form a complex, the statement is annotated as if the negation were absent (e.g. _“When A is phosphorylated, it fails to form a complex with B”_).
2. Statements expressed __speculatively__ or with __hedging__ expressions (e.g. _“may form a complex”_) are __annotated__ identically to affirmative statements (in effect, __speculation and hedging are ignored__).

#### Named Entity annotation rules

1. Entity name mentions like _ubiquitin_ or reporter genes (e.g. _GFP_) which are _GGPs_ but are in the blocklist of our NER system, will be assigned the __blocklisted__ attribute (see next section)
2. Histones: 
  * Tag _H2_, _H3_ etc. when they appear standalone
  * Include _histone_ in the span when it appears with one of the names (e.g. _histone H3_)
  * Tag _histone_ as __Protein family or group__ when it appears standalone.
  * We could then either go discontinuous or decomposed for mentions such as _histones H2A and H3_.
  * Methylated histones are also tagged as __GGP__ even though our NER system will not detect them 
3. _Amino acid residues_ should not be annotated as __Chemical__ when they are part of a polypeptide chain
4. _Glycosylphosphatidylinosiol_ (GPI) should not be annotated as __Chemical__ as it cannot be a standalone chemical
5. Determiners like _the_ should not be included in the entity span of __GGP__, __Protein-containing complex__ and __Protein family or group__
6. _Domains_ and other _protein regions_ should __NOT__ be annotated as _GGP_.
7. In order for the annotated text to be as close as possible to the ideal NE annotation produced by the NER system, cases where only part-of __mutant names__ are standalone entities, only these mentions should be annotated, e.g. __sam35__ and __NOT__ _sam35-2_ is annotated as a GGP in the following example
~~~ann
The essential protein Sam35 was addressed through use of the temperature-sensitive yeast mutant sam35-2.
T1	GGP 22 27	Sam35
T2	GGP 96 101	sam35
~~~
An exception is when __mutant names are a single word__, and then they are annotated as one mutant entity e.g. __rex1Delta__ in the following sentence:
~~~ann
However, both the rex1Delta strain and the rex1-1 strain are indistinguishable from wild type.
T1	GGP 18 27	rex1Delta
T2	GGP 43 47	rex1
~~~
8. Named entities that are part of antibodies should be annotated as the corresponding NE type and should receive a _Note: antibody_.
9. rRNAs and tRNAs are currently annotated as __GGP__ with __noncoding__ attribute.
10. __Fusion proteins__ should be treated as two entities for the purposes of annotation and during the creation of the training dataset. These should get an _Entity Attribute_: __Fusion__. The reporter protein in fusion should get an attribute: __blocklisted__ if it is not detect by tagger. E.g. in the example below __NRIF3__ will receive an _Entity Attribute_: __Fusion__ and __Gal4__ will receive an _Entity Attribute_: __Fusion__ _Entity Attribute_: __Blocklisted__:
~~~ ann
full-length NRIF3 fused to the DNA-binding domain of Gal4
T1	GGP 12 17	NRIF3
T2	GGP 53 57	Gal4
~~~
11. _FLAG_ and _6xHis_ are polypeptide protein tags and should receive an _OOS_ annotation, or should not be annotated at all.
12. _ATP_ and _ADP_ are annotated as __OOS__.
13. _GTP_ and _GDP_ are annotated as __Chemicals__ due to their function in protein signalling.

#### Named Entity Attributes

There are 5 Named Entity (NE) attributes in the corpus:
1. __Mutant__: used to mark NEs that are mutated forms or mutants of the annotated entity
2. __Fusion__: used to mark NEs which are part of fusion proteins
3. __Non-coding__: used as an attribute for GGPs to denote functional non-coding RNA molecules (e.g. transfer RNA, microRNA, piRNA, ribosomal RNA, and regulatory RNAs) among others.
4. __Small protein post-translation modification__: used as an attribute to denote GGPs that are covalently attached to other proteins as a result of a post-translational modification (e.g. ubiquitin, SUMO)
5. __Blocklisted__: used to denote NEs that belong to one of the annotated NE types, but which are not detected by our [dictionary-based NER system](https://www.biorxiv.org/content/10.1101/067132v1), since they are part of its blocklist. 

#### Specific rules for complexes/families and plural form annotations

* If a term is in Gene Ontology and is assigned a [__Protein-containing complex__](http://amigo.geneontology.org/amigo/term/GO:0032991) annotation then it is considered a Complex in this annotation effort.
* If a term is found in Gene ontology but it is NOT a __protein-containing complex__, then it will __NOT__ be considered a _Complex_ in this effort
* If a term is not at all present in Gene Ontology then other resources in the field will be used to decide whether it should be considered a _Complex_ or not (e.g. [Complex Portal](https://www.ebi.ac.uk/complexportal/home), [Reactome](https://reactome.org/)).
* There is no clear distinction in Gene Ontology between small (e.g. NF-kappaB) and large (e.g. Nuclear Pore) complexes and for this reason, all these complexes will be treated the same and receive a _Complex_ annotation
* For cases where it is difficult to distinguish [_family_](pfam.xfam.org/family/PF00110#tabview=tab6) from [_domain_](pfam.xfam.org/family/PF05207#tabview=tab6) mentions, the field type in Pfam could be used to aid in making a decision (if available)
* The words _“complex”_, _“family”_ and _"group"_ should __not__ be part of the entity annotations.
* Annotations should be applied to all variants of a name: e.g. __NF kappaB__, __NF-kappaB__, __NFkappaB__ should all be marked as __Protein-containing complex__

## Trigger word annotation

### General guidelines

* When annotators have already identified a __Complex formation__ relationship in text, it is possible to also annotate the specific word(s) which led them to make this annotation. The words that allow their interpretation of a relationship as __Complex formation__ are called trigger words. An example of a trigger word annotation is shown below:
~~~ann
CDK7 binds to SF-1
T1	Protein 0 4	CDK7
T2	Protein 14 18	SF-1
T3	Trigger 5 10	binds
~~~
* If two or more trigger words were considered as equivalently valid they will all be annotated.
~~~ann
The CD40-TRAF2 interaction
T1	Protein 4 8	CD40
T2	Protein 9 14	TRAF2
T3	Trigger 8 9	-
T4	Trigger 14 26	interaction
~~~
* If a trigger word is discontinuous, all the constituents of the trigger words will be annotated.
~~~ann
A two-hybrid screen implicated PAK1 as an OSR1 target.
T1	Protein 31 35	PAK1
T2	Protein 42 46	OSR1
T3	Trigger 2 19	two-hybrid screen
T4	Trigger 47 53	target
~~~

## Annotation process

The first step towards the annotation of ComplexTome was Named Entity (NE) annotation. Both annotators annotated the four NE types mentioned above (Protein, Chemical, Complex, and Family using the [Named Entity annotation rules](#named-entity-annotation-rules) above. 
We used an automated [dictionary-based NER system](https://ceur-ws.org/Vol-1747/BIT102_ICBO2016.pdf) to assist the manual annotation process. The system detects Protein names in text and helps speed up the process of NE annotation. Since, the system doesn't have a perfect precision or recall, the annotators manually corrected any errors produced by the NER system, and added annotations for all the aforementioned NE types. It is important to note the main objective of this study does not revolve around developing a corpus specifically for biomedical NER. Therefore, no Inter-Annotator Agreement (IAA) measurements are reported for this specific task, in contrast to the evaluation of relations. For the same reasons, no NE normalization has been performed.

After the annotation of NEs in all documents was complete, the annotators could proceed to relation annotation to calculate inter-annotator agreement. No changes to NE annotations were performed at this stage to allow for correct evaluation statistic measurements. 
The annotators were provided with 20 common documents for each round of IAA calculations. In the first round of IAA, the annotators were provided with 20 documents from the [BioNLP ST 2009 development set](https://github.com/spyysalo/binarize-events/tree/master/data/bionlp09_shared_task_development_data_rev1) and an initial set of guidelines corresponding to the [General guidelines](#general-guidelines) and [Complex formation definition](#complex-formation-definition) guidelines in this document. They were then asked to annotate to the best of their ability and note any more rules they think are relevant. 

We quantified the Inter-Annotator-Agreement (IAA) by calculating Cohen’s kappa for the pair of annotators. Cohen’s kappa is defined as kappa = (Po – Pe)/(1-Pe). Po refers to the observed probability of agreement between two annotators, whereas Pe is the expected probability of agreement by random chance. For the first round of IAA Cohen's kappa was &#954 = 0.90. 

The annotators reviewed their disagreements which led to updating the [detailed guidelines](#detailed-guidelines) section to ensure their agreement during the next rounds of IAA and the corpus annotation. 

After updating the guidelines the annotators were provided with an additional set of 20 documents from the BioNLP ST 2009 development set and their IAA was calculated again and was found to be &#954 = 0.93. 

After further updates to the [detailed guidelines](#detailed-guidelines) the annotators moved to the third round of IAA. This time they were provided with 30 documents from the 400 full-text excerpts extracted from resources enriched in positive relationships (see our [paper](https://www.biorxiv.org/content/10.1101/2023.12.10.570999v2) for more details). IAA was calculated (&#954 = 0.92) in this different set of documents and was found to be high. Annotators revisited their disagreements and updated the guidelines. 

Since, in the previous three rounds of IAA, &#954 was always found to be very high, we decided to have one final round if its value was above 0.9. This time 20 documents from the 400 abstracts extracted from resources enriched in positive relationships were selected and annotated by both annotators. Their agreement was still very high (&kappa = 0.91) and that concluded the IAA process.

During the entire period of corpus annotation, the two annotators remained in contact and had regular meetings to discuss difficult cases, when they encountered them. 


## Setting up a BRAT server

The [Zenodo project associated with ComplexTome](https://zenodo.org/doi/10.5281/zenodo.11198826) contains both [ComplexTome in BRAT format](https://zenodo.org/api/files/72327c8e-b14b-4d83-a08c-ab57cc24ec6f/ComplexTome.tar.gz) and the [trigger word corpus in BRAT format](https://zenodo.org/api/records/11198826/files/trigger_word_corpus.tar.gz/content) as well as test set predictions for both [ComplexTome](https://zenodo.org/api/records/11198826/files/test-set-predictions-RE.tar.gz/content) and the [trigger word corpus](https://zenodo.org/api/records/11198826/files/test-set-predictions-trigger.tar.gz/content). You can view all these files by installing BRAT, following these [instructions](https://brat.nlplab.org/installation.html). Then you can download and copy all the data hosted in Zenodo, using the links provided above, and add them in the `data` directory inside the directory where you installed BRAT. For more details on how to use BRAT please look at the [manual page](https://brat.nlplab.org/manual.html).


For information on Annodoc, see <http://spyysalo.github.io/annodoc/>.
