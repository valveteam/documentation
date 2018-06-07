---
author: Joshua Efiong
lastUpdated: 06-06-2018
email: ms2314@cam.ac.uk
---

# Principles of Digital Development

<!-- TOC -->

- [Principles of Digital Development](#principles-of-digital-development)
    - [Introduction](#introduction)
    - [Design With The User](#design-with-the-user)
    - [Understand the Ecosystem](#understand-the-ecosystem)
    - [Design for Scale](#design-for-scale)
    - [Build for Sustainability](#build-for-sustainability)
    - [Be Data Driven](#be-data-driven)
    - [Use Open Data, Open Standards, Open Source, Open Innovation](#use-open-data-open-standards-open-source-open-innovation)
    - [Reuse and Improve](#reuse-and-improve)
    - [Address Privacy and Security](#address-privacy-and-security)
    - [Be Collaborative](#be-collaborative)

<!-- /TOC -->

## Introduction

The Unicef Principles for Digital Development are a series of key objectives that reflect the lessons learned by the development community in technology-enabled programs. A document outlining what all these principles are and how they should be adhered to can be found [here](/documents/unicef_principles.pdf).

The purpose of this page is to self-evaluate our practices against these principles. Each of the nine principles will be briefly described, and then discussed in the context of this project.

## Design With The User

A technology-enabled development program should develop a solution that is context-appropriate. Projects should be iterated on until they meet the needs and requirements of the real user. In most development programs the end-user would probably be a user on the ground in the target country. In our case however, we were constructing a solution that would form the constituent part of a photocatalytic reactor being constructed by our partner organisation, [Majico](https://majico.org). The needs and requirements we needed to fulfil with our solution therefore came just as much from them as from the target country.

We worked closely with Peter from Majico, including going to the team's office within the Materials Science and Metallurgy Department on the West Cambridge Site. We were able to get any questions answered, as well as communicate regularly with Peter to check our proposals against Majico's requirements. Through our communication with Majico, we were also able to plan for their adoption of our designs, software and documentation.

Our communication with Majico was much closer in the second half of the project than the first, and one thing we could have improved upon is how quickly we got channels of communication between our team and the Majico team open. We did not pay as close attention to the environment and users that would be experienced in Tanzania as we could have done, though this was probably not as important for our project as it might have been for other development-based projects.

## Understand the Ecosystem

The second principle is written with the aim to both participate in networks and communities and to adhere to existing legal, technical and regulatory standards in the engineering work. There is an inherent community aspect to the work we undertook, in the sense that other teams in our department were working on other technology-enabled development projects simultaneously. However, on this front we did not really pursue anything further.

We fared much better on the standards front. We should have paid closer to attention to legal and regulatory policies, and how these might affect our design. However, in some circumstances we did try to adhere to technological standards. An example is checking our designs for through-hole pads on our circuit boards adhered to IPC standards (the relevant standard numbers are IPC-7251, IPC-2222 and IPC-2221). An improvement we could have made, with time permitting, was to apply similar attention to technological standards across every aspect of our design process rather than a select few. However, our use of standards to clarify points of uncertainty, such as the size of a through-hole pad, was encouraging.

## Design for Scale

This principle aims to ensure that these projects are designed such that they are able to scale from prototype to thousands of users or more with relative ease.

We tried to assess and mitigate dependencies that might limit scalability from the start. We aimed to ensure we did not utilise hard-to-find or obscure parts in our designs by ensuring that every component we used in our design could be sourced either from RS Online or Farnell, two major engineering suppliers in the UK. An extension that we could have conducted in this regard is to get a better idea of the stock and suppliers that are available in Tanzania, as we ended the project with little idea of how replicable or customisable our solution would be in other countries.

Throughout the project much has been made of the case for and against modularisation. Our system's three modules could be reduced to two if required. However, one benefit of modularisation in terms of the Unicef Principles is that each submodule could be redesigned independently if required due to supply or contextual issues in a given country as the solution scaled.

Another aspect of this principle is the 'systems' approach to design. Considering implications of the design beyond the current application was not something we factored into our work, and we could have done this better. It's worth noting that the scalability of our solution depends on Majico's implementation somewhat, and we look forward to seeing how this progresses.

One design decision worth highlighting is the inclusion of an 'expansion' header on the control board, to interface with additional sensors or data logging modules during testing. For example, this could be used to write data to an SD card to be analysed later during testing, or to integrate a flow rate sensor to calibrate the valve. This was inherently designed into our solution with scalability in mind.

## Build for Sustainability

This principle refers largely to good practices in terms of financial health, utilisation of local communities and developers and engagement with local governments. Much of this is beyond our control as a team, and rests more squarely with Majico's team. However, one thing we could have considered more closely is how easily our designs and work could be handed over to local communities and developers in the countries in which our solution would be deployed.

## Be Data Driven

If this principle is adhered to, each project should have milestones or impacts that can be measured at discrete points, with a focus on outcomes. The use of real-time information to inform decisions forms a part of this, as does leveraging data to evaluate solutions.

We were reasonably good at using data to drive our recommendations about which components to use, how to optimise costs and our designs and what power consumption would be demanded. An example of this is the use of data to justify a choice of driver chip on the flow module, or the choice of light sensor device on the light sensor module. We have also communicated to Majico the possibility of using our aforementioned expansion header to write data to SD cards and the like to use data to evaluate the efficacy of our solution.

## Use Open Data, Open Standards, Open Source, Open Innovation

This principle is the one on which we performed the best, from our perspective. We tried to use open source tools for our designs wherever possible, and by using KiCad to design our circuitry we have based almost all of our engineering design work on an open source tool.

In the software development aspect of this project, we have endeavoured to never 'reinvent the wheel', leveraging open source libraries to interface with our various components and devices. All of our own software is also open source, as are all of our circuit schematics, PCB layouts and files required for PCB manufacture including Gerbers and PDFs. We also have forks of the open source libraries we use in our work inside our organisation's GitHub namespace so we are able to quickly contribute to the libraries if bugs are found or modifications required.

## Reuse and Improve

We used and extended existing circuit designs surrounding our key choices of components wherever possible to minimise the possibility of error. These circuit designs were either found as examples in the datasheets for the components of interest or online via schematics for breakout boards that have been made open source or otherwise. We also reused existing open-source libraries wherever possible to minimise the software we were writing from scratch, and so we could focus on the quality of our final software.

We also designed the entire system in a modular rather than monolithic manner, which favours the iterative and independent improvement of individual parts of the subsystem. Thus we addressed the two points highlighted by Unicef under this principle fairly well.

## Address Privacy and Security

This principle is intended to ensure the risks to the security of end users and their data are mitigated as far as possible by technology-enabled development projects. As we do not inherently collect user data, in the sense that all the data considered by our systems are specific to the valve itself, this principle is not much of a concern to us and can be largely ignored. One thing worth raising is that it may be possible to identify the location of a valve by the light level data we collect, and this may present a very small risk to the end user, but this was considered to be of minimal risk.

## Be Collaborative

The documentation that this article forms a part of highlights our work, results, lessons learned, processes and best practices, and shares them openly on the internet for anyone to read. All of our work is published and free to use via our GitHub.

All members of the team participated in as many different steps of the manufacture process, for example we tried to ensure everyone participated in electronic design and manufacture, as well as ensuring everyone wrote some software for the control system. This prevented silos forming within the team that might have formed if we split our team members by function.

We engaged our demonstrator Fergus Riche at many aspects of the project to ask for his opinion on our design decisions and for advise where needed. We often discussed progress on the project with the other electrical team on this project in our department, who was working on a soil moisture detection system, which often raised interesting points. We also shared stock and resources with that team when required, including components for wiring looms and crimps as well as borrowing Arduino Unos for testing when we absolutely had to! We also shared our personal tools that we used to fill in the gaps in the Dyson Centre with the other teams when they needed things.

All the teams and personnel working on the 'Technology for the Poorest Billion' project fostered a truly collaborative atmosphere that we really appreciated, so thank you to them for that!
