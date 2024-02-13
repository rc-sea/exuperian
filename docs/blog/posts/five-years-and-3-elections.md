---
draft: true 
date: 2024-01-19
categories:
  - electionguard
  - product
  - open-source
authors:
    - rc-sea
---
# Improving Voter Confidence and Participation in US Elections: Lessons Over 5 Years and 3 Elections

## Overview

Voting in the U.S. in a singular endeavor. All democracies conduct elections in some fashion, but few if any face the complexity of implementation that American election administrators do. No other enterprise in the US is remotely similar.  

While the public typically thinks elections happen only every few years, in fact they occur continually in different formats. Elections are fixed events that impose unique operational burdens on hiring and preparedness [^1]. US elections are incredibly complex, spanning a variety of voting methods, districts, contest options, tally methods, and governing regulations as well as operational procedures and operating environments [^2]. Election administrators face asymmetric threats from geopolitical actors [^3] and heightened polarization [^4], scrutiny [^5] and threat environments [^6] within their districts. They perform elections with limited budgets [^7], a universal service obligation [^8] and a public that expects accurate results immediately upon completion [^9].

There is no perfect voting method that accommodates all voters and satisfies the unique security requirements of elections, either. Each method imposes tradeoffs and costs.

On top of it all sits how election administrators measure success: confidence and participation of the voting public.  

[ElectionGuard](https://www.electionguard.vote) is an open-source software development kit that enables voting system vendors and election administrations to implement ***end-to-end verifiable elections*** [^10] (e2e). Its intent is to **increase the confidence and participation of voters** in elections and election outcomes.

??? info "What is an end to end verifiable election?"
     An end-to-end verifiable election, among other things [^10], generates a public election record that provides all the information necessary for voters and third parties to review an entire election, including the following:

    * each and every the ballot included in the election is properly formed
    * all the contest options on those ballots include only valid selected (1) or unselected (0) items
    * all of the contest options selected are within each contest's limits (e.g., no more than 3 candidates selected for a 3-seat contest)
    * and, finally, that all of the tallies generated from the ballots are correctly decrypted and aggregated (tallied)

    The only thing not viewable in the record are the ***contents*** of the encrypted ballots themselves. All ballots are included in the record in their encrypted form, and the encryption is such that only the information above is derivable and verifiable via the appropriate [zero-knowledge proofs](https://en.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof).


Deploying software into the US election system is challenging, mostly for good or at least understandable reasons. Administering elections itself is already complex, which is often surprising to people given the simplicity of their mental model of voting. Elections also present a lot of cognitive dissonance to voters  by virtue of some of their key constraints and design principles. When the success criteria is measured as increasing confidence and participation, the challenge is even greater given the complex nature of determining what constitutes improvement.

This article outlines the considerations involved in trying to deploy a confidence- and participation-building technology into the US election system, including how to measure and determine success. The conclusions and recommendations outline the challenges and opportunities for ElectionGuard and voting as the "industry" transitions to a new era of regulation.

## What Does it Mean to Increase Confidence in Elections?

Confidence refers to the perception of the administrative conduct of elections, essentially *were votes properly counted*? There is significant research showing voters have a distinct impression of election administration separate from the political environment or actors around it [^11].

Confidence is a complex subject and varies depending on the context of the ask as much as the ask itself. Voters are generally more confident about their local elections than those in other jurisdictions or that use different voting methods [^12]. The "winner-loser gap" shows that voters who vote for a winning candidate, especially in major elections such as US President, have more confidence in the election outcome than those who voted for the losing candidate [^13]. Donald Trump and the polarization over voting methods during the COVID election of 2020 likely exacerbated the "loser" aspect of this effect with their protestations and lawsuits over the expanded adoption of voting by mail, especially with Republicans [^14]. Voters' personal experience with the voting process, especially as relates to waiting in lines or interactions with poll workers, can also increase or decrease confidence in the election itself [^15].

Determining and attributing the motivating factors for what contributes to confidence is correspondingly difficult. With voting and elections, aspects of confidence can even work against each other (see [the secret ballot](#the-cognitive-dissonance-of-the-secret-ballot) below).

!!! note "Can you increase participation and confidence in elections?"
    Yes. See our reports on how:

    * [Preston, Idaho](https://www.electionguard.vote/Reports/E2EVerifiability/), November 2022
    * [College Park, Maryland](https://civicdesign.org/reports/electionguard-in-college-park-maryland/), November 2023

For our most recent two elections, we endeavored to answer the question of confidence via exit polling and community surveys and as well as monitoring behavior around use of the confirmation code lookup website. Methods and conclusions are available for each election as outlined above. While any generalization is naive, we believe we increased confidence in the local electorate. There are many reasons that could have occurred, but given that most surveys show voter confidence *decreasing* and then rebuilding over time as new voting systems are introduced [reference needed], that we can show an increase in confidence even as we are also introducing new technology is a significant result.

## Participation is Multi-faceted

ElectionGuard and e2e address two aspects of participation. The first involves voters and other organizations more fully engaging with the voting process itself: checking that votes were counted using confirmation codes, performing a BallotCheck, developing an independent verifier, and hosting and evaluating the election record.

ElectionGuard can also expand the voting franchise by enabling new voting methods and devices, or increasing adoption of underutilized technologies. It does this by helping voting systems demonstrate [*software independence* (see below)](#electionguard-and-e2e-as-a-means-to-increase-access-for-all-voters).

Each aspect of participation matters. While the ElectionGuard experience to date has focused on engagement, long term success means extending across all the multiple voting methods involved in modern elections to enable a fully verifiable experience for all voters.

## Core Early Lessons

Voting in polling places is a public act and carries the attendant anxiety of being and appearing confident and competent in such a setting. Disabilities, vision problems, language facility, familiarity with technology, and other factors can make voting a stressful, anxiety-inducing experience irrespective of the voter's confidence in the voting methods or processes themselves. Voting occurs infrequently for most voters, and voters that move to different locales can encounter different voting methods when they do. Instructions can be confusing. Confidence can decrease with any and all of these circumstances both at the personal level (was my ballot counted? did I fill out the ballot correctly?) and systemic (how many other people had problems like mine?).

### First, Do No Harm

Several end-to-end verifiable elections have been held in the US, each using different methods to record e2e ballots. They all imposed additional steps to the voting process. Studies of these approaches reinforced the core notion that "voters expect the steps required to vote with an e2e system to be roughly the same as voting with a standard paper ballot or electronic voting machine." [^16]

ElectionGuard is a software development kit (SDK) that election vendors use to implement end-to-end verifiable elections alongside their systems. It does not and should not change the core voting process, instead it presents additional options that the voter can choose to use.

In the case of the ElectionGuard integration with Hart InterCivic's Verity Scan voting system, the only difference in the voting experience occurs when the scanner prints a confirmation code associated with the ElectionGuard encryption. (The voter can also run a BallotCheck challenge, but that process is fundamentally, and purposefully, identical to the process by which voters who make a mistake or otherwise wish to change their ballot after the fact [^17].)

### Analogs Matter

It is helpful that the confirmation-code printing step is analogous to a receipt printed during shopping checkout in stores. The simultaneous printing unambiguously associates the confirmation code with the ballot in a manner consistent with buying groceries, say. Anecdotal evidence from Preston voters indicated confidence increased for some solely based the fact of the printed confirmation code because of its *latent promise of fulfillment*; that it *could be* used if desired or necessary was sufficient [^18].

The precinct scanner and the ElectionGuard encryption process also reinforced each other. It was clear that the review screen that presented the scanner's interpretation of the ballot, and the confirmation code that was printed just prior, emanated from the same core act of inserting the ballot into the scanner. And while the Verity precinct scanners were technically new to voters in each of Preston and College Park, the core voter behavior of filling out the ballot (by hand in Preston, by hand or TouchWriter in College Park) was consistent, or at least sufficiently familiar, across elections, so as not to constitute a meaningful barrier to completion. It helped immensely that poll workers were trained to assist voters with the process, the language they used describing the process was simple enough to be easily understood, and they repeated it to each and every voter as they encountered the scanner for the first time [^19].

### *"Shoot the Messenger" Bias*: the Negative Impact of Security

There's a startup, or at least marketing, adage that "it's better to sell aspirin than vitamins"; in other words, it's  easier to succeed with a product when its value proposition is causally associated with an observable benefit immediately (headache relief from aspirin) rather than a diffuse benefit in the future (vitamins  make you healthier).

In the case of confidence and election integrity, or really almost anything involving security, it's easier to succeed if you don't mention a security or confidence benefit at all. It's hard to create trust from mistrust. Call it the user-experience equivalent of [the Streisand Effect](https://en.wikipedia.org/wiki/Streisand_effect), where calling attention to a thing brings unwanted attention to it.

!!! quote "It is hard to create trust from mistrust"

That reason discussions of security backfire is because to call attention to the potential negative outcomes security protects against focuses attention on those very things or alerts the voter to their existence as a concern. While it is correct from a modern system administration standpoint to assume an adversarial posture with respect to sensitive information and system access for organizations and individuals, for those unused to thinking in that mindset, it can raise questions about a system where previously there were none.

When organizations embrace a [Zero Trust](https://en.wikipedia.org/wiki/Zero_trust_security_model) philosophy, for example, employees are trained to see the practices they follow (multi-factor authentication, phishing prevention, data retention, etc.) as providing the security; following good security practice reinforces the security message.

There isn't an equivalent individual experience or "practice" for voters to adopt to improve the security of the voting system they use, however [^20]. So while ElectionGuard adheres to a Zero Trust model with its security practices [^21], and one could argue it is providing a Zero Trust capability for voting systems [^22], we did not focus on this capability in our public-facing communications [^23]. 

Because voting is a complex enterprise, there are many opportunities for confidence to be eroded as the system is revealed. Even if the voter believes the election administrator competent and doing their best, and the job being well done, the simple fact of the complexity, the realization of the size of the "attack surface" of an election, can reduce confidence in the system, especially since even ancillary activities such as registration waiting times have been shown to diminish confidence on their own [^24].  

#### BallotCheck is Zero Trust

BallotCheck is a feature of ElectionGuard and e2e that enables voters to "challenge" the voting system to verify it is recording ballots as they intend. The secret ballot (see below) precludes showing the voter how they voted after their ballot has been cast. A BallotCheck ballot, however, occurs not on a cast ballot but one that the system *believes* is going to be cast but isn't.

With the Preston and College Park elections, that occurred when voters inserted their ballots into the scanner.  At that moment, the scanner has to interpret the ballot, and then send the results to the ElectionGuard software so it can pass back the proper confirmation code to print. The voting system can't "undo" what it sends to ElectionGuard after the fact, and it can't decrypt the ballot on its own, either; that commitment is etched into ElectionGuard stone.

After the confirmation code has been printed, the Verity scanner shows the voter the ballot review screen. In most instances the voter will confirm the system recorded their choices correctly and no mistakes were made (e.g., overvoting or undervoting contests), and then select the "Submit" or "Cast" option on the screen to drop the ballot into the voting booth and record the ballot as cast.

Sometimes voters make mistakes or their marks are misinterpreted by the scanner, though. When that happens and the ballot needs to be changed, the voter has the option to "Cancel" or "Reject" the ballot from counting. The ballot is returned, a poll worker is notified, and the appropriate changes are made by the voter, which might necessitate filling out a new ballot.

That is the same process as performing a BallotCheck. Because the scanner has printed the confirmation code, the system already has an encrypted copy of the ballot. But because the ballot is not being cast, it is not subject to the secret ballot requirement. Ballots that have been BallotChecked can be decrypted and displayed to the voter, and made available as part of the publication of the public election record.

#### Like Nebraska, It's Not for Everyone

However, that's a lot for a voter to process on top of handling new voting methods and processes. While poll workers needed to be trained on the BallotCheck process to answer questions and manage the process for voters that did want to do a BallotCheck, the team decided not to mention BallotChecks as part of the standard explanations voters received about the voting process [^25].

### The Cognitive Dissonance of the Secret Ballot

The secret ballot is a security practice designed to solve the problem of widespread fraud or vote manipulation. It was employed across the US after vote-buying scandals in the 1888 election.

For the secret ballot to be observed, voters vote privately in a public polling place. Voters should be confident they are not being observed filling out the ballots, nor that the ballots are observed in the process of casting.

The secret ballot also requires that voters not be able to review their ballots after they have been cast, because otherwise they could be coerced to reveal their vote after the fact.

#### Does the Secret Ballot Increase Confidence?

In a secret ballot election, voters can't review how they voted after submitting their ballot. This is not common across a voter's other daily experiences. In almost every other transaction in which citizens engage, there is a record of the event that can be reviewed after the fact. Shopping has receipts; bank deposits are recognized as transactions. The secret ballot doesn't provide a mechanism to show your vote  *by design*.

For voters that don't understand why the ballot is secret, the fact that they can't check how that their vote was registered can decrease confidence. Even if a voter then remembers or eventually accepts why they can't review their ballot, that acceptance is written on the palimpsest of their initial frustration.

Studies show that while voters appreciate and understand the necessity of the secret ballot, their confidence is a function of both their recency and frequency of voting and also on whether they feel the secrecy is actually maintained by the voting process or method [^26].

Even voters that do appreciate the secret ballot *intellectually* may not appreciate the tradeoff *emotionally*. Errors occur; things happen. How does a voter know there wasn't a mistake processing their ballot, or that it was even counted?

This makes intuitive sense. Because the voter does not have the means to check the official record of the their vote, they are left to trust the system to process it correctly.  If there is a diminishment of confidence, how is it restored? What is it attached to?

#### Vote By Mail Ballots are Not Secret

Complicating the efficacy of the secret ballot is the emergence of Vote by Mail (VbM) as a prevalent voting method. A significant (and growing) percentage of voting districts use vote by mail as either their principal voting method or for absentee voting. As of early 2024, eight states and the District of Columbia allow **all** voting to occur by mail. Thirty-five states allow voters to vote by mail for absentee purposes [^27]. In total, 46% of ballots in 2020 were cast via mail-in ballots [^28].

A mail-in voting ballot is not secret because the voter fills out the ballot away from the polling place and outside the observation of poll workers. Coercion is possible because voters can be required to take a photo of their ballot or otherwise show it to the coercing party. There's also the "local" coercion of whoever is in the room when the ballot is being filled out, including disabled voters with no ability to mark the ballot being dependent on someone else to do it for them; each can undermine the privacy promised by the secret ballot, since voters in any of these situations can feel compelled to change their vote to a more "socially acceptable" option that doesn't reflect their true preference [^29].

Outside of the potential coercive effect, voters are less confident in mail-in ballots than in-person ballots because they are simply less confident their ballots will be counted at all. Other actors such as the postal service are necessary to transport the ballot to the election administrator, and there is simply concern about whether all steps of the process have been followed (and security preserved along the way) [^30].

##### ElectionGuard Offers a Silver Lining

Although ElectionGuard has not been deployed in a Vote by Mail election yet, the 2.0 Specification enables Vote by Mail support [^31]. It has the potential to *increase* confidence with Vote by Mail specifically, since voters will be able to not only verify that their ballot was included in the tally, as occurs with precinct scan, but also that it was recorded as they intended, while maintaining their privacy.

In a precinct-scan election, the voter is present, so the encryption and the associated confirmation code can be generated dynamically directly from the voter's choices. The voting machine, since it's under the control of the election administrator, also contains information for generating the encrypted ballots, including the primary nonce, hashes, manifest, and election parameters.

With Vote by Mail (or any central count election), the voter isn't present when the encryption takes place, so we need to give them the ability to generate their own confirmation code prior to submitting their ballot. This is done by using ElectionGuard's "Pre-Encrypted Ballots" capability [^32].

With pre-encrypted ballots, instead of generating the confirmation code dynamically when the ballot is submitted into the scanner, the full set of choices the voter *could* make is generated in advance. A ballot hash, which is uniquely generated for each ballot as part of the pre-encryption process, is printed on each ballot directly as well as a set of short codes (e.g., "IA", "J4", etc.) for each contest option. (The short codes are uniquely generated for each ballot, so while the codes themselves may recur, they randomly apply to different contest options on other ballots and contests) [^33].

As the voter fills out their ballot, they accumulate the set of short codes associated with each of their choices. The entire set of selected short codes constitutes the remainder of the confirmation code printed on their ballot (ideally they have a separate copy of only the short codes for the voter to keep and check the result with) [^34].

When the voter is done, they mail or drop off their ballot in a ballot box, retaining the base confirmation code and the set of the short codes they selected. When the election record is published and they go to confirm their ballot, in addition to the notification about whether the ballot was included in the election, they can also view the short codes they selected to see if their ballot was recorded as they intended.

This is potentially a meaningful benefit. Vote by mail voters concerned about the successful processing of their ballots or whether their votes were properly recorded have a means to do so without (further) compromising the secret ballot [^35].

### Does ElectionGuard Make It All Better?

In many respects, dealing with the the secret ballot and its impact on voter confidence is one of the key challenges ElectionGuard was designed to address. It encrypts ballots in such a way that we can verify many of the properties of the election and all its constituent ballots without revealing the specific votes cast by individual voters.

In addition, by virtue of the confirmation code and BallotCheck, ElectionGuard provides a means for voters to participate in the election process in ways that can exhibit either or both of Zero Trust and common shopping behaviors. By using the confirmation code to verify that their votes were properly recorded and included in the election is an affirmative step that helps with the overall security posture of ElectionGuard. A BallotCheck is a Zero Trust action. For vote by mail voters it would be worth studying whether the additional confirmation code information available for the lookup process increases confidence in the system as an auditing action. Secret ballot lemonade, if you will; tart and sweet.

Taken together, a verifiable election record can provide all the details necessary to review an election except the contents of the encrypted ballots themselves. The confidence question at its core is whether the ElectionGuard "infrastructure", voter actions, and third-party oversight compensates for the lack of emotional surety of verifying the ballot itself.

### Tell Me You're Increasing Confidence Without Telling Me You're Increasing Confidence

There's a startup or marketing adage that "it's easier to sell aspirin than vitamins". It's easier to succeed with a product when its value proposition is causally associated with an observable benefit (headache relief from aspirin) than the possibility of a diffuse benefit in the future (vitamins make you healthier). In the case of confidence and election integrity, or really almost anything involving security, it's easier to succeed if you don't say anything at all.

Beyond security and Zero Trust, explaining what ElectionGuard is and how end-to-end verifiability works is also a challenge. The cryptography, security, core capability and mathematics of homomorphic encryption, as well as the interplay of the specification, code, and independent verifiers, is simply a lot to comprehend.

However, voters of all types may want to understand why ElectionGuard might matter to their elections or  why it's being used. Being able to handle the questions of all voters, from the least to the most technical, all without overwhelming voters with unnecessarily complex detail, was critical to understanding and, ultimately, validation and acceptance.

To that end, prior to our election in Preston in 2022, we did some user research on terms to use that were  accurate and understandable but also established a sentiment in the voter reflective of the benefit of the effort. For example, we ended up using the term "independently verify" elections rather than have them "end-to-end verifiable"; the "independence" angle give the voter the proper feeling of agency while accurately reflecting the core capability of the system (creating an independent, encrypted copy of the election and all its ballots). Instead of security and prevention of malfeasance, we emphasized transparency and independent validation.

Similarly, rather than try to explain the technology behind a challenge ballot with its label, we branded it with the quasi-marketing term BallotCheck, indicating an innovation without specifying its nature. The intent was not to hide information, but enable layers of inquiry appropriate to a voter's interest. It is just as important not to overwhelm and overcomplicate the voter's conception of the ElectionGuard "feature", either.

## Future Potential

### ElectionGuard and e2e as a Means to Increase Access for all Voters

The ElectionGuard adoption effort to date has focused on using ElectionGuard with paper-based voting methods. Its first election in Fulton, Wisconsin, used a system built with VotingWorks that included a ballot-marking device, smart card, and printer to print a paper ballot and its associated confirmation code. Subsequent elections in Preston, Idaho, and College Park, Maryland, used Hart InterCivic's Verity Scan system, a precinct scanner that prints the confirmation code when a ballot is inserted.

But ElectionGuard is not limited to paper-based voting methods. It is used by Enhanced Voting as part of their electronic ballot return capability.

!!! tip "ElectionGuard's key differentiation relative to paper and any specific voting method is its  potential to span ***all*** voting methods."
    If every voting method a jurisdiction uses incorporates ElectionGuard, the entire election can be independently verified while completely respecting voter privacy.

Enabling electronic voting systems is important because paper-based voting methods are not sufficient to serve many voting constituencies. And without a technology like ElectionGuard going forward, voting systems that are strictly electronic will have no ability to be certified for voters.

### How Does ElectionGuard Enable New Voting Methods?

The mechanism that enables ElectionGuard to provide a certification pathway for non-paper-based systems is a concept called ***software independence***.

!!! info "What is Software Independence?"
    The formal definition of software independence is:
    > A voting system is software independent if an undetected change or error in its software cannot cause an undetectable change or error in an election outcome.

    Practically speaking, software independence is the ability to independently verify an election and its outcome without reliance on the system that recorded the votes and generated the initial tally. Paper-based ballots are software independent because they can be recounted by hand or scanned by independent systems to verity the results obtained. ElectionGuard is software independent because it provides an independent, encrypted copy of the election and all its ballots that can be independently verified 

### The Potential Benefit of Scale and Third-Party Oversight

## Conclusions and Recommendations

* Sporadic, event-based operation with fixed, regulatory-driven deadlines
* Offline operational bias
* Fragmented regulatory and operational practice
* Low error tolerance
* Advocacy bias toward paper-based, manual voting methods
* Small, litigious industry with largely zero-sum market potential
* Static, manual software deployments to custom embedded hardware
* Limited opportunities for new software deployments; high cost of switching
* Uncertain certification process

## Acknowledgements

[^1]:
    dynamic (covid, lightning strikes)

[^2]:

[^3]:
    Russia, China, Iran

[^4]:
    polarization

[^5]:
    scrutiny

[^6]:
    threat environment

[^7]:
    limited budgets

[^8]:
    universal service obligation

[^9]:
    immediate results

[^10]:
    The simplest way to think of ElectionGuard is as a technology similar to paper that records and encrypts ballots and elections in such a way that voters and independent observers can verify

[^11]:
   Lonna Rae Atkeson, R. Michael Alvarez, and Thad E. Hall: [Voter Confidence: How to Measure It and How It Differs from Government Support](https://www.liebertpub.com/doi/full/10.1089/elj.2014.0293)

[^12]:
    Clark, Jesse and Stewart III, Charles (2021) [The Confidence Earthquake: Seismic Shifts in Trust](http://dx.doi.org/10.2139/ssrn.3825118)

[^13]:
    Adderson, Christopher, and LoTempio, Andrew. (2002) [Winning, Losing and Political Trust in America](https://doi.org/10.1017/S0007123402000133)

[^14]:
    Clark and Stewart, The Confidence Earthquake

[^15]:
    Claudia Ziegler Acemyan, Philip Kortum, Michael D. Byrne, and Dan S. Wallach (2018) [Summative Usability Assessments of STAR-Vote: A Cryptographically Secure e2e Voting System That Has Been Empirically Proven to Be Easy to Use](https://journals.sagepub.com/doi/full/10.1177/0018720818812586)

[^16]:
    The ElectionGuard specification also makes security claims as part of its value proposition, and those are dependent on performance of certain actions of voters or other external actors. For example, as little as 1% of voters need to perform a BallotCheck to provide a sufficient disincentive to ballot manipulation. While this is no doubt a good thing, and if widely known may have a salutary effect on voter confidence, it does depend on a more sophisticated conception of security and the role of skepticism in election transparency for a subset of voters in any municipality (see security discussion).

[^17]:
    Idaho report

[^18]:
    College Park report

[^19]:
    Bridgett A. King (2020) [Waiting to vote: the effect of administrative irregularities at polling locations and voter confidence](https://www.tandfonline.com/doi/full/10.1080/01442872.2019.1694652), Policy Studies, 41:2-3, 230-248

[^20]:
    At least with respect to in-person voting methods and systems that handle paper ballots under the control of the election administrator. Electronic remote voting methods that use mobile devices or computers owned by voters present a different use case with a different security threat model and role for ElectionGuard.

[^21]:
    Zero Trust security practices

[^22]:
    Discussion of Zero Trust and ElectionGuard

[^23]:
    ElectionGuard Zero Trust extending to voting system

[^24]:
    The reason ElectionGuard is involved in an election at all is because an election administration is investing in ElectionGuard's stated purpose of **increasing** confidence in the voting public. They do not do this at their own expense, and it would be entirely unbecoming to introduce the very skepticism we intend to counter, even on the altar of Zero Trust.

[^25]:
    A Zero Trust philosophy fully adhered to would have the voter consider the election administrator as a potential malicious actor, and consider ElectionGuard and its capabilities as the means to detect their malicious behavior.

[^26]:
    Gerber AS, Huber GA, Doherty D, Dowling CM. (2013) [Is There a Secret Ballot? Ballot Secrecy Perceptions and Their Implications for Voting Behavior](doi:10.1017/S000712341200021X). British Journal of Political Science. 43(1):77-102.

[^27]:
    [Table 18: States With Mostly-Mail Elections](https://www.ncsl.org/elections-and-campaigns/table-18-states-with-all-mail-elections), from the National Council of State Legislatures report [Voting Outside the Polling Place: Absentee, All-Mail and Other Voting at Home Options](https://www.ncsl.org/elections-and-campaigns/voting-outside-the-polling-place)

[^28]:
    [The Confidence Earthquake](https://electionlab.mit.edu/sites/default/files/2021-07/clark-stewart_the_confidence_earthquake_final.pdf)

[^29]:
    Hall, Thad, (2009) [Electronic Elections in a Politicized Polity](https://vote.caltech.edu/working-papers/76)

[^30]:
    Charles Stewart, Michael Alvarez, and Thad Hall. (2015) [Voting Technology and the Election Experience: The 2009 Gubernatorial Races in New Jersey and Virginia](https://dspace.mit.edu/handle/1721.1/96629?show=full), p.4

[^31]:
    Benaloh, Josh, and Naehrig, Michael, Microsoft Research (2023) [ElectionGuard 2.0 Specification: Pre-Encrypted Ballots](https://github.com/microsoft/electionguard/releases/download/v2.0/EG_Spec_2_0.pdf), Chapter 4, p. 47.

[^32]:
    For example, due to the nature of voting contest (yes/no choices from a set of known options), we can encrypt the data and embed cryptographic "tests" (specifically, [non-interactive zero-knowledge proofs](https://en.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof)) that verify aspects of each ballot and the contest tallies of an election such as:

[^33]:

[^34]:

[^35]: