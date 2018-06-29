# Error evaluation

## Analysis

In total 40 sentences were annotated. These sentences were manually analysed for the correctness
of the annotations. The model used was trained on 275 sentences, which were also manually
annotated. This was done through a custom NER model built on *spaCy*. It uses the default English
POS tagger and dependency parser. The model was trained with word embeddings extracted from
Pubmed, which is similar to the origin of the sentences in the training set.

The model is trained with the following entities:

* BRANDNAME
* COMPANY
* PRODUCT

##### BRANDNAME
A *BRANDNAME* is the name of the product. For example, a digital camera produced by
Leica carries the *BRANDNAME* **Leica DFC 290 HD**.

##### COMPANY
A *COMPANY* is the name of the company that produced a *PRODUCT*. The previously mentioned
*PRODUCT* digital camera is produced by **Leica**.

##### PRODUCT
A *PRODUCT* is the name used to identify types of *PRODUCT*s. The previously mentioned
*PRODUCT* with *BRANDNAME* Leica DFC 290 HD is a **digital camera**.

### Observations

*(Within this document there will be referred to **entity spans**. A span in this context refers
to the collection of token that make up the entire entity)*

Out of the 40 annotated sentences only four were completely correct (003, 012, 013, 037). All of these
sentences contained exclusively *COMPANY*'s. Three of the four sentences contained only one
entity. The other sentence contained two entities, however these were identical.

The 36 remaining sentences were incorrect for various reasons. While still labeling 66 entity spans
correctly, there were other errors within the sentences. A very common reason is incomplete
annotations. In only eleven cases the model was able to find all entities, however other
mistakes occurred leading to an incorrect annotation either way.

Below, a number of patterns that have been identified will be described in more depth. Based on
these patterns a number of error classes have been defined.


### Not an entity (16 cases)
On occasion the model mislabels a span as an entity, when in fact it is not. There are two main
reason that can be identified for mislabeling. The first reason is organisational units. The name
of a specific division of a company or university closely resembles a *COMPANY* name. It is easy
to see why the model would mislabel these spans. Another common reason for mislabeling is names
of biomedical substances, chemicals, genes and proteins. These are generally short names including
abbreviations. The same structure of naming is used for a lot of *BRANDNAME*s.

A less common reason for mislabeling is when a *PRODUCT* is mentioned as part of an adjective. For
example in sentence [005](###005), where the model labeled the span '**computer driven**' as a product.


### Wrong label (20 cases)
The model has found 20 correct entity spans, but did not manage to label them correctly. In the
majority of the cases a *BRANDNAME* was labeled as a *COMPANY*. Only in four occasions there was
a different mislabel. A common reason appears to be the mention of the *COMPANY* within the *BRANDNAME*
entity span. In sentence [008](###008) for example the model labeled the span '**Olympus BX40**' as
a *COMPANY*.


### Partial matches (24 cases)
The most common error on labels is partial matching of entity spans. A partial error occurs when the model was able to identify
the correct label of an entity but was only able to match parts of the complete span, or included
tokens from outside the complete span.

In sentence [015](###015) the following span is labeled as a *BRANDNAME*: '**ALV-5000/ EPP static**'.
The correct span should exclude the token 'static'. In this case the model is close to correctly
identifying the correct span. Another common error is the exclusion of certain tokens, which should
be part of the complete span. In sentence [021](###021) for example, there is a partial match on the
span '**ASYS Hitech**, GmbH'. The token GmbH, is excluded from the span. For this specific case
this almost always occurs when the corporate structure indicator is preceded by a ','. Other cases
of incomplete spans occur mostly on *PRODUCT*s, in this case it is hard to pinpoint one specific
reason. This also means it could be hard to improve this without significantly increasing the training
data.


### Missing labels (36 cases)
While not making an error in the entity label, there were also 36 cases of not finding an entity
at all. It is interesting to see that out of the 36 cases, 28 were *PRODUCT*s. Only three *COMPANY*s
were not identified as an entity. However, these all appeared in the same sentence [036](###036).
In this specific case all three mentions were from the *COMPANY* '**Sigma**', which is not the full
*COMPANY* name. The missing labels are hard to assign to one specific problem. The assumption here
is the lack of training data. Most missing labels contain words that are very domain specific and
are not part of the models vocabulary.


### Other errors
In sentence [001](###001) the model struggles with concatenation of the mention of two
different *BRANDNAME*s. the model identifies '**Roche Hitachi 912 and**' [BRANDNAME] 917
'**systems** [PRODUCT]'. In this case it is hard to recognize that the 917 refers to
the same Roche Hitachi system but with a different model or version number.




## Sentences

### 001
Serum concentrations of apoB and apoA1 were measured and determined by the **Roche Hitachi 912 and**
[BRANDNAME] 917 **systems** [PRODUCT] (**Roche Diagnostics** [COMPANY], GmbH, Mannheim, Germany),
respectively. VA was measured as serum all-trans-retinol at **BEVITAL AS** [COMPANY] (www.bevital.no)
using liquid chromatography-tandem mass spectrometry as described previously .

Tokens: 58 <br>
Avg token length: 4.810344827586207 <br>
Labeled entities: 4 <br>
Avg entity length: 13.75 <br>

##### Labeled

* Roche Hitachi 912 and | *INCORRECT* | *PARTIAL* | Two brand names are mentioned but the second one refers to token in the first mention.
* systems | *INCORRECT* | *NO ENTITY*
* Roche Diagnostics | *INCORRECT* | *PARTIAL* | GmbH is not included in the entity span.
* BEVITAL AS | *CORRECT*

##### Missed
* N/A

The model found all entities, but mislabeled 1 and only matched 2 partially.


### 002
Merck, Darmstadt, Germany), both containing 0.1% formic acid (**Fluka Chemie GmbH** [COMPANY], Buchs,
Switzerland). PAS concentrations were determined by *API 2000* tandem *mass spectrometer* (MS/MS)
(**Applied Biosystems** [COMPANY], **MDS Sciax** [COMPANY], Foster City, Canada) equipped with an
atmospheric turbulon *ionization chamber*.

Tokens: 58 <br>
Avg token length: 4.396551724137931<br>
Labeled entities: 3<br>
Avg entity length: 14.666666666666666<br>

##### Labeled
* Fluka Chemie GmbH | *CORRECT*
* Applied Biosystems | *CORRECT*
* MDS Sciax | *CORRECT*


##### Missed
* mass spectrometer  | PRODUCT
* API 2000  | BRANDNAME
* ionization chamber | PRODUCT.

The 3 entities found were all correct. However, it also mised 3 entities.


### 003 (CORRECT)
For testing the effect of lichen metabolite on dividing lymphocytes, PBMCs (1 × 10 5 /well) were
preactivated with 90 g/ml phytoemoagglutinin (**Remel Europe Ltd.** [COMPANY], Dartford, Kent, UK) for 24 h.

Tokens: 41<br>
Avg token length: 3.926829268292683<br>
Labeled entities: 1<br>
Avg entity length: 17.0<br>

##### Labeled
* Remel Europe Ltd. | *CORRECT*

##### Missed
* N/A

All entities correct


### 004
**A potentiostat** [PRODUCT] (**AUTOLAB PGSTAT 30** [BRANDNAME], **Eco Chemie BV** [COMPANY], Utrecht,
The Netherlands) was used to perform the cyclic potentiodynamic polarization curve measurements.

Tokens: 27<br>
Avg token length: 5.0<br>
Labeled entities: 3<br>
Avg entity length: 14.666666666666666<br>

##### Labeled
* A potentiostat | *INCORRECT* | *PARTIAL* | 'A' is included in the entity span.
* AUTOLAB PGSTAT 30 | *CORRECT*
* Eco Chemie BV | *CORRECT*

##### Missed
* N/A

Almost correctly labeled. Added 1 token to many to the product span


### 005
All electrochemical experiments were performed using a **computer driven** [PRODUCT]
**Autolab PGSTAT** [BRANDNAME] 30 *potentiostat* (**Ecochemie BV** [COMPANY], the Netherlands).

Tokens: 21<br>
Avg token length: 5.619047619047619<br>
Labeled entities: 3<br>
Avg entity length: 13.666666666666666<br>

##### Labeled
* computer driven | *INCORRECT* | *NO ENTITY* | the word computer is known as a device in several training sentences
* Autolab PGSTAT | *INCORRECT* | *PARTIAL* | '30' is not included in the entity span.
* Ecochemie BV | *CORRECT*

##### Missed
* potentiostat | PRODUCT


### 006
Lipoproteins were separated using fast protein liquid chromatography (FPLC) ( 21 ) on a
**Superose 6 10/300 GL *column*** [BRANDNAME] (**GE Healthcare Bio-Sciences AB** [COMPANY],
Uppasala, Sweden). Samples were chromatographed at a fl ow rate of 0.5 ml/min,
and fractions of 500 µl each were collected and assayed for TG.

Tokens: 61<br>
Avg token length: 3.9836065573770494<br>
Labeled entities: 2<br>
Avg entity length: 28.0<br>

##### Labeled
* Superose 6 10/300 GL column | *INCORRECT* | *PARTIAL* | '10/300 GL column' is included in the entity span.
* GE Healthcare Bio-Sciences AB | *CORRECT*

##### Missed
* column | PRODUCT


### 007
Concentrations of PlGF were determined in February 2012 according to manufacturer's
instructions using the **AutoDELFIA *assay*** [COMPANY] (**Perkin Elmer Wallac Oy** [COMPANY],
Turku, Finland). Intra-and interassay coefficients of variation (CV) were <1.8% and <3.7%
(PAPP-A), 1.8% (mean) and < 8.8% (hCG), 2.2% and <10.8% (hCG-h) and 4.8% and <8.8% (PlGF).

Tokens: 87<br>
Avg token length: 3.2413793103448274<br>
Labeled entities: 2<br>
Avg entity length: 19.0<br>

##### Labeled
* AutoDELFIA assay | *INCORRECT* | *PARTIAL, MISLABELED* | '10/300 GL column' is included in the entity span.
* Perkin Elmer Wallac Oy | *CORRECT*

##### Missed
* assay | PRODUCT


### 008
 From every specimen semiquantitative evaluation regarding every variable was performed in
 20 high power fields (hpfs) at a magnification of 3400 in a **light microscope** [PRODUCT]
 (**Olympus BX40** [COMPANY], **Olympus GmbH** [COMPANY], Hamburg, Germany).

Tokens: 39<br>
Avg token length: 4.717948717948718<br>
Labeled entities: 3<br>
Avg entity length: 13.333333333333334<br>

##### Labeled
* light microscope | *CORRECT*
* Olympus BX40 | *INCORRECT* | *MISLABELED*
* Olympus GmbH | *CORRECT*

##### Missed
* N/A

In this case Olympus is both part of the brandname as well as the company


### 009
Duplicate plasma sample of 100 μl was used to measure P-selectin concentration by a **quantitative
enzyme-** [PRODUCT]**linked immunoassay** [PRODUCT] (ELISA) technique using human soluble
P-selectin/CD62P *immunoassay* (R & D **Systems Europe** [COMPANY], Ltd., Abingdon, United kingdom).
The assays were performed on an automated **ELISA processor** [PRODUCT] (**Dade-Behring BEP** [COMPANY]
2000, Liederbach, Germany).

Tokens: 70<br>
Avg token length: 4.328571428571428<br>
Labeled entities: 5<br>
Avg entity length: 16.6<br>

##### Labeled
* quantitative enzyme- | *INCORRECT* | *PARTIAL* | 'quantitative' is included in the entity span, while 'linked immunoassay' is not.
* linked immunoassay | *INCORRECT* | *PARTIAL* | ' enzyme-' is not included in the entity span.
* Systems Europe | *INCORRECT* | *PARTIAL* | 'R & D' and 'Ltd.' is not included in the entity span.
* assays
* ELISA processor | *CORRECT*
* Dade-Behring BEP | *INCORRECT* | *PARTIAL* | 'BEP' is included in the entity span.

##### Missed
* immunoassay | PRODUCT
* BEP 2000  | BRANDNAME


### 010
Briefly, root tips or developing anthers were digested in 0.3% (w/v) cellulase **Onozuka R10** [COMPANY]
(**Apollo Scientific Ltd** [COMPANY], Stockport, Cheshire, UK), 0.3% (w/v) pectolyase **Y23** [PRODUCT]
(MP Biomedicals [COMPANY], Solon, Ohio, USA) and 0.3% (w/v) drieselase
(Sigma-Aldrich Company Ltd. [COMPANY], Poole, Dorset, UK) for 28 min and transferred to 1%
citrate buffer for 2 h.

Tokens: 86<br>
Avg token length: 3.2790697674418605<br>
Labeled entities: 5<br>
Avg entity length: 15.0<br>

##### Labeled
* Onozuka R10 | *INCORRECT* | *MISLABELED, NO ENTITY* | Due to capitalization it might look like a Company, but it is however just a name for a protein
* Apollo Scientific Ltd | *CORRECT*
* Y23 | *INCORRECT* | *NO ENTITY*
* MP Biomedicals|*CORRECT*
* Sigma-Aldrich Company Ltd. | *CORRECT*

##### Missed
* N/A


### 011
Specific IgE level was determined using the **ImmunoCap *test*** [PRODUCT] (**Phadia AB** [COMPANY],
Uppsala, Sweden) for all allergens which induced clinically significant skin prick test results.

Tokens: 29<br>
Avg token length: 4.9655172413793105<br>
Labeled entities: 2<br>
Avg entity length: 11.5<br>

##### Labeled
* ImmunoCap test | *INCORRECT* | *PARTIAL* | 'ImmunoCap' is included in the entity span.
* Phadia AB | *CORRECT*

##### Missed
* test


### 012 (CORRECT)
The anthocyanidin standards (cyanidin, pelargonidin, peonidin and delphinidin chloride) were
obtained from **Sigma-Aldrich Chemie B.V.** [COMPANY] (Zwijndrecht, the Netherlands).

Tokens: 28<br>
Avg token length: 5.25<br>
Labeled entities: 1<br>
Avg entity length: 25.0<br>

##### Labeled
* Sigma-Aldrich Chemie B.V. | *CORRECT*

##### Missed
* N/A


### 013 (CORRECT)
This study was sponsored and funded by **GlaxoSmithKline Biologicals SA** [COMPANY], Belgium.
**GlaxoSmithKline Biologicals SA** [COMPANY] was involved in all stages of the study conduct and
analysis, and also took charge of all costs associated with developing and publishing the manuscript.

Tokens: 43<br>
Avg token length: 5.255813953488372<br>
Labeled entities: 2<br>
Avg entity length: 30.0<br>

##### Labeled
* GlaxoSmithKline Biologicals SA | *CORRECT*
* GlaxoSmithKline Biologicals SA | *CORRECT*

##### Missed
* N/A

### 014
*Ehrenstorfer* (Augsburg, Germany), and **Witega Laboratorien Berlin-Adlershof GmbH** [COMPANY] (Berlin, Germany). The chemical structures of target analytes are shown in Fig. 1.

Tokens: 33<br>
Avg token length: 4.303030303030303<br>
Labeled entities: 1<br>
Avg entity length: 41.0<br>

##### Labeled
* Witega Laboratorien Berlin-Adlershof GmbH | *CORRECT*

##### Missed
* Ehrenstorfer

### 015
All light scattering measurements are carried out on an **ALV-5000/ EPP static** [BRANDNAME] and
**dynamic *light*** [PRODUCT] *scattering device* (**ALV-Gmbh** [COMPANY], Langen, Germany) with a 35 mW
He-Ne *laser* operating at wavelength k 5 632.8 nm (Uniphase).

Tokens: 45<br>
Avg token length: 4.022222222222222<br>
Labeled entities: 3<br>
Avg entity length: 13.666666666666666<br>

##### Labeled
* ALV-5000/ EPP static | *INCORRECT* | *PARTIAL* | 'static' is included in the entity span.
* dynamic light | *INCORRECT* | *NO ENTITY*
* ALV-Gmbh | *CORRECT*

##### Missed
* light scattering device | PRODUCT
* laser | PRODUCT


### 017
Peripheral blood mononuclear cells (PBMC) were isolated by **Ficoll Hypaque TM PLUS** [COMPANY]
(**GE Healthcare Bio-Sciences AB** [COMPANY], Uppsala, Sweden). CD4 + T cells were then isolated
from PBMCs by positive selection using **immunomagnetic beads** [PRODUCT] (**Miltenyi Biotec GmbH**
[COMPANY], **Bergisch Gladbach** [COMPANY], Germany) and the purity of **CD3 +** [BRANDNAME] **CD4 +**
[BRANDNAME] T cells was confirmed by flow cytometry using the surface markers anti-CD4-FITC
and anti-CD3PE (*FACSCalibur*, Becton & Dickinson [COMPANY], San Jose, CA, USA).

Tokens: 93<br>
Avg token length: 4.150537634408602<br>
Labeled entities: 8<br>
Avg entity length: 17.0<br>

##### Labeled
* Ficoll Hypaque TM PLUS | *INCORRECT* | *MISLABELED*
* GE Healthcare Bio-Sciences AB | *CORRECT*
* immunomagnetic beads | *CORRECT*
* Miltenyi Biotec GmbH | *CORRECT*
* Bergisch Gladbach | *INCORRECT* | *NO ENTITY*
* CD3 + | *INCORRECT* | *NO ENTITY*
* CD4 + | *INCORRECT* | *NO ENTITY*
* Becton & Dickinson | *CORRECT*

##### Missed
* FACSCalibur  | BRANDNAME


### 018
The equipment and software used for calculating the radon concentrations were developed by
**Gammadata AB** [COMPANY], Sweden. The **Radon Laboratory** [COMPANY] at the NRPA has during the
last ten years participated in the annual intercomparisons of etched *track detectors* organised
by the **Radiation Protection Division** [COMPANY] of the **Health Protection Agency** [COMPANY] in the
United Kingdom.

Tokens: 55<br>
Avg token length: 5.381818181818182<br>
Labeled entities: 4<br>
Avg entity length: 20.25<br>

##### Labeled
* Gammadata AB | *CORRECT*
* Radon Laboratory | *CORRECT*
* Radiation Protection Division | *INCORRECT* | *NO ENTITY* | Due to the capitalization this can easily be interpreted as a company.
* Health Protection Agency | *CORRECT*

##### Missed
* track detectors | PRODUCT


### 019
To explore the effect of dietary sugars on SSB and DSB of C. albicans and E. coli, either
sucrose or galactose (both sugars from **Fluka** [COMPANY], BioChemika, GmbH, Buchs, Switzerland)
was incorporated into TSB medium to yield a final sugar concentration of 500 mM.

Tokens: 50<br>
Avg token length: 4.24<br>
Labeled entities: 1<br>
Avg entity length: 5.0<br>

##### Labeled
* Fluka | *INCORRECT* | *PARTIAL* | 'BioChemika, GmbH' is not part of the entity span.

##### Missed
* N/A


### 020
Depth of anaesthesia was improved by intravenous injection of either 0.1 mg/kg
piritramid (**Dipidolor Ò** [BRANDNAME] ; **Janssen-Cilag GmbH** [COMPANY], Neuss, Germany) or
0.1 mg/ kg l-methadon. Intra-operative analgesia was accomplished by injecting 4.5 mg/kg
carprofene (**Rimadyl Ò** [COMPANY] ; **Pfitzer Pharma GmbH** [COMPANY], Karlsruhe, Germany).

Tokens: 62<br>
Avg token length: 4.145161290322581<br>
Labeled entities: 4<br>
Avg entity length: 14.25<br>

##### Labeled
* Dipidolor Ò | *CORRECT*
* Janssen-Cilag GmbH | *CORRECT*
* Rimadyl Ò | *CORRECT*
* Pfitzer Pharma GmbH | *CORRECT*

##### Missed
* N/A


### 021
The absorbance was measured at 550 nm j o u r n a l o f f o o d a n d d r u g a n a l y s i s 2 3
( 2 0 1 5 ) 6 9 2 e7 0 0 on a **plate reader** [PRODUCT] (**ELISA reader** [PRODUCT],
**ASYS Hitech** [COMPANY], GmbH, Eugendorf, Austria). The cell viability of macrophages was
calculated using the absorbance (A) at 550 nm:

Tokens: 84<br>
Avg token length: 2.4642857142857144<br>
Labeled entities: 3<br>
Avg entity length: 11.666666666666666<br>

##### Labeled
* plate reader | *CORRECT*
* ELISA reader | *CORRECT*
* ASYS Hitech | *INCORRECT* | *PARTIAL* | 'GmbH' is not included in entity span.

##### Missed
* N/A


### 022
SPR measurements were performed using a **Biacore X100** [BRANDNAME] *optical biosensor* with a
research grade **CM5 sensor** [PRODUCT] chip (**Biacore AB** [COMPANY], Uppsala, Sweden). PGRP-LCx
was immobilized using standard amine-coupling chemistry according to the manufacturer's
instructions (**Biacore Inc.** [COMPANY]).

Tokens: 48<br>
Avg token length: 4.8125<br>
Labeled entities: 4<br>
Avg entity length: 11.0<br>

##### Labeled
* Biacore X100 | *CORRECT*
* CM5 sensor| *INCORRECT* | *PARTIAL* | 'chip' is not included in entity span.
* Biacore AB | *CORRECT*
* Biacore Inc. | *CORRECT*

##### Missed
* optical biosensor | PRODUCT


### 023
Sections were cut by a **Leica Ultracutter** [COMPANY] (**Leica Mikrosysteme Gmbh** [COMPANY], Austria).
Semithin sections (1000 nm) from the middle of the grafts of all groups were stained with
toluidine blue and observed with an **Olympus BH-2** [COMPANY] (Tokyo, Japan) *photomicroscope*
to determine the total number of myelinated nerve fibers and mean fiber diameter, using an
**image analyzer** [PRODUCT] (**Digital Prioris** [COMPANY], XL-511:IPS 4.02, Alcatel, TITN,
**Answare** [BRANDNAME], **Massy Cedex** [COMPANY], France).

Tokens: 84<br>
Avg token length: 4.392857142857143<br>
Labeled entities: 7<br>
Avg entity length: 14.142857142857142<br>

##### Labeled
* Leica Ultracutter | *INCORRECT* | *MISLABELED*
* Leica Mikrosysteme Gmbh | *CORRECT*
* Olympus BH-2 | *INCORRECT* | *MISLABELED*
* image analyzer | *CORRECT*
* Digital Prioris | *INCORRECT* | *PARTIAL, MISLABELED* | 'XL-511' is not included in entity span.
* Answare | *INCORRECT* | *PARTIAL, MISLABELED* | 'Alcatel, TITN, ' not included in entity span.
* Massy Cedex | *INCORRECT* | *NO ENTITY*

##### Missed
* photomicroscope | PRODUCT


### 024
Force was measured by a *force transducer system* (**KG7** [BRANDNAME]; **Scientific Instruments GmbH**
[COMPANY], Heidelberg, Germany) and expressed in millinewtons per square millimeter of
cross-sectional area, which was calculated as width 3 thickness and corrected by multiplying
by 0.785 because of an ellipse-shaped cross-section of the muscle (i.e., p 3 width/2 3 thickness/2)
(23,24).

Tokens: 70<br>
Avg token length: 4.414285714285715<br>
Labeled entities: 2<br>
Avg entity length: 15.0<br>

##### Labeled
* KG7 | *CORRECT*
* Scientific Instruments GmbH | *CORRECT*

##### Missed
* force transducer system | PRODUCT


### 025
The protein bands were detected with the **ECL plus** [PRODUCT] *western Blot detection system*
(**GE Healthcare** [COMPANY]) and the chemiluminescence was recorded with an **ECL camera** [PRODUCT]
(**Nikon image** [BRANDNAME] station 2000r, **Bergman Labora AB** [COMPANY], Sweden). The band
visualized in the *ECL camera* was compared with the expression and secretion levels from
the wild-type control grown in the presence of DMSO but without addition of substance.

Tokens: 72<br>
Avg token length: 4.611111111111111<br>
Labeled entities: 5<br>
Avg entity length: 11.8<br>

##### Labeled
* ECL plus | *INCORRECT* | *MISLABELED*
* GE Healthcare | *CORRECT*
* Nikon image | *INCORRECT* | *PARTIAL, MISLABELED | 'station 2000r' not included in entity span.
* Bergman Labora AB | *CORRECT*

##### Missed
* western Blot detection system | PRODUCT
* ECL camera | PRODUCT


### 026
Polypropylene-coated, aluminium **Finn Chambers Ò** [COMPANY] , **filter paper** [PRODUCT]
disc inserts (18-mm diameter) and *hypoallergenic medical tape* were purchased from
**Bio-Diagnostics Ltd** [COMPANY], UK.

Tokens: 31<br>
Avg token length: 4.774193548387097<br>
Labeled entities: 3<br>
Avg entity length: 15.333333333333334<br>

##### Labeled
* Finn Chambers Ò | *INCORRECT* | *MISLABELED*
* filter paper | *INCORRECT* | *PARTIAL* | 'disc inserts' is not included in entity span.
* Bio-Diagnostics Ltd | *CORRECT*

##### Missed
* hypoallergenic medical tape | PRODUCT


### 027
The transduction of the GM-CSF producing **GL261 cells** [PRODUCT] (**GL- **[COMPANY]GM) has
been described earlier. 6 The cell culture medium (R10) used in all cell cultures was
RPMI 1640 supplemented with 2 mM L-Glutamine, 1 mM sodium pyruvate, 10 mM HEPES, 50
lg/ml gentamicin (**Invitrogen AB** [COMPANY], Sweden), and 10% FBS (**Fetal Bovine Serum**
[COMPANY], **Biochrom AB** [COMPANY], Berlin, Germany).

Tokens: 82<br>
Avg token length: 3.4878048780487805<br>
Labeled entities: 5<br>
Avg entity length: 11.2<br>

##### Labeled
* GL261 cells | *INCORRECT* | *NO ENTITY*
* GL- | *INCORRECT* | *NO ENTITY*
* Invitrogen AB | *CORRECT*
* Fetal Bovine Serum | *INCORRECT* | *NO ENTITY*
* Biochrom AB | *CORRECT*

##### Missed
* N/A

### 028
Using this sensor, amperometric measurements were performed with a **PalmSens** [BRANDNAME]
handheld *potentiostat* (**Palm Instruments BV** [COMPANY], Netherlands) at room temperature
(25 C), by applying a constant potential (150 mV) to the working *electrode*.

Tokens: 42<br>
Avg token length: 4.690476190476191<br>
Labeled entities: 2<br>
Avg entity length: 13.5<br>

##### Labeled
* PalmSens | *CORRECT*
* Palm Instruments BV | *CORRECT*

##### Missed
* potentiostat | PRODUCT
* electrode | PRODUCT


### 029
hMSCs were obtained from **Cambrex** [BRANDNAME] (**Cambrex Bio Science Verviers** [COMPANY], Sprl.,
Verviers, Belgium). Cells were thawed according to the manufacturer's instructions and
plated at a density of 5,000 cells/cm 2 in 75-cm 2 tissue culture polystyrene fl asks
in 10 ml of MSC basal medium supplemented with MSC growth medium (**Single Quots ®** [COMPANY] ,
**Cambrex Bio Science** [COMPANY]).

Tokens: 69<br>
Avg token length: 4.246376811594203<br>
Labeled entities: 4<br>
Avg entity length: 17.0<br>

##### Labeled
* Cambrex | *INCORRECT* | *MISLABELED*
* Cambrex Bio Science Verviers | *INCORRECT* | *PARTIAL* | 'Sprl.' is not included in entity span.
* Single Quots ® | *INCORRECT* | *MISLABELED*
* Cambrex Bio Science | *CORRECT*

##### Missed
* N/A


### 030
Measurements were made using **Image Analyzer** [COMPANY] *program* (version 3.1,
**Soft imaging** [PRODUCT] system GmbH [BRANDNAME], Meunster, Germany). Fifty male
worms from each host and each study area were used for each observation and measurement
(Gharamah et al., 2011a, b).

Tokens: 49<br>
Avg token length: 4.142857142857143<br>
Labeled entities: 3<br>
Avg entity length: 10.0<br>

##### Labeled
* Image Analyzer | *INCORRECT* | *MISLABELED*
* Soft imaging | *INCORRECT* | *PARTIAL* | 'system GmbH' is not included in the entity span.
* GmbH | *INCORRECT* | *NO ENTITY*

##### Missed
* program | PRODUCT


### 031
The *spirometer* was calibrated daily with a 3-L *precision syringe* (**Vitalograph Ltd** [COMPANY],
Buckingham, UK). Forced expiratory volume in 1 second (FEV 1 ), forced vital capacity (FVC),
and FEV 1 /FVC were measured in the morning.

Tokens: 47<br>
Avg token length: 3.9361702127659575<br>
Labeled entities: 1<br>
Avg entity length: 15.0<br>

##### Labeled
* Vitalograph Ltd | *CORRECT*

##### Missed
* spirometer | PRODUCT
* precision syringe | PRODUCT


### 032
The amplified PCR products were separated on a 6% acrylamide *gel* for 90 min at 90 V along
with a **DNA ladder** [PRODUCT] (**HyperLadder™** [COMPANY] 25 bp; **Bioline Ltd.** [COMPANY], UK).
After electrophoresis, **RedSafe™ Nucleic Acid Staining Solution** [COMPANY] was added to the
*gel* according to manufacturer's instructions.

Tokens: 55<br>
Avg token length: 4.181818181818182<br>
Labeled entities: 4<br>
Avg entity length: 18.25<br>

##### Labeled
* DNA ladder | *CORRECT*
* HyperLadder™ | *INCORRECT* | *MISLABELED*
* Bioline Ltd. | *CORRECT*
* RedSafe™ Nucleic Acid Staining Solution | *INCORRECT* | *MISLABELED*

##### Missed
* gel | PRODUCT
* gel | PRODUCT


### 033
The concentrations of IL-8 and MPO were measured by **enzyme linked** [PRODUCT]
immunosorbent assay (ELISA) (IL-8, MPO-EIA, **R&D Systems** [COMPANY], UK); ECP was
measured by a **fluorescent enzyme immunoassay** [PRODUCT] technique (**CAP ECP FEIA**
[BRANDNAME], **Pharmacia Diagnostics AB** [COMPANY], Uppsala, Sweden). The concentrations
were expressed as pg/ml or ng/ml.

Tokens: 65<br>
Avg token length: 3.9846153846153847<br>
Labeled entities: 5<br>
Avg entity length: 18.0<br>

##### Labeled
* enzyme linked | *INCORRECT* | *PARTIAL* 'immunosorbent assay' not included in the entity span
* R&D Systems | *CORRECT*
* fluorescent enzyme immunoassay | *CORRECT*
* CAP ECP FEIA | *CORRECT*
* Pharmacia Diagnostics AB | *CORRECT*

##### Missed
* N/A


### 034
To objectively evaluate an index of ulnar flow, the number of cine frames required
for contrast to first reach the standardized distal ulnar landmark (the ulnar frame count)
was measured at radial sheath insertion (baseline measurement before cardiac catheterization)
and removal (final measurement after cardiac catheterization) with a **frame counter**
[PRODUCT] (**Dicom Viewer** [COMPANY] 2.0, **Rubo Medical Imaging BV** [COMPANY], Aerdenhout,
the Netherlands) after setting image acquisition at 30 frames/s.

Tokens: 82<br>
Avg token length: 4.939024390243903<br>
Labeled entities: 3<br>
Avg entity length: 16.0<br>

##### Labeled
* frame counter | *CORRECT*
* Dicom Viewer | *INCORRECT* | *MISLABELED*
* Rubo Medical Imaging BV | *CORRECT*

##### Missed
* N/A


### 035
A p value <0.05 was considered significant. *XLSTAT* (**Addinosoft SARL** [COMPANY], Paris, France)
was used for the calculation of the Kaplan-Meier [COMPANY] survival curves.

Tokens: 31<br>
Avg token length: 4.129032258064516<br>
Labeled entities: 2<br>
Avg entity length: 13.5<br>

##### Labeled
* Addinosoft SARL | *CORRECT*
* Kaplan-Meier | *INCORRECT* | *NO ENTITY*

##### Missed
* XLSTAT | BRANDNAME


### 036
., Englewood, CO), bisbenzimide (BB, 10%; *Sigma*, Germany), **Fast Blue** [COMPANY]
(FB, 3%; **Dr. Illing** [COMPANY], GmbH, GroO-Umstedt, Germany), rhodamine-bisothiocyanate
(RITC, 10%; *Sigma*, Germany), and propidium iodide (PI, 10%; *Sigma*, Germany) were
pressure injected into the proximal cut ends of identified nerves by means of a
**glass micropipette** [PRODUCT] (Jankowska, '85).

Tokens: 90<br>
Avg token length: 3.2888888888888888<br>
Labeled entities: 3<br>
Avg entity length: 12.333333333333334<br>

##### Labeled
* Fast Blue | *INCORRECT* | *MISLABELED*
* Dr. Illing | *INCORRECT* | *PARTIAL* | ', GmbH' is not included in the entity tag
* glass micropipette | *CORRECT*

##### Missed
* Sigma | COMPANY
* Sigma | COMPANY
* Sigma | COMPANY


### 037 (CORRECT)
Sewage samples were collected at the wastewater treatment plant Ryaverket
(owned and run by **Gryaab AB** [COMPANY]), Gothenburg, Sweden. The plant receives
and treats wastewater from a population of 690,000 in the Gothenburg region and
industrial wastewater and storm water.

Tokens: 45<br>
Avg token length: 4.955555555555556<br>
Labeled entities: 1<br>
Avg entity length: 9.0<br>

##### Labeled
* Gryaab AB | *CORRECT*

##### Missed
* N/A


### 038
The blood was anticoagulated with the thrombin **inhibitor lepirudin** [PRODUCT]
(**Refludan ®** [COMPANY] , **Pharmion Germany GmbH** [COMPANY], Hamburg, Germany) in a
final concentration of 50 µg/ml. 23,27 The experiments were performed by first
administering 0.5 ml of blood in each well containing the respective candidate
materials, prior to incubation at 37 ° C in a climate room with the *plate* placed
on a 8shaker platform*.

Tokens: 72<br>
Avg token length: 4.513888888888889<br>
Labeled entities: 3<br>
Avg entity length: 16.666666666666668<br>

##### Labeled
* inhibitor lepirudin | *INCORRECT* | *NO ENTITY*
* Refludan ® | *INCORRECT* | *MISLABELED*
* Pharmion Germany GmbH | *CORRECT*

##### Missed
* plate | PRODUCT
* shaker platform | PRODUCT


### 039
Our study reflects a retrospective case-control study of all 44 patients with
clavicular midshaft fractures treated with a TEN using an *end cap* (**Synthes GmbH**
[COMPANY], Oberdorf, Switzerland) (    There were 37 males and seven females with
an average age (± SD) of 31 ± 17 years ( Table 2).

Tokens: 60<br>
Avg token length: 3.8666666666666667<br>
Labeled entities: 1<br>
Avg entity length: 12.0<br>

##### Labeled
* Synthes GmbH | *CORRECT*

##### Missed
* end cap | PRODUCT


### 040
After the evaporation process the samples were re-dissolved in approximately 3 mL
water and passed through 4 g *Dowex X8* *cation exchange resin* (**Sigma Aldrich** [COMPANY],
Steinheim, Germany) and 5 g **Serdolit PAD IV** [COMPANY] adsorption resin
(**Serva Electrophoresis GmbH** [COMPANY], Heidelberg, Germany) for purification.
The carbohydrates were eluted by adding 2 mL of water eight times.

Tokens: 66<br>
Avg token length: 4.545454545454546<br>
Labeled entities: 3<br>
Avg entity length: 18.0<br>

##### Labeled
* Sigma Aldrich | *CORRECT*
* Serdolit PAD IV | *INCORRECT* | *MISLABELED*
* Serva Electrophoresis GmbH | *CORRECT*

##### Missed
* Dowex X8 | BRANDNAME
* cation exchange resin | PRODUCT
* adsorption resin | PRODUCT
