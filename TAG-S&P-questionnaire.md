# Self-Review Questionnaire: Security and Privacy

Questions from w3.org/TR/security-privacy-questionnaire

## 2. Questions to Consider

### 2.1. What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

If unpartitioned third-party cookies are still in use, this feature does not expose any additional information that is not already available if sites were already using an unpartitioned cross-site cookie.
If unpartitioned third-party cookies are blocked, this provides sites in third-party contexts the ability to use HTTP cookies that are scoped to a single top-level site.

### 2.2. Is this specification exposing the minimum amount of information necessary to power the feature?

Yes. This proposal gives the ability for third-party contexts the ability to use a familiar HTTP state primitive for use cases restricted to a users' activity in only the current top-level site.

### 2.3. How does this specification deal with personal information or personally-identifiable information or information derived thereof?

This proposal requires that cookies which use the Partitioned attribute also have the __Host- prefix.
This prefix requires cookies be hostname scoped and can only be used in [secure contexts](https://www.w3.org/TR/secure-contexts/).
The latter requirement protects these cookies from sending PII in plaintext between clients and servers.

### 2.4. How does this specification deal with sensitive information?

See 2.3.

### 2.5. Does this specification introduce new state for an origin that persists across browsing sessions?

This proposal extends an existing state mechanism that persists across browsing sessions.
It imposes different types of size limitations on these types of cookies in order to prevent them from being abused as a cross-site tracking vector.
For more information for this type of attack, see this section of the explainer.

### 2.6. What information from the underlying platform, e.g. configuration data, is exposed by this specification to an origin?

This feature should not reveal any additional information about the user's underlying platform.

### 2.7. Does this specification allow an origin access to sensors on a user’s device

No.

### 2.8. What data does thisi UI specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.

Partitioned cookies do not expose any additional information to an origin that unpartitioned cross-site cookies do not already.
Instead, that same data is now scoped to the top-level site instead of being available across different top-level contexts.

### 2.9. Does this specification enable new script execution/loading mechanisms?

No.

### 2.10. Does this specification allow an origin to access other devices?

No.

### 2.11. Does this specification allow an origin some measure of control over a user agent’s native UI?

No.

### 2.12. What temporary identifiers might this specification create or expose to the web?

This proposal is a change to an existing identifier, cookies.
The purpose of this design is to give third-party contexts the ability to use these identifiers for activity scoped within a single top-level site that is not a vector for cross-site tracking.

### 2.13. How does this specification distinguish between behavior in first-party and third-party contexts?

The Partitioned attribute is designed for use in third-party contexts, but that does not mean that they cannot be used in first-party contexts.
When used in first-party contexts, their semantics are identical to that of first-party cookies.

### 2.14. How does this specification work in the context of a user agent’s Private \ Browsing or "incognito" mode?

Like all other cookies in private browsers, partitioned cookies should be cleared when a private browsing session ends.
Some browser private settings also block the use of cross-site cookies.
User agents may wish to implement exceptions for partitioned cookies for these cross-site cookie restrictions for private browsing mode.

### 2.15. Does this specification have a "Security Considerations" and "Privacy Considerations" section?

[Yes](https://github.com/WICG/CHIPS/blob/main/README.md#security-and-privacy-considerations).

### 2.16. Does this specification allow downgrading default security characteristics?

No.

## 3. Threat Models

### 3.1. Passive Network Attackers

The Partitioned attribute can only be used for cookies being sent over secure connections, so the cookie strings are not visible to passive network attackers.

### 3.2. Active Network Attackers

See 3.1.

### 3.3. Same-Origin Policy Violations

None.

### 3.4. Third-Party Tracking

Partitioning cross-site cookies by top-level site is meant to reduce (or eliminate) third parties' ability to use cookies as a cross-site identifier.
The goal of this proposal is for cross-site contexts to be able to observe a user's activity only within a single top-level site.

### 3.5. Legitimate Misuse

A malicious third party may wish to use partitioned cookies to store a global user identifier that persists across multiple top-level domains.
This identifier could be repeatedly derived on each top-level site a user visits using some side channel or fingerprinting techniques.
Afterwards the third-party can store this identifier in a partitioned cookie so that when the user returns to each top-level site with the third-party embed, they no longer need to use the side channel to derive the cross-site identifier again.

Though partitioned cookies could introduce a new place for third parties to store a derived cross-site identifier, a third-party script running in a top-level context could store still store a cross-site identifier in the top-level site's cookie jar.
