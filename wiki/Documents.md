<p align="center">
  <picture><source media="(prefers-color-scheme: light)" srcset="https://shieldcn.dev/header/glow.svg?title=Documents&amp;subtitle=Learn+how+to+get+documents+with+Linkdirecte.&amp;logo=lu%3AFileText&amp;mode=light&amp;theme=blue&amp;align=left" /><img alt="Documents | Learn how to get documents with Linkdirecte." src="https://shieldcn.dev/header/glow.svg?title=Documents&amp;subtitle=Learn+how+to+get+documents+with+Linkdirecte.&amp;logo=lu%3AFileText&amp;mode=dark&amp;theme=blue&amp;align=left" /></picture>
</p>

The Documents module provides straightforward access to official school paperwork, such as school invoices, quarterly report cards, or administrative PDF documents.

---

## 🚀 Getting Started

Let's retrieve the list of available school documents and see what's ready to download.

```typescript
import { getDocuments, download } from "linkdirecte";
import { writeFile } from "node:fs/promises";

// Fetch lists of invoices, report cards, etc.
const result = await getDocuments();

// Check if any report cards (bulletins) are available under the "grades" document array
const latestReportCard = result.grades?.[0];

if (latestReportCard && latestReportCard.url) {
  console.log(`Downloading report card: ${latestReportCard.name}...`);

  // Download the file as an ArrayBuffer (default)
  const fileData = await download(latestReportCard.url);

  // Save it locally!
  await writeFile(`./${latestReportCard.name}.pdf`, Buffer.from(fileData));
  console.log("Download complete!");
} else {
  console.log("No report cards available to download.");
}
```

---

## 📖 API Reference

### `getDocuments`

Fetches categorized folders of files made available to the student.

```typescript
function getDocuments(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<DocumentsResult>
```

#### Parameters

- `options` *(optional)*:
  - `raw` *(boolean)*: Returns the original raw response from the server if `true`.
  - `explain` *(boolean)*: Includes debugging parameters under `_debug`.

#### Returns

A promise that resolves to a `DocumentsResult` object containing document lists grouped by categories.

---

## 📋 Example Response

Below is an example of the resolved `DocumentsResult` payload returned by `getDocuments()`:

```typescript
{
  factures: [
    {
      id: 10485,
      name: "Facture Scolarité Mars 2026",
      date: new Date("2026-03-01T00:00:00.000Z"),
      size: 145000, // 145KB
      url: "https://api.ecoledirecte.com/v3/document.awp?id=10485",
      studentId: "12345",
      signatureRequired: false,
      type: "Facture"
    }
  ],
  grades: [
    {
      id: 10486,
      name: "Bulletin du 1er Trimestre",
      date: new Date("2026-02-15T00:00:00.000Z"),
      size: 250000, // 250KB
      url: "https://api.ecoledirecte.com/v3/document.awp?id=10486",
      studentId: "12345",
      signatureRequired: true, // Needs electronic signature!
      type: "Bulletin"
    }
  ],
  viescolaire: [],
  administratives: [
    {
      id: 10487,
      name: "Fiche d'inscription 2026-2027",
      date: new Date("2026-03-10T00:00:00.000Z"),
      size: 412000,
      url: "https://api.ecoledirecte.com/v3/document.awp?id=10487",
      studentId: "12345",
      signatureRequired: false,
      type: "Administratif"
    }
  ],
  toUploadList: []
}
```

---

## 🗂️ Type Definitions

### `DocumentsResult`

EcoleDirecte categorizes documents into distinct buckets. This object maps each bucket:

```typescript
interface DocumentsResult {
  factures: DocumentEntry[];           // Invoices & financial documents
  grades?: DocumentEntry[];            // Quarterly report cards / transcripts
  viescolaire?: DocumentEntry[];       // Absences, behavior, and school life reports
  administratives?: DocumentEntry[];   // Registration files, forms, etc.
  toUploadList?: DocumentEntry[];      // List of documents the school expects you to upload
}
```

### `DocumentEntry`

Provides all the metadata you need to describe and download a specific document:

| Property | Type | Description |
| :--- | :--- | :--- |
| `id` | `number` | Unique ID of the document. |
| `name` | `string` | The title/name of the document (e.g., `"Bulletin du 1er Trimestre"`). |
| `date` | `Date` | The official publication date of this document. |
| `subjectLabel` | `string` *(optional)* | Subject label if related to a specific class. |
| `teacherName` | `string` *(optional)* | Teacher related to the document. |
| `size` | `number` *(optional)* | Size of the document file in bytes. |
| `url` | `string` *(optional)* | The secure download URL. Pass this URL to `download()` to fetch the file! |
| `studentId` | `string` *(optional)* | The ID of the student associated with the document. |
| `signatureRequired` | `boolean` *(optional)* | Whether parents or students are required to electronically sign this document. |
| `type` | `string` *(optional)* | Category code. |
