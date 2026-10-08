---
name: code-clarity
description: >
  Audit a codebase for developer clarity, navigability, ownership, traceability,
  and change predictability. Answers whether a developer unfamiliar with the
  project can locate a concept, understand its behavior, identify its
  authoritative implementation, and make a bounded change without guessing
  the architecture. LSP-first, evidence-driven, read-only for source code, and
  explicitly resistant to over-refactoring.
argument-hint: "[workspace-root | area | concept]"
user-invocable: true
license: MIT
compatibility: Works with or without agent-lsp; use available semantic tools first, otherwise repository searches and compiler references.
metadata:
  optional-integration: agent-lsp (github.com/blackwell-systems/agent-lsp)
  optional-capabilities: documentSymbolProvider referencesProvider callHierarchyProvider implementationProvider workspaceSymbolProvider
---

# code-clarity

Audit a codebase from the perspective of a developer who is unfamiliar with
the project and needs to make a real change safely.

The core question is:

> If a junior developer opened this project today, could they understand where
> a concept lives, how its behavior flows, what owns it, and where to modify it
> without guessing the architecture?

This is **not** a Clean Code audit.

This skill does not optimize for elegance, abstraction count, file size,
minimum LOC, maximum DRY, or stylistic consistency.

It audits the cost of building a correct mental model of the system.

The skill is read-only for source code. It may create or update audit and issue
documents, but it must not modify application source, tests, configuration, or
runtime behavior.

---

## Core principles

### 1. Clarity over elegance

Do not propose a refactor merely because another design is cleaner, more
fashionable, more abstract, or more aesthetically pleasing.

A finding must demonstrate a concrete comprehension or modification cost.

Accept genuine domain complexity.

> Complexity is acceptable. Unexplained, misplaced, duplicated, or
> unpredictable complexity is the problem.

---

### 2. Evidence before findings

Do not create an issue based only on:

- file size
- function length
- dependency count
- number of layers
- naming preference
- stylistic disagreement
- duplicated-looking code
- architectural taste

These may trigger investigation, but they are not findings by themselves.

Before reporting a problem, demonstrate at least one concrete cost such as:

- unexpected ownership
- multiple plausible locations for the same concept
- competing authoritative implementations
- unnecessary semantic indirection
- wide or surprising change surface
- unrelated responsibilities that must be understood together
- hidden side effects
- hidden invariants
- hard-to-locate domain rules
- architecture convention drift
- error behavior that cannot be traced with the happy path

---

### 3. Raw hop count is not clarity cost

Do not treat every abstraction hop as negative.

For each meaningful hop in a trace, classify its semantic purpose when useful:

- boundary
- dispatch
- policy
- mapping
- persistence
- orchestration
- ceremony

A hop is justified when it contributes meaningful ownership, contract, policy,
isolation, orchestration, or transformation.

Example:

    Controller
      → ServiceInterface
      → ServiceFacade
      → MovementWriteService

may be perfectly clear if the layers establish a stable API boundary,
predictable dispatch, and explicit ownership.

Do not recommend collapsing layers merely because a path contains many hops.

---

### 4. First-action sufficiency beats root-cause completeness

The goal is not to discover the final ideal architecture in one pass.

If a bounded refactor is already clearly justified and deeper tracing is
unlikely to change what should be done first, stop.

The audit should identify the next healthy move, not redesign the entire
system.

---

## Audit dimensions

Evaluate the target against these dimensions.

### Findability

Can a developer reasonably predict where a product concept or behavior lives
before performing a repository-wide search?

Look for:

- misleading directory ownership
- multiple competing colocation strategies
- generic dumping-ground directories
- domain concepts scattered across unrelated locations
- substantial product code hidden below infrastructure directories

---

### Ownership clarity

Can the developer identify which module or layer owns a behavior?

Determine whether:

- one place is clearly authoritative
- interfaces and facades clarify ownership
- responsibilities overlap between neighboring layers
- duplicate implementations appear authoritative
- infrastructure accidentally owns product behavior

---

### Traceability

Can behavior be followed from its entry point to its authoritative
implementation without repeatedly losing context?

Typical traces may include:

Frontend:

    route
      → product module/page
      → hook
      → query bridge
      → API client
      → DTO

Backend:

    route/controller
      → contract
      → service/facade
      → specialized service
      → persistence/domain policy

Do not require every trace to reach persistence.

Stop when the implementation relevant to the question is understood.

---

### Semantic indirection

Determine whether abstraction hops add meaning.

Good indirection commonly introduces:

- ownership
- interface boundaries
- policy
- mapping
- orchestration
- external-system isolation

Suspicious indirection merely forwards behavior while obscuring where the real
implementation lives.

---

### Context width

Estimate how many independent concepts a developer must understand to make a
local change.

Investigate when a seemingly bounded change requires understanding unrelated
policies, infrastructure, or domains.

Do not penalize unavoidable domain complexity.

---

### Change locality and predictability

Ask:

> Once the developer understands this feature, can they reasonably predict
> which files or modules must change?

Use references and blast-radius analysis where useful.

A wide change surface is not automatically bad.

Flag it only when the change surface is surprising, accidental, or caused by
unclear ownership.

---

### Concept-to-code mapping

For important product concepts, determine whether terminology maps naturally
to code.

Examples in a typical application might include:

- access policy
- membership
- synchronization
- retry policy
- scheduled job

A developer should be able to move from product vocabulary toward the
authoritative implementation without discovering unrelated synonyms or
multiple competing implementations.

---

### Error-path clarity

Where relevant, inspect whether failure behavior is understandable alongside
normal behavior.

Examples:

- validation
- retries
- idempotency
- cancellation
- synchronization failures
- external API errors
- compensating actions

Do not perform a generic error-handling audit.

Only report error-path problems that materially obscure the behavior being
investigated.

---

## Investigation depth model

Always begin at the shallowest useful depth.

Do **not** automatically progress through all levels.

### Depth 0 — Structural inspection

Use project structure, filenames, symbols, and immediately visible ownership.

Suitable for obvious questions such as:

- product UI living inside routing directories
- duplicate module ownership conventions
- misleading folder names
- obvious dumping grounds

If the problem and bounded first intervention are already demonstrated,
perform the Early Exit Gate.

---

### Depth 1 — Bounded semantic trace

Trace only enough references, definitions, implementations, or callers to
determine local ownership and behavior.

Typical examples:

    component
      → hook
      → query layer

or:

    controller
      → service facade
      → specialized service

Do not cross another architectural boundary unless doing so could change the
first recommended action.

---

### Depth 2 — Cross-boundary trace

Use when the question concerns ownership across architectural boundaries.

Examples:

- Where is an authorization decision actually made?
- Is synchronized state owned by frontend or backend?
- Which layer is authoritative for resource update semantics?
- Does a DTO transformation duplicate backend business logic?

Trace only the boundaries necessary to answer the question.

---

### Depth 3 — Runtime / infrastructure trace

Use only when runtime or deployment topology materially contributes to the
behavior under investigation.

Example where Depth 3 may be justified:

> Who guarantees that a scheduled external-data import executes exactly once?

Possible relevant path:

    application command
      → scheduler
      → runtime job definition
      → deployment scheduling semantics

Example where Depth 3 is not justified:

> Is the external-data importer class difficult to understand?

If a bounded application-level refactor already addresses the clarity problem,
do not investigate Kubernetes, Helm, CI/CD, networking, or runtime topology.

---

## Early Exit Gate

After every meaningful investigation step, evaluate:

1. Is the clarity problem demonstrated with concrete evidence?
2. Is there a bounded first intervention?
3. Did LSP tracing reveal any hidden contract that invalidates that intervention?
4. Would learning more materially change what should be done first?

If:

- the problem is demonstrated,
- the first intervention is bounded,
- no discovered contract invalidates it,
- and deeper investigation is unlikely to materially change the first action,

**STOP INVESTIGATING.**

Record the finding and move to the next area.

The governing rule is:

> Use the shallowest investigation sufficient to justify a concrete first
> improvement.

And:

> First-action sufficiency beats root-cause completeness.

Do not seek the final architecture of the system when the next safe improvement
is already clear.

---

## LSP usage

When agent-lsp is installed, use it as the primary source of structural evidence.
If it is unavailable, use the project's language server, code-navigation tools,
compiler/static analysis, and scoped repository search. Be explicit about the
lower confidence of a text-only trace when semantics are uncertain.

### Workspace initialization

If the agent-lsp integration is available and needed:

    mcp__lsp__detect_lsp_servers(...)
    mcp__lsp__start_lsp(...)

Inspect server capabilities before relying on optional operations:

    mcp__lsp__get_server_capabilities(...)

If a capability is unavailable, degrade gracefully.

Do not treat missing call hierarchy or implementation support as evidence about
the codebase.

---

### Prefer semantic navigation over text search when available

Prefer:

- `go_to_definition`
- `go_to_implementation`
- `find_references`
- `find_callers`
- `inspect_symbol`
- `get_symbol_source`
- `get_editing_context`

over inferring relationships from matching text.

Text search may identify candidates. Prefer semantic evidence for ownership
when tools are available; without LSP, corroborate relationships with imports,
call sites, tests and type information rather than inferring from names alone.

---

### Call hierarchy depth

Call hierarchy is investigative, not exhaustive.

Default:

- incoming callers: 1 level
- outgoing calls: 1 level

Extend one additional level only when it could materially change the first
action.

Never recursively follow callers simply because more callers exist.

Summarize large caller sets instead of enumerating them.

---

### Blast radius

Use `blast_radius` when available to understand impact, not to manufacture severity.
Otherwise, estimate impact from verified call sites and tests, noting limits.

High caller count does not imply poor clarity.

Use it to answer questions such as:

- Is this abstraction architectural infrastructure?
- Would moving it affect many unrelated features?
- Is a supposedly local concept actually shared globally?
- Is the proposed first action safely bounded?

---

## Audit workflow

### Step 0 — Read project guidance first

Before evaluating the code, read the repository's relevant guidance.

Prefer, when present:

- `AGENTS.md`
- architecture docs
- coding guides
- package-boundary docs
- project workflow docs
- local README files

Treat documented architecture as intended architecture.

Do not invent a parallel architecture merely because you prefer another design.

When documentation and code disagree, record the drift as evidence rather than
silently choosing one.

---

### Step 1 — Build a small structural map

Identify:

- major application/package boundaries
- likely product/domain areas
- entry points
- route/controller layers
- API/client layers
- shared infrastructure
- obvious architectural hotspots

Do not fully model the entire repository before starting useful investigation.

The map exists only to support targeted probes.

---

### Step 2 — Select representative task probes

Do not audit every function mechanically.

Prefer realistic developer tasks that test whether the architecture can be
understood.

Generate a small set of representative probes from the target area.

Examples:

> Where would I change how a resource row is displayed?

> Where is scheduled-job state calculated?

> Where is idempotency enforced for resource creation?

> If I add a field to a resource response, which layers must change?

> Where is authorization policy authoritative?

For each probe, record:

- predicted starting location
- actual starting location
- semantic trace
- surprising detours
- competing ownership candidates
- point where authoritative behavior became clear
- whether early exit occurred

Do not turn each probe into an issue.

Probes are evidence for root causes.

---

### Step 3 — Investigate only as deep as needed

Use the Investigation Depth Model.

After every meaningful step, apply the Early Exit Gate.

Do not keep tracing because more information is available.

---

### Step 4 — Cluster symptoms into root causes

Repeated symptoms should normally become one bounded root-cause finding.

Example:

Bad:

    Move ProfileCard to modules
    Move ProfileDialog to modules
    Move ResourceCard to modules
    Move ResourceDialog to modules
    Move ResourceEditSheet to modules

Good:

    Establish product-module ownership for frontend composition currently
    colocated under the route tree.

The finding may list all affected areas as evidence.

Split findings only when:

- fixes are independently actionable
- they have meaningfully different risk profiles
- they belong to different architectural owners
- or combining them would create an unbounded refactor

---

### Step 5 — Write bounded issues

Each issue should define the smallest coherent first intervention.

Do not require the implementation agent to solve adjacent architecture problems
unless they are necessary for the issue to succeed safely.

---

## Finding requirements

A finding must answer all of the following.

### Problem

What specifically makes the code harder to locate, understand, or modify?

### Why this hurts clarity

Why would this slow down or mislead an unfamiliar developer?

### Evidence

Include concrete paths, symbols, reference patterns, or bounded LSP traces.

### First intervention

What is the smallest coherent architectural or refactoring move that improves
the situation?

### Why stop here

State why deeper investigation or broader redesign is not necessary for the
first intervention.

If this cannot be answered confidently, continue investigation or do not create
the finding.

---

## Do not generate findings for

- purely stylistic preferences
- arbitrary LOC limits
- arbitrary function-length limits
- arbitrary dependency-count limits
- abstractions that are semantically useful
- unavoidable domain complexity
- normal framework conventions
- code that is merely unfamiliar
- duplication whose consolidation would increase coupling
- theoretical future problems without present clarity cost
- speculative architecture improvements
- refactors whose only benefit is elegance

---

## Project architecture overlay

When auditing a project, treat its **actual** documented architecture, nearest
`AGENTS.md`, installed packages, and committed code as the sources of intended
boundaries.

Do not invent a parallel architecture or pretend a non-existent package is
already installed.

### Frontend ownership

When the project uses a React router plus feature/module organization, a
common responsibility split is:

    routes/
        Routing concerns: route declarations, loaders, guards, navigation.

    features/ or modules/
        Feature-specific UI, composition and local behavior.

    shared UI package (only when actually present)
        Reusable, feature-agnostic interaction primitives.

    API client and React hooks
        Transport/DTO boundaries and server-state integration.

Routes should remain intentionally boring when that convention is established.

Substantial feature composition should usually not accumulate under route-local
`-components`, `-hooks`, or `-utils` directories when these bypass clear
feature ownership. Prove the comprehension cost before flagging an exception.

Treat repeated violations as one structural pattern, not one issue per file.

Do not create mandatory cathedral structures such as:

    components/
    hooks/
    services/
    types/
    utils/
    models/

inside every feature.

Start flat and add subdirectories only when a real cluster emerges.

A component should not move into a shared UI package simply because multiple
screens reuse it. If it understands business semantics, it generally belongs
to its feature/module.

### Frontend API boundaries

If the project uses tiered API packages, inspect the real responsibilities:

    Core transport layer
        HTTP client config, error parsing, shared API infrastructure.

    Domain client layer
        Typed endpoints and DTO mirror types, without React dependencies.

    React integration layer
        TanStack Query hooks and app/session bridge behavior.

For a small application this may live in a single directory rather than
multiple workspace packages.

Do not propose collapsing meaningful layers merely to reduce raw hop count.
Investigate whether each layer preserves its intended responsibility.

### Backend

For Symfony projects using the optional three-directory convention:

    include/
        Owned DTOs, value objects, contracts, enums and boundary exceptions.

    lib/
        Extractable adapters, parsing and integration helpers.

    src/
        Symfony runtime, service implementations, Doctrine and controllers.

If the actual project uses a different documented architecture, audit *that*
structure rather than enforcing these directories.

Do not interpret facade or interface layers as clarity debt merely because
they add hops.

Investigate whether each layer establishes useful ownership, policy, contract,
mapping, orchestration, or isolation.

Large services are investigation triggers only. Do not recommend splitting
them until evidence shows distinct change reasons, unclear ownership, or
excessive context width.

### Domain complexity

Projects contain legitimate domain behavior such as authorization, validation,
state transitions, scheduled workflows, synchronization and audit trails.

Do not flatten domain behavior merely to make call graphs shorter.

Distinguish between:

- essential domain complexity
- accidental implementation complexity

Only the latter is a clarity problem.

---
## Output

Produce two levels of output.

### 1. `CODE_CLARITY_AUDIT.md`

Use this structure:

    # Code Clarity Audit

    ## Executive assessment

    ## Strong areas

    ## Systemic friction

    ## Architecture observations

    ## Task probes

    ## Prioritized first actions

    ## Areas intentionally not investigated deeper

    ## Confidence and limitations

Explicitly record early exits when useful.

Example:

> Investigation stopped at the frontend query boundary because deeper backend
> tracing would not change the recommended first intervention.

This demonstrates intentional bounded reasoning rather than incomplete work.

---

### 2. Issue files

Create issue files only for actionable root causes.

Follow the repository's existing issue workflow and naming convention.

Before creating issue files, inspect that workflow rather than inventing a new
directory or status convention.

Each issue should contain:

    # <title>

    ## Problem

    ## Why this hurts clarity

    ## Evidence

    ## Relevant paths / LSP trace

    ## Desired outcome

    ## Scope

    ## Constraints

    ## Do not fix

    ## Acceptance criteria

Suggested implementation details are optional.

Do not prescribe implementation mechanics unless evidence strongly supports one
approach.

---

## Prioritization

Prioritize findings using:

1. clarity impact
2. frequency / affected developer tasks
3. breadth of affected concepts
4. confidence in the finding
5. expected cost and risk of the first intervention

Blast radius is context, not an automatic severity score.

Prefer a small set of high-confidence structural improvements over a long list
of minor observations.

There is no target number of findings.

**Zero findings is a valid result.**

---

## Completion criteria

The audit is complete when:

- the target areas have representative task probes
- important ownership boundaries have been tested
- actionable clarity problems are supported by evidence
- repeated symptoms have been clustered
- deeper investigation has stopped where it would not change the first action
- remaining uncertainty is explicitly documented

Do not continue exploring merely to make the audit appear comprehensive.

A successful audit reduces uncertainty.

It does not maximize issue count.