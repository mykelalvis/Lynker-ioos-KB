---
date updated: '2021-04-28T09:25:01-05:00'

---

# Testing

<dl>
	<dt>testing</dt>
	<dd> the means by which quality is ascribed to some artifact.  </dd>
</dl>

In software engineering, [testing](https://en.wikipedia.org/wiki/Software_testing) is how the quality of an [[artifact]] is described.  This can be [[#manual]] testing, [[#automated]], or a combination of both.  

All forms of testing are done to determine the fitness to which the testing element is with regard to one or more [[requirement|requirements]].  Tests are [usually] constructed to determine that a requirement is fulfilled or not.  A passing test is usually one in which it is detecable that some tested requirement _is actually_ fulfilled.

## Manual
In manual testing, some human performs one or more [[#test actions]] against an [[environment]].  Testing an [[environment]] is done because it is usually very challenging to perform validation just against a simple [[artifact]][^possible]. 

The nominal "tester" performs these actions, usually recording the results to supply back upstream to someone involved in the production of the [[artifact]](s) that produced the tested [[environment]].  This feedback often comes in the form of prose, such as "The screen needs to be blue, not green, on the login page". 

Most manual testing lies in the area of [[#acceptance]] tests or similar forms of validation.  Generally automated testing is preferred as it tends towards repeatability, as well as lacking the innate frailty that humans exhibit during repetition.  Manual testing often presents additional challenges with regard to proving formality, since much of the output is decided _in situ_.

## Automated

Automated testing applies systems to the task of replacing [[#manual]] testing with some form of robotic process automation, often including [[continuous integration]] as a means for removing the need to have a human launch the automation.

The results of automated testing are usually some structured output, often formed as or transformed into a report.  One of the key quality points for some teams is [[code coverage]], which is a testing quality metric usually synthesized from this structured output.  Without automated testing, this metric is nigh-impossible to produce.


## Types 
There are numerous types and styles of tests, as well as testing frameworks.  Which one(s) a project uses are often wildly variant.  

If a given language lacks the ability to produce easily-manageable and well-formed tests, then that language should probably be avoided.

### Unit

[Unit tests](https://en.wikipedia.org/wiki/Unit_testing) are tests in isolation.  In opposition to [[#manual]] testing, almost all real unit testing is [[#automated]].  Unit tests are typically ["white-box" tests](https://en.wikipedia.org/wiki/White-box_testing), where the tester can see all the parts as they run.  They will also often make use of "mock" implementations using a mocking framework like [Mockito](https://site.mockito.org/), [Sinon.js](https://sinonjs.org/), or [Mirage JS](https://miragejs.com/.  This increases the internal visibility of the tests, among other things.

In that unit tests are typically performed in isolation from other components[^sinceint], mocks allow supply and consumption to be more closely monitored.  This is also part of the "white-box" paradigm, allowing the understanding that nothing is interfering with the internals of the tested code.

As a rule, unit testing completeness implies testing all possible branching behaviors of all possible branches of a given path of exercise.  If a function receives a "string" value as a parameter but performs some additional action if that string is `null`/`nil`, then a test that sends a valid string would need to be conducted with a `null` string, as well.

### Integration

[Integration testing](https://en.wikipedia.org/wiki/Integration_testing) is the practice of taking individual modules (probably [[#unit]] testing), wiring them together, then exercising them with a testing framework.  This is another type of "white box" testing, although the results are often more difficult to ascertain.

Like unit tests, integration tests have very well-defined boundaries.  In unit tests, the boundaries are a single object or module; integration boundaries are the collection of objects or modules.

### Acceptance
[Acceptance tests](https://en.wikipedia.org/wiki/Acceptance_testing) are typically a form of ["black-box" testing](https://en.wikipedia.org/wiki/Black-box_testing)

### Others
There are many other forms of testing, but they typically revolve around some iteration of the aforementioned three types with some orthoganal directive.  

For instance, a performance test on a system is usually an acceptance test where speed is the measured metric.  Other types, such as A/B testing, are [usually] acceptance tests with the metric of choice per differentiated target type.  Nominal "smoke tests" are acceptance tests with reduced overall scope.

Variants of testing are more often a variant of the chosen target metric or means for administering the test rather than an entirely different variety.

## Test Actions

Test actions are simply the processes executed within a test to exercise the relevant code.  These can be automated or manual.  The quality of the actions is usually determined by the quality, and often the extent, of the test action.  This is not entirely true, but generally "more, or more-complete, testing" would probably be considered superior to less.

For example, it is technically true to say that a function that writes a file could be tested by writing a single-byte file, but writing many files or some of varying lengths most likely exercises the actions of the item under test more completely.  


[^possible]: althought it _is_ possible
[^sinceint]:since that would be an [[#integration]] test