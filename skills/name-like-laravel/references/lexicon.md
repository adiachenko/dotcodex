# The Laravel Naming Lexicon

Harvested directly from cloned default branches of `laravel/framework` and first-party packages (`laravel/horizon`, `pennant`, `folio`, `sanctum`, `prompts`, `scout`, `cashier-stripe`) as of August 2026, by extracting all public method names from the core fluent APIs (Collections, Str/Stringable, Eloquent and Query builders, the scheduler, TestResponse) and all class, trait, event, and middleware names across the packages. Use it to study recurring naming families and the semantic distinctions their words carry. Treat the examples as evidence about Laravel's language — how meaning is allocated to words — not as candidates to transplant or as independent justification for a rename.

The corpus is not uniformly exemplary: it carries compatibility fossils that survive because renaming published APIs costs more than living with them. Examples observed during harvesting: redundant-suffix traits predating the `Concerns/` convention (`ConfirmableTrait`, `CapsuleManagerTrait`, `RouteDependencyResolverTrait`); alias pairs kept for compatibility (`avg`/`average`, `trans`/`__`, `studly`/`pascal`); and `ArrayRule`/`StringRule` breaking the bare-noun rule of `Rules\In` only because PHP reserves the words. Copy the discipline, not every surviving identifier — a pattern below counts as evidence when it recurs across young and old code alike, not when it appears once in a legacy corner.

All class names here are framework-style **stems**: the framework puts the role in the namespace, so its names carry no structural suffix. If the target project's convention appends role suffixes (`Job`, `Action`, `Listener`, `Mail`, `Notification`, `Data`, …), every pattern below still applies unchanged to the part before the suffix — read `MarkJobAsComplete` as the stem of a hypothetical `MarkJobAsCompleteJob`.

## Contents
1. Plain-speech verbs (the human-word principle)
2. Boolean question forms
3. The combinator algebra, exhaustively
4. Events: tense and headline forms
5. Imperative classes: jobs, commands, middleware
6. Traits: -able adjectives and verb phrases
7. Contracts: modal verbs
8. Nouns and metaphors: value objects and services
9. Pending, Fake, and other role prefixes
10. Fluent time vocabulary (the scheduler)
11. Global helpers
12. Package-level metaphors

---

## 1. Plain-speech verbs

The single strongest source of "delight". Each is an ordinary English word doing precise technical work:

`forget` (cache/session key removal) · `pluck` (extract one column) · `sole` (exactly one or fail) · `squish` (collapse whitespace) · `headline` / `slug` / `title` / `studly` / `kebab` / `snake` / `camel` (case transforms named after what they look like) · `mask` · `excerpt` · `scan` · `swap` · `wrap` / `unwrap` · `finish` / `start` (ensure suffix/prefix) · `chopStart` / `chopEnd` · `squish` · `sliding` (windows) · `chunk` · `collapse` · `flatten` · `flip` · `pad` · `splice` · `zip` · `tap` (touch and pass along) · `rescue` (run and swallow) · `retry` · `blank` / `filled` · `fresh` vs `refresh` · `latest` / `oldest` · `only` / `except` · `replicate` (clone a model) · `prune` (delete stale) · `flush` · `hydrate` · `fill` · `guess` (`guessExtension`) · `fake` · `sometimes` (validation: only when present) · `bail` (stop on first failure) · `nullable` · `touch` (update timestamps) · `cursor` · `lazy` · `defer` · `sleep`. Note the pattern: every vivid word has an exact structural meaning — Laravel never uses a cute word that doesn't predict behavior.

Counter-lexicon (words Laravel avoids as filler): `process`, `handle` (except the conventional single entry point `handle()`), `execute`, `perform`, `doX`, `data`, `info`, `item` (as generic), `object`, `util`, `helper` (as class name), `misc`, `common`.

## 2. Boolean question forms

`isEmpty` / `isNotEmpty` · `doesntContain` / `contains` · `containsOneItem` / `containsManyItems` · `hasSole` · `hasAny` · `exists` / `doesntExist` · `is` / `isNot` (model identity) · `routeIs` / `fullUrlIs` · `isMatch` · `isUlid` / `isUuid` / `isJson` / `isUrl` · `secure` · `ajax` / `pjax` · `expectsJson` · `wantsJson`

Notes:
- Negation is always the spoken contraction attached to the verb: `doesnt…`, `isNot…`, `whereNot…`. Never `not` as prefix (`notContains`), never `no`/`non`.
- `is` takes a complement when the subject is obvious: `$request->routeIs('admin.*')` — subject folded into the object receiving the call.

## 3. The combinator algebra

Fixed-meaning modifiers, composable and predictable:

| Modifier | Meaning | Instances |
|---|---|---|
| `Or<Verb>` | fallback on miss | `firstOrFail`, `firstOrCreate`, `firstOrNew`, `findOrFail`, `findOrNew`, `findOr`, `createOrFirst`, `updateOrCreate`, `getOrPut`, `deleteOrFail`, `saveOrFail`, `existsOr`, `doesntExistOr`, `incrementOrCreate` |
| `when` / `unless` | conditional | `when`, `unless`, `whenEmpty`, `whenNotEmpty`, `unlessEmpty`, `whenContains`, `whenIs`, `whenTest`, `unlessBetween`, `sometimes` |
| `Until` / `While` | bounded loops | `takeUntil`, `takeWhile`, `skipUntil`, `skipWhile`, `chunkWhile` |
| `By` | key/criterion | `groupBy`, `keyBy`, `countBy`, `sortBy`, `sortByDesc`, `intersectByKeys` |
| `Quietly` | suppress events | `saveQuietly`, `deleteQuietly`, `createQuietly`, `pushQuietly`, `forceCreateQuietly`, `replicateQuietly` |
| `Strict` | `===` semantics | `containsStrict`, `uniqueStrict`, `duplicatesStrict`, `whereInStrict` |
| `Raw` | verbatim SQL/string | `selectRaw`, `whereRaw`, `orderByRaw`, `havingRaw`, `fromRaw` |
| `Sub` | subquery variant | `selectSub`, `joinSub`, `fromSub`, `crossJoinSub` |
| `Recursive` | deep variant | `mergeRecursive`, `replaceRecursive` |
| `Missing` | only-if-absent / assert-absent | `loadMissing`, `mergeIfMissing`, `assertHeaderMissing`, `assertSessionMissing` |
| `Many` / `Each` | plural variant | `findMany`, `retrievesMultipleKeys`, `incrementEach`, `decrementEach`, `eachSpread` |
| `Spread` | unpack tuple args | `mapSpread`, `eachSpread`, `reduceSpread` |
| `Into` | construct target class | `mapInto`, `pipeInto`, `reduceInto` |
| `WithKeys` / `Assoc` | key-aware variant | `mapWithKeys`, `collapseWithKeys`, `reduceWithKeys`, `diffAssoc`, `intersectAssoc` |
| `Using` | custom comparator/callable | `diffUsing`, `sortKeysUsing`, `insertOrIgnoreUsing`, `fetchUsing` |
| `Desc` | reversed order | `sortDesc`, `sortByDesc`, `orderByDesc`, `reorderDesc`, `sortKeysDesc` |
| `Force` | ignore guards | `forceDelete`, `forceFill`, `forceCreate`, `forceIndex` |
| `Without` / `With` | scoped configuration | `withoutOverlapping`, `withoutMiddleware`, `withoutModelEvents`, `withTrashed`, `cloneWithoutBindings` |
| `to<Type>` | conversion | `toArray`, `toJson`, `toPrettyJson`, `toBase64`, `toBoolean`, `toDate`, `toUri` |
| `from<Source>` | construction | `fromQuery`, `fromBase64`, `fromRaw`, `newFromBuilder` |

Symmetric pairs found throughout: `first/last`, `before/after`, `beforeLast/afterLast`, `skip/take`, `push/pop`, `shift/unshift`, `pad/padLeft/padRight/padBoth`, `only/except`, `min/max`, `asc/desc`, `abort_if/abort_unless`, `throw_if/throw_unless`, `report_if/report_unless`, `intro/outro` (Prompts), `chopStart/chopEnd`, `replaceFirst/replaceLast`, `replaceStart/replaceEnd`.

## 4. Events

**Lifecycle pairs** — present participle before, past tense after:
`JobProcessing`/`JobProcessed` · `JobQueueing`/`JobQueued` · `JobPopping`/`JobPopped` · `TransactionBeginning`/`TransactionCommitting`/`TransactionCommitted`/`TransactionRolledBack` · `CacheFlushing`/`CacheFlushed`/`CacheFlushFailed` · `MessageSending`/`MessageSent` · `NotificationSending`/`NotificationSent` · `WritingKey`/`KeyWritten` · `ForgettingKey`/`KeyForgotten` · `MigrationStarted`/`MigrationEnded` · `WorkerStarting`/`WorkerStopping`/`WorkerPausing`/`WorkerResuming`/`WorkerIdle`

**Single past-tense verbs** when subject is obvious from namespace: `Registered`, `Verified`, `Attempting`, `Authenticated`, `Failed`, `Lockout`, `Login`, `Logout`, `Validated`, `Routing`, `Terminating`.

**News-headline observations** (subject + finding, no verb morphology gymnastics): `LongWaitDetected` · `MasterSupervisorOutOfMemory` · `NoPendingMigrations` · `DatabaseBusy` · `QueueBusy` · `CacheHit` / `CacheMissed` · `JobExceptionOccurred` · `JobTimedOut` · `UnexpectedNullScopeEncountered` (Pennant) · `UnknownFeatureResolved` (Pennant).

Horizon's job lifecycle reads as a complete story in the file listing alone: `JobPushed`, `JobReserved`, `JobReleased`, `JobDeleted`, `JobFailed`, `JobsMigrated`.

## 5. Imperative classes

**Jobs / actions** (an order): `MarkJobAsComplete`, `MarkJobAsFailed`, `MarkJobsAsMigrated`, `ExpireSupervisors`, `MonitorMasterSupervisorMemory`, `PruneExpired` (Sanctum), `StopIterating` / `ContinueIterating` (Folio's pipeline verdicts — named as the instruction they represent).

**Middleware** (the guarantee enforced on the request):
`EnsureEmailIsVerified` · `EnsureFrontendRequestsAreStateful` (Sanctum) · `EnsureFeaturesAreActive` (Pennant) · `EnsureNoDirectoryTraversal` (Folio) · `PreventRequestsDuringMaintenance` · `PreventRequestForgery` · `RedirectIfAuthenticated` · `TrimStrings` · `ConvertEmptyStringsToNull` · `EncryptCookies` · `AddQueuedCookiesToResponse` · `ShareErrorsFromSession` · `SubstituteBindings` · `ThrottleRequests` · `TrustProxies` / `TrustHosts` · `ValidateSignature` · `RequirePassword` · `SetCacheHeaders`

Three families of middleware verb: `Ensure…` (assert a state), `Prevent…` (block a case), and a bare transitive verb (`Trim`, `Encrypt`, `Convert`, `Share`, `Substitute` — transform in passing). Choose by what the middleware actually does.

**Console commands**: `<Noun>Command` class, but the *signature* is the real name and reads `context:verb` — `horizon:pause`, `queue:work`, `schedule:run`, `migrate:fresh`.

## 6. Traits

**`-able` capability adjectives**: `Macroable`, `Tappable`, `Conditionable`, `Dumpable`, `Queueable`, `Dispatchable`, `Batchable`, `Prunable`, `MassPrunable`, `Notifiable`, `Localizable`, `Prohibitable`, `Onceable`, `Authenticatable`, `Authorizable`.

**Verb-phrase capabilities** (third person, subject = the using class):
- `Has<Thing>`: `HasFactory`, `HasTimestamps`, `HasRelationships`, `HasEvents`, `HasUlids`, `HasApiTokens`, `HasFeatures` (Pennant), `HasFlushableCache`
- `InteractsWith<System>`: `InteractsWithQueue`, `InteractsWithSession`, `InteractsWithDatabase`, `InteractsWithTime`, `InteractsWithIO` — the standard "talks to X" trait
- Specific verbs: `CompilesLoops`, `CompilesConditionals` (Blade compiler is split into ~25 `Compiles*` traits), `ManagesFrequencies`, `ManagesTransactions`, `GuardsAttributes`, `HidesAttributes`, `BuildsQueries`, `ForwardsCalls`, `ListensForSignals` (Horizon), `FindsWildcardViews` (Folio), `DetectsLostConnections`, `PromptsForMissingInput`, `RoutesNotifications`, `SerializesModels`
- `Can<Verb>`: `CanBeOneOfMany`, `CanResetPassword`, `CanListStoredFeatures` (Pennant)

**Test-environment traits read as switches**: `RefreshDatabase`, `LazilyRefreshDatabase`, `DatabaseTransactions`, `WithFaker`, `WithoutMiddleware`, `WithoutModelEvents` — named for the effect of merely *using* them.

## 7. Contracts (interfaces)

Modal verbs stating obligation or promise: `ShouldQueue` · `ShouldBroadcast` · `ShouldBroadcastNow` · `ShouldBeEncrypted` · `ShouldBeUnique` · `ShouldDispatchAfterCommit` · `ShouldBeDiscovered` · `MustVerifyEmail` · `CanResetPassword`. Also plain capability nouns: `Arrayable`, `Jsonable`, `Htmlable`, `Responsable`, `Castable`.

Marker interfaces named for the *consequence* of implementing them, not their structure — implementing `ShouldQueue` *causes* queueing.

## 8. Nouns and metaphors

Small classes named with one concrete noun whose everyday meaning maps to the behavior:
- `Lottery` — probabilistic execution (`Lottery::odds(1, 100)->winner(...)`)
- `Sleep` — testable time-waiting (`Sleep::for(2)->seconds()`)
- `Timebox` — run a callback in a fixed wall-clock duration (crypto timing safety)
- `Benchmark` — measure a callback
- `Once` — memoization
- `Fluent` — a free-form fluent bag
- `Optional` — null-safe wrapper
- `MessageBag`, `ViewErrorBag` — a bag you carry messages in
- `Lock`, `Timebox`, `Lottery` all support the read-aloud test at the call site: `Lottery::odds(1, 100)`.

Pennant internals show noun precision: `PendingScopedFeatureInteraction`, `LazilyResolvedFeature` — long but exact, low-frequency internals.

## 9. Role prefixes

- `Pending<Thing>` — a fluent builder for something not yet real: `PendingMail`, `PendingRequest`, `PendingDispatch`, `PendingBatch`, `PendingChain`, `PendingProcess`, `PendingCommand`, `PendingRoute` (Folio). The moment `send()`/`dispatch()` is called, the pending thing becomes real. Never `XBuilder` for this role (Laravel reserves `Builder` for query builders and schema builder).
- `<Thing>Fake` — test double swapped in behind a facade: `PendingMailFake`, `BusFake`, `QueueFake`.
- `<Driver>Manager` — only for the specific pattern "resolves and caches named drivers": `CacheManager`, `QueueManager`, `AuthManager`, `FeatureManager`. Do not use `Manager` for anything else.
- `<Thing>Repository` — storage abstraction over a backend: Horizon's `JobRepository`, `MetricsRepository`.
- `New<Thing>` — a just-created value that carries its one-time-visible secret: Sanctum's `NewAccessToken`. `Transient<Thing>` — exists only for this request: `TransientToken`.

## 10. Fluent time vocabulary

The scheduler is the masterclass in call-site design. The entire API is adverbs of frequency, exactly as spoken:

`everySecond`, `everyTwoSeconds` … `everyMinute`, `everyFiveMinutes` … `hourly`, `hourlyAt`, `everyOddHour`, `daily`, `dailyAt`, `twiceDaily`, `twiceDailyAt`, `weekdays`, `weekends`, `mondays` … `sundays`, `weekly`, `weeklyOn`, `monthly`, `monthlyOn`, `twiceMonthly`, `lastDayOfMonth`, `quarterly`, `yearly`, `yearlyOn`, `between`, `unlessBetween`, `timezone`, `onOneServer`, `withoutOverlapping`, `runInBackground`, `evenInMaintenanceMode`.

Note `mondays()` — plural noun as adverb ("on Mondays"). And `everyOddHour` — no committee would approve it; it's exactly what a human says.

## 11. Global helpers

snake_case, extremely short, extremely common: `collect`, `value`, `tap`, `with`, `optional`, `retry`, `rescue`, `once`, `blank`, `filled`, `str`, `now`, `today`, `old`, `back`, `abort`, `abort_if`, `abort_unless`, `throw_if`, `throw_unless`, `report`, `dispatch`, `event`, `view`, `route`, `to_route`, `fake`, `literal`, `transform`, `e`, `__`.

Data-path family with a shared prefix: `data_get`, `data_set`, `data_fill`, `data_forget`, `data_has`.

## 12. Package-level metaphors

Product names are evocative single words (Horizon, Telescope, Pennant, Folio, Sail, Breeze, Vapor, Octane, Forge, Envoy, Scout, Cashier, Sanctum, Reverb, Pulse); each package's *internal* vocabulary then commits to one coherent field:

- **Horizon** (overseeing queues): `Supervisor`, `MasterSupervisor`, `AutoScaler`, `Balance`, `WaitTimeCalculator`, `WorkloadRepository`, events like `SupervisorLooped`, `MasterSupervisorDeployed`, `MasterSupervisorReviving`.
- **Pennant** (flags flown): `Feature`, feature "scopes", `FeaturesPurged`, `AllFeaturesPurged`.
- **Folio** (pages of a book): page-based routing, `MatchedView`, `MountPath`, matcher pipeline steps named as imperative match instructions (`MatchLiteralViews`, `MatchWildcardDirectories`, `MatchRootIndex`).
- **Prompts** (a conversation): functions are speech acts — `intro`, `outro`, `note`, `alert`, `warning`, `error`, `confirm`, `suggest`, `pause`, `spin`.

The lesson: metaphor lives at the boundary (package name, top-level concepts); precision lives inside. Never let the metaphor force an inaccurate internal name.
