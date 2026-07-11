# Documents

The documents module handles official administrative documents provided by the school.

## Functions

### `getDocuments`

Fetches the list of official documents (invoices, report cards, etc.) available for the student.

```typescript
function getDocuments(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<DocumentsResult>
```

### `DocumentEntry`
| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the document. |
| `name` | `string` | Title of the document. |
| `subjectLabel` | `string` | Subject name (optional). |
| `teacherName` | `string` | Teacher name (optional). |
| `date` | `Date` | Publication date. |
| `size` | `number` | File size in bytes (optional). |
| `url` | `string` | Download URL (optional). |
| `studentId` | `string` | Student ID (optional). |
| `signatureRequired` | `boolean` | Whether the document requires a signature (optional). |
| `type` | `string` | Category of document (optional). |

#### Example

```typescript
import { getDocuments } from "linkdirecte";

const data = await getDocuments();
// data.factures contains invoices
// data.grades contains report cards
```

## Downloading Documents

To download the actual file associated with a document entry, use the `download` function from the core module with the provided URL.

```typescript
import { getDocuments, download } from "linkdirecte";

const data = await getDocuments();
const doc = data.grades?.[0];
if (doc?.url) {
  await download(doc.url, { filename: `${doc.name}.pdf` });
}
```

---

## Types

### `DocumentsResult`
```typescript
interface DocumentsResult {
  factures: DocumentEntry[];
  grades?: DocumentEntry[];
  viescolaire?: DocumentEntry[];
  administratives?: DocumentEntry[];
  toUploadList?: DocumentEntry[];
}
```
