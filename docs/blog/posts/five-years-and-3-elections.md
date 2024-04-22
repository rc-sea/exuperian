---
 
date: 2024-04-19
categories:
  - electionguard
  - product
  - open-source
authors:
    - rc-sea
---
# Improving Confidence and Participation in US Elections: Lessons from Five Years and Three Elections with ElectionGuard

## Abstract

ElectionGuard is an open-source software development kit that enables election system vendors to implement end-to-end verifiable elections (e2e-v) in their systems.  Its intent is to increase participation in elections and confidence in election outcomes.

Unlike previous e2e-v systems, ElectionGuard is designed to be integrated into other voting systems and existing election procedures, and does not add any steps to the core voting process.
This article outlines some of the key learnings in deploying ElectionGuard with multiple election vendors in three different US elections over five years of software development, including how success was measured and what was learned from user testing and voter interaction.  
In addition to the work preparing the core software capability and services with vendor partners VotingWorks and Hart InterCivic, the ElectionGuard team and the Center for Civic Design worked extensively with election administrators to develop usable and useful documentation, poll-worker training, and voting procedures that presented ElectionGuard as an aspect of the entire voting process, not a standalone technology.  Enhanced Voting hosted the confirmation code lookup site, and MITRE wrote two independent verifiers.

Due to the small sample size and qualitative nature of the user research, it is difficult to say definitively that confidence was improved, and even if it were how much would be attributable to ElectionGuard. However it is reasonable to say the community was supportive of the efforts and appreciated the investment in voting and elections generally. In addition, finding the proper ways to articulate the value and importance of a complex technology to a universal audience is critical for scaling adoption and understanding going forward.

## What does it mean to increase confidence and participation?

Confidence refers to the perception of the administrative conduct of elections:  were votes properly counted? Research shows voters have a distinct impression of election administration separate from the political environment or actors around it [^11].

Many factors contribute to a voter's confidence. Voters are generally more confident about their local elections than those in other jurisdictions or that use different voting methods [^12]. The "winner-loser gap" shows that voters who vote for a winning candidate, especially in major elections such as US President, tend to have more confidence in the election outcome than those who voted for the losing candidate [^13]. Confidence can be affected by a voters' personal experience with the voting process, especially as relates to waiting in lines or interactions with poll workers [^15].

During the elections, feedback was collected from voters through exit interviews, post-election surveys, observations in the polling place, and monitoring the use of the confirmation code lookup website.  While there was not sufficient volume and robust enough sampling to make any declarations with scientific rigor, anecdotal review speaks to support for investing in election technology generally as valuable. Many voters appreciated the mission of the endeavor if not its technical nuances. Few criticisms were voiced other than concerns about funding, and all the election administrators found the experience worthwhile.

## Implementing ElectionGuard

ElectionGuard is a free open-source software development kit (SDK) that enables voting system manufacturers to incorporate end-to-end verifiable election capabilities into their systems. It is not a voting system in and of itself. It is deployed as a component of a voting system and enables elections produced by that system to be end-to-end verifiable.

The configuration of an ElectionGuard election can vary based on the voting method, the integration approach used, as well as any relevant voting regulations and processes concerning what can be printed on paper ballots, whether rescans are allowed, etc.

ElectionGuard was initiated and incubated within Microsoft's Democracy Forward initiative in 2018 and has been used in three elections since: Fulton, Wisconsin (2020); Preston, Idaho (2022), and College Park, Maryland (2023). Enhanced Voting has incorporated ElectionGuard as part of its Electronic Ballot Return solution that is currently being used in several US states. ElectionGuard is also used in multiple European voting systems.

A fully-realized ElectionGuard SDK would support all voting methods: paper-based, electronic, remote/portable  and in-person. This extends to different tally methods and electoral systems: plurality, ranked-choice, approval voting, and more. By extending across all the voting methods used within any jurisdiction, the full election record generated becomes a public resource that can identify areas of potential tampering but hopefully mostly serve as a publicly verifiable validation of the released tallies.

### Elections overview

Real-world elections offer the final test of any new voting technology.  Only in real-world conditions can it be determined whether the technology is properly working and resonating with voters.  Even though the ElectionGuard tally in these elections would not serve as the official tally, it was nevertheless inherently intended to validate the results of the system of record.  To not either validate a valid election or invalidate an invalid one would be to fail. There were also concerns about whether ElectionGuard would overburden and slow down the voting process, especially if a lot of voters decided to challenge ballots.

In the first election in Fulton, Wisconsin, the voting system was new to voters: would ElectionGuard make it too complex? When the voter submitted their ballot on the device, the ballot was encrypted and the confirmation code generated. The voters then went to the printing station with their smart card to print the ballot and the confirmation code. Then they went to a vote checking station to verify the printed ballot was correct. And then they could drop the ballot in a ballot box or challenge it via the ballot spoil process.  

For the next two elections in Preston, Idaho and College Park, Maryland, ElectionGuard was integrated into Hart InterCivic's VerityScan precinct scanner, Enhanced Voting hosted the confirmation code lookup site, and MITRE wrote an independent verifier to evaluate the election record.  Voters filled out the ballot by hand (in College Park voters could also use the TouchWriter ballot marking device to print their ballot) and inserted the ballot into the VerityScan scanner to scan and encrypt the ballot, print the confirmation code, and present a results screen to the voter. Voters cast the ballot by pressing the submit button, or initiated a ballot challenge or spoil process by cancelling submission.

In Preston, voters had the choice of using the VerityScan/ElectionGuard system or a simple ballot box (in either case, the ballots were hand marked).  Only when poll workers assured voters that the VerityScan/ElectionGuard option wouldn't take much more time did uptake increase; by election close to two out of every three voters were opting in to use the VerityScan/ElectionGuard option.

In College Park, the VerityScan option was new to voters, but hand-marking ballots and using a precinct scanner was not .  The tally included a vote-by-mail component for absentee voters, but those voters did not receive a confirmation code.

In-person voters in both College Park and Preston appreciated the real-time review of the scanned ballot presented by the VerityScan device coupled with the confirmation code for them to take home. Informal time and motion studies confirmed active review of the interpreted ballot, and exit polling validated the perceived utility.  No meaningful process slowdowns were created due to ElectionGuard or ballot review tasks; bottlenecks, if they occurred, were elsewhere in the process.

### Specification

ElectionGuard starts with its specification, a statement of intent about:

* what base election parameters are used
* how ballots should be encrypted
* the homomorphic properties of the encryptions
* their associated non-interactive zero knowledge-proofs
* how to perform the key and tally ceremonies of the the election guardians
* how to enable specific ElectionGuard "configurations" such as pre-encrypted ballots
* what validations an independent verifier should perform

The specification provides guidance for coders to develop the SDK and independent verifier developers to validate published election records. It documents the procedures to follow and artifacts to generate during the encryption process as well as how to enable guardian key and tally ceremonies to be performed by an administrative device.

The ElectionGuard specification has evolved over time, and will continue to as different features are enabled, additional mathematical optimizations and voting methods are supported and, if necessary, updates if vulnerabilities are discovered. 

### SDK

SDK stands for software development kit. There are many types of SDKs, but for ElectionGuard it consists of a set of Github repos that execute the features and capabilities described in the ElectionGuard spec. The goal with the SDK is to provide all the tools a voting system vendor would need to easily integrate ElectionGuard into their systems.

The ElectionGuard SDK has evolved its development approach  over time in response to iterations of the specification itself and learnings from the different operating environments it's been used in. The core components currently consist of:

* a ballot encryption software package
* a software application and administrative interface that performs the key and guardian ceremonies
* a set of developer utilities for testing integrations
* sample ballots and data
* automated tests and deployment utilities

### Ballot encryption codebase

Ballot encryption is the atomic unit of activity of ElectionGuard. Every ballot in an end-to-end verifiable election is encrypted according to the ElectionGuard specification. Each and every encrypted ballot should be able to participate in the tally ceremony of the election guardians as well as be evaluated by an independent verifier to ensure the contents are valid and well-formed.

Over the course of three elections, the encryption module of the SDK underwent significant evolution. For Fulton, Wisconsin, the encrypting device was a standalone modern Surface tablet that also served as the ballot marking device for voters. For the Preston and College Park elections, the Hart VerityScan device encrypted the ballot.

The compute environments were markedly different. The Surface device used in Fulton was a fully configurable COTS tablet and used an aggressive processor and memory configuration. The only other application running was a web browser with the voting application. Due to the election size and ballot encryption time there was no need for further optimization.

Engineering to support the memory and compute capabilities of the VerityScan was significant, however.
The processor was cost-optimized for an embedded device not accustomed to sharing memory or application space with any other software. No memory leaks or any other disruptive activities were allowed for security purposes and operational stability; one does not "reboot" in the middle of an election.  

Outside of ensuring operational stability, the biggest effort was spent reducing the raw time to encrypt ballots. The very first unoptimized encryption time of a sample ballot on a VerityScan in 2023 was over two minutes. Through a variety of optimizations and "pre-compute" routines, the encrypt time was reduced to sub-second performance.

### Guardian ceremony and encryption codebase

With ElectionGuard the encryption keys as well as election tally are administered by a set of independent actors called guardians. At the beginning of the election, the guardians present generate the encryption key that will encrypt ballots during the election. At the end of the election the guardians reconvene to generate an encrypted tally, and then decrypt that tally and publish the election record and its associated artifacts and zero-knowledge proofs.

For College Park, thresholding capability was finally enabled in the SDK admin software. Thresholding requires only a pre-set quorum of guardians to need to be present to conduct the tally after a successful key ceremony of all guardians. In College Park, the tally ceremony was performed live by the election guardians on election night; five guardians were used with a quorum of three.

While the technical rationale for the guardian ceremonies is the security overlay it provides to the encryption and tally processes, public ceremony is an act of oversight as well. In each election recognized leaders of the community were asked to serve as guardians. The software process they followed was very simple; no technical capability was necessary. Their independence, reputation, and participation together with other recognized community leaders was intended to lend the procedure wider community legitimacy.  While the elections ElectionGuard was used in were not tightly contested nor controversial, this capability could be an important component of bolstering confidence when adjudicating elections that are.

The software process underneath the user interface is more complex. With ElectionGuard, the homomorphic properties of the encryption need to be developed and tested, and that means a whole discipline of working with data in its encrypted state. Figuring out when an encrypted value is incorrect, and if so how, and then if so what to do to fix it, can't be done by traditional decryption and "eyeballing" processes.

### Confirmation code generation

Confirmation codes are unique codes generated for each ballot by the ElectionGuard ballot encryption process. When voters go to a confirmation-code-lookup site when the election is over, their confirmation code is used to find out whether their ballot was included in the tally. The confirmation code can be attached to a URL and QR code, or the voter can search for or enter the code manually on a lookup site directly. 

The confirmation code is the only way for the voter to track what happens with their ballot, so it is important they have an easy means to take it with them when they leave the polling place or submit their ballot. For the College Park and Preston elections, the confirmation code lookup and hosting functionality was performed by Enhanced Voting.

In Fulton, the VotingWorks ballot marking-device encrypted the ballot, and then saved the VotingWorks ballot and a print version of the ElectionGuard confirmation code to a smartcard to be printed on a separate device.

For Preston and College Park, Hart InterCivic's VerityScan scanned, interpreted, and then encrypted the ballot, printing the confirmation code immediately using an internal thermal printer.
Other voting methods may use different mechanisms to generate the confirmation code depending on whether the voter is present when their ballot is interpreted by the voting system.

### Challenging a ballot

Challenging a ballot refers to the process where the voting system is forced to reveal how it stored a voter's choices on a ballot. Because the ballot choices are revealed, the ballot cannot be cast, but the process nonetheless reveals how the system would have recorded a voter's ballot if it were.

If the system is recording the ballots without error or malign intent the voter's confidence is bolstered that the voting system is working as intended. If however it is systematically misinterpreting ballots the challenge process will reveal the error because the system has already committed its choices to ElectionGuard and can't modify the ballot after the fact.

For in-person paper-based voting, ElectionGuard intentionally patterned the challenge process on the existing "spoil" process election administrators already use for voters that change their mind or make mistakes filling out their ballot.

When the voter is not voting in person, other challenge mechanisms may obtain, and not all involve the voter actively challenging ballots themselves.

### Independent verifiers

Independent verifiers should be able to verify an e2e-v election based solely on the specification and an understanding of how the SDK will present the different election parameters, ballot, manifest, and tally artifacts produced in the final election record. A verifier should not have to know what voting methods were used to generate the encrypted ballots, nor anything about the voting system itself.

If the election record is implemented correctly and the independent verifier is able to process it and reach the same tally conclusion, the verifier is effectively rendering a verdict on the election, that it has been end-to-end verified. If it doesn't find any problems with the ballots and the derived tally, it is certifying that what happened happened.

This is the approach MITRE took in writing verifiers for the Preston and College Park elections. They wrote their verifiers to be open source and easily inspectable, and also to report where a failure occurred in the verifications outlined in the spec. While they participated in discussions and submitted bugs that contributed to the final output and format of the election record, it was in service of having the specification and the election record reflect their evaluation intent.

In working to deploy ElectionGuard for the College Park election, the ElectionGuard specification had been updated to a new version and the SDK code was trying to "catch up" to the new version. The code was not fully up to date by the date of an agreed-upon code freeze and as such did not implement every validation in the election record called for in the spec. Even so, MITRE did give a provisional approval of the record as demonstrating what it claimed, which included the most important demonstrations of tally and ballot correctness.

## Key Lessons

### First, do no harm

ElectionGuard is a software development kit (SDK) for adding the feature of end-to-end election verification; it does not and should not change the core voting process.

Several end-to-end verifiable elections have been previously held in the US, each using different methods to record e2e-v ballots.  They all imposed additional steps to the voting process itself: to successfully cast a vote, the voter had to engage with the e2e-v methodology. Studies of these approaches reinforced the core notion that "voters expect the steps required to vote with an e2e system to be roughly the same as voting with a standard paper ballot or electronic voting machine."  
In all of the elections, from the perspective of voter, the ElectionGuard "experience" consisted solely of receiving an accompanying confirmation code printout. The actions the voter had to perform were solely for the purpose of voting.

Voters could also perform a BallotCheck challenge, but that process was purposefully identical to the process by which voters who make a mistake or wish to change their ballot before casting it follow. ElectionGuard created a procedural wrinkle about whether all spoils would be treated as challenges, but the software supported either approach. The core process from the voter's perspective in either case was identical, however.

### Analogies matter

People use analogies to make sense of new experiences. With ElectionGuard, the action of printing the confirmation code during voting is similar to generate a tracking number used to track packages: you can't see what is being delivered, just whether it has been.  Anecdotal evidence from both Preston and College Park voters indicated confidence increased for some solely based the fact of the printed confirmation code because of its familiarity and latent promise of fulfillment; that it could be used was sufficient [^18].

The voting system and the ElectionGuard encryption process reinforced each other. The combination of having a ballot scanner’s (in the case of the Hart system)  interpretation of the ballot on a review screen and that the confirmation code came from the same device made the association of the two actions clear. Even the pause before the review screen was displayed – waiting for the confirmation code to begin printing - helped focus voters’ attention on this final moment to confirm their votes before casting and establish that the confirmation code was already committed.

### The Proper Value Prop

ElectionGuard, and end-to-end verifiability in general, are complex systems using advanced cryptography and security practices in a fully open source environment. However, those features may not resonate when explaining the value of ElectionGuard to the voting public.

In testing voter education messages before the elections, both ad hoc and formally, it emerged that discussions of security as a benefit can backfire because they often call attention to the potential negative outcomes the security procedure is designed to protect against.  In the worst case, doubt can be introduced where previously there was none.

The results of user testing unambiguously resonated around the individual agency and transparency benefits ElectionGuard afforded:

* Voters could use confirmation codes to demonstrate to themselves that their ballot was included.
* Making the entire election record public and verifying it independently increases transparency, enabling better scrutiny

### Bite. Snack. Meal.

There's a tendency when discussing a complex topic to feel the need to to try to explain it so people fully understand. Even if that were advisable, end-to-end verifiability in full scope is an intimidating topic. When it came to explaining what ElectionGuard was, it was almost impossible to strike a balance between being explicitly technically accurate while using language everyday voters could understand. Discussing ElectionGuard's technical rationale or capabilities could confuse and thus quickly alarm a voter we were hoping to excite.

People are showing up to vote, not to participate in the launch of a new technology. Voters really only need to understand how to vote if they don't already (the voting system itself was new to voters in Fulton and College Park elections). Beyond that, the minimum viable takeaway was for voters to understand the meaning of the confirmation code printout generated by the voting process. 
In Wisconsin, the separately printed confirmation code page provided a place to provide additional information explaining its purpose. Because we had a fully standard separate page for printing the code, we included explanatory information and where to find more on the entire process.

For Preston and College Park, because the printout size of the code was much smaller, receipt-holders were pre-printed for each voter to store the printout generated by the scanner. The receipt-holders explained what the confirmation code was for and how to check their ballot was included, and where to go for more information if they're interested.

Poll workers needed to know more because they had to be prepared to answer questions from voters. Even still, they were spared discussions of homomorphic encryption and cryptography; the goal was to help them feel confident about what they're imparting to voters. In this case it was easier to articulate a value proposition than an explainer: what the process was supposed to accomplish rather than how it worked. The point of the confirmation code printout was so the voter could check their ballot was included in the published election results. All poll workers needed to tell the voter was to tear the printout off and put it in the sleeve we handed them, and that the sleeve had all the info they needed to know if they had any questions.

This bite --> snack --> meal approach permeated all discussions and documentation. The ballot challenge process is another example. Rather than tell the voter they could "challenge a ballot", they were told they could run a BallotCheck. The term was intentionally made to sound like a generic product innovation than explicitly a zero trust exercise to prove no malicious actor was present.

### It takes an Ecosystem

ElectionGuard is an open source SDK. It ultimate success resides in those who contribute to and adopt it for real-world use. The election administrators who green-lighted ElectionGuard for use in their locales took risks they didn't have to, and took work upon themselves they likely didn't realize the full scope of when work began.

VotingWorks, Hart InterCivic, and Enhanced Voting dedicated significant resources to working with and QA-ing different aspects of the voting codebase to integrate ElectionGuard into their systems. And MITRE independently wrote two distinct verifiers to support the Preston and College Park elections.

The Center for Civic Design provided user research, documentation and training materials, and exit polling. InfernoRed developed the production codebase and admin software of the SDK, and numerous academics and open source researchers contributed to the security techniques and cryptographic review along the way. This project wouldn't be successful without all their contributions.

### An iterative process

Election administrators will often use multiple voting methods to address different voting populations or phases of an election. ElectionGuard will be its most effective when it can address all of the voting methods and configurations in common use, but achieving that capability will necessarily take many iterations across elections of different makeup and size.

Each election to date has presented operational and regulatory differences. Innocuous regulatory requirements can render ElectionGuard ineligible, such as a requirement to re-scan all the ballots scanned previously when a scanner is taken out of service  .  The largest impediments to wider adoption are regulatory-driven requirements that prevent modification of certified systems to accommodate new features such as ElectionGuard without triggering recertification of the entire system.

## Conclusions and recommendations

### Success at the local level

With each of the three elections the ElectionGuard program has been involved in to date, the local populace has been supportive of the effort. Some voters were happy to see any investment in voting technology, others were happy Microsoft was involved. Most voters found positive things to say about the experience even if it wasn't directly related to ElectionGuard.

That is unambiguous good news, even if it isn't solely due to ElectionGuard itself. If the goal of the endeavor is increasing confidence in election outcomes with the voting public, it some sense it doesn't matter whether that confidence manifests in checking a confirmation code or challenging a ballot. The awareness of  being able to matters in providing emotional confidence. Seeing investment in election technology at all can also be the "source" of confidence.

In many ways a better test of efficacy is not how much ElectionGuard was noticed as part of an election, but how much it wasn't. It didn't get in the way of voters' productivity. It didn't cause confusion for voters. It looked like it was part of the voting system.

Exposure to ElectionGuard over time should allow it to be seen in its own analytic light, but that it can coexist peacefully with the base system is its own accomplishment considering the significance of the code and cryptographic principles being applied.

### Bolstering the Secret Ballot

The secret ballot is a cornerstone of modern democratic voting, and arguably the last major widespread voting innovation in the voting process itself introduced in the last hundred years. However, voting and the secret ballot introduce actions and conditions not common in a voter's daily experience. Voters concerned about coercion place confidence in the system not to reveal their preferences. Voters unconcerned about coercion can still be concerned that their votes are processed accurately, and the lack of visibility into whether and how the system processed their vote can diminish confidence on its own.

ElectionGuard and e2e-v bolster and complement the secret ballot. 

Having a confirmation code is analogous to having a shipping receipt from Fedex and UPS. The emotional weight of surety of delivery is significant for packages, and so, too, potentially, with voting. It is reasonable to assume a similar emotional benefit accrues, especially for voters voting remotely or using systems that use centralized tabulation. Voters that take the extra step of challenging a ballot can test the system entrusted with keeping it secret, and in doing so help ensure that it does.

### Other potential benefits

ElectionGuard and e2e-v enable election systems to exhibit software independence, a capability recognized by the Election Assistance Commission for systems to comply with the VVSG 2.0 election system guidelines. End-to-end verifiability and paper-based systems are the only two mechanisms recognized.  
Paper-based systems do not serve many voting populations well, especially for the core criteria of voting independently. Electronic voting systems can serve multiple populations simultaneously, especially when combining capabilities such as visual, language, and audio support simultaneously, or integrating with voters' adaptive controllers. ElectionGuard could provide the software independence component of innovative accessible electronic systems while also working with paper-based systems to achieve the real systemic benefit of transparency and independent verifiability.

Voters tend to trust their local elections and election officials more than those of other methods and states. Were ElectionGuard to become truly widespread while maintaining or building confidence along the way, it offers the potential to become a connective link across elections that increases confidence in the system overall.

### Only active e2e-v project

ElectionGuard is currently the only e2e-v offering being developed for use in US elections for e2e-v purposes  .  More ElectionGuard elections should be supported and encouraged to build on the foundation established by these three: additional voting methods and vendors, more complex elections supporting multiple methods simultaneously, and larger elections generally.
