<p align="center">
  <picture><source media="(prefers-color-scheme: light)" srcset="https://shieldcn.dev/header/glow.svg?title=Messages&amp;subtitle=Learn+how+to+interact+with+mail+with+Linkdirecte.&amp;logo=lu%3AMessagesSquare&amp;mode=light&amp;theme=blue&amp;align=left" /><img alt="Messages | Learn how to interact with mail with Linkdirecte." src="https://shieldcn.dev/header/glow.svg?title=Messages&amp;subtitle=Learn+how+to+interact+with+mail+with+Linkdirecte.&amp;logo=lu%3AMessagesSquare&amp;mode=dark&amp;theme=blue&amp;align=left" /></picture>
</p>

The Messages module connects you directly to the school's communication channels. It handles reading received mail, viewing attachments, sending replies, and organizing drafts.

---

## 🚀 Getting Started

Let's fetch the list of received messages and read the most recent unread email.

```typescript
import { getMessages, getMessage } from "linkdirecte";

// Fetch the inbox
const inbox = await getMessages();
const unreadEmails = inbox.messages?.received?.filter(msg => !msg.isRead) ?? [];

console.log(`You have ${unreadEmails.length} unread message(s).`);

if (unreadEmails.length > 0) {
  const firstUnread = unreadEmails[0];
  console.log(`Reading: "${firstUnread.subject}" from ${firstUnread.fromName}...`);

  // Load full content (EcoleDirecte handles base64-decoded HTML automatically inside the SDK!)
  const fullDetail = await getMessage(firstUnread.id);
  console.log("\nMessage body:");
  console.log(fullDetail.content);
}
```

---

## 📖 API Reference

### `getMessages`

Retrieves a simplified directory of messages.

```typescript
function getMessages(options?: GetMessagesOptions): Promise<MessagesResult>
```

#### Parameters

- `options` *(optional)*:
  - `folderId` *(number)*: Pass a folder ID to retrieve messages from custom archives or folders.
  - `withContent` *(boolean)*: If set to `true`, automatically makes individual parallel queries to retrieve the content bodies for all returned messages. Defaults to `false`.
  - `raw` *(boolean)*: Returns original unmodified JSON.
  - `explain` *(boolean)*: Adds HTTP caching and retry diagnostics to `_debug`.

---

### `getMessage`

Loads the detailed envelope and content body for a single message.

```typescript
function getMessage(
  id: number,
  options?: { raw?: boolean; explain?: boolean }
): Promise<MessageEntry>
```

> **Note**: Opening a message with `getMessage` automatically marks it as **read** on EcoleDirecte's servers.

---

### `sendMessage`

Composes and sends a new message.

```typescript
function sendMessage(
  data: SendMessageData,
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

#### Example

```typescript
import { sendMessage } from "linkdirecte";

const result = await sendMessage({
  subject: "Absence Excuse",
  content: "Hello, this is to inform you that...",
  destinataires: [
    { id: 98765, type: "P" } // Target recipient object
  ]
});

if (result.success) {
  console.log("Message sent successfully!");
}
```

---

## 📋 Example Response

Below is an example of the resolved `MessagesResult` payload returned by `getMessages()`:

```typescript
{
  messages: {
    received: [
      {
        id: 746352,
        subject: "Information regarding parent-teacher meetings",
        date: new Date("2026-03-12T14:20:00.000Z"),
        isRead: false,
        fromName: "M. le Proviseur",
        isAnswered: false,
        isTransferred: false,
        canAnswer: false
      },
      {
        id: 746351,
        subject: "Class schedule update",
        content: "<p>Hello, please note that the math class next Friday has been moved to room 204...</p>",
        date: new Date("2026-03-10T08:30:00.000Z"),
        isRead: true,
        fromName: "Mme. Dupont",
        isAnswered: true,
        isTransferred: false,
        canAnswer: true
      }
    ],
    sent: [
      {
        id: 746399,
        subject: "RE: Question on homework #3",
        date: new Date("2026-03-11T16:00:00.000Z"),
        isRead: true,
        fromName: "Jane Doe",
        isAnswered: false,
        isTransferred: false,
        canAnswer: true
      }
    ],
    drafts: []
  }
}
```

And below is an example of a single `MessageEntry` returned by `getMessage(id)`:

```typescript
{
  id: 746352,
  subject: "Information regarding parent-teacher meetings",
  content: "Dear students and parents,<br><br>The next parent-teacher meeting will take place on March 25th at 17:00. Please schedule your time slots accordingly.<br><br>Best regards,<br>Administration",
  date: new Date("2026-03-12T14:20:00.000Z"),
  isRead: true, // Marked as read upon opening
  fromName: "M. le Proviseur",
  isAnswered: false,
  isTransferred: false,
  canAnswer: false
}
```

---

## 🗂️ Type Definitions

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

### `MessageEntry`

| Property | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the message. |
| `subject` | `string` | Subject header of the email. |
| `date` | `Date` | Date and time the email was received/sent. |
| `isRead` | `boolean` | `true` if the message has been read. |
| `content` | `string` *(optional)* | Fully decoded HTML or text of the message body (available when fetching via `getMessage` or `withContent: true`). |
| `fromName` | `string` *(optional)* | Readable name of the sender. |
| `isAnswered` | `boolean` *(optional)* | `true` if this message has already been replied to. |
| `isTransferred` | `boolean` *(optional)* | `true` if the message was forwarded. |
| `canAnswer` | `boolean` *(optional)* | Whether replies are permitted for this message. |

### `SendMessageData`

```typescript
interface SendMessageData {
  subject: string;
  content: string;
  destinataires: Array<{
    id: number;
    type: string; // e.g. "P" (Teacher), "E" (Student)
    [key: string]: any;
  }>;
}
```
