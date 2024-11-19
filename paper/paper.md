---
title: 'Bridging Machine Learning and Semantic Web: A Case Study on Converting Hugging Face Metadata to RDF'
title_short: 'BioHackJP24 #26: Hugging Face RDF'
tags:
  - machine learning
  - RDF
  - schema
authors:
  - name: David Steinberg
    affiliation: 1
    orcid: 0000-0001-6683-2270
  - name: Jerven Bolleman
    affiliation: 2
    orcid: 0000-0002-7449-1266
affiliations:
  - name: University of California, Santa Cruz, Genomics Institute
    index: 1
  - name: Swiss Institute of Bioinformatics
    index: 2
date: 28 August 2024
cito-bibliography: paper.bib
event: BH24JP
biohackathon_name: "DBCLS BioHackathon Japan 2024"
biohackathon_url:   "https://2024.biohackathon.org/"
biohackathon_location: "Fukushima, Japan, 2024"
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/david4096/huggingface-rdf
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: David Steinberg
---

# **Bridging Machine Learning and Semantic Web: A Case Study on Converting Hugging Face Metadata to RDF**

David Steinberg

Abstract

At BioHackathon 2024 in Fukushima, we explored enhancing machine learning dataset metadata management by developing a tool for converting the Croissant ML format from MLCommons, implemented on Hugging Face[UnknownUnknown-ll] and expressed in JSON-LD, into RDF using Turtle format. This allowed us to load the data into triple stores like Qlever and Apache Jena Fuseki, improving interoperability and querying capabilities. RDF's graph-based structure and SPARQL querying enable advanced integration and analysis across heterogeneous datasets, addressing challenges like blank nodes, undefined ontologies, and non-resolvable URIs in the Croissant schema. Despite its alignment with ML workflows, Croissant's limited connection to controlled vocabularies and lack of resolvable URLs highlight interoperability challenges compared to other metadata standards. Feedback emphasized the need for more detailed metadata specifications, meaningful annotations, and extensibility while recognizing Croissant’s potential for advancing metadata management in machine learning and bioinformatics. This approach underscores the value of standardizing and extending metadata frameworks and tools to facilitate dataset discovery, reproducibility, and integration across diverse domains.

# **1\. Introduction**

In the realm of machine learning (ML), the complexity of managing and utilizing datasets is exacerbated by the sheer volume and diversity of available data. The BioHackathon 2024, held in Fukushima, provided an opportunity to address this challenge by leveraging the Croissant metadata format from MLCommons implemented in Hugging Face. This format, exposed via JSON-LD, was transformed into RDF using the Turtle format and loaded into a triple store. The primary objective was to enhance data interoperability and facilitate efficient querying and integration of dataset metadata across different platforms.

# **Importance of Metadata in Machine Learning**

In the evolving landscape of machine learning, metadata plays a critical role in improving the efficiency and effectiveness of data-driven workflows. As multimodal approaches to ML become more prevalent, the ability to integrate diverse data types—such as text, images, and audio—becomes increasingly important. Accurate and comprehensive metadata ensures that these various data types are appropriately aligned and utilized.

The advent of models capable of fine-tuning on specific data types further underscores the need for detailed metadata. These models require precise information about the datasets they are trained on to achieve optimal performance and generalization. Additionally, as researchers explore new and emerging domains, having well-structured metadata allows for better discovery, reproducibility, and validation of research findings.

Metadata also supports quality control and management by providing insights into the provenance, structure, and semantics of datasets. This transparency helps mitigate issues related to data biases and inaccuracies, ensuring that ML models are built on reliable and representative datasets.

# **2\. Background and Motivation**

Hugging Face, a leading platform in machine learning and natural language processing, recognized the need to improve the accessibility and interoperability of dataset metadata. To address this, Hugging Face adopted the Croissant metadata format and exposed it using JSON-LD. Croissant, developed as part of MLCommons, provides a robust structure for describing datasets, enhancing their discoverability and usability. By integrating JSON-LD, Hugging Face ensures that dataset metadata is easily machine-readable and semantically rich, paving the way for better integration across various data systems and applications.

MLCommons, a consortium focused on advancing machine learning through collaboration and open standards, supports the use of Croissant to standardize dataset documentation. This alignment with MLCommons' goals underscores the importance of standardized metadata formats in fostering a more cohesive ML ecosystem.

## **Need for RDF and Semantic Web Technologies**

Converting dataset metadata to RDF (Resource Description Framework) offers significant advantages for managing and querying complex datasets. RDF’s capability to represent data in a graph-based structure aligns well with the needs of bioinformatics and machine learning, where relationships between data elements are critical. RDF provides a flexible and extensible framework for linking datasets, enabling more sophisticated data integration and analysis.

The use of RDF is particularly valuable for triple stores and SPARQL (SPARQL Protocol and RDF Query Language). Triple stores facilitate efficient storage and retrieval of RDF data, while SPARQL allows for powerful querying across diverse datasets. Unlike traditional flat APIs, SPARQL offers advanced querying capabilities that can handle complex relationships and federated queries across multiple data sources.

Federation using SPARQL enables querying over distributed datasets, providing a unified view of interconnected data. This capability is crucial for integrating heterogeneous data sources in bioinformatics and ML, where data is often spread across different repositories and formats.

In addition to Hugging Face, Croissant has been integrated into several major dataset repositories, including Google Dataset Search, Kaggle, and OpenML. These integrations enhance dataset discoverability and accessibility by allowing users to search for Croissant-formatted datasets across various platforms. The adoption of Croissant by these repositories highlights its growing significance in the data management landscape. Although Croissant is implemented in JSON-LD rather than RDF, this represents a significant step forward in standardizing dataset metadata. The use of JSON-LD facilitates broad adoption and improved interoperability, laying the groundwork for future advancements in dataset metadata management.

# **3\. Challenges and Limitations of the Current Approach**

**A New Standardized Vocabulary**

Croissant introduces a controlled vocabulary aimed at providing a structured approach to dataset metadata[Akhtar2024-fo]. However, some of these vocabulary types are specific to the JSON-LD format and were not initially designed as RDF triples. This results in certain types of metadata that are unique to the Croissant implementation and may not align with more broadly accepted standards. The local nature of these types can lead to difficulties in standardizing and integrating metadata across different platforms. For example, the JSON-LD format’s flexibility allows for custom types and attributes that do not always map neatly onto established RDF vocabularies, potentially limiting interoperability with other metadata systems.

**Croissant Schema Limitations**

A significant limitation of the Croissant schema is its lack of a resolvable URL for each schema element, unlike schemas.org which provides globally accessible URLs for its schema definitions. This absence of resolvable URLs means that the schema definitions in Croissant are not easily discoverable or verifiable on the web. Without a direct web reference, users and systems cannot easily access the detailed specifications or confirm the validity of the schema elements, which can hinder the schema’s adoption and integration into broader data ecosystems. This also complicates efforts to link Croissant metadata with other datasets or standards that rely on well-defined, resolvable URLs for interoperability.

**Potential Issues with Data Integration and Interoperability**

Integrating Croissant metadata with other datasets or platforms poses several challenges. One major issue is the need to map metadata types and structures from Croissant’s JSON-LD-based format to more universally recognized RDF standards. This process can be complex and may require extensive transformation efforts to ensure compatibility with other systems. Additionally, the unique characteristics of Croissant’s metadata schema can create barriers to achieving seamless data integration and interoperability. For instance, when federating data using SPARQL, the lack of standardization and direct web references can complicate querying and data merging processes. These challenges highlight the need for more standardized and universally accessible metadata frameworks to facilitate easier integration and interoperability across diverse data systems.

# **4\. Methods**

# **Methods**

**Conversion Process to RDF**

The conversion of JSON-LD metadata from Hugging Face to RDF in Turtle format involved several steps. The Hugging Face API was used to fetch the dataset metadata directly, as existing Python clients do not support the Croissant metadata format. This process required making HTTP requests to retrieve the JSON-LD data.

Due to the sequential nature of data retrieval, loading the data was time-consuming. Only one dataset Croissant file could be retrieved at a time, extending the loading time to several hours. Managing this process carefully was crucial to minimize delays.

After obtaining the JSON-LD metadata, it was converted to RDF Turtle format using the `rdflib` library[UnknownUnknown-wz]. `rdflib` enabled parsing of JSON-LD and serialization into Turtle format, ensuring compatibility with RDF standards.

**Loading into a Triple Store**

The RDF data was loaded into a triple store using two solutions: Qlever [Bast2017-mj] and Apache Jena Fuseki[UnknownUnknown-fa]. These tools were chosen for their robust support for RDF data and SPARQL querying capabilities.

| Classes |
| :---- |
| [http://mlcommons.org/croissant/fileSet](http://mlcommons.org/croissant/fileSet) |
| [http://mlcommons.org/croissant/extract](http://mlcommons.org/croissant/extract) |
| [http://mlcommons.org/croissant/column](http://mlcommons.org/croissant/column) |
| [http://mlcommons.org/croissant/includes](http://mlcommons.org/croissant/includes) |
| [http://www.w3.org/1999/02/22-rdf-syntax-ns\#type](http://www.w3.org/1999/02/22-rdf-syntax-ns#type) |
| [https://schema.org/name](https://schema.org/name) |
| [https://schema.org/description](https://schema.org/description) |
| [https://schema.org/containedIn](https://schema.org/containedIn) |
| [https://schema.org/encodingFormat](https://schema.org/encodingFormat) |
| [http://mlcommons.org/croissant/source](http://mlcommons.org/croissant/source) |
| [http://mlcommons.org/croissant/dataType](http://mlcommons.org/croissant/dataType) |
| [http://mlcommons.org/croissant/field](http://mlcommons.org/croissant/field) |
| [http://mlcommons.org/croissant/repeated](http://mlcommons.org/croissant/repeated) |
| [https://schema.org/url](https://schema.org/url) |
| [https://schema.org/license](https://schema.org/license) |
| [http://mlcommons.org/croissant/recordSet](http://mlcommons.org/croissant/recordSet) |
| [https://schema.org/keywords](https://schema.org/keywords) |
| [http://purl.org/dc/terms/conformsTo](http://purl.org/dc/terms/conformsTo) |
| [https://schema.org/alternateName](https://schema.org/alternateName) |
| [https://schema.org/distribution](https://schema.org/distribution) |
| [https://schema.org/creator](https://schema.org/creator) |
| [https://schema.org/sameAs](https://schema.org/sameAs) |
| [http://mlcommons.org/croissant/transform](http://mlcommons.org/croissant/transform) |
| [https://schema.org/identifier](https://schema.org/identifier) |
| [http://mlcommons.org/croissant/jsonPath](http://mlcommons.org/croissant/jsonPath) |
| [https://schema.org/sha256](https://schema.org/sha256) |
| [https://schema.org/contentUrl](https://schema.org/contentUrl) |

 Table 1\. Showing the terms that were in the Hugging Face RDF reflecting the controlled vocabulary provided by MLCommons Croissant.

**Overcoming Metadata Challenges**

Several strategies were employed to address challenges related to metadata conversion. Many nodes in the JSON-LD data were represented as “blank nodes” with arbitrary identifiers. Unique identifiers were assigned to these blank nodes in the Turtle format to ensure consistency and traceability.

Some elements, such as the `plain_text` type, lacked associated ontologies. To address this, placeholders were created in the RDF schema, and additional annotations were added as needed. This approach allowed for the representation of undefined elements while maintaining metadata integrity.

The MLCommons namespace URIs did not link to active web pages, making it challenging to retrieve definitions. As a workaround, definitions were manually extracted from MLCommons specification documents and incorporated into the RDF schema. This ensured that all metadata elements were properly defined despite the lack of live URIs.

# **5\. Practical Applications and Potential Use Cases**

**Examples of Queries Made Easier through SPARQL Interface**

The SPARQL interface offers significant advantages over traditional APIs when it comes to querying RDF data. For instance, complex queries involving relationships between multiple datasets can be executed with ease. Traditional APIs often require multiple sequential calls and intricate logic to gather and process related information. In contrast, SPARQL allows for sophisticated querying capabilities, such as:

* **Federated Queries:** SPARQL can perform federated queries across different RDF data sources, integrating information from various datasets in a single query. This is particularly useful for combining data from multiple repositories or sources.  
* **Complex Pattern Matching:** SPARQL enables querying based on intricate patterns and relationships within the data, such as finding all datasets that include a specific type of file or have certain metadata attributes.  
* **Inferencing and Reasoning:** SPARQL can leverage inferencing rules to derive additional insights based on the existing data relationships, enhancing the depth of queries beyond what is explicitly stated in the metadata.

**Learning on Dataset Metadata**

Applying machine learning to dataset metadata presents numerous opportunities for enhancing data discovery, quality assessment, and gaining actionable insights:

* **Model Suggestion Based on Data Types:** Machine learning algorithms can analyze dataset metadata to recommend suitable models for various types of data. For example, metadata indicating image data types could suggest appropriate deep learning models for image recognition tasks.  
* **Designing Multi-Modal Datasets:** By examining metadata columns from existing datasets, machine learning can assist in designing new multi-modal datasets. This involves combining data from different sources and formats to create comprehensive datasets for training complex models.  
* **Quality Assessment:** Machine learning can be employed to assess the quality of datasets by analyzing metadata attributes and identifying anomalies or inconsistencies. This helps in maintaining high-quality datasets and improving the reliability of data-driven insights.

**Integration with Other Bioinformatics Resources**

Integrating RDF metadata with other bioinformatics databases and resources can significantly enhance data utility:

* **Leveraging Ontologies:** The bioinformatics community has developed numerous ontologies, such as EDAM for data and analysis types. Integrating these ontologies with RDF metadata allows for standardized representation of datasets, improving interoperability and data integration.  
* **Health Data Ontologies:** For health-related datasets, ontologies like those for disease, gene, and protein annotations can be used to provide a richer context. Integrating health data ontologies into RDF metadata facilitates more meaningful queries and better alignment with existing biomedical resources.  
* **Enhanced Data Utility:** Combining RDF metadata with bioinformatics resources enables more effective data exploration and retrieval. For instance, querying RDF metadata alongside genomic databases can reveal connections between datasets and assist in comprehensive analyses of biological data.

This approach not only enhances the usability of dataset metadata but also fosters better integration across diverse bioinformatics tools and resources. The flexibility of RDF and SPARQL in handling complex queries and integrating disparate data sources makes it a powerful asset in the bioinformatics domain.

# **Comparison with Other Approaches**

**Comparison with Metadata Standards**

The Hugging Face approach to dataset metadata, utilizing Croissant in JSON-LD format, represents a significant shift in how metadata is handled for machine learning datasets. To provide context and depth, it is useful to compare this approach with other established metadata standards, such as DataCite, DCAT, schema.org, and Ro-crate. Each of these standards offers different features and capabilities, impacting their applicability to various use cases.

**DataCite and DCAT**

* **DataCite:** DataCite[Brase2009-ye] is a widely recognized metadata standard used primarily for publishing and citing research datasets. It emphasizes bibliographic information such as authorship, publication date, and access details. While DataCite is robust in providing citation metadata, it does not address the rich, detailed metadata needed for machine learning datasets, such as dataset structure, data types, or integration with computational frameworks.  
* **DCAT (Data Catalog Vocabulary):** DCAT[Version_undated-uv] is a W3C standard designed to enhance the interoperability of data catalogs published on the web. It focuses on describing datasets and their distributions, making it useful for cataloging purposes. However, DCAT is less specialized in representing detailed machine learning metadata or supporting advanced querying capabilities, which are crucial for ML workflows.

**Schema.org**

* **Schema.org:** Schema.org provides a broad vocabulary for structured data on the web, including schemas for datasets. The Croissant format builds on Schema.org's Dataset schema, extending it with additional metadata fields relevant to machine learning. While Schema.org offers a foundational structure for dataset descriptions, Croissant introduces more specialized features and semantics tailored to ML needs, such as detailed data types and integration with machine learning frameworks.

**Ro-Crate**

* **Ro-Crate:** The RO-Crate[Soiland-Reyes2022-ys] (Research Object Crate) specification is designed to support the packaging and sharing of research data and its associated metadata. Similar to Croissant, RO-Crate focuses on organizing datasets in a way that facilitates reproducibility and sharing. RO-Crate's emphasis on research object packaging aligns with the goal of making datasets more accessible and reusable. However, Croissant extends beyond this by incorporating semantic typing and advanced querying capabilities through RDF and SPARQL, providing a more nuanced approach to metadata for machine learning.

**Hugging Face and Croissant Approach**

The approach adopted by Hugging Face, leveraging Croissant in JSON-LD format, offers several distinct advantages:

* **Integration with ML Frameworks:** Croissant is designed with machine learning workflows in mind, offering detailed metadata that supports various ML tasks. Its ability to describe dataset structures, file types, and relationships enhances its utility for ML applications compared to more general metadata standards.  
* **Semantic Typing and Querying:** Croissant’s use of RDF and SPARQL allows for sophisticated querying and reasoning over dataset metadata. This contrasts with traditional standards like DataCite or DCAT, which do not offer the same level of querying flexibility or semantic richness.  
* **Extensibility and Future-Proofing:** Croissant’s alignment with Schema.org provides a foundation for interoperability while extending its capabilities to address specific ML needs. This makes it a versatile choice that can adapt to evolving requirements in the ML landscape.

In summary, while established standards like DataCite, DCAT, and Schema.org provide essential metadata frameworks, Croissant’s integration with RDF and SPARQL, combined with its focus on machine learning datasets, offers a more specialized and flexible solution. RO-Crate's emphasis on reproducibility and packaging aligns with some of Croissant’s goals, but Croissant’s semantic and querying capabilities offer additional benefits for complex ML use cases.

# **Community Feedback and Involvement**

**Feedback from Biohackathon participants**

The feedback gathered from participants of the Biohackathon 2024 and other stakeholders in the bioinformatics and machine learning communities has provided valuable insights into the current approach and its potential improvements. This feedback highlights several key areas for enhancement and consideration:

**1\. Specification of Data**

Participants noted that the dataset metadata provided was not as well-specified as it could be. Detailed specifications are crucial for ensuring that metadata accurately represents the dataset’s contents and structure. In particular, more comprehensive descriptions and clearer documentation are needed to enhance usability and reduce ambiguity. Improved data specifications can help users better understand the dataset’s context and make more informed decisions about its use.

**2\. Automatic Annotations**

Feedback also pointed out that automatic annotations were repetitive and not particularly useful. For instance, the annotations related to parquet columns were seen as generic and lacking in detail. To address this, there is a need for more meaningful and contextually relevant annotations. Incorporating additional metadata that reflects the unique characteristics of each dataset could improve the overall usefulness of the annotations.

**3\. Connection to Controlled Vocabularies**

Another significant concern raised was the lack of connection to established controlled vocabularies. Controlled vocabularies provide standardized terms and definitions, which are essential for ensuring consistency and interoperability across datasets. The current approach’s limited integration with these vocabularies may hinder its ability to seamlessly align with other data sources and standards. Expanding the use of controlled vocabularies and developing mechanisms for linking with existing ontologies could enhance the richness and usability of the metadata.

**4\. Extensibility**

The feedback also highlighted the need for greater extensibility in the metadata schema. The ability to extend and adapt the schema to accommodate new types of data and emerging requirements is crucial for maintaining relevance and usefulness. Future work should focus on designing a more flexible and extensible schema that can evolve in response to the needs of the community and advancements in the field.

Other feedback we would like to briefly mention:

* Wikidata may have information on institutions as described in Croissant. These could be linked and provide interesting geospatial or funding related queries.  
* Users in the Hugging Face Croissant implementation are simply names. By using ORCID or a permanent identifier it would be possible to cross reference with the citation network.  
* Metadata on Hugging Face is limited to what is provided automatically. The implementation could be improved by allowing users to specify further metadata.  
* SPARQL queries could allow for an interface where query inputs are translated into a target language to help with internationalization.  
* By presenting CroissantML metadata in RDF it becomes easier to generate summary statistics.  
* An analysis of freetext keywords should be made so that they can be connected to existing vocabularies.  
* The same analysis of Hugging Face CroissantML should be done with Kaggle and OpenML.

# **6\. Future Directions**

**Standardization and Schema Development**

The development of more robust schemas and standardized vocabularies for dataset metadata is crucial for enhancing interoperability and usability. Key proposals for future improvements include:

* **Enhanced Data Types in MLCommons Croissant:** Expanding Croissant to include more detailed data types and attributes would provide a richer representation of dataset contents. This could involve incorporating specific data types relevant to various fields and domains, ensuring that metadata accurately reflects the nature of the data.  
* **Improved Tagging Automation:** While Hugging Face’s automation significantly aids in tagging datasets, there is potential to further streamline this process by integrating controlled vocabularies. Making it easier for individuals to tag datasets using these standardized terms would enhance consistency and usability across different datasets.  
* **Integration of Model Card Data:** Currently, metadata for Hugging Face models is not included in the Croissant schema. Incorporating model card data into the metadata could provide valuable insights into model characteristics, performance, and intended use cases.  
* **Citation network:** By integrating metadata provided from services like ORCID it would be possible to access citations   
* **Linkages Between Datasets and Models:** Facilitating connections between datasets and relevant models could improve the discoverability and applicability of models for specific datasets. This linkage would enable researchers to identify suitable models based on dataset characteristics and vice versa.

**Expanding RDF Use Cases**

RDF and semantic technologies have significant potential to further improve data management and analysis in machine learning and bioinformatics. Suggested areas for expansion include:

* **Model JSON-LD Representation:** Extending RDF schemas to include model architectures and their associated metadata in JSON-LD format would provide a structured and standardized way to describe models. This approach would enhance flexibility and openness in model descriptions, making it easier to discover, combine, and fine-tune models.  
* **Discoverability of Models via SPARQL:** By providing a comprehensive schematic representation of models and their metadata, it should be possible to use SPARQL queries to find and combine models more effectively. This would support advanced search and discovery capabilities, facilitating better model selection and application.  
* **Permanent SPARQL Services:** Establishing a permanent SPARQL endpoint for Croissant-compatible datasets would enable continuous querying and analysis. Federating multiple locations would further enhance the availability and accessibility of metadata, supporting more comprehensive and scalable data management solutions.

These future directions aim to advance the field of dataset metadata management by addressing current limitations and expanding the potential applications of RDF and semantic technologies. Enhanced standardization, improved tagging, and better integration with model data will contribute to more effective data discovery and utilization in machine learning and bioinformatics.

# **7\. Conclusion**

**Summary of Findings**

This report has explored the use of Croissant for metadata representation in the context of the Hugging Face JSON-LD endpoint, highlighting several key aspects:

* **Metadata Complexity and Standardization:** Croissant introduces a new approach to representing dataset metadata using JSON-LD, which was successfully converted to RDF in Turtle format. However, the approach faces challenges related to the absence of a direct URL resolution for the schema and the handling of “blank nodes” from JSON-LD.  
* **Metadata Integration and SPARQL:** Converting and loading metadata into a triple store using tools like rdflib, QLever, and Jena Fuseki demonstrates the utility of SPARQL over traditional APIs for querying complex metadata. SPARQL's flexibility and federation capabilities enable more sophisticated data queries and integration.  
* **Challenges and Opportunities:** Issues with controlled vocabularies, schema resolution, and integration with other datasets underscore the need for further development in standardization. Feedback from the community highlights the need for more detailed schemas and improved automation for tagging datasets.  
* **Future Directions:** Enhancing Croissant with detailed data types, integrating model card data, and establishing permanent SPARQL services are proposed as critical steps for advancing metadata management. Expanding RDF use cases to include model descriptions and improving discoverability through SPARQL are also recommended.

**Implications for Future Work**

The findings from this report have significant implications for future projects and research in dataset metadata management:

* **Enhanced Metadata Standards:** The need for more robust schemas and controlled vocabularies highlights the importance of continued development in metadata standards. Future efforts should focus on creating detailed and universally applicable schemas to improve data interoperability and usability.  
* **Improved Automation and Integration:** Integrating automation tools for metadata tagging and linking datasets with relevant models could streamline workflows and enhance the applicability of metadata. This could lead to more efficient data discovery and utilization in machine learning and bioinformatics.  
* **Broader RDF Applications:** The successful use of RDF and SPARQL demonstrates their potential for broader application in managing complex datasets. Expanding these technologies to cover additional aspects of model metadata and establishing permanent querying services will support more advanced and scalable data management solutions.

Overall, these findings provide a foundation for advancing metadata practices and highlight the need for ongoing innovation and collaboration in the field. Future work should build on these insights to address current limitations and leverage emerging technologies to enhance dataset metadata management and analysis.

**Acknowledgements**

BiohackathonJP 24 staff and organizers

# Citation Typing Ontology annotation

You can use [CiTO](http://purl.org/spar/cito/2018-02-12) annotations, as explained in [this BioHackathon Europe 2021 write up](https://raw.githubusercontent.com/biohackrxiv/bhxiv-metadata/main/doc/elixir_biohackathon2021/paper.md) and [this CiTO Pilot](https://www.biomedcentral.com/collections/cito).
Using this template, you can cite an article and indicate _why_ you cite that article, for instance DisGeNET-RDF [@citesAsAuthority:Queralt2016].

The syntax in Markdown is as follows: a single intention annotation looks like
`[@usesMethodIn:Krewinkel2017]`; two or more intentions are separated
with colons, like `[@extends:discusses:Nielsen2017Scholia]`. When you cite two
different articles, you use this syntax: `[@citesAsDataSource:Ammar2022ETL; @citesAsDataSource:Arend2022BioHackEU22]`.

Possible CiTO typing annotation include:

* citesAsDataSource: when you point the reader to a source of data which may explain a claim
* usesDataFrom: when you reuse somehow (and elaborate on) the data in the cited entity
* usesMethodIn
* citesAsAuthority
* citesAsEvidence
* citesAsPotentialSolution
* citesAsRecommendedReading
* citesAsRelated
* citesAsSourceDocument
* citesForInformation
* confirms
* documents
* providesDataFor
* obtainsSupportFrom
* discusses
* extends
* agreesWith
* disagreesWith
* updates
* citation: generic citation


# Results


# Discussion

...

## Acknowledgements

...

## References
