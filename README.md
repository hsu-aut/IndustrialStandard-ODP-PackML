# PackML StateMachine: Ontology-Design-Pattern

## Introduction

The development of software functionalities, or applications in general, that monitor and analyze manufacturing related data in order to improve, support or automate processes, is becoming increasingly important in industry. These applications require several information from different data sources in their context. An application that is planning a maintenance workers daily schedule for instance, requires several information about machine statuses, production plans and inventory, which resides in different systems likes Programmable Logical Controllers (PLC) or Structured Query Language (SQL) databases. Furthermore, manufacturing companies usually run machines and software systems from different vendors or of different ages. The schemata used in such systems do therefore not follow a certain standard, i.e. they are very heterogeneous in their semantics. When building such applications, accessing, searching and understanding the data sources is becoming a very time intensive, manual and error prone procedure that is repeated for every newly build application and for every newly introduced data source. To allow for an eased access, searching and understanding of these heterogeneous data sources, an ontology can be used to integrate all heterogeneous data sources in one schemata. 

This repository contains an ontology of PackML which defines a standardized state machine. We maintain a whole list of standard-based ontologies, check out these links:
 - [VDI 2206](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI2206)
 - [VDI 2860](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI2860)
 - [VDI 3682](https://github.com/hsu-aut/IndustrialStandard-ODP-VDI3682)
 - [DIN 8580](https://github.com/hsu-aut/IndustrialStandard-ODP-DIN8580)
 - [DIN EN 61360](https://github.com/hsu-aut/IndustrialStandard-ODP-DINEN61360)
 - [WADL](https://github.com/hsu-aut/IndustrialStandard-ODP-WADL)
 - [DIN EN 62264-2](https://github.com/hsu-aut/IndustrialStandard-ODP-DINEN62264-2)
 - [OPC UA](https://github.com/hsu-aut/IndustrialStandard-ODP-OPC-UA)
 - [ISO 22400-2](https://github.com/hsu-aut/IndustrialStandard-ODP-ISO22400-2)


## PackML

The ANSI/ISA-TR88.00.02-2015 [1] technical report is intended to build upon and formalize the concepts of the PackML guidelines and to show application examples. PackML is used to provide a standard description of states for comparison of machine statuses 
among different types of machines. The concepts used in this standard describe general states and transitions of a packaging machine, but could be used to describe the states and transitions of any machine. This is due to the reason that the state machine is very generic and does not contain any concept specific to packaging machines.

Figure 1 shows all classes along with their subclasses of this ODP. A `StateMachine` individual is used as kind of a container for all states and transitions, which are connected to a  state machine via `consistsOfState` and `consistsOfTransition`, respectively. Accordin to PackML, there are two types of states, so called `Acting` and `Wait` states. While Acting States are states in which some kind of logic is executed, Wait States signal that a certain condition has been achieved and that the state machine is now awaiting an input to further proceed. 
Accordingly, there are two types of transitions: While a `Command` needs to be actively invoked by an operator or another system, a `StateComplete` is automatically triggered by completion of the previous state. A `StateComplete` almost always leads from an `Acting` into a `Wait` State. 
In order to correctly model the state machine as an ontology, the four transitions `hasOutgoingTransition`,  `hasIncomingTransition`, `hasSourceState` and `hasTargetState` need to be used. The first two connect states with transitions. While `hasOutgoingTransition` always connects a state with the following transition, `hasIncomingTransition` is used to relate a state to the transition(s) leading to this state (i.e., its predecessor transitions).
The latter two connect transition with states. While `hasSourceState` always connects a transition with its source state, `hasTargetState` connects transitions with their target state.
Two of these four transitions may be considered "syntactical sugar", as `hasOutgoingTransition` is the inverse of `hasSourceState` and `hasIncomingTransition` is the inverse of `hasTargetState`.

Please see the file `PackML-Example.ttl` for one complete PackML state machine. You might think that this is a very verbose state machine for a coffee machine. This state machine was purposefully modeled to show a full state machine. Note that PackML defines different so-called control "modes" that allow modeling less verbose state machines (e.g. for manual mode). See [1] for more information.

<p align="center">
    <img width="80%" src="./pictures/PackML_ClassDiagram.png?raw=true"><br>
Figure 1: Class diagram showing all classes and object properties of the PackML ODP
</p>


There are mainly three separate state machines (see Fig. 2):
* Active, with two main sub-states:
  * Acting state: States which represents some processing activity
  * Wait state: States used to identify that a machine has achieved a defined set of conditions
* Stopping: states and transitions that result in a stopped machine
* Aborting: states and transitions that result in an aborted machine program

A detailed description of all states can be found in the rdfs:comment of the ODP or the standard [ANSl/ISA ANSl/ISA-TR88.00.02-2015].

<p align="center">
    <img width="60%" src="./pictures/PackML_SM1.png?raw=true"><br>
Figure 2: PackML state machine overview
</p>

Fig. 3 shows the states that are contained within the “Active” state machine of Fig. 2. The state machine contains PackML 
defined states and transitions. Transitions called „SC“ do fire as soon as the state is complete. The state „Held“ represents 
the interrupt of executing because of internal disturbances, while the „Suspended“ state reflects the interrupt because of 
external disturbances.

<p align="center">
    <img width="100%" src="./pictures/PackML_SM2.png?raw=true"><br>
	Figure 3: PackML main state machine
</p>


Exemplary Competency Questions that could be answered with this ODP:<br></br>

Table 1: Example Competency Questions<br>
|  ID  | Competency Question | Answer | Restrictions / Notes |
|:----:|:-------------------:|:------:|:------------:|
|  1   | What's the current state of machine X? | Current state of the machine (e.g., Idle) |    |
|  2   | Why is machine X not in state Execute  | <ul><li>Machine X went from Execute to Held due to an internal issue</li><li>Machine X went from Execute to Suspended due to an external issue</li><li>Machine X went from Execute to Stopped due to a Stop command</li><li>Machine X went from Execute to Aborted due to an Abort command</li></ul> |    |
|  3   | Which machines are in Idle right now? | List of machines in Idle | May need separate ODP to model machines |
|  4   | What are possible states of machine X? | List of possible states | Useful if a subset of the full PackML SM is used |
|  5   | How much time did machine X spend in Idle? | Duration spend in Idle | Needs an ODP to model time and / or application to track state changes |
|  6   | For a given time frame, what was the availability of machine X? | Availability as a time or percentage (e.g. by looking at time spent in "normal" states) | Needs an ODP to model time and / or application to track state changes |

[1] ANSl/ISA-TR88.00.02-2015] ANSl/ISA-TR88.00.02-2015. Machine and Unit States: 
An implementation example of ANSl/ISA-88.00.01, 02.2015.

## Usage

If you want to this ontology design pattern, the easiest way is to directly import it into your ontology via `owl:imports` statements. Simply use the ontology IRI (W3ID) to import and make sure to reference a fixed release version so that you can't get surprised by future changes. Simply take the latest release or pick a specific version in case you don't need the latest updates. To import the latest version, you would import `http://www.w3id.org/hsu-aut/PackML/3.0.0`. You can use this IRI in an `owl:imports` statement of your ontology. If you're having trouble using this IRI in a tool like Protégé, try opening your ontology with a text editor and simply inserting your imports manually.
An example of an imports section looks like this:

```xml
<owl:Ontology rdf:about="http://www.hsu-ifa.de/ontologies/capability-model#">
    <owl:versionIRI rdf:resource="http://www.hsu-ifa.de/ontologies/capability-model/1.0.0#"/>
    <owl:imports rdf:resource="http://www.w3id.org/hsu-aut/PackML/3.0.0"/>
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
- C. Hildebrandt, S. Törsleff, T. Bandyszak, B. Caesar, A. Ludewig, A. Fay: Ontology Engineering for Collaborative Embedded Systems – Requirements and Initial Approach. In: Schäfer, Karagiannis (Hrsg.): Fachtagung Modellierung, 2018.
- C. Hildebrandt, S. Törsleff, B. Caesar, A. Fay: Ontology Building for Cyber-Physical Systems: A domain expert centric approach. In: 2018 14th IEEE Conference on Automation Science and Engineering (CASE 2018), 2018.
- C. Hildebrandt, A. Scholz, A. Fay, T. Schröder, T. Hadlich, C. Diedrich, M. Dubovy, C. Eck, R. Wiegand: Semantic Modeling for Collaboration and Cooperation of Systems in the production domain. In: 22nd IEEE Emerging Technology and Factory Automation (ETFA), 2017.
