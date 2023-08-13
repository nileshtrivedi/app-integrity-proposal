# Website and App Integrity Certificate Proposal

Browsers and Operating Systems are [busy](https://httptoolkit.com/blog/apple-private-access-tokens-attestation/) [shipping](https://github.com/RupertBenWiser/Web-Environment-Integrity) [client environment integrity](https://developer.android.com/google/play/integrity) [APIs](https://azure.microsoft.com/en-in/products/azure-attestation) that proclaim to protect the websites and apps from users (!!), who might want to install adblockers or browser extensions or jailbreak their OS. 

We believe that it is much more critical to implement the _reverse_: Protocols and APIs that help the _users_ determine whether a website or app is comprehensively safe _for users_, both individually and collectively.

This is motivated by [RFC 8890 - The Internet is for End Users](https://datatracker.ietf.org/doc/html/rfc8890) and is written as the inverse of the [Web Environment Integrity API](https://github.com/RupertBenWiser/Web-Environment-Integrity) which tries to justify further restriction of users in the name of user safety or developer profits. Our proposal aims to provide users a way to assess whether a website or app respects users' rights, agency, creativity and inventiveness.

We hope that developers at Google (Android & Chrome/Chromium), Apple (macOS, iOS & Safari/Webkit) and Microsoft (Windows & Edge/Blink) will prioritize the flourishing of good tech ecosystem (encompassing both users and developers) over restricting the users' inventiveness in the name of user safety or developer profits.

## Participate
- [Issue tracker](https://github.com/nileshtrivedi/app-integrity-proposal)
- Spread the word!

## Introduction

~~Users~~ _Developers_ often depend on ~~websites~~ _users_ trusting the ~~client environment they run in~~ _websites and apps they use_. This trust may assume that the ~~client environment~~ _websites and apps_ are honest about certain aspects, ~~keeps user data and intellectual property secure~~ _respect user rights_, and are transparent about whether or not ~~a human is using it~~ they encourage users' agency and inventiveness. This trust is the backbone of the open internet, critical for the ~~safety of user data~~ flourishing of people and for the ~~sustainability of the website’s business~~ progress of society.

Some examples of scenarios where ~~users~~ _developers_ depend on ~~client trust~~ _users trusting websites and apps_ include:

- ~~Users like visiting websites that are expensive to create and maintain, but they often want or need to do it without paying directly. These websites fund themselves with ads, but the advertisers can only afford to pay for humans to see the ads, rather than robots. This creates a need for human users to prove to websites that they're human, sometimes through tasks like challenges or logins.~~

- Websites and apps like being used by people, but they often want or need to do it ethically, without restricting users' inventiveness for customizing their digital environments. These users empower themselves with extensions/plugins and jailbreaks, but the developers of such tools only desire to collaborate with other websites and apps that respect user rights. This creates a need for websites and apps to prove to users that they are good tech participants, sometimes through documents like terms of use or privacy policies or copyright licenses or certificates of good governance and ethics. However, such documents are not machine-readable or verifiable, making it difficult for both users and developrs of tools to assess websites and apps.

- ~~Users want to know they are interacting with real people on social websites but bad actors often want to promote posts with fake engagement (for example, to promote products, or make a news story seem more important). Websites can only show users what content is popular with real people if websites are able to know the difference between a trusted and untrusted environment.~~

- Developers want to know they are interacting/interoperating with apps and APIs of good tech participants but bad actors often want to diminish user rights and place themselves as centralized gatekeepers or rent-seekers. Users can only combine apps and APIs in new inventive ways if they are able to know the difference between a rights-respecting and rights-violating website/app.

- ~~Users playing a game on a website want to know whether other players are using software that enforces the game's rules.~~

- Websites/apps interoperating with other websites/apps in order to empower users' creativity want to know whether those other websites/apps are respecting users' preferences about sovereignty, privacy, openness, interoperability, decentralization and ethics.

- ~~Users sometimes get tricked into installing malicious software that imitates software like their banking apps, to steal from those users. The bank's internet interface could protect those users if it could establish that the requests it's getting actually come from the bank's or other trustworthy software.~~

- Developers sometimes get tricked into integrating with malicious APIs or platforms that diminish user rights. Developers' digital environments could protect them if they could establish that the requests they're getting actually come from rights-respecting websites and apps.

We would like to explore whether a mechanism - App Integrity Certificates - could help address these use cases.

## Implementation

SSL Certificates are already widely adopted in platforms like web browsers and operating systems. Unfortunately, their scope has been defined extremely narrowly - to verify only the _identity_ of websites/apps, not their respect for user rights. The current issuers of SSL certificates do not evaluate websites/apps for privacy, interoperability or other ethical concerns. The certificate metadata has no provisions for including terms of service, privacy policy, interoperability policy, copyright license, policy for use of user data in machine learning systems etc.

We propose implementing machine-readable versions of these policies and corresponding audit reports at a standard, well-known URL: `https://example.com/.well-known/user-policies.json`

The contents should look like:

```
{
  "terms_of_use": "",
  "privacy_policy_url": "",
  "copyright_license": "",
  "interoperability_policy": "",
  "machine_learning_policy": "",
  "security_audit_policy": "",
  "open_source_policy": "",
  "decentralization_policy": ""
}
```

The values of each property in the above JSON will be URLs pointing to latest digitally-signed machine-readable audit reports of each of the aspect by a third-party auditor. This is a proposal and a work-in-progress. Please [share your feedback]((https://github.com/nileshtrivedi/app-integrity-proposal))

SSL certificate issuing authorities like Verisign or LetsEncrypt, when adopting our standard, should extract and evaluate these machine-readable user policies and the corresponding audit reports as a mandatory part of their certification workflow.

The end-users will then be able to exercise their choice by curating Certificate Authorities from their trust store. This is already available in web browsers and operating systems.

![certificate manager](https://docs.vmware.com/en/VMware-Adapter-for-SAP-Landscape-Management/2.1.0/Installation-and-Administration-Guide-for-VLA-Administrators/images/GUID-1FC4EFA4-4DAD-44B1-9E09-56783583589F-low.png)

When visiting a new website or a new app, the browser or app store should clearly highlight the status and any conflict with users' stored preferences on each of these aspect.

We recommend that browsers and operating yank those certificate authorities from their default trusted CA set which do not perform any checks beyond identity verification. These policies are critical important for the health of digital ecosystem and can no longer be ignored.

A new app or website can begin and gradually implement good policies. Therefore, an app could evolve over time from using a self-signed certificate (for developer's own personal use) to a top-quality CA-signed certificate when it needs to reach wide adoption. Developers need to perform more third-party audits only when they want to reach more users.

#### Alternative implementation approaches considered:

- [Google Safebrowsing](https://safebrowsing.google.com/): This is used by some, not all, web browsers to protect users against malware or phishing sites. We found it unsuitable because of its lack of ubiquitousness and Google's conflict of interest against users.
- Online Certificate Status Protocol: OCSP is meant for _status_ of certificates to handle revocation based on identity. We will continue to use OCSP for identity-based revocation only.