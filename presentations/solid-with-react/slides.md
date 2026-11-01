---
theme: default
title: SOLID with React
info: |
  Workshop on applying SOLID in React — composition, hooks, and boundaries with bad vs good examples.
author: MentorBridge
website: https://mentorbridge.in/
category: technical
tags: [react, solid, architecture, clean-code]
duration: 75m
level: intermediate
colorSchema: light
highlighter: shiki
transition: fade-out
mdc: true
fonts:
  sans: DM Sans
  mono: JetBrains Mono
---

<!--
Opening. Frame SOLID as design pressure for React apps students already ship — not OOP theory class.
Ask: "Who has a component file over 300 lines that everyone is afraid to touch?"
-->

<div class="mb-cover absolute inset-0" />

<div class="relative h-full flex flex-col justify-between py-1">
  <BrandLogo />

  <div>
    <p class="mb-kicker !mb-3">MentorBridge Workshop</p>
    <h1 class="!text-[2.2rem] !mb-2">
      SOLID with React<br />
      <span class="mb-pink">Principles that survive real components</span>
    </h1>
    <div class="mb-rule mb-3" />
    <p class="mb-lead">
      Apply S · O · L · I · D through composition, hooks, and boundaries —
      with bad vs good code, when each principle applies, and common pitfalls.
    </p>
  </div>

  <div class="flex items-center justify-between text-sm mb-muted">
    <span>Intermediate · React patterns assumed</span>
    <span>mentorbridge.in</span>
  </div>
</div>

---

<!--
Why this workshop. Students know hooks — now make change safer.
-->

# Why SOLID in a React codebase?

<div class="mb-three mt-3">
  <div class="mb-card-pink">
    <div class="mb-kicker">Change</div>
    <p class="!m-0 text-sm mb-slate">Features keep landing. Designs that fight change create merge pain and regressions.</p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Ownership</div>
    <p class="!m-0 text-sm mb-slate">Teams split work across hooks, UI, and data. Boundaries make parallel work safer.</p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Tests</div>
    <p class="!m-0 text-sm mb-slate">Injected dependencies and focused modules are easier to unit-test than god components.</p>
  </div>
</div>

<div class="mb-callout mt-4">
  SOLID is not “write more classes”. In React it shows up as <strong>composition, focused hooks, and stable contracts</strong>.
</div>

<SessionFooter />

---

<!--
Map of the five principles — overview before deep dives.
-->

# The map

<div class="mt-3 grid grid-cols-5 gap-2 text-center text-sm">
  <div class="mb-card-pink">
    <div class="text-2xl font-bold mb-pink">S</div>
    <div class="font-bold mt-1">Single<br/>Responsibility</div>
  </div>
  <div class="mb-card-pink">
    <div class="text-2xl font-bold mb-pink">O</div>
    <div class="font-bold mt-1">Open /<br/>Closed</div>
  </div>
  <div class="mb-card-pink">
    <div class="text-2xl font-bold mb-pink">L</div>
    <div class="font-bold mt-1">Liskov<br/>Substitution</div>
  </div>
  <div class="mb-card-pink">
    <div class="text-2xl font-bold mb-pink">I</div>
    <div class="font-bold mt-1">Interface<br/>Segregation</div>
  </div>
  <div class="mb-card-pink">
    <div class="text-2xl font-bold mb-pink">D</div>
    <div class="font-bold mt-1">Dependency<br/>Inversion</div>
  </div>
</div>

<div class="mb-callout mt-4">
  Workshop rhythm for each letter: <strong>define → bad → good → when + pitfalls</strong>
</div>

<SessionFooter />

---

<SectionDivider
  number="S"
  title="Single Responsibility"
  subtitle="One reason to change — one clear job per module."
/>

---

<!--
SRP definition in React terms.
-->

# S · One reason to change

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Classic idea</div>
    <p class="!m-0 text-sm mb-slate">
      A module should have one reason to change —
      one axis of responsibility, not “everything this screen needs”.
    </p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">In React</div>
    <ul class="mb-list">
      <li>UI rendering ≠ data fetching ≠ formatting</li>
      <li>Custom hooks own stateful logic</li>
      <li>Presentational pieces stay dumb & stable</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Smell: opening a file to change copy <em>and</em> API shape <em>and</em> validation rules.
</div>

<SessionFooter />

---

<!--
SRP bad example — god component.
-->

# S · Bad — the god component

```tsx
function UserDashboard() {
  const [user, setUser] = useState(null)
  useEffect(() => { fetch('/api/me').then(r => r.json()).then(setUser) }, [])
  const label = user ? `${user.first} ${user.last}`.toUpperCase() : '…'
  function onSave(form) { /* validate + PATCH + toast + analytics */ }
  return ( /* 200 lines of JSX: header, form, charts, modals */ )
}
```

<div class="mb-card mt-3 text-sm mb-slate">
  Fetching, formatting, validation, side effects, and layout share one reason-to-open.
  Any product tweak risks breaking something unrelated.
</div>

<SessionFooter />

---

<!--
SRP good example.
-->

# S · Good — split by reason to change

```tsx
// data
function useCurrentUser() { /* fetch + cache + error */ }

// formatting (pure)
function formatUserLabel(user: User) { return `${user.first} ${user.last}` }

// actions
function useSaveProfile() { /* validate + mutate + toast */ }

// UI
function UserDashboard() {
  const { user } = useCurrentUser()
  const save = useSaveProfile()
  return <ProfileForm label={formatUserLabel(user)} onSave={save} />
}
```

<div class="mb-callout mt-3">
  Each piece changes for a different reason — API, copy rules, UX, or persistence.
</div>

<SessionFooter />

---

<!--
SRP when + pitfalls.
-->

# S · When it applies · Pitfalls

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Apply when</div>
    <ul class="mb-list">
      <li>File grows past comfortable review size</li>
      <li>Two features always collide in the same PR</li>
      <li>You can't unit-test logic without mounting UI</li>
    </ul>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Pitfalls</div>
    <ul class="mb-list">
      <li>Over-splitting into 1-line files</li>
      <li>Fake SRP: `utils.ts` that does everything</li>
      <li>Prop-drilling hell after extraction</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Prefer <strong>cohesion</strong> over file count. Split on change axis, not on line count alone.
</div>

<SessionFooter />

---

<SectionDivider
  number="O"
  title="Open / Closed"
  subtitle="Open for extension, closed for modification."
/>

---

<!--
OCP definition.
-->

# O · Extend without rewriting the core

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Classic idea</div>
    <p class="!m-0 text-sm mb-slate">
      Behavior grows by adding new code —
      not by endlessly editing a central switch.
    </p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">In React</div>
    <ul class="mb-list">
      <li>Composition over <code>type="…"</code> switches</li>
      <li><code>children</code> / slots / render props</li>
      <li>Strategy maps you register into</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Smell: every new variant means editing the same mega-component again.
</div>

<SessionFooter />

---

<!--
OCP bad — type prop switch.
-->

# O · Bad — prop-switching hell

```tsx
function Button({ type, children }) {
  if (type === 'primary') return <button className="bg-pink">{children}</button>
  if (type === 'danger')  return <button className="bg-red">{children}</button>
  if (type === 'link')    return <a className="underline">{children}</a>
  if (type === 'icon')    return <IconButton>{children}</IconButton>
  // next sprint: type === 'ghost' | 'premium' | …
}
```

<div class="mb-card mt-3 text-sm mb-slate">
  Closed for nothing — every product request reopens <code>Button.tsx</code> and risks regressions.
</div>

<SessionFooter />

---

<!--
OCP good — composition.
-->

# O · Good — compose variants

```tsx
function Button({ className, ...props }: ButtonProps) {
  return <button className={cx('btn', className)} {...props} />
}

const PrimaryButton = (p) => <Button className="bg-pink text-white" {...p} />
const DangerButton  = (p) => <Button className="bg-red text-white" {...p} />

// Or: registry for renderers
const fields = { text: TextField, select: SelectField }
function DynamicField({ kind, ...rest }) {
  const Comp = fields[kind]
  return <Comp {...rest} />
}
```

<div class="mb-callout mt-3">
  New variant = new module (or registry entry). Core stays stable.
</div>

<SessionFooter />

---

<!--
OCP when + pitfalls.
-->

# O · When it applies · Pitfalls

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Apply when</div>
    <ul class="mb-list">
      <li>Variants keep arriving on a stable shell</li>
      <li>Third parties / plugins need extension points</li>
      <li>Design system primitives must stay calm</li>
    </ul>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Pitfalls</div>
    <ul class="mb-list">
      <li>Premature abstraction (YFG — you ain't gonna need it)</li>
      <li>Indirection nobody can navigate</li>
      <li>“Config objects” that recreate the switch</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Wait for the <strong>second or third</strong> real variant before inventing a plugin system.
</div>

<SessionFooter />

---

<SectionDivider
  number="L"
  title="Liskov Substitution"
  subtitle="Swap a piece — callers should not break."
/>

---

<!--
LSP definition.
-->

# L · Substitutable without surprises

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Classic idea</div>
    <p class="!m-0 text-sm mb-slate">
      If B claims to replace A, using B wherever A was expected must not violate A's contract.
    </p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">In React</div>
    <ul class="mb-list">
      <li>Shared prop contracts must hold</li>
      <li>Hooks with the same signature behave alike</li>
      <li>Polymorphic components honor base promises</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Smell: “Drop-in replacement” that throws, ignores props, or changes required timing.
</div>

<SessionFooter />

---

<!--
LSP bad.
-->

# L · Bad — lookalike APIs, different contracts

```tsx
// Documented as drop-in for <Input onChange={(v: string) => …} />
function PhoneInput({ onChange }) {
  // breaks callers: sends event, not string
  return <input onChange={(e) => onChange(e)} />
}

function useUser() { return { data, loading } }          // ok
function useAdminUser() {                                 // “same hook”
  if (!isAdmin) throw new Error('denied')                 // surprises
  return { data }                                         // no loading
}
```

<div class="mb-card mt-3 text-sm mb-slate">
  Same names, broken expectations. Substitution becomes a footgun.
</div>

<SessionFooter />

---

<!--
LSP good.
-->

# L · Good — honor the contract

```tsx
type FieldProps = {
  value: string
  onChange: (value: string) => void
  disabled?: boolean
}

function TextField(props: FieldProps) { /* … */ }
function PhoneField(props: FieldProps) {
  return (
    <TextField
      {...props}
      onChange={(v) => props.onChange(normalizePhone(v))}
    />
  )
}
```

<div class="mb-callout mt-3">
  Variants may add behavior — they must not weaken or reinterpret the shared props contract.
</div>

<SessionFooter />

---

<!--
LSP when + pitfalls.
-->

# L · When it applies · Pitfalls

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Apply when</div>
    <ul class="mb-list">
      <li>Design-system swaps (Input → CurrencyInput)</li>
      <li>Feature flags choose implementations</li>
      <li>Tests replace modules with fakes</li>
    </ul>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Pitfalls</div>
    <ul class="mb-list">
      <li>Inheritance theater (<code>extends</code> for reuse)</li>
      <li>“Optional” props that are secretly required</li>
      <li>Silent no-ops for unsupported props</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Prefer <strong>explicit types + composition</strong> over deep component inheritance trees.
</div>

<SessionFooter />

---

<SectionDivider
  number="I"
  title="Interface Segregation"
  subtitle="Don't force callers to depend on what they don't use."
/>

---

<!--
ISP definition.
-->

# I · Small, focused contracts

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Classic idea</div>
    <p class="!m-0 text-sm mb-slate">
      Many specific interfaces beat one fat interface that every consumer must know.
    </p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">In React</div>
    <ul class="mb-list">
      <li>Slim props — not 30 optional knobs</li>
      <li>Focused hooks instead of one mega-hook</li>
      <li>Split context by concern</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Smell: child re-renders because it subscribed to a giant Context it barely uses.
</div>

<SessionFooter />

---

<!--
ISP bad.
-->

# I · Bad — props bag & mega Context

```tsx
type UserPanelProps = {
  user: User; theme: Theme; permissions: string[]
  onEdit; onDelete; onExport; onImpersonate
  analytics; featureFlags; locale; currency
  // …
}

const AppContext = createContext({ user, cart, theme, flags, api, … })

function Avatar() {
  const ctx = useContext(AppContext) // pulls the world for a URL
  return <img src={ctx.user.avatarUrl} />
}
```

<div class="mb-card mt-3 text-sm mb-slate">
  Every consumer depends on everything. Changes ripple; renders fan out.
</div>

<SessionFooter />

---

<!--
ISP good.
-->

# I · Good — depend on what you use

```tsx
function Avatar({ url }: { url: string }) {
  return <img src={url} alt="" />
}

function useAuthUser() { /* only auth slice */ }
function useTheme() { /* only theme */ }

const AuthContext = createContext<AuthValue>(…)
const ThemeContext = createContext<ThemeValue>(…)

// Component asks for narrow props / narrow hooks
function ProfileHeader() {
  const { user } = useAuthUser()
  return <Avatar url={user.avatarUrl} />
}
```

<div class="mb-callout mt-3">
  Narrow inputs → easier reuse, clearer tests, fewer accidental couplings.
</div>

<SessionFooter />

---

<!--
ISP when + pitfalls.
-->

# I · When it applies · Pitfalls

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Apply when</div>
    <ul class="mb-list">
      <li>Props lists become scrollable</li>
      <li>Context updates re-render half the tree</li>
      <li>Storybook needs 12 unrelated mocks</li>
    </ul>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Pitfalls</div>
    <ul class="mb-list">
      <li>Hundreds of micro-contexts</li>
      <li>Duplicating the same data in five hooks</li>
      <li>Passing 20 props “just in case”</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Segregate along <strong>usage</strong>, not along “every field the API returns”.
</div>

<SessionFooter />

---

<SectionDivider
  number="D"
  title="Dependency Inversion"
  subtitle="Depend on abstractions — invert ownership of details."
/>

---

<!--
DIP definition.
-->

# D · Policy shouldn't own the HTTP client

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Classic idea</div>
    <p class="!m-0 text-sm mb-slate">
      High-level policy depends on abstractions.
      Low-level details (fetch, SDK, storage) plug in.
    </p>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">In React</div>
    <ul class="mb-list">
      <li>Inject fetchers / clients into hooks</li>
      <li>UI talks to ports, not <code>axios</code> directly</li>
      <li>Tests swap fakes without rewriting UI</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Smell: every screen imports <code>api.ts</code> and hard-codes endpoints + headers.
</div>

<SessionFooter />

---

<!--
DIP bad.
-->

# D · Bad — hard-wired details

```tsx
function useOrders() {
  const [orders, setOrders] = useState([])
  useEffect(() => {
    fetch('https://api.shop.com/orders', {
      headers: { Authorization: `Bearer ${localStorage.token}` },
    }).then(r => r.json()).then(setOrders)
  }, [])
  return orders
}
```

<div class="mb-card mt-3 text-sm mb-slate">
  Hook owns transport, auth, URL, and parsing. Swapping API or testing needs surgery.
</div>

<SessionFooter />

---

<!--
DIP good.
-->

# D · Good — inject the dependency

```tsx
type OrdersApi = { list: () => Promise<Order[]> }

function useOrders(api: OrdersApi) {
  const [orders, setOrders] = useState<Order[]>([])
  useEffect(() => { api.list().then(setOrders) }, [api])
  return orders
}

// app composition root
const api = createOrdersApi(httpClient)
function OrdersPage() {
  const orders = useOrders(api)
  return <OrderList orders={orders} />
}
```

<div class="mb-callout mt-3">
  UI/hook depend on <code>OrdersApi</code>. HTTP is a detail provided at the edge.
</div>

<SessionFooter />

---

<!--
DIP when + pitfalls.
-->

# D · When it applies · Pitfalls

<div class="mb-two mt-3">
  <div class="mb-card">
    <div class="mb-kicker">Apply when</div>
    <ul class="mb-list">
      <li>You need tests without network</li>
      <li>Environments differ (mock, staging, prod)</li>
      <li>Multiple backends behind one UI flow</li>
    </ul>
  </div>
  <div class="mb-card-pink">
    <div class="mb-kicker">Pitfalls</div>
    <ul class="mb-list">
      <li>DI theater — interfaces for every line</li>
      <li>Passing 12 deps through every component</li>
      <li>Hiding globals in Context “just because”</li>
    </ul>
  </div>
</div>

<div class="mb-callout mt-4">
  Invert at <strong>boundaries</strong> (data, device, third parties) — not inside every button.
</div>

<SessionFooter />

---

<!--
How principles overlap.
-->

# How the letters work together

<div class="mt-3 space-y-2 text-sm">
  <div class="mb-card flex gap-3 items-start">
    <strong class="mb-pink w-8">S</strong>
    <span class="mb-slate">keeps a module about one change axis — often enables O/I/D</span>
  </div>
  <div class="mb-card flex gap-3 items-start">
    <strong class="mb-pink w-8">O</strong>
    <span class="mb-slate">needs stable cores (S) and extension points (I)</span>
  </div>
  <div class="mb-card flex gap-3 items-start">
    <strong class="mb-pink w-8">L</strong>
    <span class="mb-slate">protects callers when you swap extensions or fakes (O/D)</span>
  </div>
  <div class="mb-card flex gap-3 items-start">
    <strong class="mb-pink w-8">I</strong>
    <span class="mb-slate">stops fat contracts from coupling everything to everything</span>
  </div>
  <div class="mb-card flex gap-3 items-start">
    <strong class="mb-pink w-8">D</strong>
    <span class="mb-slate">lets details move without rewriting policy modules</span>
  </div>
</div>

<SessionFooter />

---

<!--
Smell → principle cheat sheet.
-->

# Smell → principle cheat sheet

<div class="mt-3 grid grid-cols-1 gap-2 text-sm">
  <div class="mb-card flex justify-between gap-4">
    <span>God component / hook does fetch + UI + format</span>
    <strong class="mb-pink">→ S</strong>
  </div>
  <div class="mb-card flex justify-between gap-4">
    <span>Endless <code>type</code> / <code>variant</code> switches</span>
    <strong class="mb-pink">→ O</strong>
  </div>
  <div class="mb-card flex justify-between gap-4">
    <span>“Drop-in” breaks callers' assumptions</span>
    <strong class="mb-pink">→ L</strong>
  </div>
  <div class="mb-card flex justify-between gap-4">
    <span>Mega props / mega Context</span>
    <strong class="mb-pink">→ I</strong>
  </div>
  <div class="mb-card flex justify-between gap-4">
    <span>UI hard-codes fetch / SDK / storage</span>
    <strong class="mb-pink">→ D</strong>
  </div>
</div>

<SessionFooter />

---

<!--
Readiness checklist.
-->

# Workshop checklist

<div class="mt-2 max-w-3xl">
  <Checklist
    :items="[
      'I can spot a god component and name its change axes',
      'I can replace a variant switch with composition or a registry',
      'I can state a props/hook contract and keep substitutes honest',
      'I can slim a props bag or split a bloated Context',
      'I can inject a data dependency for tests or environments',
      'I know when NOT to abstract yet',
    ]"
  />
</div>

<p class="mt-2 text-xs mb-muted">Use on a real PR — pick one smell and apply one letter.</p>

<SessionFooter />

---
layout: center
class: text-center
---

<!--
Close. Principles serve change, not purity.
-->

<div class="flex justify-center mb-6">
  <BrandLogo />
</div>

# Design for change,<br /><span class="mb-pink">not for dogma</span>

<p class="mt-6 mb-lead mx-auto !text-center">
  SOLID in React is composition discipline.
  Start from a smell, apply one principle, ship a safer module.
</p>

<div class="mb-rule mx-auto mt-6 mb-6" />

<p class="mb-kicker">MentorBridge</p>
<p class="text-sm mb-muted mt-2">Guiding your journey to success</p>
<p class="text-sm mt-3">
  <a class="mb-pink font-semibold" href="https://mentorbridge.in/" target="_blank">mentorbridge.in</a>
</p>
