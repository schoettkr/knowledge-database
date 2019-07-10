+++
title = "Security of Distributed Software - Lecture 10"
author = ["eo shiru"]
date = 2019-07-02
lastmod = 2019-07-10T15:53:38+02:00
tags = ["uni", "security-ds"]
draft = false
+++

**Part III: Trustworthy Software Engineering**<br />


## Identity in the Light of Privacy, Security and Trust (Chapter 2) {#identity-in-the-light-of-privacy-security-and-trust--chapter-2}

-   7 Laws of Identity define requirements of dealing with identities
    -   first focus on conceptual/basic understanding
-   identity in global context has to comply with different levels
    -   layered approach of identity management:
    -   {{< figure src="/knowledge-database/images/identity-layers.png" >}}


### Identity - Security - Privacy {#identity-security-privacy}

-   identity (in a digital setting) is often "only" closely linked to security, identity is more!
    -   security = protect data from unauthorized access, removal, tampering
    -   privacy = protect attributes, preferences, etc which are associated with identity, against unnecessary use by subject
    -   identity is in relation to others &rarr; attributes realize trust relation ships


### Identity & Trust {#identity-and-trust}

-   Trust (wiki) = In a social context, trust has several connotations. Definitions of trust typically refer to a situation characterised by the following aspects: One party (trustor) is willing to rely on the actions of another party (trustee); the situation is directed to the future. In addition, the trustor (voluntarily or forcedly) abandons control over the actions performed by the trustee. As a consequence, the trustor is uncertain about the outcome of the other's actions; he can only develop and evaluate expectations. The uncertainty involves the risk of failure or harm to the trustor if the trustee will not behave as desired
-   **Trust = Conviction and belief in the sincerity, honesty and good intentions of another party with respect to a risk-prone action.**


#### Trust examples {#trust-examples}

-   shopping with credit card, which trust relationships & risks exist?
    -   identity and Credit Card company
    -   identity and service
    -   identity/service and card register
    -   identity/service and money
-   &rarr; trust is always associated with risk
-   trust is something one connects to a person
    -   one cannot enforce trust by another person


#### Trust properties {#trust-properties}

-   trust is rarely transitive
    -   example: I trust Sara's taste in music, she, in turn, trusts Peter's - therefore I would possibly trust Peter in selecting music for my Birthday Party
-   trust cannot be shared
    -   example: A trusts B and C, which does not imply that B and C trust each other
-   trust is not symmetric
    -   example: if I trust you, you don't necessarily trust me in return
-   trustworthiness cannot be self-declared ("trust me!")
-   trust is a value closely related to evidence
    -   buying a brand computer which is more expensive because I trust the brand
    -   Computer allows access upon login, since the provided evidence (login/password) serve as proof
-   trust is hard to quantify
    -   I trust Sara more than Peter - what does that mean?
    -   in business context trust can be evaluated against risks (given obvious risk levels)
    -   otherwise a contract is used as a basis: analysis is required, risks are evaluated and thereby cotractual relationships are defined; leads to Service Level Agreements (SLA) providers and users
-   Trust by reputation
    -   trust in a person can develop from other people's statements about him/her (Communities of Trust)
    -   examples:
        -   all security experts advise caution when traveling in the following countries
        -   ebay: one buys a product from a handler he doesnt know, but which has good reviews (high reputation)


### Identity & Privacy {#identity-and-privacy}

-   privacy is an important and complicated topic (tightly coupled with data protection)
-   identity and privacy are closely related
    -   what does privacy mean for a person?
        -   generally: private data shouldn't become public
        -   however, often: private data disclosure is ok if it yields considerable benefits
    -   privcacy must be observed in context
        -   eg discount systems: provide us your address and date of birth and we'll give you a 15% discount
-   privacy is partially legally regulated
    -   eg Federal Data Protection Act, European Data Protection Directive, Patriot Act
-   conclusion in legal context: own applications systems must take identity and privacy into account (see Laws of Identity)
    -   embed the concepts of identity and privacy in design
    -   use of identity and privacy-relevant information must be comprehensible, verifiable and reportable at any point in time
    -   identity management system or an identity metasystem must be able to answer questions on identity privacy terms
    -   legal requirements forcte system operators to testify on privacy policy
        -   eg webshop sends cookies to customers, what should the privacy policy say? (eg We use cookies)
-   privacy principle - respect privacy
    -   accountability
    -   identifying purposes
    -   requirement of affected person's consent
    -   minimal privacy data collection (time limit)
    -   limitations of use
    -   data collection accuracy
    -   protection
    -   access to personal data (to the owner)
    -   comprehensible regulations


## Identity Management Systems (Chapter 3) {#identity-management-systems--chapter-3}

-   what is needed for identity implemenation?
    -   some kind of **identity metasystem** &rarr; contains 3 certain roles (can be more)
    -   **identity provider**
        -   person or an organization, which creates digital identities, either for themselves or on behalf, eg online shop could create identities for customers, authorities provide identities for their employees
    -   **relying party** (human/legal person)
        -   person or organization, which requires digital identity before allowing entry/acess
        -   example: users willing to revoke a contract - the relying party defines which claims are required in order to execute cancellation, as well as which formats and credentials are accepted
    -   **digital subject**
        -   individual or entity for which claims are made

Definition **Identity Management**: "Identity management is the set of processes, tools and social contracts surrounding the creation, maintenance and termination of a digital Identity for people or, more generally, for systems and services to enable secure access to an expanding set of systems and applications."

**Identity Management Lifecycle**<br />
![](/knowledge-database/images/identity-management-lifecycle.png)

There are three identity management levels:

-   personal identity management
-   organization-related identity management
-   federated identity mangament


#### Personal Identity Management {#personal-identity-management}

Entity-perspective

-   Management of different identities (different accounts for different systems)
-   Management and control of which information is provided to a service (z.B. Email, phone number etc.)
-   eg MS Passport, MS Cardspace


#### Organization-related Identity Management {#organization-related-identity-management}

Organizational perspective

-   Management of identities of an organization
-   Different services of an organization are provided with and updated by identity information.
-   Traceability of data flows and data accesses
-   Management of privileges and roles within the organization
-   Definition of organization‘s policies as to the entities i.e. which data can be accessed
-   eg SUN Identity Management Suite (SUN Identity Manager), Microsoft Identity Integration Server, IBM Tivoli Identity Manager


#### Federated identity Management {#federated-identity-management}

What is meant by “Federation“?

-   “Federation is an association of independent organizational units, which have

a trust relationship.“

-   Among the latest developements in the field of IdM.
-   Is driven both by the state and industry
    -   Common and simplified resource access
    -   Complex problems/business processes and a high level of specialization require cooperation.
    -   Harmonization of business pocesses
    -   Cost savings with respect to administration and resource use
-   Frequently used technologies
    -   SAML (Security Assertion Markup Language)
    -   XML (Schema, Encryption, Signature etc.)
    -   Web Service interfaces
-   Federation perspective
    -   association of organizational units, organizations or even nations
    -   Shared use of resources and services of Federation partners
    -   Cross-organizational business processes within the Federation
    -   Modeling and definition of trust relationships
    -   Federative services are then made available according to the defined trust relationships providing ease of access to resources/data (i.e. Single Sign-on)
-   Example projects/products/approaches: Liberty Alliance Projekt (SAML 2.0) , WS-Federation specification , SUN Identity Management Suite (SUN Federation/Access Manager) , Ping-ID, PingFederate , Shibboleth , FOAF+SSL


### Anticipitated Benefits of IDMS {#anticipitated-benefits-of-idms}

Reduced management overhead

-   Better optimization/automatization of business processes
-   Reduced time required for providing a new employee with access rights to resources
-   Reduced risk of a former employee accessing resources
-   Policy and legal requirement compliance support (privacy)
-   Data consistency (data matching, modification checks, …)
-   Standard interfaces (APIs, standards …) to data/services/resources


### Components of IDM systems {#components-of-idm-systems}

"he focus of identity management is on user provisioning — the creation, maintenance, and termination of user accounts and management of credentials in support of authentication and access control." (HurwitzGroup, 2001)<br />
![](/knowledge-database/images/idm-components.png)


#### Basic Components {#basic-components}

-   **Repository**
    -   Repository represents the core component for many identity management systems
    -   It is a **logical data storage** (i.e. database, directory service), in which identity information, guidelines and other organization information can be stored
-   **Propagation**
    -   Depending on the system in use, an identity entry could need to be transferred from the current reposiroty to another one
-   **Authentication Provider/Identity Provider**
    -   is responsible for primary identity authentication
    -   often issues a credential, which can be used for further authentication and authorization (z.B. SAML Token)
    -   provides multiple interfaces (z.B. LDAP, Kerberos), by means of which service can perform authentication
-   **Policy Control**
    -   policy control governs rules of information usage, disclosure and logging
    -   authorization policies determine which identity can access and manipualte which information
    -   policy control monitors the defined guidelines, creates events to be audited and signalled of according to certain rules (for example, security warnings)
-   **Auditing, Monitoring**
    -   auditing provides necessary mechanisms for information detection and storage
    -   that information normally contains access protocols and data operations (specifically in the repository)
    -   if form a basis for tracking whether the policies are being adhered to and is used for subsequent security checks


#### Lifecycle Components {#lifecycle-components}

-   **(De-)Provisioning and propagation/synchronization**
    -   applies automation of all the procedures and tools to manage the identity lifecycle
    -   this Lifecycle is split into initial provosioning, synchronization and de-provisioning phases
    -   in the initial provisioning phase the according service is supplied with the necessary identity information such that the new identity can use the service (provisioning process)
    -   in the synchronzsation phase identity information is updated and compared between services (synchronization and propagation process)
    -   in the de-provisioning phase all the identity information is removed (de-provisioning process)
-   **History, Longevity**
    -   History and longevity tools create historical records, by means of which one can examine evolution of an identity overtime (i.e. creation, activation, locking, new status, removal)
    -   these components provide means for such activites as investigating whether or not a certain identity exists in the system and which changes it underwent


#### Usage Components {#usage-components}

-   **Single Sign-on**
    -   Single Sign-on enables an identity to perform its initial authentication and access numerous services and data without further re-authentication.
    -   Initial authentication is typically performed by an associated Identity Provider, which issues a credential.
    -   That Credential is then used to authenticate to other systems.
-   **Personalization**
    -   Personalization and preference management tools provide the identity an ability to set up individual settings for applications/services bound to that identity.
-   **Access Management**
    -   Similar to policy control
    -   Identity can define policies as to which identity can access/modify which information.


## WAM (WebComposotion Architecture Model) {#wam--webcomposotion-architecture-model}

WAM is a modeling language for designing distributed, organization-spanning web applications and (who would've thought) was developed by the lecturer/prof and is absolutely industry irrelevant for that matter. But hey that's just how things are at Chemnitz University. By the way are you looking for irrelevant + outdated material? You'd love to study some unstructured slides enhanced by Internet Explorer 6 screenshots? You hate effective use of slides and think that using results of pedagogical research to help students learn better is for nonbelievers? MAN I HAVE SOMETHING FOR YOU! Go to <https://www.tu-chemnitz.de> and get yourself the new and updated "Bullshit University Education"-StarterPack right now!
