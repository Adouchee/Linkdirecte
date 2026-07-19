<p align="center">
  <picture><source media="(prefers-color-scheme: light)" srcset="https://shieldcn.dev/header/glow.svg?title=Timetable&amp;subtitle=Learn+how+to+get+the+timetable+with+Linkdirecte.&amp;logo=lu%3ACalendar&amp;mode=light&amp;theme=blue&amp;align=left" /><img alt="Timetable | Learn how to get the timetable with Linkdirecte." src="https://shieldcn.dev/header/glow.svg?title=Timetable&amp;subtitle=Learn+how+to+get+the+timetable+with+Linkdirecte.&amp;logo=lu%3ACalendar&amp;mode=dark&amp;theme=blue&amp;align=left" /></picture>
</p>

The Timetable module fetches, parses, and normalizes the student's calendar. It also allows you to retrieve a subscription URL for external calendar systems (Apple Calendar, Google Calendar, Outlook, etc.).

---

## 🚀 Getting Started

Let's fetch the student's timetable classes for the upcoming week:

```typescript
import { getTimetable } from "linkdirecte";

const schedule = await getTimetable({
  startDate: new Date(), // Start today
  endDate: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), // Next 7 days
});

console.log(`Loaded ${schedule.timetable.length} timetable events.`);

schedule.timetable.forEach(classSession => {
  const status = classSession.isCancelled ? "❌ CANCELLED" : "✅ ACTIVE";
  console.log(`\n[${status}] Subject: ${classSession.subjectLabel}`);
  console.log(`  Time: ${classSession.startDate.toLocaleTimeString()} - ${classSession.endDate.toLocaleTimeString()}`);
  console.log(`  Room: ${classSession.room || "No room assigned"}`);
  console.log(`  Teacher: ${classSession.teacherName || "Unspecified teacher"}`);
});
```

Let's fetch an iCal subscription link:

```typescript
import { getTimetableIcalUrl } from "linkdirecte";

const calendarUrl = await getTimetableIcalUrl();
console.log("Add this link to your Google or Apple Calendar subscription feed:");
console.log(calendarUrl);
```

---

## 📖 API Reference

### `getTimetable`

Retrieves a chronologically ordered array of scheduled class sessions.

```typescript
function getTimetable(options?: {
  startDate?: string | Date;
  endDate?: string | Date;
  raw?: boolean;
  explain?: boolean;
}): Promise<TimetableResult>
```

#### Parameters

- `options` *(optional)*:
  - `startDate` *(string | Date)*: The beginning of the timetable window. Can be a standard JavaScript `Date` object or an ISO-style date string (`"YYYY-MM-DD"`). Defaults to today.
  - `endDate` *(string | Date)*: The end of the timetable window. Defaults to the same value as `startDate`.
  - `raw` *(boolean)*: If set to `true`, disables key translation and type conversions.
  - `explain` *(boolean)*: Includes networking and caching metrics under a `_debug` property.

---

### `getTimetableIcalUrl`

Retrieves the URL for the student's timetable in iCal format for integration with external calendar systems.

```typescript
function getTimetableIcalUrl(): Promise<string>
```

---

## 📋 Example Response

Below is an example of the resolved `TimetableResult` payload returned by `getTimetable()`:

```typescript
{
  timetable: [
    {
      id: 998811,
      subjectCode: "MATH",
      subjectLabel: "Mathématiques",
      teacherName: "Mme. Dupont",
      room: "Salle 204",
      group: "Groupe A",
      startDate: new Date("2026-03-16T08:00:00.000Z"),
      endDate: new Date("2026-03-16T09:55:00.000Z"),
      isCancelled: false,
      isDetention: false,
      isExempted: false,
      color: "#FF5733"
    },
    {
      id: 998812,
      subjectCode: "HIST",
      subjectLabel: "Histoire-Géographie",
      teacherName: "M. Bertrand",
      room: "Salle 102",
      startDate: new Date("2026-03-16T10:10:00.000Z"),
      endDate: new Date("2026-03-16T12:00:00.000Z"),
      isCancelled: true, // This class is cancelled!
      isDetention: false,
      isExempted: false,
      color: "#33C1FF"
    }
  ]
}
```

---

## 🗂️ Type Definitions

### `TimetableResult`

```typescript
interface TimetableResult {
  timetable: TimetableEntry[];
}
```

### `TimetableEntry`

Provides a standardized structure for school calendar classes:

| Property | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the schedule slot. |
| `subjectCode` | `string` | Short identifying code of the subject. |
| `subjectLabel` | `string` | Full display name of the subject (e.g. `"Mathématiques"`). |
| `startDate` | `Date` | Start date and time of the class. |
| `endDate` | `Date` | End date and time of the class. |
| `teacherName` | `string` *(optional)* | Name of the teacher hosting this class session. |
| `room` | `string` *(optional)* | Classroom label or number. |
| `group` | `string` *(optional)* | Specific classroom group assigned. |
| `isCancelled` | `boolean` *(optional)* | `true` if the session has been cancelled. |
| `isDetention` | `boolean` *(optional)* | `true` if this session represents a detention period. |
| `isExempted` | `boolean` *(optional)* | `true` if the student is exempted from attending this class. |
| `color` | `string` *(optional)* | Calendar color hex code (provided by the school's workspace). |
