# Behavior-Driven Development - BDD
## Intro
A way for software teams to work so that the gap between technical people and business people is small.
If a team works using any agile methodology i.e. planning work in small increments, BDD enhances this by relying on Tapid Iterations: continuously breaking down each increment into smaller pieces that can flow through the dev process fast.

BDD relies on Agile, so you should already be working using any agile method.

## How it works
1. **Discovery**: Take a small upcoming change to the system e.g. a User Story (A small piece of functionality), discuss it using concrete examples, and agree on the details of what's expected to be done in it.
    - Basically, this is a meeting between SWEs and BAs to discuss user stories, but in the meething they should 1) create concrete examples, 2) agree on as much functionality as possible.
    - There are methods to structure this meeting, called *discovery workshops*.

2. **Formulation**: Document the concrete examples in a way so that they can be automated, use this documentation to ensure that SWEs and BAs agree on what's in it.
    - This ensures that we really do have a mutual understanding of what to build.
    - We use an intermediate language that can be parsed by a computer, and easily readable by people e.g. *gherkin*.
    - This means 1) We can get feedback on agreement easily, and 2) We can automate that easily.

3. **Automation**: Implement the behavior described by each documented example. This is similar to TDD.
    - Take one example from the docs.
    - Use it as a test for your *piece of code*.
    - Now write the *piece of code* to pass that test, use low-level examples to infer the internal details of the *piece of code* you're writing, so you're more accurate.

So the documentation consists of concrete examples that kinda serve as tests, the code reflects the documentation, which reflects the mutual understanding of the problem i.e both tech and business people understand it.

This is awesome because:
- We are pretty sure we all understand what to build i.e. less useless code.
- Still follows TDD so it gets all its benefits.
- Still follows agile so it gets all its benefits.
- All the examples can be used for implementation as well as automated regression testing.

## What makes an example a good example?
Good examples are:
- **Concrete**, not *abstract* i.e. use real people's names, places, exact dates, amounts, anything that is relevant to the problem domain of the software.
- Don't mention technical details. Purely business examples.
- Most of the software does something that people can do manually but not efficiently.
- Imagine it's 1922 where there are no computers.

## Making Discovery Meetings Productive - Exmaple Mapping
Before we start developing a user Story, we need to make sure we all understand and agree on its functionality, we need to hold a Discovery meeting for that. One method to make that meeting very productive and short is **Example Mapping**.

When discussing examples, these might come up, and they are important to be captured:
- Rules that apply to some examples.
- Constraints.
- Questions that cannot be answered during the meeting.
- Assumptions.
- Other user stories that are discovered during the meeting but are out of scope.

How to do that efficiently?
1. Get a bunch of yellow, blue, green, and red cards..Yes I'm serious.
2. Get your example, write it on a yellow card.
3. Write any rules or constraints on blue cards, put them below the yellow card.
4. Write examples that illustrate these rules on green cards, place each underneth the relevant rule.
5. Write any unanswered questions or assumptions on red cards.

Repeat that until:
- The group is satisfied that the scope of this user story is clear.
- Time has run out.

## Myths about BDD
- You can shuffle the main BDD steps (discovery, formulation, automation).
    - **NO**, your discovery sessions need to be first, if not, formulation will be a waste of time, automation too.
- You can automate scenarios after implementation.
    - **NO**, BDD relies on TDD in its automation step, so write the tests FIRST, then write the code.
- Discovery doesn't need a meeting.
    - **NO**, BDD relies entirely on the fact that both SWEs and BAs understand the requirements, so yeah we need to make a meeting.

## Who does what in BDD?
There are different people involved in the dev cycle: product owner, BAs, SWEs, testers, and so on. So who does what in BDD?

There is a meeting that takes User Stories and turns them into clean thorough scenarios, so at lease these three people attend the meeting:
- **Product owner**: That person is concerted with the scope of the application the most, when examples are generated, he/she is responsible for deciding if they are in or out of scope.
- **Testers**: They will be generating scenarios, edge cases, etc.
- **SWEs**: They will add steps to the scenarios, think of the details of each requirement, any roadblocks, etc.

We can use *Example Mapping* in that meeting, another technique to use is *Event Storming*.

During this meeting, we can use something called **Gherkin** (جركن xD)

# Gherkin

