# Multi Area OSPF

OSPF is an open standard protocol used by routers to determine the *shortest path first* to a given network.

## Link State Advertisements

*LSAs seem to be the backbone to mastering OSPF. Spend the time to really understand these.*

Link state update packet sends LSAs.

OSPF routers send and receive *link state advertisements* (LSAs). LSAs are used to update the *link state database* in each router. This database is used to build a routing table.

| LSA Type | LSA Name | Link State ID |
|----------|----------|---------------|
| 1 | Router LSA | Router ID |
| 2 | Network LSA | Interface for IP of DR |
| 3 | Summary LSA | Network Address |
| 4 | Summary ASBR LSA | Router ID of ASBR |
| 5 | Autonomous system external LSA |  Network Address |
| 6 | Multicast OSPF LSA |
| 7 | Not-so-stubby area LSA |
| 8 | External attribute LSA |

### Type 1

**Name:** Router LSA

**ID:** Router ID

**Purpose:** Build SPF tree. Teaches the router topology. Keeps every routing table in the area identical.

Type 1 LSA contains *all* the advertised interfaces of the router inside of it.

Not aloud to go between areas.

Router ID is not changed when forwarded.

## OSPF Areas

### Backbone area

Area 0, contains all other areas.

### Normal Area

Contains all internal and external routing information.

### Stub and Totally Stubby Areas

Area can be stubby or totally stubby *if*:

* There is one or more ABR.
* All routers that are members of the stub are configured as stub routers.
* There is no ASBR in the area.
* The area is not Area 0.
* No virtual links go through the area.

### Stub Area

Contains all internal and area routing information, *but not external routing information.*

Reduce amount of links in the database by filtering Type 5 LSAs. External LSAs are blocked.

To get connection to the outside world, OSPF sends a default route into the stub network.

**Every router in the area must be configured for stub with the `area # stub` command.**

### Totally Stubby Area

Contains area routing information *only*; Cisco proprietary.

Filters Type 3 and Type 5.

**To configure, only the ASBR must be configured with the `area # stub no-summary`**

## Not-So-Stubby-Area (NSSA)

Contains area and external routing information.

All routers must be configured with the `area # nssa` command.

---

## Glossary

| Term | Definition |
|------|-------------|
| ABR | Area Border Router |
| LSA | Link State Advertisement |

## Commands

| Command | Description |
|---------|-------------|
| `area # stub` | Designates area as stub |
| `area # stub no-summary` | Designates area as a totally stubby area |
| `area # nssa` | Designates router in NSSA |
| `show ip ospf database` | Shows all LSA headers |
| `show ip ospf database router` | Shows all LSA bodies |