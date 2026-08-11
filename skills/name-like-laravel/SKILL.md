---
name: name-like-laravel
description: Methodology for naming classes, methods, files, folders, and variables with the taste of the Laravel framework source. Use whenever a name must be invented or judged in PHP/Laravel work — new classes, methods, jobs, events, middleware, traits, helpers, config keys — or when the user asks to name something better, calls a name mechanical or clunky, wants idiomatic/expressive names, or requests a readability review. Also applies to non-Laravel code when names should read like natural language.
---

# Laravel-Style Naming

Distilled from the actual `laravel/framework` source and first-party packages (Horizon, Pennant, Folio, Sanctum, Prompts). The core insight: Laravel's names aren't creative one by one — they come from a small system (a grammar, a lexicon, a read-aloud test) applied everywhere. Learn the system and good names become derivable, not lucky.

## The Prime Directive: optimize the call site

A name is written once and read hundreds of times, almost always at the *call site*. Laravel names things so the line *using* the name reads as English:

```php
$schedule->command('emails:send')->dailyAt('13:00')->weekdays()->onOneServer();
abort_unless($user->owns($post), 403);
Str::of($path)->beforeLast('/')->finish('/');
```

`dailyAt` is odd in isolation, perfect after `->`. **Never judge a candidate alone — write the realistic call site and read it aloud.** If you'd stumble saying it to a colleague, rename. This one habit produces most of the "Laravel feel".

## Stem vs. structural affix

A class name has up to two parts; this skill governs only the first:

- **Semantic stem** — the meaning: `MarkJobAsComplete`, `OrderShipped`. All rules and examples below are about stems.
- **Structural affix** — an optional role marker set by project convention: `…Job`, `…Action`, `…Listener`, `…Mail`, `…Notification`, `…Controller`, `…Data`.

The framework omits affixes because its namespaces carry the role (`Events\Registered`, `Jobs\PruneExpired`) — hence the affix-free examples. Many app codebases (Spatie-style) put the role in a suffix instead (`CreateOrderAction`, `PerformDatabaseCleanupJob`). Both satisfy the real rule — **say the role exactly once, in the path or the name, never twice** — so treat the project's choice as law, never a taste question. The one interaction: the stem must not repeat the affix's role, and its grammar follows the table regardless — with `…Job` the stem is a bare imperative, with `…Notification` a past-tense fact (`EmployeeAccountCreated`), with `…Mail` the message's subject as a noun (`OrderConfirmation`).

## The Grammar: part of speech follows the role

Match the shape and the stem explains its own role before anyone reads the body:

| Element | Stem shape | Real examples |
|---|---|---|
| Event | Past tense (happened) or present participle (happening / about to) | `JobProcessed` / `JobProcessing`, `TransactionCommitted` / `TransactionCommitting`, `PasswordReset`, `Registered` |
| Event (observation) | News headline: subject + finding | `LongWaitDetected`, `MasterSupervisorOutOfMemory`, `NoPendingMigrations`, `DatabaseBusy` |
| Job / Action / Command | Imperative verb phrase — an order | `MarkJobAsComplete`, `ExpireSupervisors`, `PruneExpired` |
| Middleware | Imperative stating the guarantee enforced on the request | `EnsureEmailIsVerified`, `PreventRequestsDuringMaintenance`, `RedirectIfAuthenticated`, `TrimStrings`, `ShareErrorsFromSession` |
| Trait (capability) | `-able` adjective, or third-person verb phrase ("this class …") | `Macroable`, `Tappable`, `Prunable`; `HasFactory`, `InteractsWithQueue`, `CompilesLoops`, `GuardsAttributes`, `ListensForSignals` |
| Interface (contract) | Modal verb: obligation or ability | `ShouldQueue`, `ShouldBeDiscovered`, `MustVerifyEmail`, `CanResetPassword` |
| Value object / small service | One evocative concrete noun | `Lottery`, `Sleep`, `Timebox`, `Once`, `Optional`, `MessageBag` |
| Deferred/builder object | `Pending` + thing being built | `PendingMail`, `PendingRequest`, `PendingDispatch` — never `MailBuilder` |
| Query/lookup method | `where`/`find`/`first` + condition, left to right | `firstWhere`, `whereNotNull`, `findSole` |
| Boolean method | A yes/no question about the subject | `isEmpty`, `doesntContain`, `exists`, `routeIs`, `containsOneItem` |
| Assertion (tests) | `assert` + what a human would check | `assertSee`, `assertRedirectBack`, `assertDatabaseHas` |
| Listener | Imperative: the reaction taken, not the event heard | `SendInvitationMail` (+`Listener`) — never `OrderShippedListener` |
| Mailable / Notification | Message's subject: noun phrase (mail), past-tense fact (notification) | `OrderConfirmation` (+`Mail`), `EmployeeAccountCreated` (+`Notification`) |
| DTO | The noun carried, singular | `Order`, `UserProfile` (+`Data`) |
| Invokable controller | Imperative, verb-first — the class *is* one action | `StorePost` (+`Controller`) |

Trait test: a `use` block should read like a bio — `use HasAttributes, HidesAttributes, GuardsAttributes;` — three verb phrases, one subject.

The shapes are also a side-effect promise: a noun or query-shaped name (`nextDelivery`, `firstWhere`, `count`) promises the receiver is unchanged. If the body mutates, pick a verb that admits the mutation — any verb that does: Laravel reaches for `pull` (get and forget), queues say `reserve` and `pop`, but the requirement is only that the name confesses something is taken or changed. A name that reads like a peek but consumes is a bug waiting for its second caller.

## The Lexicon: the human word over the technical word

When several words are correct, Laravel picks the one a person says aloud: `forget` a cache key (not `remove`/`unset`); `pluck` a column; `sole` (exactly one, or fail); `squish`, `headline`, `mask`, `excerpt`, `sliding`; `rescue(fn)` instead of a try/catch wrapper; `retry`, `tap`, `blank`/`filled`; `latest()`/`oldest()` instead of `orderByCreatedAtDesc()`; `fresh()` (new instance) vs `refresh()` (mutate this one) — everyday words carrying precise distinctions. Treat `process`, `handle`, `manage`, `execute`, `data`, `util`, `helper` as smells unless nothing more specific exists. Vocabulary is also scoped by setting: `assert…` belongs to tests; a production guard that validates and throws states its guarantee with `ensure…` (`ensureNameIsAvailable`), mirroring the middleware convention.

Delight in Laravel is almost always this: an *exact, ordinary* word where you expected jargon — never cuteness for its own sake.

## The Combinators: a modifier algebra

Fixed-meaning modifiers make the huge API predictable. Compose from them instead of inventing new adverbs:

- `Or<Fallback>` — on miss: `firstOrFail`, `firstOrCreate`, `getOrPut`
- `when`/`unless` — conditional: `whenEmpty`, `unlessBetween`
- `Until`/`While` — bounded iteration: `takeUntil`, `chunkWhile`
- `By` — key/criterion: `groupBy`, `countBy`
- `Quietly` — no events: `saveQuietly` · `Strict` — `===`: `containsStrict` · `Raw` — verbatim: `selectRaw` · `Desc`, `Recursive`, `Many`, `WithKeys`, `Sub`, `Missing` — likewise fixed
- Negation is spoken English attached to the verb: `doesntContain`, `whereNotNull`, `isNotEmpty` — never `notContains` or `no`/`non` prefixes

Names arrive in pairs: `when`/`unless`, `before`/`after`, `skip`/`take`, `only`/`except`, `abort_if`/`abort_unless`. When introducing one half, check the other exists or could — a name with no natural opposite is often the wrong axis.

## The Length Budget

- Called constantly → very short: `tap`, `dd`, `now`, `old`
- Common in a domain → one or two words: `paginate`, `firstOrFail`
- Internal, rare, needs precision → a sentence is fine: Folio's `MatchWildcardViewsThatCaptureMultipleSegments`

Never pay for length with abbreviation: Laravel spells out `attributes`, `configuration`; no `cfg`/`attr`/`mgr` anywhere, only universals (`id`, `url`, `sql`).

Frequency is one axis; **context distance** the other. After `$collection->` the subject is supplied, so `all`, `nth` suffice. A name appearing *alone* — import list, queue dashboard, log line — must be self-contained: `LongWaitDetected` works in a Slack alert with zero code around it. Picture the loneliest place the name will appear.

## Namespaces carry context

The path is part of the sentence: `Validation\Rules\In`, `Events\Registered`. Name the leaf for what's unique; let the path say the rest. In suffix-convention projects the suffix is the "once-place" and this applies to the stem: `Orders\CreateOrderAction` is fine ("Order" is the verb's object, not role filler); repeating the role twice or padding with meaning-free words (`PasswordValidationRuleClass`) fails everywhere.

Folders: plural domain nouns — `Concerns/` (traits), `Contracts/` (interfaces), `Events/`, `Jobs/`, `Rules/`. File name = class name. Config keys read as plain-word paths (`queue.connections.redis.retry_after`); command signatures as `context:verb` (`horizon:pause`, `queue:work`).

## Metaphor: commit or abstain

Horizon commits fully: *supervisors*, *balancing*, *wait times* — one coherent workplace, not a pun sprinkled on top. Rules: one metaphor per bounded context; it must map structurally (a supervisor really supervises worker processes), not decoratively; and in application code prefer the domain's own language (your users' and PMs' words) — metaphor is for infrastructure whose real names would be dull; business code already has a metaphor, the business.

## The Procedure

0. **Detect the affix convention** from sibling classes or the project styleguide. Whatever it is, it's law: derive only the stem below, attach the affix at the end, and never add/remove an affix on taste grounds.
1. **Say the behavior aloud** in one sentence describing the observable outcome, not the implementation — a good name stays true if the body is rewritten. (Private algorithmic helpers may name their mechanism: `qualifyColumn`.) The verb and noun you naturally used are the leading candidates. A use case in a name (`toDatabase`, `resolveForQueuedMessage`) is a flaw only when it's incidental: when the use case is why the method exists beside its plainer sibling, and the name stays true if the body is rewritten, the context *is* the contract — keep it.
2. **Classify the element** — now that you know what it does — and look up its stem shape in the table.
3. **Generate 3–5 candidates**, at least one using a plain-speech word you'd normally reject as "not technical enough".
4. **Write the real call site** for each and read it aloud; discard whatever you stumble on.
5. **Check the neighbors.** Reuse existing combinators and pairs (if `whenEmpty` exists, your conditional is `when<X>`, not `if<X>`). Then find the *nearest* sibling — the operation most easily confused with yours — and make the distinguishing word carry exactly that difference, as `fresh`/`refresh` do. If the differing word doesn't name the behavioral difference, one name is wrong.
6. **Check the negation and the pair.** Does it negate as spoken English? Does its opposite exist or make sense?
7. **Apply the length budget**; drop words the namespace or receiver already supplies.
8. **Smell check the stem**: reject filler (`Manager`/`Handler`/`Processor`/`Helper`/`Util`/`Service`/`data`/`info` — `Manager` is legitimate only for the driver-manager pattern), abbreviations, repeated namespace/affix words, foreign metaphors. Structural affixes from step 0 are role markers, never filler.
9. **Attach the affix** verbatim; confirm the stem doesn't duplicate it.

Two overrides apply throughout. **Precedence:** a truthful, discriminating domain term or established local term outranks any framework-derived default here — if the codebase already says `MailBuilder`, or `Manager` is genuinely the most accurate word for a new domain role, domain truth and consistency beat framework taste; the rules above set defaults, not vetoes over accurate vocabulary. **Design smell:** if step 1's honest sentence won't compress — the best candidate is a procedural chain like `validateAndSaveOrderThenNotify` on a public, frequently-read element — stop polishing the name. That's a missing concept or mixed responsibility, and the right deliverable is a design suggestion (extract the concept, split the method), not a prettier label for a confused abstraction.

For reviews, run it in reverse: reconstruct the call-site sentence, name the rule broken, show the before/after call site — not just "rename to X". Judge the proposed name as adversarially as the original — a rename whose justification fits an un-renamed sibling equally well refutes itself. Keep two verdicts separate: the best name, and whether the migration cost (published API, serialized job class) justifies renaming now. When renaming, propagate everywhere the old word lives: references, tests, docs, config keys, translation strings. The code's own strings are a naming oracle: compare each method name against the error and log messages it emits — when the message says "reading outgoing bindings" but the method says `exchangeSourceBindings`, the author already found the human words under pressure to be understood; rename toward the string.

## Reference

`references/lexicon.md` — hundreds of harvested method/class/trait/event/middleware names organized by pattern; use during candidate generation and when justifying a rename. It is evidence of the *discipline* — how meaning is allocated to words — not a list of identifiers to transplant: that a word exists in Laravel doesn't make it right for your domain, and the domain's own established word always beats a framework word of similar meaning.
