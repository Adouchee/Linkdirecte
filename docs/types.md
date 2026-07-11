# Type Reference

This page provides a reference for the TypeScript interfaces and types exported by the SDK.

## Core Configuration

### `EdConfig`
Global configuration options.
```typescript
interface EdConfig {
  userAgent?: string;
  proxyUrl?: string;
  maxRetries?: number;
  retryDelay?: number;
  concurrency?: number;
  timeout?: number;
  storage?: StorageAdapter;
  offlineQueue?: boolean;
  prefetch?: PrefetchConfig;
  onError?: ErrorMiddleware;
  on2faRequired?: (question: string, choices: string[]) => number | Promise<number>;
  onCredentialsRequired?: () =>
    | { identifiant: string; motdepasse: string }
    | Promise<{ identifiant: string; motdepasse: string }>;
  cache?: CacheConfig;
}
```

### `PrefetchConfig`
```typescript
interface PrefetchConfig {
  enabled?: boolean;
  interval?: string | false; // e.g. "30s", "5m", "1h", or false to disable
  modules?: string[];
}
```

### `CacheConfig`
Per-module cache duration overrides. Each value can be a duration string (e.g. `"5m"`) or `false` to disable caching.
```typescript
interface CacheConfig {
  grades?: string | false;
  timetable?: string | false;
  messages?: string | false;
  homework?: string | false;
  documents?: string | false;
  cloud?: string | false;
  attendance?: string | false;
  timeline?: string | false;
}
```

### `StorageAdapter`
Interface for implementing custom storage strategies.
```typescript
interface StorageAdapter {
  get(key: string): string | null | Promise<string | null>;
  set(key: string, value: string): void | Promise<void>;
  delete(key: string): void | Promise<void>;
}
```

### `ErrorMiddleware`
```typescript
type ErrorMiddleware = (
  error: any,
  retry: (options?: { delay?: number }) => Promise<unknown>,
) => void | Promise<void>;
```

## API Response

### `EdResponse<T>`
Generic API response envelope.
```typescript
interface EdResponse<T> {
  host: string;
  code: number;
  token?: string;
  message: string;
  data: T;
}
```

## Account Types

### `AccountType`
```typescript
type AccountType = "E" | "P" | "A" | "F";
```
- `"E"`: Student (Élève)
- `"P"`: Parent
- `"A"`: Admin
- `"F"`: Family

### `Account`
The user account information returned upon successful login.
```typescript
interface Account {
  loginId: number;
  id: number;
  uid: string;
  identifiant: string;
  accountType: AccountType;
  firstName: string;
  lastName: string;
  email: string;
  schoolName: string;
  main: boolean;
  accessToken?: string;
  profile: {
    sexe: "M" | "F";
    photoUrl: string;
    classe?: {
      id: number;
      code: string;
      label: string;
    };
  };
  modules: Array<{
    code: string;
    enable: boolean;
    badge: number;
    params: Record<string, any>;
  }>;
}
```

### `UserProfile`
```typescript
interface UserProfile {
  token: string;
  accounts: Account[];
}
```

### `AccountSettings`
```typescript
interface AccountSettings {
  id: number;
  identifiant: string;
  email: string;
  mobilePhone: string;
  secretQuestion: string;
  answer: string;
  accessToken: string;
  possibleQuestions: string[];
}
```

## Auth Types

### `LoginResult`
```typescript
type LoginResult = LoginSuccess | LoginChallenge;
```

### `LoginSuccess`
```typescript
interface LoginSuccess {
  user: Account;
  token: string;
  sessionId: string;
}
```

### `LoginChallenge`
```typescript
interface LoginChallenge {
  type: "securityQuestion";
  question: string;
  choices: string[];
  answer: (choiceIndex: number) => Promise<LoginSuccess>;
}
```

### `LoginOptions`
```typescript
interface LoginOptions {
  rememberMe?: boolean;
  on2faRequired?: (question: string, choices: string[]) => number | Promise<number>;
}
```

## Error Types

All errors extend the base `EdError` class.

| Error Class | Description |
| :--- | :--- |
| `EdError` | Base error class. Contains `message`, `code`, `statusCode?`, and `raw?`. |
| `EdAuthError` | Authentication or session errors. |
| `EdNetworkError` | Network or transport errors. |
| `EdRateLimitError` | HTTP 429 — rate limit exceeded. |
| `EdApiError` | Non-success API response code. |
| `EdTransformError` | Data transformation errors. |

## Grades Types

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
```typescript
interface GradeEntry {
  value: string;
  outOf: string;
  coefficient: number;
  isLetter: boolean;
  isTest: boolean;
  date: Date;
  subjectCode: string;
  subjectLabel: string;
  periodCode: string;
  entryDate: Date;
  teacherName?: string;
  testType?: string;
  subSubjectCode?: string;
  subSubjectLabel?: string;
}
```

### `SubjectEntry`
```typescript
interface SubjectEntry {
  subjectCode: string;
  subjectLabel: string;
  coefficient: number;
  teacherName?: string;
  grades: GradeEntry[];
  average?: number;
  classAverage?: number;
}
```

## Timetable Types

### `TimetableResult`
```typescript
interface TimetableResult {
  timetable: TimetableEntry[];
}
```

### `TimetableEntry`
```typescript
interface TimetableEntry {
  id: number;
  subjectCode: string;
  subjectLabel: string;
  teacherName?: string;
  room?: string;
  group?: string;
  startDate: Date;
  endDate: Date;
  isCancelled?: boolean;
  isDetention?: boolean;
  isExempted?: boolean;
  color?: string;
}
```

## Messages Types

### `MessagesResult`
```typescript
interface MessagesResult {
  messages?: {
    received?: MessageEntry[];
    sent?: MessageEntry[];
    drafts?: MessageEntry[];
  };
}
```

### `MessageEntry`
```typescript
interface MessageEntry {
  id: number;
  subject: string;
  content?: string;
  fromName?: string;
  date: Date;
  isRead: boolean;
  isAnswered?: boolean;
  isTransferred?: boolean;
  canAnswer?: boolean;
}
```

### `GetMessagesOptions`
```typescript
interface GetMessagesOptions {
  folderId?: number;
  withContent?: boolean;
  raw?: boolean;
  explain?: boolean;
}
```

### `SendMessageData`
```typescript
interface SendMessageData {
  subject: string;
  content: string;
  destinataires: unknown[];
}
```

## Homework Types

### `HomeworkResult`
```typescript
interface HomeworkResult {
  [date: string]: HomeworkEntry[];
}
```

### `HomeworkEntry`
```typescript
interface HomeworkEntry {
  id: number;
  subjectCode: string;
  subjectLabel: string;
  teacherName?: string;
  givenOn: Date;
  forDate: Date;
  content: string;
  isDone: boolean;
  submitOnline?: boolean;
  documentsToDo?: Array<{ id: number; label: string; url?: string }>;
}
```

### `MarkAsDoneResult`
```typescript
interface MarkAsDoneResult {
  success: boolean;
}
```

## Timeline Types

### `TimelineEntry`
```typescript
interface TimelineEntry {
  id: number;
  elementType: string;
  title?: string;
  subtitle?: string;
  content?: string;
  creationDate: Date;
  subjectLabel?: string;
  teacherName?: string;
}
```

### `CorrelationType`
```typescript
type CorrelationType =
  | "gradeVsPresence"
  | "gradeVsDayOfWeek"
  | "gradeVsTimeOfDay"
  | "homeworkVsGrade"
  | "gradeTrend";
```

### `Correlation`
```typescript
interface Correlation {
  type: CorrelationType;
  subject: string;
  finding: string;
  data: Record<string, number>;
  confidence: number;
  observations: number;
}
```

## Attendance Types

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

### `AttendanceEntry`
```typescript
interface AttendanceEntry {
  id: number;
  date: Date;
  type: string;
  subjectLabel?: string;
  isJustified?: boolean;
  justificationType?: string;
  teacherName?: string;
  licensePoints?: number;
  studentId?: number;
  reason?: string;
  justifiedOnline?: boolean;
  dontNeedJustification?: boolean;
  day?: Date;
}
```

## Cloud Types

### `CloudNode`
```typescript
interface CloudNode {
  id: string;
  type: "file" | "folder";
  label: string;
  date: string;
  size: number;
  isReadOnly: boolean;
  isHidden: boolean;
  isTrash: boolean;
  isLoaded?: boolean;
  quota?: number;
  displayText?: string;
  children?: CloudNode[];
  owner?: {
    id: number;
    type: string;
    lastName: string;
    firstName: string;
    particule: string;
  };
}
```

### `CloudFolderNode`
```typescript
interface CloudFolderNode extends CloudNode {
  type: "folder";
  children: CloudNode[];
}
```

### `CloudFileNode`
```typescript
interface CloudFileNode extends CloudNode {
  type: "file";
}
```

### `CloudEntry`
```typescript
type CloudEntry = CloudFolderNode | CloudFileNode;
```

### `GetCloudOptions`
```typescript
interface GetCloudOptions {
  depth?: number;
  raw?: boolean;
  explain?: boolean;
}
```

## Document Types

### `DocumentEntry`
```typescript
interface DocumentEntry {
  id: number;
  name: string;
  subjectLabel?: string;
  teacherName?: string;
  date: Date;
  size?: number;
  url?: string;
  studentId?: string;
  signatureRequired?: boolean;
  type?: string;
}
```

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

## Forms (QCM) Types

### `QcmsResult`
```typescript
interface QcmsResult {
  associations?: QcmEntry[];
}
```

### `QcmEntry`
```typescript
interface QcmEntry {
  id: number;
  qcmId: number;
  subjectLabel?: string;
  title?: string;
  teacherName?: string;
  date?: Date;
  status?: string;
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

## Listen (Polling) Types

### `PollingConfig`
```typescript
interface PollingConfig {
  interval?: number; // Default: 60,000 ms
}
```

## Download Types

### `DownloadFormat`
```typescript
type DownloadFormat = "buffer" | "blob" | "stream";
```

### `DownloadOptions`
```typescript
interface DownloadOptions {
  as?: DownloadFormat;
  filename?: string;
  params?: Record<string, unknown>;
}
```

## Debug Types

### `DebugInfo`
```typescript
interface DebugInfo {
  rawResponse: unknown;
  transformLog: any[];
  requestDump: {
    url: string;
    headers: Record<string, string>;
    body: any;
  };
  cacheHit: boolean;
  retries: number;
}
```

### `WithDebug<T>`
```typescript
type WithDebug<T> = T & { _debug?: DebugInfo };
```
