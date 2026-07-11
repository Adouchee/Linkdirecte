# Timeline & Correlations

The timeline module provides access to the student's activity feed and advanced data analysis between different modules.

## Data Fetching

### `getTimeline`

Fetches the student's personal timeline (new grades, homework assigned, etc.).

```typescript
function getTimeline(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<TimelineEntry[]>
```

### `getCommonTimeline`

Fetches the school's general timeline/news feed.

```typescript
function getCommonTimeline(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<TimelineEntry[]>
```

### `TimelineEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the entry. |
| `elementType` | `string` | Type of event (e.g., `"Note"`, `"Devoir"`). |
| `title` | `string` | Main title (optional). |
| `subtitle` | `string` | Secondary information (optional). |
| `content` | `string` | Details or description (optional). |
| `creationDate` | `Date` | Creation date. |
| `subjectLabel` | `string` | Subject name (optional). |
| `teacherName` | `string` | Teacher name (optional). |

#### Example

```typescript
import { getTimeline } from "linkdirecte";

const entries = await getTimeline();
entries.forEach(entry => {
  console.log(`[${entry.elementType}] ${entry.title ?? entry.subtitle ?? "No title"}`);
});
```

### `getCommonTimeline`

Fetches the school's general timeline/news feed.

```typescript
function getCommonTimeline(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<TimelineEntry[]>
```

#### Example

```typescript
import { getCommonTimeline } from "linkdirecte";

const commonEntries = await getCommonTimeline();
console.log(`${commonEntries.length} shared entries.`);
```

---

## Data Analysis

### `correlate`

Analyzes historical data across modules to find patterns and insights.

```typescript
function correlate(): Promise<Correlation[]>
```

The function currently analyzes relationships such as:
- **Grade Trends** (`gradeTrend`): Performance evolution over time per subject.
- **Day of Week Patterns** (`gradeVsDayOfWeek`): Whether certain days of the week consistently yield better or worse grades.

Only subjects with at least 5 grades are analyzed.

#### Returns

An array of `Correlation` objects.

#### `Correlation`
| Field | Type | Description |
| :--- | :--- | :--- |
| `type` | `CorrelationType` | The type of analysis performed. |
| `subject` | `string` | The subject being analyzed. |
| `finding` | `string` | A summary of the insight. |
| `data` | `Record<string, number>` | Numerical data supporting the finding. |
| `confidence` | `number` | Reliability score (0 to 1). |
| `observations` | `number` | Number of data points analyzed. |

#### `CorrelationType`
```typescript
type CorrelationType =
  | "gradeVsPresence"
  | "gradeVsDayOfWeek"
  | "gradeVsTimeOfDay"
  | "homeworkVsGrade"
  | "gradeTrend";
```

#### Example

```typescript
import { correlate } from "linkdirecte";

const insights = await correlate();
insights.forEach(insight => {
  if (insight.type === "gradeVsDayOfWeek") {
    console.log(`In ${insight.subject}, you perform best on ${findBestDay(insight.data)}`);
  }
});
```
