<img src="https://github.com/OntologyCarDamage/OCD/blob/main/images/Logo_OCD.png" style="width: 200px; height: auto;" align="right" />
# Ontology for Car Damage (OCD)
* **Ontology for Car Damage (OCD)** is a domain-specific ontology designed for modeling car damage information. It provides a formal representation of entities and relationships involved in damage assessment, enabling automated data analysis for decision support in assessment, cost estimation, and repair planning. The ontology is structured using RDF (Resource Description Framework) and OWL (Web Ontology Language) standards to ensure accurate and consistent representation of car-related data.

### Version Information

- **Ontology IRI:** [https://developer.iziflo.com/research/ocd](https://developer.iziflo.com/research/ocd)
- **Version IRI:** [https://developer.iziflo.com/research/ocd/1.0](https://developer.iziflo.com/research/ocd/1.0)
- **Acronym:** OCD
- **Developed By:** Syartec
- **Contact:** Hamid Ahaggach, hahaggach@syartec.com

### Data Properties

The ontology includes various data properties such as:

- **CarBrand:** Represents the brand of a car.
- **CarModel:** Represents the model name of a car.
- **CarYear:** Represents the manufacturing year of a car.
- **CarColor:** Represents the exterior color of a car.
- **CarPrice:** Represents the price of a car.
- **CarRegistration:** Represents the registration number of a car.
- **FuelType:** Represents the type of fuel used by a car.
- **CarMileage:** Represents the total mileage of a car.
- **hasPartName:** Represents the name of a car part.
- **CarPartMaterial:** Represents the material used to make a car part.
- **IsDamaged:** Specifies whether a car part is damaged.
- **Place:** Represents the location of a damaged car part.
- **PartPrice:** Represents the price of a car part.
- **DamageType:** Represents the type of damage sustained by a car or car part.
- **RepairCost:** Represents the estimated cost of repairing the damage.
- **Severity:** Specifies the severity of the damage to a car part.
- **RepairAction:** Represents the recommended repair action for the damage.

### Object Properties

The ontology includes object properties such as:

- **hasCarPart:** Describes the connection between a vehicle and its constituent parts.
- **hasDamage:** Illustrates the link between a car part and the specific damage it has incurred.
- **hasComponent:** Defines the relationship between $CarParts$ and their sub-components, signifying that a particular car part encompasses other individual parts. This property facilitates the modeling of hierarchical structures and compositions within the ontology, enabling a detailed representation of relationships among different car components.
- **inPart:** The reverse of the 'hasDamage' property, indicating the specific part within which damage is present.
- **isPartOf:** The reverse of the 'hasCarPart' property, denoting that a specific component is part of a larger car entity.

  
## Usage
The OCD ontology facilitates the integration and analysis of car damage data. It allows users to represent complex relationships between cars, their parts, and the damages they incur. Researchers, automotive industry professionals, and decision-makers can utilize this ontology for automating data analysis, decision support, and research purposes related to car damage assessment and repair planning.

## Data Format
The ontology is represented in RDF format and adheres to the XML schema specified in the document. The RDF data consists of individuals, classes, and properties, defining the relationships between them.

For more information and detailed documentation, visit the [OCD Ontology Documentation](http://industryportal.enit.fr/ontologies/OCD).

---

# Car Damage Ontology SPARQL Queries

## Introduction

This repository provides a set of SPARQL queries to work with a car damage ontology. The ontology contains information about different types of damages that can occur to a car, various car parts affected by damages, severity of each type of damage, relationships between car parts and damages, and how extracted information from unstructured reports can be linked to specific concepts in the ontology.

## Competency Questions and Queries

### What are the different types of damages that can occur to a car?
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?damageType
WHERE {
  ?damageType rdf:type ocd:Damage .
}
```

### 2: What are the various car parts that can be affected by damages?
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?carPart
WHERE {
  ?carPart rdf:type ocd:CarParts .
}
```
Explanation: This query retrieves all instances of the class `ocd:CarParts`, representing the various car parts that can be affected by damages.

### 3: How severe is each type of damage?
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?damageType ?severity
WHERE {
  ?damageType rdf:type ocd:Damage .
  ?damageType ocd:Severity ?severity .
}
```
Explanation: This query retrieves the severity of each type of damage by selecting instances of `ocd:Damage` and their associated severity using the property `ocd:Severity`.

### 4: What are the possible relationships between car parts and damages?
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?carPart ?damageType
WHERE {
  ?carPart rdf:type ocd:CarParts .
  ?carPart ocd:hasDamage ?damageType .
}
```
Explanation: This query retrieves instances of `ocd:CarParts` that are related to specific damage types through the property `ocd:hasDamage`.

### 5: How can the extracted information from unstructured reports be linked to specific concepts in the ontology?
*(To link unstructured reports to ontology concepts, we use the immatriculation number for identification.)*
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?carBrand ?carModel ?carRegistration 
?carPartName ?place ?damageType ?severity 
?repairAction WHERE { ?car rdf:type ocd:Car .
  ?car ocd:CarBrand ?carBrand .
  ?car ocd:CarModel ?carModel .
  ?car ocd:CarRegistration ?carRegistration .
  FILTER (CONTAINS(?carRegistration, "FR-759-803"))
  ?car ocd:hasCarPart ?carPart .
  ?carPart rdf:type ocd:CarParts .
  ?carPart ocd:PartName ?carPartName .
  ?carPart ocd:hasDamage ?damage .
  ?damage rdf:type ocd:Damage .
  ?damage ocd:DamageType ?damageType .
  OPTIONAL {?carPart ocd:Place ?place .}
  OPTIONAL {?damage ocd:RepairAction ?repairAction .}
  OPTIONAL {?damage ocd:Severity ?severity .}}
```
This SPARQL query retrieves information of the report FR-759-803 from the ontology based on the immatriculation number of the car ("FR-759-803"). It selects specific details such as car brand, car model, car registration, car part names, places, damage types, severities, and repair actions associated with the provided immatriculation number, linking the unstructured reports to ontology concepts.

### 6: How can the ontology be used to enhance the accuracy and relevance of extracted relations between car components and damages?
In this query:

- `?car`, `?carBrand`, `?carModel`, `?carPart`, `?damage`, `?damageType`, and `?severity` are variables representing individual instances of cars, car brands, car models, car components, damages, damage types, and severity, respectively.
- The query retrieves instances of cars, their brands, models, associated car components, damages, damage types, and severity information from the ontology.
- By executing this query against your RDF triplestore, you can verify that the extracted information aligns with the ontology, ensuring accuracy and relevance in the relationships between car components and damages.

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?car ?carBrand ?carModel ?carPart ?damage ?damageType ?severity
WHERE {
    ?car rdf:type ocd:Car .
    ?car ocd:CarBrand ?carBrand .
    ?car ocd:CarModel ?carModel .
    ?car ocd:hasCarPart ?carPart .
    ?carPart rdf:type ocd:CarParts .
    ?carPart ocd:hasDamage ?damage .
    ?damage rdf:type ocd:Damage .
    ?damage ocd:DamageType ?damageType .
    ?damage ocd:Severity ?severity .
}

```

### 7: What is the overall structure and hierarchy of car components and damage types within the ontology?
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?class ?subclass
WHERE {
  ?subclass rdfs:subClassOf ?class .
  VALUES ?class { ocd:CarParts ocd:Damage }
}
```
### SPARQL Query for Damage Price Prediction

This SPARQL query retrieves data relevant for predicting damage repair prices. It includes details about cars, their attributes, parts, and damages. The retrieved information can be used to build a dataset for regression analysis to predict damage repair costs.

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ocd: <https://developer.iziflo.com/research/ocd/>

SELECT ?carBrand ?carModel ?carYear ?carColor ?carPrice ?carRegistration 
?fuelType ?carMileage ?hasPartName ?carPartMaterial ?isDamaged ?place 
?partPrice ?damageType ?severity ?repairAction ?repairCost WHERE { 
    ?car rdf:type ocd:Car .
    ?car ocd:CarBrand ?carBrand .
    ?car ocd:CarModel ?carModel .
    ?car ocd:CarYear ?carYear .
    ?car ocd:CarColor ?carColor .
    ?car ocd:CarPrice ?carPrice .
    ?car ocd:CarRegistration ?carRegistration .
    ?car ocd:FuelType ?fuelType .
    ?car ocd:CarMileage ?carMileage .
    ?car ocd:hasCarPart ?carPart .
    ?carPart rdf:type ocd:CarParts .
    ?carPart ocd:PartName ?hasPartName .
    ?carPart ocd:CarPartMaterial ?carPartMaterial .
    ?carPart ocd:IsDamaged ?isDamaged .
    OPTIONAL {?carPart ocd:Place ?place .}
    OPTIONAL {?carPart ocd:PartPrice ?partPrice .}
    OPTIONAL {?carPart ocd:hasDamage ?damage .
              ?damage rdf:type ocd:Damage .
              ?damage ocd:DamageType ?damageType .
              ?damage ocd:Severity ?severity .
              OPTIONAL {?damage ocd:RepairAction ?repairAction .}
              OPTIONAL {?damage ocd:RepairCost ?repairCost .}
    }
}


```
### Example of results :


| carBrand | carModel | carYear | carColor | carPrice | carRegistration | fuelType | carMileage | hasPartName | carPartMaterial | isDamaged | place   | partPrice | damageType | severity | repairAction | repairCost |
|----------|----------|---------|----------|----------|-----------------|----------|------------|-------------|-----------------|-----------|---------|-----------|------------|----------|--------------|------------|
| Honda    | Civic    | 2019    | Red      | $20,000  | ABC123          | Petrol   | 50000      | Engine      | Steel           | true      | -  | $500      | Mechanical | Moderate | Repair       | $80       |
| Toyota   | Camry    | 2018    | Blue     | $22,000  | XYZ789          | Petrol   | 60000      | Transmission| Aluminum        | true     | -  | $300      | Electrical | Minor    | Replacement  | $360       |
| Ford     | Mustang  | 2020    | Black    | $25,000  | DEF456          | Petrol   | 45000      | Headlight   | Glass         | true      | - | $150      | Dent   | Minor    | Replacement    | $180       |

