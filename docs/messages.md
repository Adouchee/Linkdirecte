# Messages

The messages module handles communication through the EcoleDirecte messaging system.

## Functions

### `getMessages`

Fetches the list of messages in the user's inbox or a specific folder.

```typescript
function getMessages(options?: GetMessagesOptions): Promise<MessagesResult>
```

#### `GetMessagesOptions`
- `folderId`: (Optional) ID of the folder to fetch.
- `withContent`: If true, automatically fetches the full content of every received message. Warning: this may result in many API calls.
- `raw`: If true, returns the raw API response.
- `explain`: If true, returns debug information.

#### Returns

A `MessagesResult` object containing:
- `messages.received`: Array of received `MessageEntry` objects.
- `messages.sent`: Array of sent `MessageEntry` objects.
- `messages.drafts`: Array of draft `MessageEntry` objects.

#### Example

```typescript
import { getMessages } from "linkdirecte";

const result = await getMessages({ folderId: 1 });
const unread = result.messages?.received?.filter(m => !m.isRead) ?? [];
console.log(`You have ${unread.length} unread messages.`);
```

### `MessageEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the message. |
| `subject` | `string` | Subject of the message. |
| `content` | `string` | Base64 encoded HTML content (only if `withContent` is used). |
| `fromName` | `string` | Sender name (optional). |
| `date` | `Date` | Reception or sending date. |
| `isRead` | `boolean` | Whether the message has been read. |
| `isAnswered` | `boolean` | Whether the message has been answered (optional). |
| `isTransferred` | `boolean` | Whether the message was forwarded (optional). |
| `canAnswer` | `boolean` | Whether the message can be replied to (optional). |

### `getMessage`

Fetches the full details and content of a specific message.

```typescript
function getMessage(id: number, options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<MessageEntry>
```

### `sendMessage`

Sends a new message.

```typescript
function sendMessage(data: SendMessageData, options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<{ success: boolean }>
```

#### `SendMessageData`
| Field | Type | Description |
| :--- | :--- | :--- |
| `subject` | `string` | The subject of the message. |
| `content` | `string` | The body of the message (plain text). |
| `destinataires` | `unknown[]` | An array of recipient objects. |

#### Example

```typescript
import { sendMessage } from "linkdirecte";

const result = await sendMessage({
  subject: "Question about homework",
  content: "Hello, I have a question regarding...",
  destinataires: [
    { id: 123, type: "P" } // Example recipient
  ]
});

if (result.success) {
  console.log("Message sent!");
}
```

---

## Types

### `MessagesResult`
```typescript
interface MessagesResult {
  messages?: {
    received?: MessageEntry[];
    sent?: MessageEntry[];
    drafts?: MessageEntry[];
  };
}
```

### `GetMessagesOptions`
```typescript
interface GetMessagesOptions {
  folderId?: number;
  withContent?: boolean;
  raw?: boolean;
  explain?: boolean;
}
```

## Related Information

- **Auto-Refresh**: Messages are automatically marked as read when fetched via `getMessage`.
- **Formatting**: The SDK handles the Base64 encoding of message content required by the EcoleDirecte API.
