# Forms & QCMs

The forms module manages online quizzes (QCMs) and assessments.

## Functions

### `getQcms`

Lists all QCM associations available for the student.

```typescript
function getQcms(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<QcmsResult>
```

#### Returns
A `QcmsResult` object with an `associations` array of `QcmEntry` objects.

### `QcmEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Entry ID. |
| `qcmId` | `number` | ID of the questionnaire. |
| `subjectLabel` | `string` | Subject name (optional). |
| `title` | `string` | Title of the QCM (optional). |
| `teacherName` | `string` | Teacher name (optional). |
| `date` | `Date` | Date (optional). |
| `status` | `string` | Status (optional). |

#### Example

```typescript
import { getQcms } from "linkdirecte";

const result = await getQcms();
result.associations?.forEach(qcm => {
  console.log(`${qcm.title ?? "QCM"} (${qcm.status ?? "unknown status"})`);
});
```

### `getQcmDetail`

Retrieves the detailed structure and questions of a specific QCM.

```typescript
function getQcmDetail(
  idQcm: number,
  idAssociation: number,
  options?: { raw?: boolean; explain?: boolean }
): Promise<QcmDetailResult>
```

#### Returns

A `QcmDetailResult` containing the question structure:
- `qcmId`: The QCM ID.
- `questions`: Array of questions, each with `id`, `label`, and `choices` (array of `{ id, label }`).

#### Example

```typescript
import { getQcmDetail } from "linkdirecte";

const detail = await getQcmDetail(456, 789);
detail.questions.forEach(q => {
  console.log(`Q: ${q.label}`);
  q.choices.forEach(c => console.log(`  - ${c.label}`));
});
```

### `updateQcmStatus`

Updates the status of a QCM (e.g., starting or finishing the assessment).

```typescript
function updateQcmStatus(
  idQcm: number,
  idAssociation: number,
  idParticipant: number,
  action: "updateStartDate" | "updateEndDate",
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

- `idParticipant`: The participant ID obtained from the QCM detail response.

### `submitQcmAnswer`

Submits an answer for a specific question within a QCM.

```typescript
function submitQcmAnswer(
  params: {
    idQcm: number;
    idAssociation: number;
    idParticipant: number;
    idReponse: number;
    idQuestion: number;
    choiceIds: number[];
  },
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

- `idParticipant`: The participant ID obtained from the QCM detail response.

---

## Types

### `QcmsResult`
```typescript
interface QcmsResult {
  associations?: QcmEntry[];
}
```

### `QcmDetailResult`
```typescript
interface QcmDetailResult {
  qcmId: number;
  questions: Array<{
    id: number;
    label: string;
    choices: Array<{ id: number; label: string }>;
  }>;
}
```
