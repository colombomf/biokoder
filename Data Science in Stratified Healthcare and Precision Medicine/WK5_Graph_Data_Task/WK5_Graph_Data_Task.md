
# Graph Data Task

## Importing rdflib

_rdflib_ is one of the most widely used Python libraries for working with RDF data. 
We first need to tell Python that we're going to be using rdflib, with the use of the import command.


```python
import rdflib
```

## Familiarising ourselves with the data

In this programming task we are going to be using a small RDF dataset about hospitals. The dataset includes information about 4 different hospitals:
* Glasgow Royal Infirmary
* Royal Infirmary of Edinburgh
* Aberdeen Royal Infirmary
* University Hospital Heidelberg

The information about each hospital may include: URI, name, number of beds, country, the fact that it is a hospital.

Here is an extract of the dataset:
<div class="alert alert-block alert-info">
<</span>http://dbpedia.org/resource/Glasgow_Royal_Infirmary>	<</span>http://www.w3.org/1999/02/22-rdf-syntax-ns#type>	<</span>http://schema.org/Hospital> . <br>
<</span>http://dbpedia.org/resource/Glasgow_Royal_Infirmary>	<</span>http://xmlns.com/foaf/0.1/name>	"Glasgow Royal Infirmary" .<br>
<</span>http://dbpedia.org/resource/Glasgow_Royal_Infirmary>	<</span>http://dbpedia.org/ontology/country>	<</span>http://dbpedia.org/resource/Scotland> .<br>
<</span>http://dbpedia.org/resource/Glasgow_Royal_Infirmary>	<</span>http://dbpedia.org/ontology/bedCount>	1077 .
</div>


### Parsing the data

The dataset is called *hospitals.ttl* and it can be found in the _readonly_ directory. <br>

Let's parse the dataset with the use of the *parse* function. You can ignore anything printed.


```python
from rdflib import Graph
g = Graph()
g.parse("./readonly/hospitals.ttl", format="ttl")
```




    <Graph identifier=Nc5322f7cf40d44da89138b0f54aa53ae (<class 'rdflib.graph.Graph'>)>



This creates a new Graph object called *g*. 

### Getting a sense of the data

We can get the number of triples in our dataset with the use of *len*. 

Run the code below (the result should be 14).


```python
len(g)
```




    14



To print out all the triples in the dataset in the form of *subject predicate object*, run the code below.

Note that it is not recommended to print an entire dataset, as it may be very large! For this example it is ok, though.


```python
for s, p, o in g:
    print(s, p, o)
```

    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://dbpedia.org/ontology/bedCount 922
    http://dbpedia.org/resource/University_Hospital_Heidelberg http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://dbpedia.org/resource/University_Hospital_Heidelberg http://xmlns.com/foaf/0.1/name University Hospital Heidelberg
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://dbpedia.org/ontology/bedCount 1077
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://xmlns.com/foaf/0.1/name Aberdeen Royal Infirmary
    http://dbpedia.org/resource/University_Hospital_Heidelberg http://dbpedia.org/ontology/country http://dbpedia.org/resource/Germany
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://xmlns.com/foaf/0.1/name Royal Infirmary of Edinburgh
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://xmlns.com/foaf/0.1/name Glasgow Royal Infirmary
    

## Querying the data

Let's formulate SPARQL queries and run them in Python. 

Note that the general syntax for running SPARQL queries using rdflib is as follows:

```python
qres = g.query(""" your_SPARQL_Query_goes_here """)

for row in qres:
    print( ""%s" % row)
```

So all we need to do is substitute *your_SPARQL_Query_goes_here* with our SPARQL query. We can leave the last two code lines as they are to print out the results. In the following examples, however, we're using different printing variations, just for fun. You can ignore these if you want - or you can substitute the last line with what we have above.

### Query 1: Get the name of the Royal Infirmary of Edinburgh

We want to get the name of the resource identified through the following URI: <</span>http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh> <br><br>
One could think that this is self-evident, but there's no such thing when working with data! We want to know what the official name is for this URI, according to our data.


```python
qres = g.query(
    """SELECT ?n
       WHERE {
          <http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh>  <http://xmlns.com/foaf/0.1/name> ?n .
       }""")

for row in qres:
    print("The name of <http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh> is: %s" % row)
```

    The name of <http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh> is: Royal Infirmary of Edinburgh
    

Suppose that we used an incorrect URI for the Royal Infirmary of Edinburgh, e.g. <</span>http://dbpedia.org/resource/Edinburgh_Royal_Infirmary>. <br> 
When running the query we wouldn't get an error, but simply no results would be returned.
This is because SPARQL doesn't know whether a URI is "correct" or not - it just searches for the information thatv we've specified. In this case, there is simlpy no triple in our data to match this query.


```python
qres = g.query(
    """SELECT ?n
       WHERE {
          <http://dbpedia.org/resource/Edinburgh_Royal_Infirmary>  <http://xmlns.com/foaf/0.1/name> ?n .
       }""")

for row in qres:
    print("The name of <http://dbpedia.org/resource/Edinburgh_Royal_Infirmary> is: %s" % row)
```

### Query 2: Get the URIs of all hospitals

Here we want to get the URIs of all "things" that are *of type* Hospital. There is a special term to express this: <</span>http://www.w3.org/1999/02/22-rdf-syntax-ns#type>. This is used in the SPARQL query below.

Run the following cell to get the results.


```python
qres = g.query(
    """SELECT ?h
       WHERE {
          ?h  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>  <http://schema.org/Hospital> .
       }""")

for row in qres:
    print("%s" % row)
```

    http://dbpedia.org/resource/University_Hospital_Heidelberg
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary
    

### Query 3: Get the names of all hospitals

Now we are going to build on the previous SPARQL query to get the names of all those "things" that are of type Hospital. All we need to do is add a new triple in the WHERE-part of the query to express this.

Run the following cell to get the results.


```python
qres = g.query(
    """SELECT ?n
       WHERE {
          ?h  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>  <http://schema.org/Hospital> .
          ?h  <http://xmlns.com/foaf/0.1/name> ?n .
       }""")

for row in qres:
    print("The name of one of the hospitals in our graph database is: %s" % row)
```

    The name of one of the hospitals in our graph database is: University Hospital Heidelberg
    The name of one of the hospitals in our graph database is: Royal Infirmary of Edinburgh
    The name of one of the hospitals in our graph database is: Aberdeen Royal Infirmary
    The name of one of the hospitals in our graph database is: Glasgow Royal Infirmary
    

### Query 4: Get the names of all hospitals based in Scotland

Now we are going to build even further on the previous SPARQL query to specify that those "things" (i.e. the hospitals) need to be based in Scotland. All we need to do is add a new triple in the WHERE-part of the query to express this.

Run the following cell to get the results.


```python
qres = g.query(
    """SELECT ?n
       WHERE {
          ?h  <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>  <http://schema.org/Hospital> .
          ?h  <http://dbpedia.org/ontology/country>  <http://dbpedia.org/resource/Scotland> .
          ?h  <http://xmlns.com/foaf/0.1/name> ?n .
       }""")

print("The names of all hospitals in Scotland are:")
for row in qres:
    print("%s" % row)
```

    The names of all hospitals in Scotland are:
    Glasgow Royal Infirmary
    Royal Infirmary of Edinburgh
    Aberdeen Royal Infirmary
    

## Have a go at querying the data

### Quiz 5 - Question 5

Here's the code that is provided in this week's quiz (Question 5). Run it to see what it does. You may also want to modify it further to get a better understanding.


```python
qres = g.query(
    """SELECT ?b
    WHERE {
    <http://dbpedia.org/resource/Glasgow_Royal_Infirmary>  <http://dbpedia.org/ontology/bedCount> ?b .
    }""")

for row in qres:
    print("The result is: %s" % row)
```

    The result is: 1077
    

### Other queries of interest

Why not formulate your own SPARQL queries? You can use the starter code below. Just make sure you don't forget any quotes, curly brackets, etc.


```python
qres = g.query(
    """SELECT ?b
    WHERE {
    <http://dbpedia.org/resource/Aberdeen_Royal_Infirmary>  <http://dbpedia.org/ontology/bedCount> ?b .
    }""")

for row in qres:
    print("The result is: %s" % row)    
```

    The result is: 922
    


```python
#Exploring linked data from http://www.productontology.org
g.parse("http://www.productontology.org/doc/Head_and_neck_cancer#rdfa")

```




    <Graph identifier=Nc5322f7cf40d44da89138b0f54aa53ae (<class 'rdflib.graph.Graph'>)>




```python
#Prints the number of triples in the Head and Neck Cancer dataset
g.parse("http://www.productontology.org/doc/Head_and_neck_cancer#rdfa")
print("Graph has %s statements." % len(g))
```

    Graph has 70 statements.
    


```python
qres = g.query(
    """SELECT ?b
    WHERE {
    <http://www.productontology.org/id/Head_and_neck_cancer>  <http://www.w3.org/2000/01/rdf-schema#comment> ?b .
    }""")

for row in qres:
    print("The result is: %s" % row)

```

    The result is: Head and neck cancer is a group of cancers that starts in the mouth, nose, throat, larynx, sinuses, or salivary glands. Symptoms for head and neck cancer may include a lump or sore that does not heal, a sore throat that does not go away, trouble swallowing, or a change in the voice. There may also be unusual bleeding, facial swelling, or trouble breathing.
    About 75% of head and neck cancer is caused by the use of alcohol or tobacco. Other risk factors include betel quid, certain types of human papillomavirus, radiation exposure, certain workplace exposures, and Epstein-Barr virus. Head and neck cancers are most commonly of the squamous cell carcinoma type. The diagnosis is confirmed by tissue biopsy. The degree of spread may be determined by medical imaging and blood tests.
    Not using tobacco or alcohol can reduce the risk for head and neck cancer. While screening in the general population does not appear to be useful, screening high risk groups by examination of the throat might be useful. Head and neck cancer often is curable if it is diagnosed early; however, outcomes are typically poor if it is diagnosed late. Treatment may include a combination of surgery, radiation therapy, chemotherapy, and targeted therapy. Following treatment of one head and neck cancer, people are at higher risk of having a second cancer.
    In 2015, head and neck cancers globally affected more than 5.5 million people (2.4 million mouth, 1.7 million throat, and 1.4 million larynx cancer), and it has caused over 379,000 deaths (146,000 mouth, 127,400 throat, 105,900 larynx cancer). Together, they are the seventh most-frequent cancer and the ninth most-frequent cause of death from cancer. In the United States, about 1% of people are affected at some point in their life, and males are affected twice as often as females. The usual age at diagnosis is between 55 and 65 years old. The average 5-year survival following diagnosis in the developed world is 42-64%.
     
    
    (Source: Wikipedia, the free encyclopedia, see http://en.wikipedia.org/wiki/Head_and_neck_cancer)
    


```python
#Iterates over triples in the dataset and print them outin the form of subject, predicate, object
g.parse("http://www.productontology.org/doc/Head_and_neck_cancer#rdfa")
for s, p, o in g:
    print(s, p, o)
```

    http://dbpedia.org/resource/University_Hospital_Heidelberg http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://www.productontology.org/ http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://xmlns.com/foaf/0.1/Document
    http://purl.org/dc/elements/1.1/title http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/# http://www.w3.org/2000/01/rdf-schema#seeAlso http://purl.org/goodrelations/v1
    http://purl.org/dc/elements/1.1/rights http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/# http://www.w3.org/2002/07/owl#imports http://purl.org/goodrelations/v1
    http://purl.org/dc/elements/1.1/creator http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://xmlns.com/foaf/0.1/homepage http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/# http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#Ontology
    http://purl.org/dc/elements/1.1/contributor http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://xmlns.com/foaf/0.1/name Royal Infirmary of Edinburgh
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://www.productontology.org/ http://xmlns.com/foaf/0.1/primaryTopic http://www.productontology.org/#
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#Class
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label 頭頸部癌
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://www.productontology.org/# http://purl.org/dc/elements/1.1/title PTO: The Product Types Ontology for Semantic Web-based E-Commerce
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Cáncer de cabeza y cuello
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Kanker kepala dan leher
    http://dbpedia.org/resource/University_Hospital_Heidelberg http://dbpedia.org/ontology/country http://dbpedia.org/resource/Germany
    http://www.w3.org/2002/07/owl#deprecated http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#seeAlso http://dbpedia.org/resource/Head_and_neck_cancer
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://dbpedia.org/ontology/bedCount 922
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label سرطان‌های سر و گردن
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://xmlns.com/foaf/0.1/name Glasgow Royal Infirmary
    http://www.productontology.org/id/Head_and_neck_cancer http://xmlns.com/foaf/0.1/homepage http://www.productontology.org/doc/Head_and_neck_cancer.html
    http://www.productontology.org/doc/Head_and_neck_cancer http://xmlns.com/foaf/0.1/primaryTopic http://www.productontology.org/id/Head_and_neck_cancer
    http://dbpedia.org/resource/Royal_Infirmary_of_Edinburgh http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://www.w3.org/2007/05/powder-s#describedby http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#isDefinedBy http://www.productontology.org/#
    http://www.productontology.org/# http://purl.org/dc/elements/1.1/creator Martin Hepp
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#comment Head and neck cancer is a group of cancers that starts in the mouth, nose, throat, larynx, sinuses, or salivary glands. Symptoms for head and neck cancer may include a lump or sore that does not heal, a sore throat that does not go away, trouble swallowing, or a change in the voice. There may also be unusual bleeding, facial swelling, or trouble breathing.
    About 75% of head and neck cancer is caused by the use of alcohol or tobacco. Other risk factors include betel quid, certain types of human papillomavirus, radiation exposure, certain workplace exposures, and Epstein-Barr virus. Head and neck cancers are most commonly of the squamous cell carcinoma type. The diagnosis is confirmed by tissue biopsy. The degree of spread may be determined by medical imaging and blood tests.
    Not using tobacco or alcohol can reduce the risk for head and neck cancer. While screening in the general population does not appear to be useful, screening high risk groups by examination of the throat might be useful. Head and neck cancer often is curable if it is diagnosed early; however, outcomes are typically poor if it is diagnosed late. Treatment may include a combination of surgery, radiation therapy, chemotherapy, and targeted therapy. Following treatment of one head and neck cancer, people are at higher risk of having a second cancer.
    In 2015, head and neck cancers globally affected more than 5.5 million people (2.4 million mouth, 1.7 million throat, and 1.4 million larynx cancer), and it has caused over 379,000 deaths (146,000 mouth, 127,400 throat, 105,900 larynx cancer). Together, they are the seventh most-frequent cancer and the ninth most-frequent cause of death from cancer. In the United States, about 1% of people are affected at some point in their life, and males are affected twice as often as females. The usual age at diagnosis is between 55 and 65 years old. The average 5-year survival following diagnosis in the developed world is 42-64%.
     
    
    (Source: Wikipedia, the free encyclopedia, see http://en.wikipedia.org/wiki/Head_and_neck_cancer)
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#subClassOf http://schema.org/Product
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Kopf-Hals-Karzinom
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#subClassOf http://purl.org/goodrelations/v1#ProductOrService
    http://xmlns.com/foaf/0.1/primaryTopic http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/# http://purl.org/dc/elements/1.1/contributor The class abstracts and translations of labels are taken from Wikipedia, the free encyclopedia.
    http://purl.org/dc/elements/1.1/subject http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2007/05/powder-s#describedby http://www.productontology.org/doc/Head_and_neck_cancer.ttl
    http://xmlns.com/foaf/0.1/Document http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#Class
    http://www.productontology.org/doc/Head_and_neck_cancer.rdf http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://xmlns.com/foaf/0.1/Document
    http://dbpedia.org/resource/University_Hospital_Heidelberg http://xmlns.com/foaf/0.1/name University Hospital Heidelberg
    http://www.productontology.org/# http://www.w3.org/2000/01/rdf-schema#comment The Product Types Ontology: Good identifiers for product types based on Wikipedia
    
    This service provides GoodRelations-compatible class definitions for any type of product or service that has an entry in the English Wikipedia.
    
    Vocabulary:    http://www.productontology.org/#
    Namespace:     http://www.productontology.org/
    
    The Product Types Ontology is designed to be used in combination with GoodRelations, a standard vocabulary for the commercial aspects of offers.
    
    See http://purl.org/goodrelations/ for more information.
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://schema.org/Hospital
    http://dbpedia.org/resource/Glasgow_Royal_Infirmary http://dbpedia.org/ontology/bedCount 1077
    http://www.productontology.org/doc/Head_and_neck_cancer.ttl http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://xmlns.com/foaf/0.1/Document
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Halskræft
    http://purl.org/dc/terms/license http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Head and neck cancer
    http://www.productontology.org/doc/Head_and_neck_cancer.rdf http://xmlns.com/foaf/0.1/primaryTopic http://www.productontology.org/id/Head_and_neck_cancer
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Tumori della testa e del collo
    http://www.productontology.org/# http://purl.org/dc/terms/license http://creativecommons.org/licenses/by-sa/3.0/
    http://www.productontology.org/doc/Head_and_neck_cancer http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://xmlns.com/foaf/0.1/Document
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#seeAlso http://www.productontology.org/
    http://www.productontology.org/# http://www.w3.org/2000/01/rdf-schema#label The Product Types Ontology for Semantic Web-based E-Commerce
    http://schema.org/Product http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#Class
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://dbpedia.org/ontology/country http://dbpedia.org/resource/Scotland
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#seeAlso http://www.productontology.org/doc/Head_and_neck_cancer
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Cancer des voies aérodigestives supérieures
    http://dbpedia.org/resource/Aberdeen_Royal_Infirmary http://xmlns.com/foaf/0.1/name Aberdeen Royal Infirmary
    http://xmlns.com/foaf/0.1/page http://www.w3.org/1999/02/22-rdf-syntax-ns#type http://www.w3.org/2002/07/owl#AnnotationProperty
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label Գլխի և պարանոցի շրջանների քաղցկեղ
    http://www.productontology.org/id/Head_and_neck_cancer http://xmlns.com/foaf/0.1/page http://en.wikipedia.org/wiki/Head_and_neck_cancer
    http://www.productontology.org/# http://purl.org/dc/elements/1.1/subject E-Commerce, E-Business, GoodRelations, Ontology, Wikipedia, DBPedia
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2007/05/powder-s#describedby http://www.productontology.org/doc/Head_and_neck_cancer.rdf
    http://www.productontology.org/# http://www.w3.org/2002/07/owl#versionInfo 2019-09-12T18:11:54.533773
    http://www.productontology.org/doc/Head_and_neck_cancer.ttl http://xmlns.com/foaf/0.1/primaryTopic http://www.productontology.org/id/Head_and_neck_cancer
    http://www.productontology.org/# http://purl.org/dc/elements/1.1/rights The class definition texts are taken from Wikipedia, the free encyclopedia under a Creative Commons Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0) license, see http://creativecommons.org/licenses/by-sa/3.0/. Accordingly, all ontology class definitions provided in here are available under the very same license.
    http://www.productontology.org/id/Head_and_neck_cancer http://www.w3.org/2000/01/rdf-schema#label سرطان الرأس والعنق
    
