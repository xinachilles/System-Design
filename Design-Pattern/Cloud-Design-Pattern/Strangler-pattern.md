# Strangler pattern

Created: 2018-12-13 01:00:00 -0600

Modified: 2019-01-23 00:03:38 -0600

---

Completely replacing a complex system can be a huge undertaking. Often, you will need a gradual migration to a new system, while keeping the old system to handle features that haven't been migrated yet. However, running two separate versions of an application means that clients have to know where particular features are located. Every time a feature or service is migrated, clients need to be updated to point to the new location.



Solution

Incrementally replace specific pieces of functionality with new applications and services.

Create a façade that intercepts requests going to the backend legacy system.

The façade routes these requests either to the legacy application or the new services.

Existing features can be migrated to the new system gradually, and consumers can continue using the same interface, unaware that any migration has taken place.Mourinho
