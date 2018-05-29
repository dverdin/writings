**Disclaimer: This document expresses the views of the 2018 hackathon attendees. Consider all the points described as request for comments. This document should be the only reference for further discussions about the topics tackled during that hackathon.**

# 2018 Sympa Hackathon report
The 2018 Sympa hackathon was held in Strasbourg, France, from 22 to 24 of May.
It was organized by March chantreux, from RENATER, and hosted by the Strasbourg university.

## Attendees
Here is the list of the attendees and their main areas of expertise (in last name  - or handle - alphabetical order).

 - [March Chantreux](https://github.com/eiro) (RENATER): Perl expert, listmaster, Perlude creator
 - [Luc Didry](https://github.com/ldidry) (Framasoft): Framasoft listmaster, Sysadmin, Perl expert and experienced free software manager
 - Mathieu Durero (French young researchers confederation) ; "The noob": Mathieu is an experienced listmaster but didn't know anything about Perl or Ansible prior to the hackathon. His "naive" - though thoughtfull -  input has been very valuable during the three days.
 - [François Menabe](https://github.com/fmenabe) (Strasbourg University): Sysadmin, Ansible expert
 - [Racke](https://github.com/racke) (Linuxia.de): Debian package developer, member of the Dancer2 community, Linux and Perl expert
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
   - [Dancer2](https://metacpan.org/pod/Dancer2) for the REST API implementation,
   - [OpenAPI](https://github.com/OAI/OpenAPI-Specification) for the REST API specification,
   - [DBIx::Class](https://metacpan.org/pod/DBIx::Class) for Sympa database backend management and access.



## Sympa 7.0 target and methods of development

As we will not have the perfect Sympa right now, we should set some goals. A reasonable aim for Sympa 7.0 would be to be iso-functional with a refactored, testable code and a full  REST API.

Here is a proposed methodology:

### Work on new features

Write a Dancer2+DBIC proof of concept. Racke volunteers to work on it. The REST API would run on Dancer2 and directly query the database through DBIC. This would create a good base for future Sympa with actually running code.

### Refactoring

- Create a testing framework to run unit tests on existing code (including mock databases, configurations, list directories, etc.). David volunteers to work on it.
- Create functional tests based on the [https://github.com/sympa-community/sympa-ansible] project and [Test::BDD::Cucumber](https://metacpan.org/pod/distribution/Test-BDD-Cucumber/lib/Test/BDD/Cucumber/Manual/Tutorial.pod). Olivier volunteers to wok on it.
- Start improving the code, step by step. Anyone can do this.

#### A proposed tagging system to track refactoring:

(Ideas by Olivier and I on the train back home Friday morning)

The idea is to know easily what's awaiting in the refactoring queue.
We could use Travis CI to generate report based on tags left in comment of Perl code:

  # WORK: <task>: <state>
  with:
  <task>: unit-tests|Moo|function-parameters|types-standard|any other improvment we could do
  <state>: FIXME (nothing done yet)|DONE|ONGOING:<username> (work in progress by <username>)

That way, anyone could now how far we are. We even could add a progression tracker to the main Sympa web site.

## Community: roadmap

## Tests

## Coding practices

The aim of the discussion was to find a way to get rid of the *infrastructure code*. That's the part of the code that handles language mechanics instead focusing on application logic. For example, the "bless" command used to cast anything into an object could be easily replaced by Moo.

### What we agreed on

All these modules allow to drastically decrease the number of Sympa code lines without changing anything in the application logic. It would malke the code more readable and less error-prone.

Baring any strong opposition from the community, developers should use it when working on Sympa.

 - [use Moo](https://metacpan.org/pod/Moo): for *object orientation*
 - [use Types::Standard](https://metacpan.org/pod/Types::Standard): to make *type checking* both automatic and explcit in the code
 - [use Function::Parameters](https://metacpan.org/pod/Function::Parameters): to get functions (and object methods) signatures.
 - [use Path::Tiny](https://metacpan.org/pod/Path::Tiny): for any filesystem manipulation.

### What we did not agree on

  - [use MooX::LValueAttribute](https://metacpan.org/pod/MooX::LvalueAttribute): would allow to use attributes getters and setters as simple attribute, such as in '$self->atribute = $value;'.
  - [use autodie](https://metacpan.org/pod/autodie): throws a "die" whenever a system call fails. In addition to reservations regarding the autodie principle, the main concern was that dying is only half the job in exception systems. We also need an exception handling methodology. Without a concrete proposal about it, raising exception will only unstabilize the software.
  - [use do](http://perldoc.perl.org/functions/do.html): there was a proposal to use "do" whenever possible to make variable affectations explicit when they require complex code; the discussion reached no actual consensus. For now, if you want to use "do", well, do it, but this would not be enforced as a coding style.
  - [use utf8::all](https://metacpan.org/pod/utf8::all): Thought using it would make sense in a pure web environment, the diversity of data handled by Sympa, especially in mails, make encoding issues complex. It is impossible to use it until we have a complete tesing framework and advices of people very well learned in this topic.


Pseudo-cyclomatic complexity removal. that means, replace this:
  if ($a eq 'value') {
    
  } else {
    
  }
With this:


### What is left aside for now

  - Functional programming: Looks like a hot stuff right now but far from being a priority. Once the code is greatly improved, we might have new contributors that demand we switc to this; Well, we'll see then,
