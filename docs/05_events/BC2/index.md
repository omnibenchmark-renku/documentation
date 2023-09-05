
# [BC]2 Half-day tutorial - September 11th 2023

## Resources 

 - [__Working document__](https://docs.google.com/document/d/1P5OdJnTJjsivWTV5-BpOhCQKAywOWM_o1FXkZ_YhukE/edit?usp=sharing)

## __Sections__

- [__Step-by-step guide__](step_by_step.md)

- [__Presentations__](presentations.md)

- [__Hands-on sessions__](hands-on_sessions.md)

## __Overview__

_Intermediate level ––  Method benchmarking  ––  benchmarks  ––  omics datasets_

The rise of large-scale omics datasets has led to a growing number of methods to model and interpret them, making it hard to identify performant computational methods to use in discovery research. Method benchmarking is critical to dissect important steps of an analysis pipeline, formally assess performance across common situations and edge cases, and ultimately guide users. While some form of comparison is standard practice in method development, current approaches have several limitations (Sonrel et al. 2022). One main issue is that benchmarks are not easily extensible and with the constant emergence of new approaches, this often leads to rapidly outdated or even contradicting and irreproducible conclusions. We believe that if benchmarks were organized in a systematic way and conducted at a higher standard, they will have a considerably greater impact in guiding best practice in employing computational methods for discovery research.

To address these issues, we created Omnibenchmark, a modular and extensible framework based on the free open-source platform renku. The framework connects data, method, and metric modules via a knowledge graph, which tracks relevant metadata (e.g. software environment, parameters and commands). Results can be viewed in a dashboard or openly accessed, and new modules can be added easily with pre-configured templates. To facilitate community contributions for adding new reference datasets, evaluation metrics or computational methods, we maintain a series of pre-configured templates. Each element of Omnibenchmark is packaged with all dependencies and can be inspected, re-used, modified, and integrated onto other platforms in compliance with the FAIR principles.
Learning objectives

At the end of the tutorial, participants should be able to:

- understand components of the Omnibenchmark framework
    
- submit new reference datasets, methods or metrics modules
    
- visualize and re-use output results and performance summaries

| **Time**      | **Activity**                                                              |
|---------------|---------------------------------------------------------------------------|
| 09:00 – 09:30 | Introduction to Omnibenchmark framework                                   |
| 09:30 – 10:30 | Group creation & first hands-on session following our step-by-step guide  |
| 10:30 – 10:45 | _Coffee break_                                                            |
| 10:45 - 11:15 | Technical aspects of Omnibenchmark module creation                        |
| 11:15 – 12:00 | Second hands-on session                                                   |
| 12:00 – 12:15 | Q&A and Closing remarks                                                   |
| 12:15 – 13:00 | Lunch -  Join us for lunch, even if you only attend the morning workshop! |

## __Audience and requirements__

Maximum number of participants: 20

This tutorial is addressed to computational-minded researchers who are tasked with benchmarking methods that they are familiar with (or that they developed) as well as for benchmarkers that would like to port their work to our system.

Participants are expected to have :
- Intermediate knowledge of R or Python
- Basic knowledge of UNIX and version control systems (Git)
- Personal laptop with Wifi connection
- Code for method(s) and/ or benchmark(s)- (optional)

## __Organizers__

- Main presenters: Almut Lütge and Anthony Sonrel, PhD Students and co-developers of Omnibenchmark, Statistical Bioinformatics Group at the University of Zurich, Switzerland

- Technical collaborators and knowledge exchange: Dr. Izaskun Mallona, Statistical Bioinformatics Group at the University of Zurich (co-developer of Omnibenchmark); Dr. Charlotte Soneson, Research Associate, Computational Biology Platform, FMI and SIB Swiss Institute of Bioinformatics, Switzerland

- Introductions and support: Mark D. Robinson, Associate Professor, University of Zurich and SIB Group Leader, Switzerland

