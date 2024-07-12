![penetration-testing-process.png](https://github.com/pjobsina/PENETRATION-TESTER/blob/b57e19f7a01c956a5b96ddc591a8789d4583641b/Images/penetration-testing-process.png)
# Pre-Engagement
Pre-engagement is the stage of preparation for the actual penetration test. During this stage, many questions are asked, and some contractual agreements are made. The client informs us about what they want to be tested, and we explain in detail how to make the test as efficient as possible.
The entire pre-engagement process consists of three essential components:
1. Scoping questionnaire
2. Pre-engagement meeting
3. Kick-off meeting

| Document                                                   | Timing for Creation                   |
|------------------------------------------------------------|---------------------------------------|
| 1. Non-Disclosure Agreement (NDA)                          | After Initial Contact                 |
| 2. Scoping Questionnaire                                   | Before the Pre-Engagement Meeting     |
| 3. Scoping Document                                        | During the Pre-Engagement Meeting     |
| 4. Penetration Testing Proposal (Contract/Scope of Work)   | During the Pre-engagement Meeting     |
| 5. Rules of Engagement (RoE)                               | Before the Kick-Off Meeting           |
| 6. Contractors Agreement (Physical Assessments)            | Before the Kick-Off Meeting           |
| 7. Reports                                                 | During and after the conducted Penetration Test |
# Information Gathering
Information gathering is an essential part of any security assessment. This is the phase in which we gather all available information about the company, its employees and infrastructure, and how they are organized. Information gathering is the most frequent and vital phase throughout the penetration testing process, to which we will return again and again.
## Open-Source Intelligence
OSINT is a process for finding publicly available information on a target company or individuals that allows the identification of events (i.e., public and private meetings), external and internal dependencies, and connections. OSINT uses public (Open-Source) information from freely available sources to obtain the desired results.
## Infrastructure Enumeration
During the infrastructure enumeration, we try to overview the company's position on the internet and intranet. For this, we use OSINT and the first active scans. We use services such as DNS to create a map of the client's servers and hosts and develop an understanding of how their `infrastructure` is structured. This includes name servers, mail servers, web servers, cloud instances, and more. We make an accurate list of hosts and their IP addresses and compare them to our scope to see if they are included and listed.
## Service Enumeration
In service enumeration, we identify services that allow us to interact with the host or server over the network (or locally, from an internal perspective). Therefore, it is crucial to find out about the service, what `version` it is, what `information` it provides us, and the `reason` it can be used. Once we understand the background of what this service has been provisioned for, some logical conclusions can be drawn to provide us with several options.
## Host Enumeration
Once we have a detailed list of the customer's infrastructure, we examine every single host listed in the scoping document. We try to identify which `operating system` is running on the host or server, which `services` it uses, which `versions` of the services, and much more. Again, apart from the active scans, we can also use various OSINT methods to tell us how this host or server may be configured.
## Pillaging
After hitting the `Post-Exploitation` stage, pillaging is performed to collect sensitive information locally on the already exploited host, such as employee names, customer data, and much more. However, this information gathering only occurs after exploiting the target host and gaining access to it.
# Vulnerability Assessment
During the `vulnerability assessment` phase, we examine and analyze the information gathered during the information gathering phase. The vulnerability assessment phase is an analytical process based on the findings.

| Analysis Type | Description                                                                                                                                                                                                                                                                                                                                                             |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Descriptive   | Descriptive analysis is essential in any data analysis. On the one hand, it describes a data set based on individual characteristics. It helps to detect possible errors in data collection or outliers in the data set.                                                                                                                                                |
| Diagnostic    | Diagnostic analysis clarifies conditions' causes, effects, and interactions. Doing so provides insights that are obtained through correlations and interpretation. We must take a backward-looking view, similar to descriptive analysis, with the subtle difference that we try to find reasons for events and developments.                                           |
| Predictive    | By evaluating historical and current data, predictive analysis creates a predictive model for future probabilities. Based on the results of descriptive and diagnostic analyses, this method of data analysis makes it possible to identify trends, detect deviations from expected values at an early stage, and predict future occurrences as accurately as possible. |
| Prescriptive  | Prescriptive analytics aims to narrow down what actions to take to eliminate or prevent a future problem or trigger a specific activity or process.                                                                                                                                                                                                                     |
## Vulnerability Research and Analysis
In `Vulnerability Research`, we look for known vulnerabilities, exploits, and security holes that have already been discovered and reported. Therefore, if we have identified a version of a service or application through information gathering and found a [Common Vulnerabilities and Exposures (CVE)](https://www.cve.org/ResourcesSupport/FAQs), it is very likely that this vulnerability is still present.
## Assessment of Possible Attack Vectors
We analyze historical information and combine it with the current information that we have been able to find out. Whether we have received specific evasion level requirements from our client, we test the services and applications found `locally` or `on the target system`.
## The Return
Suppose we are unable to detect or identify potential vulnerabilities from our analysis. In that case, we will return to the `Information Gathering` stage and look for more in-depth information than we have gathered so far. It is important to note that these two stages (`Information Gathering` and `Vulnerability Assessment`) often overlap, resulting in regular back and forth movement between them.

Speed is more important than quality. In a CTF, the goal is to get on the target machine and `capture the flags` with the highest privileges as fast as possible instead of exposing all potential weaknesses in the system.

**`A (real) Penetration Test is not a CTF.`**
# Exploitation
During the `Exploitation` stage, we look for ways that these weaknesses can be adapted to our use case to obtain the desired role (i.e., a foothold, escalated privileges, etc.). If we want to get a reverse shell, we need to modify the PoC to execute the code, so the target system connects back to us over (ideally) an encrypted connection to an IP address we specify.
## Prioritization of Possible Attacks
Once we have found one or two vulnerabilities during the `Vulnerability Assessment` stage that we can apply to our target network/system, we can prioritize those attacks. Which of those attacks we prioritize higher than the others depends on the following factors:
- Probability of Success
- Complexity
- Probability of Damage
## Preparation for the Attack
Sometimes we will run into a situation where we can't find high-quality, known working PoC exploit code. Therefore, it may be necessary to reconstruct the exploit locally on a VM representing our target host to figure out precisely what needs to be adapted and changed.
# Post-Exploitation
The `Post-Exploitation` stage aims to obtain sensitive and security-relevant information from a local perspective and business-relevant information that, in most cases, requires higher privileges than a standard user.
This stage includes the following components:
- Evasive Testing
- Information Gathering
- Pillaging
- Vulnerability Assessment
- Privilege Escalation
- Persistence
- Data Exfiltration
## Evasive Testing
Evasive testing is divided into three different categories:
**`Evasive`**
**`Hybrid Evasive`**
**`Non-Evasive`**
# Lateral Movement
The goal here is that we test what an attacker could do within the entire network. After all, the main goal is not only to successfully exploit a publicly available system but also to get sensitive data or find all ways that an attacker could render the network unusable.
We can move to this stage from the `Exploitation` and the `Post-Exploitation` stage. Sometimes we may not find a direct way to escalate our privileges on the target system itself, but we have ways to move around the network. This is where `Lateral Movement` comes into play.
# Proof-of-Concept
`Proof of Concept` (`PoC`) or `Proof of Principle` is a project management term. In project management, it serves as proof that a project is feasible in principle. The criteria for this can lie in technical or business factors. Therefore, it is the basis for further work, in our case, the necessary steps to secure the corporate network by confirming the discovered vulnerabilities. In other words, it serves as a decision-making basis for the further course of action. At the same time, it enables risks to be identified and minimized.
# Post-Engagement
## Cleanup
Once testing is complete, we should perform any necessary cleanup, such as deleting tools/scripts uploaded to target systems, reverting any (minor) configuration changes we may have made, etc. We should have detailed notes of all of our activities, making any cleanup activities easy and efficient. If we cannot access a system where an artifact needs to be deleted, or another change reverted, we should alert the client and list these issues in the report appendices.
## Documentation and Reporting
We must make sure to have adequate documentation for all findings that we plan to include in our report. This includes command output, screenshots, a listing of affected hosts, and anything else specific to the client environment or finding. We should also make sure that we have retrieved all scan and log output if the client hosted a VM in their infrastructure for an internal penetration test and any other data that may be included as part of the report or as supplementary documentation. We should not keep any Personal Identifiable Information (PII), potentially incriminating info, or other sensitive data we came across throughout testing.
## Post-Remediation Testing

| # | Finding Severity | Finding Title                           | Status          |
|---|------------------|-----------------------------------------|-----------------|
| 1 | High             | SQL Injection                           | Remediated      |
| 2 | High             | Broken Authentication                   | Remediated      |
| 3 | High             | Unrestricted File Upload                | Remediated      |
| 4 | High             | Inadequate Web and Egress Filtering     | Not Remediated  |
| 5 | Medium           | SMB Signing Not Enabled                 | Not Remediated  |
| 6 | Low              | Directory Listing Enabled               | Not Remediated  |
