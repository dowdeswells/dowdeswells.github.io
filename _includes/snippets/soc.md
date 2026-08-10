---
name: layered-design-separation-of-concerns
description: >-
  Applies a three-layer separation-of-concerns pattern to AI-agent features:
  (1) a business layer with the domain logic and an input-parameter validation
  mechanism, (2) a data storage layer (interface + concrete implementation) that
  gets injected into the business object, and (3) an agent-specific layer that
  translates the business object's input and output into AI function tools. Use
  when adding a new tool, feature, or domain to an AI agent, or when refactoring
  tool code that mixes business logic, persistence, and agent-facing formatting.
---

# Layered Design for Separation of Concerns

A three-layer pattern for building AI-agent features cleanly. The rule of thumb:
**each layer only knows about the layer directly beneath it**, and nothing below
the agent layer knows that an AI exists.

```
agent layer  ──▶  business layer  ──▶  storage layer
 (translates)      (domain logic)      (persistence, injected)
```

## The three layers

### 1. Business layer — `Business/` (namespace `…Business`)
- A class (`*Service`) with the **business functionality**: constructs domain
  objects (with sensible defaults), enforces business rules, orchestrates
  persistence through the injected store.
- A **validation mechanism** (`*Validator`) for the input parameters: required
  fields, ranges, dates (e.g. reject future dates), defined enum values.
- A result type (`*Result`) for operation outcomes: success + domain object, or
  a plain-language error. **No emojis, no Markdown, no `AIFunction`** — this
  layer returns domain objects only.

### 2. Data storage layer — `Storage/` (namespace `…Storage`)
- An **interface** (`I*Store` / `I*Repository`) plus a **concrete implementation**
  (in-memory, EF Core, SQL, ...).
- **Injected into the business object's constructor** — the business class never
  news up its own store.
- Pure persistence. No business rules (no future-date checks, no amount
  validation), no AI concepts. Swapping the implementation must not touch the
  other layers.

### 3. Agent-specific layer — `Tools/` (namespace `…Tools`)
- A **translation class** (`*ToolActions`): converts the AI tool's raw string
  parameters (dates, ids, enums) into typed values, calls the business object,
  and formats the domain results into the Markdown/emoji responses the model reads.
- A **tool-registration class** (`*Tools`): creates the `AIFunction`s with a name
  and a description (via `AIFunctionFactory.Create`) that tells the model when to
  call the tool and what parameters it needs.
- All `AIFunction`, tool-description, and response-formatting concerns live here.

## Workflow: applying this to a new feature

1. **Model the domain** — a shared entity in `Models/` (used by all three layers).
2. **Storage layer** — write the interface, then a concrete implementation.
   Make it thread-safe if it is shared across sessions.
3. **Business layer** — result type → validator → service class with the store
   injected in its constructor. Every business rule goes through the validator.
4. **Agent layer** — one translation method per tool (raw strings in → typed
   business calls → formatted responses), registered with `AIFunctionFactory.Create`.
5. **Wire it up** — register store + service in DI, pass the service into the
   agent factory. In this repo the agent project owns the composition root:
   ```csharp
   // ExpenseAgent/DependencyInjection/ExpenseTrackerServiceCollectionExtensions.cs
   services.AddSingleton<ITransactionStore, InMemoryTransactionStore>(); // interface (ExpenseLib) → impl (Expense.Infrastructure)
   services.AddSingleton<TransactionService>(); // business layer (ExpenseLib), store injected by the container
   services.AddSingleton<AIAgent>(sp =>
       sp.GetRequiredService<AgentFactory>().CreateExpenseTrackerAgent(
           sp.GetRequiredService<TransactionService>()));
   // WebApi: builder.Services.AddExpenseTracker(agentConfig);
   ```
6. **Test each layer** — deterministic tests that do NOT need an LLM or API key:
   - Business layer: unit tests against the service (construct, validate, edit, remove).
   - Agent layer: invoke the `AIFunction` directly with a JSON-like argument
     dictionary, bypassing the model entirely.

## Copy-paste templates

All templates live in [`templates/`](templates/) (C# / .NET + Microsoft.Extensions.AI).
Substitute the `Transaction*` names with your domain:

| Template | File | Replace |
|---|---|---|
| Domain model | [`templates/domain-model.cs`](templates/domain-model.cs) | `Transaction` → your entity |
| Storage layer (interface + in-memory impl) | [`templates/storage-layer.cs`](templates/storage-layer.cs) | `ITransactionStore`, `InMemoryTransactionStore` |
| Business layer (result + validator + service) | [`templates/business-layer.cs`](templates/business-layer.cs) | `TransactionResult`, `TransactionValidator`, `TransactionService` |
| Agent layer (tool actions + AIFunction creation) | [`templates/agent-layer.cs`](templates/agent-layer.cs) | `TransactionToolActions`, `TransactionTools` |

## Reference implementation

This repository implements the pattern split across three projects — the AI-free core in
`ExpenseLib`, the concrete storage in `Expense.Infrastructure`, and the agent layer in
`ExpenseAgent` (which references the other two and composes everything):

- Domain + business: `src/ExpenseAgent/ExpenseLib/` — `Models/Transaction.cs`,
  `Business/TransactionService.cs`, `Business/TransactionValidator.cs`,
  `Business/TransactionResult.cs` (namespace `ExpenseLib.*`, **no AI references**)
- Storage interface: `src/ExpenseAgent/ExpenseLib/Storage/ITransactionStore.cs`
- Storage implementation: `src/ExpenseAgent/Expense.Infrastructure/InMemoryTransactionStore.cs`
- Agent: `src/ExpenseAgent/ExpenseAgent/Tools/TransactionToolActions.cs`,
  `src/ExpenseAgent/ExpenseAgent/Tools/TransactionTools.cs`
- Wiring: `AgentFactory.CreateExpenseTrackerAgent(TransactionService)` + the composition root
  `ExpenseAgent/DependencyInjection/ExpenseTrackerServiceCollectionExtensions.cs`
  (`AddExpenseTracker`), called from `ExpenseAgent.WebApi/Startup.cs`
- Tests: `ExpenseAgent.Tests/TransactionServiceTests.cs` (business layer),
  `ExpenseAgent.Tests/TransactionToolTests.cs` (agent layer, no LLM)

## Rules and anti-patterns

- ❌ Agent tool methods reaching straight into the store — always go through the
  business object.
- ❌ Business layer returning `❌`/`✅`/Markdown strings — return a `*Result`;
  the agent layer formats.
- ❌ Validation (future dates, positive amounts, required fields) living in the
  agent layer — it belongs in the `*Validator`, behind the business object.
- ❌ Business layer referencing `AIFunction`, `Microsoft.Extensions.AI`, or tool
  descriptions.
- ❌ A store without an interface — the business object must receive storage by
  injection.
- ❌ Passing raw, unparsed strings into the business layer. Parsing
  (string → `DateTime`/`Guid`/enum) is translation and lives in the agent layer;
  validating parsed values against business rules lives in the business layer.

## Quick check before you call it done

- [ ] Does the business class construct and validate without any AI types?
- [ ] Is the store injected via constructor, not instantiated inside the business class?
- [ ] Do the agent tool actions only parse, translate, and format?
- [ ] Do deterministic tests cover the business rules without an LLM or API key?
- [ ] Can the storage implementation be swapped without touching the business or
      agent layers?
