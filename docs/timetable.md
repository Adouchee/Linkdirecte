# Timetable

The timetable module allows fetching and analyzing the student's schedule.

## Data Fetching

### `getTimetable`

Fetches the student's timetable for a given date range.

```typescript
function getTimetable(options?: {
  startDate?: string | Date;
  endDate?: string | Date;
  raw?: boolean;
  explain?: boolean;
}): Promise<TimetableResult>
```

- `startDate`: Defaults to today.
- `endDate`: Defaults to `startDate`.

#### Example

```typescript
import { getTimetable } from "linkdirecte";

const result = await getTimetable({
  startDate: "2025-09-01",
  endDate: "2025-09-07"
});

result.timetable.forEach(entry => {
  console.log(`${entry.subjectLabel} at ${entry.room} with ${entry.teacherName}`);
});
```

### `getTimetableIcalUrl`

Retrieves the URL for the student's timetable in iCal format for integration with external calendars (Google Calendar, Apple Calendar, etc.).

```typescript
function getTimetableIcalUrl(): Promise<string>
```

#### Example

```typescript
import { getTimetableIcalUrl } from "linkdirecte";

const url = await getTimetableIcalUrl();
// Subscribe to this URL in your calendar app
console.log(url);
```

---

## Types

### `TimetableResult`
```typescript
interface TimetableResult {
  timetable: TimetableEntry[];
}
```

### `TimetableEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the entry. |
| `subjectCode` | `string` | Subject code. |
| `subjectLabel` | `string` | Subject name. |
| `teacherName` | `string` | Teacher name (optional). |
| `room` | `string` | Room name (optional). |
| `group` | `string` | Student group (optional). |
| `startDate` | `Date` | Start time. |
| `endDate` | `Date` | End time. |
| `isCancelled` | `boolean` | Whether the class is cancelled (optional). |
| `isDetention` | `boolean` | Whether it's a detention (optional). |
| `isExempted` | `boolean` | Whether the student is exempted (optional). |
| `color` | `string` | Calendar color code (optional). |
