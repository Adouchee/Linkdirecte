# Grades & Statistics

The grades module provides functions to fetch student grades.

## Data Fetching

### `getGrades`

Fetches the student's grades for the current or a specific period.

```typescript
function getGrades(options?: {
  periodId?: string;
  raw?: boolean;
  explain?: boolean;
}): Promise<GradesResult>
```

- `periodId`: (Optional) ID of the period (e.g., "A001").
- `raw`: If true, returns the raw API response.
- `explain`: If true, returns debug information along with the response.

#### Returns

A `GradesResult` object containing:
- `grades`: Array of `GradeEntry` objects (flat list of all grades).
- `subjects`: Array of `SubjectEntry` objects (grades grouped by subject).
- `averages`: Array of subject averages (code, average, classAverage).
- `periods`: Array of available periods (code, label).

#### Example

```typescript
import { getGrades } from "linkdirecte";

const result = await getGrades();

console.log(`You have ${result.grades.length} grades across ${result.subjects.length} subjects.`);

result.subjects.forEach(subject => {
  console.log(`${subject.subjectLabel}: ${subject.average ?? "N/A"} (class avg: ${subject.classAverage ?? "N/A"})`);
});
```

---

## Types

### `GradesResult`
```typescript
interface GradesResult {
  grades: GradeEntry[];
  subjects: SubjectEntry[];
  averages?: Array<{
    subjectCode: string;
    average: number;
    classAverage?: number;
  }>;
  periods?: Array<{ code: string; label: string }>;
}
```

### `GradeEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `value` | `string` | The grade value (e.g., "15"). |
| `outOf` | `string` | Maximum possible value (e.g., "20"). |
| `coefficient` | `number` | Coefficient of the grade. |
| `isLetter` | `boolean` | Whether the grade is a letter grade. |
| `isTest` | `boolean` | Whether it was a test (vs continuous assessment). |
| `date` | `Date` | Date of the grade. |
| `subjectCode` | `string` | Subject code. |
| `subjectLabel` | `string` | Subject name. |
| `periodCode` | `string` | Period code. |
| `entryDate` | `Date` | Date the grade was entered. |
| `teacherName` | `string` | Teacher name (optional). |
| `testType` | `string` | Type of test (optional). |

### `SubjectEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `subjectCode` | `string` | Subject code. |
| `subjectLabel` | `string` | Subject name. |
| `coefficient` | `number` | Overall coefficient. |
| `teacherName` | `string` | Teacher name (optional). |
| `grades` | `GradeEntry[]` | Grades for this subject. |
| `average` | `number` | Student's average (optional). |
| `classAverage` | `number` | Class average (optional). |
