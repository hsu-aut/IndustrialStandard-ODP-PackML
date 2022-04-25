# ISA88 Ontology-Design-Pattern

## Introduction

The development of software functionalities, or applications in general, that monitor and analyze manufacturing related data in order to improve, support or automate processes, is becoming increasingly important in industry. These applications require several information from different data sources in their context. An application that is planning a maintenance workers daily schedule for instance, requires several information about machine statuses, production plans and inventory, which resides in different systems likes Programmable Logical Controllers (PLC) or Structured Query Language (SQL) databases. Furthermore, manufacturing companies usually run machines and software systems from different vendors or of different ages. The schemata used in such systems do therefore not follow a certain standard, i.e. they are very heterogeneous in their semantics. When building such applications, accessing, searching and understanding the data sources is becoming a very time intensive, manual and error prone procedure that is repeated for every newly build application and for every newly introduced data source. To allow for an eased access, searching and understanding of these heterogeneous data sources, an ontology can be used to integrate all heterogeneous data sources in one schemata. 

This repository contains an ontology of ISA88 which is a standard to describe properties. We maintain a whole list of standard-based ontologies, check out these links:
 - [VDI 2206](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI2206)
 - [VDI 2860](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI2860)
 - [VDI 3682](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI3682)
 - [DIN 8580](https://github.com/hsu-aut/IndustrialStandard-ODP-DIN8580)
 - [DIN EN 61360](https://github.com/hsu-aut/IndustrialStandard-ODP-DINEN61360)
 - [WADL](https://github.com/hsu-aut/IndustrialStandard-ODP-WADL)
 - [DIN EN 62264-2](https://github.com/hsu-aut/IndustrialStandard-ODP-DINEN62264-2)
 - [OPC UA](https://github.com/hsu-aut/IndustrialStandard-ODP-OPC-UA)
 - [ISO 22400-2](https://github.com/hsu-aut/IndustrialStandard-ODP-ISO22400-2)


## ISA88

The ANSl/ISA-TR88.00.02-2015 [1] technical report is intended to build upon and formalize the concepts of the PackML guidelines 
and to show application examples. PackML is used to provide a standard description of states for comparison of machine statuses 
among different types of machines. The concepts used in this standard describe general states and transitions of a 
packaging machine, but could be used to describe the states and transitions of any machine. This is due to the reason, 
that the state machine is very generic and does not hold any concept specific to packaging machines.

There are mainly three separate state machines (see Fig. 1):
* Active, with two main sub-states:
  * Acting state: States which represents some processing activity
  * Wait state: States used to identify that a machine has achieved a defined set of conditions
* Stopping: states and transitions that result in a stopped machine
* Aborting: states and transitions that result in an aborted machine program

A detailed description of all states can be found in the rdfs:comment of the ODP or the standard [ANSl/ISA ANSl/ISA-TR88.00.02-2015].

![](./pictures/ISA88_SM1.png?raw=true "ISA 88 State Machine")<br></br>
Figure 1: PackML state machine overview

Fig. 2 shows the states that are contained within the “Active” state machine of Fig. 1. The state machine contains PackML 
defined states and transitions. Transitions called „SC“ do fire as soon as the state is complete. The state „Held“ represents 
the interrupt of executing because of internal disturbances, while the „Suspended“ state reflects the interrupt because of 
external disturbances.

![](./pictures/ISA88_SM2.png?raw=true "ISA 88 State Machine")<br></br>
Figure 2: PackML main state machine

Exemplary Competency Questions that could be answered with this ODP:<br></br>
Table 1: Example Competency Questions
![](./pictures/ISA88_exCQ.png?raw=true "exampleCQ")

[1] ANSl/ISA-TR88.00.02-2015] ANSl/ISA-TR88.00.02-2015. Machine and Unit States: 
An implementation example of ANSl/ISA-88.00.01, 02.2015.

## Usage

If you want to this ontology design pattern, the easiest way is to directly import it into your ontology via `owl:imports` statements. Make sure to reference a fixed release version so that you can't get surprised by future changes. To do so, click on the branch selection right below the number of commits and select a tag from the dropdown, e.g. v1.4.2. Then navigate to the .owl-file and open the raw file. For this example it would be https://raw.githubusercontent.com/hsu-aut/IndustrialStandard-ODP-ISA88/v1.4.2/ISA88.owl. You can use this URL in an `owl:imports` statement of your ontology. If you're having trouble using this URL in a tool like Protégé, try opening your ontology with a text editor and simply inserting your imports manually.
An example of an imports section looks like this:

```xml
<owl:Ontology rdf:about="http://www.hsu-ifa.de/ontologies/capability-model#">
    <owl:versionIRI rdf:resource="http://www.hsu-ifa.de/ontologies/capability-model/1.0.0#"/>
    <owl:imports rdf:resource="https://raw.githubusercontent.com/hsu-aut/IndustrialStandard-ODP-ISA88/v1.4.2/ISA88.owl"/>
</owl:Ontology>
```
Of course you can also clone or download this repository and import an ODP from a local copy. The advantage of the first approach is that tools like Protégé or TopBraid Composer will directly use the ontologies from the internet and you can simply increase the version number in case you want to use a newer version of our ODPs.

## Tool Support
In case you want to make creating individuals from these TBoxes a lot easier, check out our 'Lightweight Industrial Ontology Design Support Tool' (LiOnS). It is designed to create RDF models using the Ontology Design Patterns of this repository. This enables users to:
- Semi-automatically design RDF models (only variable parts of the graph have to be defined)
- Consistent modelling, without being an ontology expert
- Downloading Turtle serialized models or SPARQL INSERTs

For more information, see https://github.com/hsu-aut/lion.

## Further reading:
- C. Hildebrandt, A. Köcher, C. Kustner, C.-M. Lopez-Enriquez, A.W. Muller, B. Caesar, C.S. Gundlach, A. Fay: Ontology Building for Cyber-Physical Systems: Application in the Manufacturing Domain. IEEE Transactions on Automation Science and Engineering, 2020, S. 1–17.
-  C. Hildebrandt, S. Törsleff, T. Bandyszak, B. Caesar, A. Ludewig, A. Fay: Ontology Engineering for Collaborative Embedded Systems – Requirements and Initial Approach. In: Schäfer, Karagiannis (Hrsg.): Fachtagung Modellierung, 2018.
- C. Hildebrandt, S. Törsleff, B. Caesar, A. Fay: Ontology Building for Cyber-Physical Systems: A domain expert centric approach. In: 2018 14th IEEE Conference on Automation Science and Engineering (CASE 2018), 2018.
- C. Hildebrandt, A. Scholz, A. Fay, T. Schröder, T. Hadlich, C. Diedrich, M. Dubovy, C. Eck, R. Wiegand: Semantic Modeling for Collaboration and Cooperation of Systems in the production domain. In: 22nd IEEE Emerging Technology and Factory Automation (ETFA), 2017.
