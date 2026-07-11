# Event Listening & Polling

The listen module allows you to react to real-time updates from EcoleDirecte by polling the API and emitting events when changes are detected.

## Functions

### `startPolling`

Starts the background polling process.

```typescript
function startPolling(config?: PollingConfig): void
```

- `config.interval`: The polling interval in milliseconds. Defaults to `60,000` (1 minute).

### `stopPolling`

Stops the background polling process.

```typescript
function stopPolling(): void
```

---

## Event Management

### `on`

Subscribes to a specific event.

```typescript
function on(event: string, handler: (data: any) => void): () => void
```

- Returns an unsubscribe function.

### `off`

Unsubscribes from a specific event.

```typescript
function off(event: string, handler: (data: any) => void): void
```

---

## Configuration

### `PollingConfig`
```typescript
interface PollingConfig {
  interval?: number; // Polling interval in ms. Default: 60,000 (1 minute).
}
```

| Event | Description | Data |
| :--- | :--- | :--- |
| `newGrade` | Emitted when a new grade is detected. | The grade object. |
| `newMessage` | Emitted when a new message is received. | The message object. |
| `newTimelineEntry` | Emitted when a new entry appears in the timeline. | The timeline entry object. |
| `pollingError` | Emitted when an error occurs during polling. | The error object. |

#### Example

```typescript
import { startPolling, on } from "linkdirecte";

on("newGrade", (grade) => {
  console.log(`New grade: ${grade.value}/${grade.outOf} in ${grade.subjectLabel}`);
});

on("newMessage", (msg) => {
  console.log(`New message from ${msg.fromName}: ${msg.subject}`);
});

startPolling({ interval: 30000 }); // Poll every 30 seconds
```

### Using the Unsubscribe Function

The `on` function returns an unsubscribe function for clean teardown:

```typescript
const unsubscribe = on("newGrade", (grade) => {
  console.log("New grade:", grade);
});

// Later, stop listening:
unsubscribe();
```
