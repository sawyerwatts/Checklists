# Security

[OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) are a great resource
here, as well as OWASP's top 10 lists. Additionally, specific tools and
languages have their own security weaknesses as well that will need to be
considered.

As with [app.md](./app.md), more specific information may be found in the
various subdirs of [app/](./app/).

## STRIDE

STRIDE threat moedlling is a thought experiment to identify potential security
issues.

Depending on the specific subsection, the relevance or applicability of the
attack vector is subject to business relevance.

This section draws quotes or paraphrases a lot from *Code that fits in your
head* by Mark Seemann. This book uses a restuarant application as its example.

### Spoofing

Attackers try to pose as someone they're not in order to gain unauthorized
access to the system.

An example of spoofing would a reservation system that allows users to enter
whatever name they like, and then an attacker using someone else's name, like
Keanu Reeves. So long as there isn't specific logic or treatment for certain
people, then this vector doesn't require protection.

### Tampering

Attackers try to tamper with system data, such as via SQL injection.

### Repudiation

Attackers deny that they've performed an action, such as receiving an item
they've already paid for.

An example of repudiation would be someone not showing up to a reservation.
There could be mitigations, but for restuarants, they're generally not
necessary, and can be actively undesirable as they needlessly increase
difficulty of reserveration.

### Information Disclosure

Attackers can read data they shouldn't be able to, such as man-in-the-middle
attacks and SQL injection.

So long as https is used, man-in-the-middle attacks aren't really relevant.
Instead, the most likely way this would occur would be an overly permissive
API.

### Denial of Service

Attackers attempt to make the system unavailable to its regular users. This
includes a distributed denial of service (DDoS) attack, particularly if it
targets a login service; this could have monetary impacts as well if the system
is built to scale up as needed.

This can be done via a buffer overflow attack (at least in less safe
languages).

Systems may need to be written such that they can handle spikes in volume, even
real volume, such as utilizing queues to defer DB mutations like CQRS.
Additionally, consider how client/user inputs can determine the amount of used
memory and cycles.

### Elevation of Privilege

Attackers attempt to gain more permissions than they have.

The best protection here is to have accounts with as little perms as needed.

