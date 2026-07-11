# Homework

The homework module manages the student's "Cahier de Texte" (assignment diary).

## Functions

### `getHomework`

Fetches the list of homework assignments.

```typescript
function getHomework(options?: {
  withContent?: boolean;
  raw?: boolean;
  explain?: boolean;
}): Promise<Record<string, HomeworkEntry[]>>
```

- `withContent`: If true, automatically fetches the detailed description and attachments for all assignments by calling `getHomeworkForDate` for each day.

#### Returns
A record where keys are dates (`YYYY-MM-DD`) and values are arrays of `HomeworkEntry`.

### `HomeworkEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the assignment. |
| `subjectLabel` | `string` | Name of the subject. |
| `subjectCode` | `string` | Code of the subject. |
| `teacherName` | `string` | Teacher name (optional). |
| `givenOn` | `Date` | Date when the homework was assigned. |
| `forDate` | `Date` | Date when the homework is due. |
| `isDone` | `boolean` | Whether the student marked it as done. |
| `submitOnline` | `boolean` | Whether an online submission is required (optional). |
| `content` | `string` | Base64 decoded HTML content (only with `withContent: true`). |
| `documentsToDo` | `Array<{ id: number; label: string; url?: string }>` | Attachments (optional). |

### `getHomeworkForDate`

Fetches detailed homework information for a specific day.

```typescript
function getHomeworkForDate(date: string | Date, options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<HomeworkEntry[]>
```

- `date`: A `Date` object or a string in `YYYY-MM-DD` format.

### `markAsDone`

Marks one or more homework assignments as completed.

```typescript
function markAsDone(homeworkIds: number[], options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<MarkAsDoneResult>
```

#### Returns
A `MarkAsDoneResult` object: `{ success: boolean }`.

#### Example

```typescript
import { getHomework, markAsDone } from "linkdirecte";

const homework = await getHomework({ withContent: true });
// ... find assignment ID ...
await markAsDone([123456]);
```

## Resilience

If the `offlineQueue` is enabled in the global configuration, `markAsDone` calls made while offline will be queued and can be synchronized later using `offlineQueue.flush()`.

---

## Types

### `HomeworkResult`
```typescript
interface HomeworkResult {
  [date: string]: HomeworkEntry[]; // Keys are YYYY-MM-DD dates
}
```

### `MarkAsDoneResult`
```typescript
interface MarkAsDoneResult {
  success: boolean;
}
```
