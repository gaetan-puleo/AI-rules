
# Frontend Project Convention

## General Rules

* Follow all rules strictly.
* Organize code by **business feature** (feature-first).
* Each feature lives in its **own folder**.
* Group related features into a **feature group** (theme-based folder).
* Use `shared/` inside `src/features/` for common code **shared across all feature groups**.
* Place components, hooks, and pages in `src/front-end/`.
* All code lives under `src/`.
* Remove unnecessary comments.
* Exclude example or placeholder comments from committed code.

---

## Feature Groups

* A **feature group** is a set of related features (e.g. `messaging`, `settings`, `calendar`).
* Located in:

  ```
  src/features/[featureGroup]/
  ```
* Common/shared logic across groups goes in:

  ```
  src/features/shared/
  ```

---

### Example Structure (Messaging)

```
src/
  features/
    messaging/
      sendMessage/
        sendMessage.reducer.ts
        sendMessage.selector.ts
        sendMessage.usecase.ts
        sendMessage.usecase.spec.ts
        sendMessage.events.ts
      getMessageThread/
        getMessageThread.reducer.ts
        getMessageThread.selector.ts
        getMessageThread.usecase.ts
        getMessageThread.usecase.spec.ts
        getMessageThread.events.ts
    shared/
      gateways/
        interfaces/
          messaging.gateway.ts
        implementations/
          ApiMessaging.gateway.ts
          FakeMessaging.gateway.ts
      entities/
        message.entity.ts
        thread.entity.ts
        notification.entity.ts
```

---

## Feature File Types

* `*.reducer.ts` – Redux slice logic
* `*.selector.ts` – State selectors
* `*.usecase.ts` – Business logic (typically async thunk)
* `*.usecase.spec.ts` – Unit tests for use cases
* `*.events.ts` – Redux actions
* `*.entity.ts` – TypeScript entities in `src/features/shared/entities/`

---

## Store Setup

* `src/store/init-store.ts`:

  * Exports `initStore`, `AppStore`, `AppDispatch`, `AppState`
  * Used to inject gateway implementations via `options.gateways`

---

## 🔌 Dependency Injection

* `src/di/gateways.ts`:

  * Exports:

    * `Gateways` – all gateway implementations
    * `PartialGateways` – for unit tests or partial injection

---

## ⚛️ React Structure

* Located in `src/front-end/`:

  * `hooks/`
  * `components/`
  * `pages/`
  * Other (router, theme)

---

## ⚙️ Tech Stack

* TypeScript
* Redux Toolkit (RTK)
* Tanstack Query (used only inside gateways)
* React
* Shadcn UI
* Vite
* Vitest
* Wouter
* Tailwind CSS (when needed)
* [... updates this list with your own tech stack]

---

## Code Conventions

### ✅ React Components

```tsx
// Good
export function Component(props: ComponentProps) {
  return <div>Hello</div>
}

// Bad
export const Component = (props: ComponentProps) => (<div>Hello</div>)
```

---

### ✅ Props Handling

```tsx
// Good
function Component(props: Props) {
  const { propName } = props
}

// Bad
function Component({ propName }: Props) {}
```

---

### ✅ Reducer Example

```ts
export const featureReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(actionStart, (state) => {
      state.status = 'loading'
    })
    .addCase(actionSuccess, (state, action) => {
      state.status = 'success'
      state.data = action.payload.data
    })
    .addCase(actionError, (state) => {
      state.status = 'failed'
    })
})
```

---

### ✅ Tests

```ts
describe('getMessageThread use-case', () => {
  let store: AppStore
  let fakeGateway: FakeMessagingGateway

  beforeEach(() => {
    fakeGateway = new FakeMessagingGateway()
    store = initStore({
      gateways: {
        messagingGateway: fakeGateway,
      },
    })
  })

  it('should load the thread successfully', async () => {
    await store.dispatch(getMessageThread('thread-123'))
    const state = store.getState()
    expect(state.messaging.getMessageThread.status).toBe('success')
  })
})
```

```ts
// ❌ Avoid
import { describe, expect, vi } from 'vitest'
```

---

### ✅ Use Case Example

```ts
export const sendMessage = (input: MessageInput) => {
  return async (dispatch, getState, { gateways }) => {
    dispatch(sendMessageIsSending())
    try {
      const result = await gateways.messagingGateway.sendMessage(input)
      dispatch(sendMessageSuccess(result))
    } catch {
      dispatch(sendMessageError())
    }
  }
}
```

---

## Other Rules

* Do **not** remove `// @ts-ignore` unless the cause is fixed.
* Do **not** commit commented-out code or unused example snippets.
* Keep test cases focused and clear.

