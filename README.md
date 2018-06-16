# EECE 514: assignment01

### For each tool (Monkey & Randoop), generate tests for the appropriate applications. Measure the time taken to generate tests. For example, Randoop uses a time limit to generate tests. How many tests does it generate in that duration?

Monkey | Time taken to generate 10,000 tests
------ | -----------------------------------
MovieGuide | 352.698s
LeafPic | 318.769s

Randoop | Test generated in 60 seconds
------- | ----------------------------
JPacman | 2099(mac) / 506 (windows)
ActiveMQ | 869 (3 error / 866 regression)

### Is the test coverage adequate?

##### Monkey 

We found the tool’s methods chaotic and not systematic at all. It’s analogous to chess. If a player moves the piece, they can’t move anything else during the same turn. So that’s what Monkey does - a forward branching testing method. Better coverage would be like Deep Blue, a process that analyzes every possible branch. 

Monkey can only test sequential actions in hopes of providing a “good enough” coverage, but it will never cover 100%.

##### Randoop

We imported Randoop tests into IntelliJ and ran the tests to retrieve the following coverage:

JPacman | Class | Methods | Line
------- | ----- | ------- | ----
Mac | 70% (31/44) | 52% (128/245) | 41% (397/946)
Windows | 70% (31/44) | 52% (128/245) |41% (396/946)

ActiveMQ | Class | Methods | Line
-------- | ----- | ------- | ----
All code scope | 7% (140/1800) | 5% (987/16806) | 3% (2946/76306)
Broker namespace | 6% (21/301) | 8% (324/3924) | 5% (940/16509)

Increasing the time limit only gave a marginal increase in coverage rates; we generated tests for 120s for JPacman and ended with the exact same coverage numbers. 

A reason for low percentages in ActiveMQ: the coverage is calculated for the whole code base while we limited test generation to a small subset of classes, so it’s not always an issue of adequacy.

### Do the tools find bugs in the applications? You can artificially create bugs to determine if the tools find the bugs.

##### Monkey 

- Yes, it can. For example: we inserted a null pointer exception into the donation screen in LeafPic. Monkey found the bug quickly when it navigated to that screen. While not guaranteed, it is possible.

##### Randoop

- Randoop outputs an ErrorTest suite “that detect bugs in your current code”, as defined by the user’s manual. In addition, when making changes that don’t change behaviour or API, the generated regression tests will detect changes in behaviour or bugs affecting expected execution behaviour.

### Are the tools easy to use?

##### Monkey 

- Basic commands were straightforward
- But any further tweaking/advanced details were not fully documented. We found the documentation lacking which required a fair amount of experimentation.
- Many options were unclear and overlapped with each other
- OS and application animations coupled with a low throttle value had the potential to decrease the tools usefulness. If the throttle value was too low (or not specified), subsequent user events were triggered before animations finished running. This led to pointless test runs that didn’t actually accomplish anything. 

##### Randoop

- The documentation isn’t as complete as it could be. For instance, there are no simple examples demonstrating how to use the tool.
- When it works, it’s smooth. But there is a lack of support for reporting and troubleshooting errors.
- Increasing logging verbosity produced a lot of essentially useless data. Didn’t help diagnose problems.
- Little feedback when generated tests are being written to disk. For long time limits, this left us wondering if the tool had frozen or was still making progress. 

### What are the limitations of the tools?

##### Monkey 

- Seed was not intuitive. While seemingly the actions are the same, it does not reset to an initial state. So if the starting state was different, the results were as well, even if seed was specified.  For instance, scrolling to a different set of titles in MovieGuide and running Monkey with a seed value resulted in different behaviour
- As mentioned before, events are carried out in sequence
- Monkey interacts with events and OS elements outside the app, like system, wifi, or guest accounts, which potentially invalidates the testing. 

##### Randoop

- Only supports up until Java 8 (now 2 major versions behind)
- Poor documentation and support
- RegressionTest files had a limit of 500 test cases per file. 
- There seems to be a different amount of generated tests for Mac and Windows, although the coverage were nearly identical. Unclear why this happened.
- Generating tests for package-level code is more difficult than public code as tests have to be in the correct package. Randoop has support for testing this code, however, it doesn’t appear able to generate tests for both public and package-private code in one execution.

### How would you improve these tools? Identify at least one improvement for each tool that you believe would be most useful for developers.

##### Monkey

- Figure out way to prevent / limit interactions with the OS, in particular the drop down notification and settings tray.
Stopping emulator simulation when exiting command line execution. 

##### Randoop

- Handle flaky tests more gracefully
- Have more meaningful errors messages instead of throwing generic errors
- Add capability to generate tests across multiple packages in a single execution. This could be done by generating test that exist in same package as the class under test.
