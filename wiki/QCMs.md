<p align="center">
  <picture><source media="(prefers-color-scheme: light)" srcset="https://shieldcn.dev/header/glow.svg?title=QCMs&amp;subtitle=Learn+how+to+get+QCM+data+with+Linkdirecte.&amp;logo=lu%3AMailQuestion&amp;mode=light&amp;theme=blue&amp;align=left" /><img alt="QCMs | Learn how to get QCM data with Linkdirecte." src="https://shieldcn.dev/header/glow.svg?title=QCMs&amp;subtitle=Learn+how+to+get+QCM+data+with+Linkdirecte.&amp;logo=lu%3AMailQuestion&amp;mode=dark&amp;theme=blue&amp;align=left" /></picture>
</p>

The Forms module manages online assessments, questionnaires, and multi-choice quizzes (QCMs) assigned to students by teachers.

---

## 🚀 Getting Started

Here's how to fetch assigned quizzes, inspect their structures, and respond to questions.

```typescript
import { getQcms, getQcmDetail } from "linkdirecte";

// 1. Fetch available quizzes
const result = await getQcms();
const activeQuiz = result.associations?.[0];

if (activeQuiz) {
  console.log(`Let's work on: ${activeQuiz.title || "Untitled Quiz"}`);

  // 2. Fetch questions for this quiz
  const details = await getQcmDetail(activeQuiz.qcmId, activeQuiz.id);

  details.questions.forEach((question, index) => {
    console.log(`Question ${index + 1}: ${question.label}`);
    question.choices.forEach(choice => {
      console.log(`  [ ] ID: ${choice.id} | ${choice.label}`);
    });
  });
} else {
  console.log("No quizzes assigned right now!");
}
```

---

## 📖 API Reference

### `getQcms`

Retrieves a list of all assigned questionnaires/QCMs.

```typescript
function getQcms(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<QcmsResult>
```

---

### `getQcmDetail`

Retrieves the question set and candidate choices for a specific QCM.

```typescript
function getQcmDetail(
  idQcm: number,
  idAssociation: number,
  options?: { raw?: boolean; explain?: boolean }
): Promise<QcmDetailResult>
```

---

### `updateQcmStatus`

Updates the student's status on a quiz (e.g., when they start or complete it).

```typescript
function updateQcmStatus(
  idQcm: number,
  idAssociation: number,
  idParticipant: number,
  action: "updateStartDate" | "updateEndDate",
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

- `idParticipant`: Participant ID returned in the QCM details.
- `action`: Use `"updateStartDate"` when the student opens/starts the test, and `"updateEndDate"` when finalizing and submitting the complete exam.

---

### `submitQcmAnswer`

Submits selected choice IDs for a single question.

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

- `choiceIds`: An array of numeric choice IDs the student selected for this question.

---

## 📋 Example Response

Below is an example of the resolved `QcmsResult` payload returned by `getQcms()`:

```typescript
{
  associations: [
    {
      id: 48572,
      qcmId: 1042,
      title: "Quiz de Révision - Les fractions",
      subjectLabel: "Mathématiques",
      teacherName: "Mme. Dupont",
      date: new Date("2026-03-12T00:00:00.000Z"),
      status: "Not Started"
    }
  ]
}
```

Below is an example of the resolved `QcmDetailResult` payload returned by `getQcmDetail(qcmId, associationId)`:

```typescript
{
  qcmId: 1042,
  questions: [
    {
      id: 9001,
      label: "Quelle est la valeur de 1/2 + 1/4 ?",
      choices: [
        { id: 301, label: "a) 1/6" },
        { id: 302, label: "b) 3/4" },
        { id: 303, label: "c) 2/3" }
      ]
    },
    {
      id: 9002,
      label: "Une fraction dont le numérateur est supérieur au dénominateur est une fraction :",
      choices: [
        { id: 304, label: "a) Propre" },
        { id: 305, label: "b) Impropre" },
        { id: 306, label: "c) Équivalente" }
      ]
    }
  ]
}
```

---

## 🗂️ Type Definitions

### `QcmsResult`

```typescript
interface QcmsResult {
  associations?: QcmEntry[];
}
```

### `QcmEntry`

| Property | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | The association ID. |
| `qcmId` | `number` | The questionnaire ID. |
| `title` | `string` *(optional)* | Title of the test. |
| `subjectLabel` | `string` *(optional)* | Subject label. |
| `teacherName` | `string` *(optional)* | Teacher who assigned the QCM. |
| `date` | `Date` *(optional)* | The date when the quiz was assigned. |
| `status` | `string` *(optional)* | Quiz state (e.g. `"Not Started"`, `"In Progress"`). |

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
