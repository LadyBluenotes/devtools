---
title: Configuration
id: configuration
---

TanStack Devtools configuration is split between `TanStackDevtools` and `EventClient`. `TanStackDevtools` uses two separate config objects: `config` controls shell UI behavior, and `eventBusConfig` controls event transport settings.

> [!IMPORTANT]
> All options are optional unless marked as required.

## Configuration model

Configuration usually starts with shell behavior (`config`) and then adds transport settings (`eventBusConfig`) when server bus features are enabled.

- `config`: trigger behavior, panel behavior, hotkeys, and theme.
- `eventBusConfig`: client connection settings for the server event bus.

The separation is intentional. UI behavior and network behavior are configured independently.

## `config` options

- `defaultOpen`: Sets the initial panel state to open instead of closed.

```ts
{ defaultOpen: boolean }
```

- `hideUntilHover`: Keeps the trigger hidden until the pointer reaches its hover region.

```ts
{ hideUntilHover: boolean }
```

- `position`: Controls which screen edge or corner the floating trigger is anchored to.

```ts
{
  position:
    | 'top-left'
    | 'top-right'
    | 'bottom-left'
    | 'bottom-right'
    | 'middle-left'
    | 'middle-right'
}
```

- `panelLocation`: Controls whether the panel opens from the top edge or bottom edge.

```ts
{ panelLocation: 'top' | 'bottom' }
```

- `openHotkey`: Defines the key combination that opens and closes the panel.

```ts
type ModifierKey = 'Alt' | 'Control' | 'Meta' | 'Shift' | 'CtrlOrMeta'
type KeyboardKey = ModifierKey | (string & {})

{ openHotkey: Array<KeyboardKey> }
```

- `inspectHotkey`: Defines the key combination that turns source inspection on and off.

```ts
type ModifierKey = 'Alt' | 'Control' | 'Meta' | 'Shift' | 'CtrlOrMeta'
type KeyboardKey = ModifierKey | (string & {})

{ inspectHotkey: Array<KeyboardKey> }
```

- `requireUrlFlag`: Requires a matching query parameter before Devtools is rendered.

```ts
{ requireUrlFlag: boolean }
```

- `urlFlag`: Sets the query parameter name checked when `requireUrlFlag` is enabled.

```ts
{ urlFlag: string }
```

- `theme`: Forces the shell theme to `light` or `dark`.

```ts
{ theme: 'light' | 'dark' }
```

- `triggerHidden`: Removes the floating trigger button from the UI.

```ts
{ triggerHidden?: boolean }
```

- `customTrigger`: Replaces the default trigger by rendering custom trigger UI into the provided element.

```ts
{ customTrigger?: (el: HTMLElement, props: { theme: 'light' | 'dark' }) => void }
```

## `eventBusConfig` options

- `connectToServerBus`: Connects the client event bus to the Vite server event bus.

```ts
{ connectToServerBus?: boolean }
```

- `debug`: Enables diagnostic logging for client-to-server event transport.

```ts
{ debug?: boolean }
```

- `port`: Sets the server bus port used for the transport connection.

```ts
{ port?: number }
```

- `host`: Sets the server bus host used for the transport connection.

```ts
{ host?: string }
```

- `protocol`: Sets the transport protocol used to connect (`http` or `https`).

```ts
{ protocol?: 'http' | 'https' }
```

## Configuration object example

Example values for both `config` and `eventBusConfig`:

```ts
const devtoolsConfig = {
  config: {
    hideUntilHover: true,
    position: 'bottom-right' as const,
    panelLocation: 'bottom' as const,
    openHotkey: ['Control', '~'] as const,
  },
  eventBusConfig: {
    connectToServerBus: true,
    host: 'localhost',
    port: 4206,
    protocol: 'http' as const,
    debug: false,
  },
}
```

For framework-specific mounting, see [Quick Start](./quick-start).

## `EventClient` configuration

Use `EventClient` when building custom plugins that emit or subscribe to devtools events.

- `pluginId` (required): Sets the plugin namespace prefix used for emitted events.

```ts
{ pluginId: string }
```

- `debug`: Enables diagnostic logging for `EventClient` emit and subscribe activity.

```ts
{ debug?: boolean }
```

- `enabled`: Turns `EventClient` event emit/subscribe behavior on or off.

```ts
{ enabled?: boolean }
```

- `reconnectEveryMs`: Sets how often `EventClient` retries server bus connection attempts.

```ts
{ reconnectEveryMs?: number }
```

```ts
import { EventClient } from '@tanstack/devtools-event-client'

type EventMap = {
  'custom-state': { state: string }
}

class CustomEventClient extends EventClient<EventMap> {
  constructor() {
    super({
      pluginId: 'custom-devtools',
      debug: true,
      enabled: true,
      reconnectEveryMs: 300,
    })
  }
}

export const customEventClient = new CustomEventClient()
```

For a complete plugin implementation, see [Custom Plugins](./custom-plugins).
