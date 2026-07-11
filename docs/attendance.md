# Attendance & Life School

The attendance module provides information regarding student absences, delays, and other school life events.

## Functions

### `getAttendance`

Fetches the student's attendance records (absences, lateness, sanctions).

```typescript
function getAttendance(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<AttendanceResult>
```

#### Returns

An `AttendanceResult` object containing:
- `absences`: Array of absence `AttendanceEntry` objects.
- `delays`: Array of delay `AttendanceEntry` objects.
- `punishments`: Array of punishment `AttendanceEntry` objects.
- `attendance`: Array of general attendance entries.
- `settings`: Display settings.

### `AttendanceEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the entry. |
| `date` | `Date` | Date of the event. |
| `type` | `string` | Event type (e.g., `"Absence"`, `"Retard"`). |
| `subjectLabel` | `string` | Related subject (optional). |
| `isJustified` | `boolean` | Whether it has been justified (optional). |
| `justificationType` | `string` | Type of justification (optional). |
| `teacherName` | `string` | Teacher name (optional). |
| `licensePoints` | `number` | License points deducted (optional). |
| `studentId` | `number` | Student ID (optional). |
| `reason` | `string` | Reason for absence/delay (optional). |
| `justifiedOnline` | `boolean` | Whether it was justified online (optional). |
| `dontNeedJustification` | `boolean` | Whether justification is not required (optional). |
| `day` | `Date` | Day reference (optional). |

---

## Types

### `AttendanceResult`
```typescript
interface AttendanceResult {
  absences?: AttendanceEntry[];
  delays?: AttendanceEntry[];
  punishments?: AttendanceEntry[];
  attendance?: AttendanceEntry[];
  settings?: Record<string, unknown>;
}
```

#### Example

```typescript
import { getAttendance } from "linkdirecte";

const data = await getAttendance();
console.log(`Total absences: ${data.attendance.length}`);
```
