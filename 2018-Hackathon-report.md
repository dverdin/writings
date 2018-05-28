**Disclaimer: This document express the views of the 2018 hackathon attendees. Consider all the points described as request for comments. This document should be the only reference for further discussions about the topics tackled during that hackathon.**

# 2018 Sympa Hackathon report
The 2018 Sympa hackathon was held in Strasbourg, France, from 22 to 24 of May.
It was organized by March chantreux, from RENATER, and hosted by the Strasbourg university.

## Attendees
Here is the list of the attendees and their main areas of expertise (in last name  - or handle - alphabetical order).

 - [March Chantreux](https://github.com/eiro) (RENATER): Perl expert, listmaster, Perlude creator
 - [Luc Didry](https://github.com/ldidry) (Framasoft): Framasoft listmaster, Sysadmin, Perl expert and experienced free software manager
 - Mathieu Durero (French young researchers confederation) ; "The noob": Mathieu is an experienced listmaster but didn't know anything about Perl or Ansible prior to the hackathon. His "naive" - though thoughtfull -  input has been very valuable during the three days.
 - [François Menabe](https://github.com/fmenabe) (Strasbourg University): Sysadmin, Ansible expert
 - [Racke](https://github.com/orgs/racke) (Linuxia.de): Debian package developer, member of the Dancer2 community, Linux and Perl expert
 - [Olivier Salaün](https://github.com/salaun-urennes1) (Rennes university): Sympa creator, Devops expert,
 - [David Verdin](https://github.com/dverdin) (RENATER): Former Sympa lead developer, experienced listmaster

Other people also came and went during the three days, stopping by for a short discussion and then leaving, but the list above contains those who remained most of the three days.

## Preamble

The main problem in Sympa development is that, considering the large opportunities of evolution that awaits the software, it is very hard to know where to start working on it ; And considering the heterogeneity of the community, it is very hard to know *how* to work.

Don't get me wrong: most people know exactly how and where *they* should start. Problem is: their personal point of view is, at best, only partially shared by other.

Consequently, amongst the three days of the hackathon, the first two were merely dedicated to discussions and the last one to actual coding experimentations.

The good news is that we achieved to agree on several points which could be very promising starts and, hopefully, lead to make actual progress in Sympa 7.0 development and in community involvement.

Here is the list of points discussed and the agreements we reached.

## Desirable future application design

 - all the code should be testable, with both unit tests on modules and functional tests on features
 - the code should expose several interfaces: REST, web, CLI, mail. SOAP could be deprecated once a REST interface is complete.
 - all Sympa executables (being daemons, web process or command line) should make use of a business object layer which should be independant from the persistance layer.
 - Reminder: during last year hackathon, we already agreed on using the following technologies:
   - [Dancer2](https://metacpan.org/pod/Dancer2) for the REST API,
   - [OpenAPI](https://github.com/OAI/OpenAPI-Specification) for the REST API specification,
   - [DBIx::Class](https://metacpan.org/pod/DBIx::Class) for Sympa database backend management and access.

## Sympa 7.0 target and methods of development

As we will not have the perfect Sympa right now, we should set some goals. A reasonable aim for Sympa 7.0 would be to be iso-functional with a refactored, testable code and a full  REST API.



## Community: roadmap

## Tests

## Coding practices

The aim of the discussion was to find a way to get rid of the *infrastructure code*. That's the part of the code that handles language mechanics instead focusing on application logic. For example, the "bless" command used to cast anything into an object could be easily replaced by Moo.

### What we agreed on

All these modules allow to drastically decrease the number of Sympa code lines without changing anything in the application logic. It would malke the code more readable and less error-prone.

Baring any strong opposition from the community, developers should use it when working on Sympa.

 - [use Moo](https://metacpan.org/pod/Moo) for *object orientation*
 - [use Types::Standard](https://metacpan.org/pod/Types::Standard) to make *type checking* both automatic and explcit in the code
 - [use Function::Parameters](https://metacpan.org/pod/Function::Parameters) to get functions (and object methods) signatures.
 - [use Path::Tiny](https://metacpan.org/pod/Path::Tiny) for any filesystem manipulation.

