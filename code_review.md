# Code Review in Software Engineering

## What is Code Review?

Code review is the process where software developers examine each other's code to identify bugs, ensure code quality, and maintain consistent coding standards. It's a crucial part of building better quality software and is a fundamental skill to learn, both as a reviewer and a reviewee.

According to formal definitions, code review is a systematic examination of source code that helps guarantee the quality, correctness, and maintainability of code. It involves developers reviewing each other's work before merging code into a shared repository.

## Why Review Code?

Code reviews provide numerous benefits:

1. **Help to find and fix bugs early**
   - Two brains are better than one!
   - Catching defects before they enter production

2. **Help to improve code structure**
   - Identifying better design patterns and approaches
   - Preventing overly complex implementations

3. **Enforce coding standards**
   - Ensuring consistent code style across the codebase
   - Following team and organization conventions

4. **Spread knowledge among team members**
   - Code review helps developers learn the codebase, as well as new technologies and techniques that grow their skill sets
   - Good training opportunities for new hires
   - Protection against the "bus factor" (what if the original developer leaves?)

5. **Psychological effect**
   - Developers know their code will be reviewed, so they will work harder
   - When developers know their code will be reviewed by a teammate, they make an extra effort to ensure that all tests are passing and the code is as well-designed as they can make it

## When and How Often to Review?

- **Not too soon, not too late**
  - Typically after unit testing has been done
  - After basic features have been tested
  - Before merging into the main codebase

- **Frequency**
  - Weekly, or after each major feature
  - At Atlassian, many teams require two reviews of any code before it's checked into the code base

## Review Philosophy

- A forum to discuss and learn from everyone
- Not an opportunity to criticize people!
- Not to demonstrate who is a better programmer!

## Types of Reviews

### Management Review
- **Objectives**:
  - Inform management of project status
  - Resolve higher-level issues (management decisions)
  - Agree on risk mitigation strategies
- **Decisions**:
  - Identify corrective actions
  - Change resource allocation
  - Change project scope
  - Change schedule

### Formal Technical Review
There are several types of formal technical reviews ranging in formality and effect:

#### Buddy Checking
- Having a person other than the author informally review a piece of work
- Generally does not require collection of data
- Difficult to put under managerial control
- Generally does not involve the use of checklists to guide inspection and is therefore not repeatable

#### Walkthroughs
- A walkthrough is an activity in which people other than the author walk through every sentence of the software information artifacts (mainly documents) and every line of code for the software code artifacts
- The CMMI model defines walkthroughs as "the review of work products performed by peers during development of the work products to identify defects for removal"
- There is generally no preparation ahead of the walkthrough
- Usually little or no documentation is produced
- Generally involve the author of an artifact presenting that document or program to an audience of their peers
- The audience asks questions and makes comments on the artifact being presented in an attempt to identify defects

#### Audits
- Audits are usually performed by some external group, rather than the development team
- Audits may be conducted by a SQA group, a project group, an outside agency, or possibly a government standards agency
- Audits are not primarily concerned with finding defects -- the main concern is conformance to some expectations, either internal or external
- Audits may be required by contract, and an unsatisfactory audit usually results in expensive corrective actions
- Audits are conducted for the purpose of uncovering nonconformances (NCs), if any, in a project. The output of an audit is a nonconformance report (NCR). An NCR lists all NCs uncovered during an audit.

#### Technical Inspection
- An inspection is a visual examination of a software product to detect and identify software anomalies, including errors and deviations from standards and specifications
- Pioneered by Michael Fagan while he was at IBM in the 1970s, technical inspections are the most effective form of software reviews

## Using a Defined Inspection Process

To be most effective, inspections should follow a defined process. A defined process tells you what steps to take and when to take them.

### Planning
- **Objectives**:
  - Gather review package: work product, checklists, references and data sheets
  - Form inspection team
  - Determine dates and locations for meetings
- **Procedure**:
  - Moderator assembles team and review package
  - Moderator enhances checklist if needed
  - Moderator plans dates for meetings/books rooms
  - Moderator checks work product for readiness
  - Moderator helps Author prepare overview
  - Moderator checks with the reviewers to make sure that they are prepared and agree that the product is indeed ready to be reviewed

### Orientation
- **Objectives**:
  - Author provides overview
  - Reviewers obtain review package
  - Preparation goals established
  - Reviewers commit to participate
- **Procedure**:
  - Moderator distributes review package
  - Author presents overview (if necessary)
  - Scribe duty for Inspection Meeting assigned
  - Moderator reviews preparation procedure

### Preparation
- **Objectives**:
  - Find maximum number of non-minor issues
- **Procedure**:
  - Allocate recommended time to preparation
  - Perform individual review of work product
  - Use checklists and references to focus attention
  - Note critical, severe and moderate issues on Reviewer's Data Sheet
  - Note minor issues and author questions on work product

## The Review Team

- Review is not full-time!
  - Productivity drops quickly during a session
  - Reviewers are usually borrowed from other roles
  - Tradeoff between cost, background and perspective
- More expert = more valuable, but more costly
- More perspectives = bigger teams

### Different levels of reviews
- Junior engineers can do simpler checks while self-training on standards and practices
- Senior engineers can be paired with junior engineers when checking more complex properties
- Larger groups when special expertise is required

### Roles and Responsibilities
- **Moderator**: Leads the review process, ensures meeting remains focused
- **Scribe**: Records issues and action items during review meetings
- **Reader**: Presents or reads through the code during the meeting
- **Author**: The developer who wrote the code being reviewed
- **Management**: Supports the review process but typically doesn't participate directly
- **Reviewer**: Any team member who examines the code

## Review Guidelines

1. Review the product, not the producer.
2. Set an agenda and maintain it.
3. Limit debate and rebuttal.
4. Enunciate problem areas, but don't attempt to solve every problem noted.
5. Take written notes.
6. Limit the number of participants and insist upon advance preparation. Two heads are better than one, but 14 are not necessarily better than 4.
7. Develop a checklist for each product that is likely to be reviewed.
8. Allocate resources and schedule time for reviews.
9. Conduct meaningful training for all reviewers.

## What to Look For in a Code Review

Good code reviews cover the correctness of the code, test coverage, functionality changes, and confirm that they follow the coding guides and best practices. They point out obvious improvements, such as hard to understand code, unclear names, commented out code, untested code, or unhandled edge cases.

Specific areas to focus on include:

1. **Logic errors**: programming mistakes, incorrect assumptions, misunderstanding of requirements
2. **Adherence to coding standards**
3. **Use of common code modules**
4. **Robustness** – adequate error handling
5. **Readability**: meaningful names, easy-to-understand code structure
6. **Bad smells**: opportunities for refactoring
7. **Tests**: make sure unit tests are provided, and sufficient coverage is achieved
8. **Comments**: adequate comments must be provided, especially for logic that is more involved
9. **Design/complexity**:
   - Does this change belong in this code, or somewhere else?
   - Does each unit of code (class, method, etc.) follow good code-level design principles?
   - Is it over-engineered?
10. **Functionality/testing**:
    - Are there tests? Are the tests testing the right thing?
    - Is the code doing something difficult to understand (such as concurrency)?

### Control-Flow Errors Example
- Will every loop eventually terminate?
- If the program contains a multi-way branch, can the index variable ever exceed the number of branch possibilities?
- Will the program, module, or subroutine eventually terminate?
- Is it possible that a loop will never execute due to some initial conditions?
- Are there any "off by one" errors?
- Are there any non-exhaustive decisions?

## How to Write Code Review Comments

- Be kind, courteous, and respectful
- Explain your reasoning
- Balance giving explicit directions with just pointing out problems and letting the developer decide
- Encourage developers to simplify code or add code comments instead of just explaining the complexity to you

### Comment Severity Labels
- **Must Fix**: Critical issues that must be addressed
- **Nit**: Minor things that technically should be done, but won't hugely impact things
- **Optional**: Suggestions that might be good ideas, but aren't strictly required
- **FYI**: Informational comments that don't require any action

## Rules for Reviewers

If it's too hard for you to read the code and this is slowing down the review, then you should let the developer know that and wait for them to clarify it before you try to review it. If you can't understand the code, it's very likely that other developers won't either.

Additional rules:
- If you find something, assume it's a mistake. These are your peers, not your enemy.
  - Remember people are involved.
- Avoid "why did you..." or "why didn't you..." - say instead "I don't understand..."
  - For example, instead of "Why did you set the upper bound to 10 here?" say "I don't understand why the upper bound is 10 here."
  - Seems trivial, but it's not.
- If it makes things less maintainable, that's an issue.
- If there are standards, then either stick to the standard, or dispose of the standard.
- Do not get bogged down in style issues.
  - For example, if efficiency isn't an issue, then don't make it one.

## Choosing Reviewers

### Possibilities
- Specialists in reviewing (e.g., QA people)
- People from the same team as the author
- People invited for specialist expertise
- People with an interest in the product
- Visitors who have something to contribute
- People from other parts of the organization

### Exclude
- Anyone responsible for reviewing the author
- Anyone with known personality clashes with other reviewers
- Anyone who is not qualified to contribute
- Anyone whose presence creates a conflict of interest
- All management

## Number of Reviewers
- Ensure material is covered
- Have enough reviewers (but not too many)
- 4-7 is generally a good number
- Count participants only
- Outsiders can be good but must be unbiased

## Checklists

An inspection is as good as its checklist, whether explicit (written down) or implicit (in the inspector's mind). The work product is examined with reference to specific checklists:
- Compliance to standards
- Domain correctness
- Consistency with requirements items
- Avoidance of errors in historical list (frequency ranked)

## Group Review Meeting

The code review process typically follows these steps:
1. Code submission: A developer writes code and submits it for review, usually via a pull request (PR) in platforms like GitHub.
2. Review assignment: The developer assigns one or more code reviewers—often peers or senior developers—who are responsible for examining the submitted code.
3. Code review: Reviewers analyze the code, looking for potential issues, improvements, and adherence to coding standards.

Once reviewers complete their examination of the work product, they submit an individual review report form to the moderator/review leader.

Before the group review meeting is scheduled, the moderator first checks whether all reviewers are prepared. This verification involves a brief examination of the effort and defect data in the self-preparation logs to confirm that sufficient time and attention have gone into the preparation.

If preparation is not adequate, the group review is deferred until all participants are fully prepared.

When everything is ready, the group review meeting is held. The basic purpose of the group review meeting is to come up with the final defect list; this list is based on the initial list of defects and issues reported by the reviewers and any new ones found during the meeting discussion.

### When conducting group review meeting
- A reviewer goes over the product line by line
- At any line, all issues are raised
- Discussion follows to identify if a defect
- Decision recorded (by the scribe)

The scribe prepares a log of the group review meeting that lists the defects and issues that were identified during the meeting. Hence, it includes all defects found by individual reviewers in their self-reviews that were validated as defects or issues during the meeting, as well as the additional defects found during the meeting.

### The moderator's role
The moderator is in charge of the meeting and plays a central role:
- Ensures that focus is on defect detection and solutions are not discussed/proposed
- Work product is reviewed, not the author of the work product
- Amicable/orderly execution of the meeting
- Uses summary report to analyze the overall effectiveness of the review

## Rework and Follow-up

### Rework Procedure
- Author obtains scribe data sheet containing consolidated issues list as well as copies of work products
- Author assesses each issue and notes action taken using the author data sheet
- Author determines the "type" of each defect
- When finished, author provides author data sheet and reworked products to moderator for follow-up

### Follow-up Objectives
- Assess the (reworked) work product quality
- Pass or fail the work product

### Follow-up Procedure
- Obtain reworked product and author data sheet
- Review work product/data sheet for problems
- Provide recommendation for work product
- Perform sign-off with reviewers
- Compute summary statistics for inspection
- Enter review data into quality database

## Reviews and Agile Methods

Code review is essential for building high-quality software, enabling teams to share knowledge, find defects early, and align on best practices. However, implementing an effective review process requires nurturing a collaborative culture focused on continuous improvement.

The review process in agile software development is usually informal:
- In Scrum, there is a review meeting after each iteration of the software has been completed (a sprint review), where quality issues and problems may be discussed.
- In Extreme Programming, pair programming ensures that code is constantly being examined and reviewed by another team member.

## Reviews in Industry

At Atlassian, many teams require two reviews of any code before it's checked into the code base. This decentralizes the process so that no one is a bottleneck, and ensures good coverage for code review across the team. Requiring code review before merging upstream ensures that no code gets in unreviewed.

Some commonly used code review tools are Bitbucket, GitHub, GitLab, Gerrit, etc. Automated static analytics tools, syntax checkers, style checkers, etc., must be leveraged to provide faster and more authentic reviews with automatic log reports, analysis of review comments, and a further line of action.

## Best Practices

A successful peer review strategy requires balance between strictly documented processes and a non-threatening, collaborative environment. Highly regimented peer reviews can stifle productivity, yet lackadaisical processes are often ineffective. Managers are responsible for finding a middle ground.

While building a code review culture, make sure your developers aren't intimidated by the process. Avoid evaluating the performance of your developers by reviewing the defects identified during their code reviews. If this becomes a benchmark for promotion or compensation, your developers are likely to feel threatened by the process and hostile to it.

Encourage developers to solve the problem they know needs to be solved now, not the problem that the developer speculates might need to be solved in the future. The future problem should be solved once it arrives and you can see its actual shape and requirements in the physical universe.

## Metrics and Measurement

The metrics for code review could include inspection rate, defect density, defect rate, review depth, review quality, review impact, lines of code, code coverage, etc. These key metrics offer insight into how the code behaves and how it will affect the entire application's performance.

### Inspection Metrics

- **Total Defects Found** = A + B - C, where A and B are the number found by reviewer A and B respectively and C is the number found by both A and B.
- **Estimated Total Defects** = (A×B)/C
- **Inspection yield/Defect Removal Efficiency (DRE)** = (Total Defects Found / Estimated Total Defects) × 100%
- **Defect density** = Total Defects Found / Size
- **Inspection rate** = size / total inspection hours
- **Defect finding efficiency** = Total defects found / total inspection hours

### Sample Re-Inspection Criteria
- Inspection rate too high
- Yield too low
- Major defects found / Total defects found too low
- High defect density
- Unusual defect distribution
- Should be specified in plan

