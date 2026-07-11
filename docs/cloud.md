# Cloud Storage

The cloud module provides access to the "Porte-documents" (Cloud storage) feature of EcoleDirecte.

## Functions

### `getCloud`

Fetches the contents of the user's cloud storage.

```typescript
function getCloud(options?: GetCloudOptions): Promise<CloudEntry[]>
```

#### `GetCloudOptions`
- `depth`: (Optional) How deep to traverse the folder structure. Defaults to `3`.
- `raw`: If true, returns the raw API response.
- `explain`: If true, returns debug information.

### `createFolder`

Creates a new folder in the cloud storage.

```typescript
function createFolder(
  name: string,
  parentNode: CloudNode,
  options?: { raw?: boolean; explain?: boolean }
): Promise<CloudNode>
```

- `name`: Name of the new folder.
- `parentNode`: The parent folder node where the new folder should be created.

### `deleteNodes`

Deletes files or folders from the cloud storage.

```typescript
function deleteNodes(
  nodes: CloudNode[],
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

- `nodes`: An array of file/folder nodes to be deleted (moved to trash).

#### Example

```typescript
import { getCloud, deleteNodes } from "linkdirecte";

const tree = await getCloud();
const fileToDelete = tree.find(node => node.type === "file" && node.label === "old.pdf");
if (fileToDelete) {
  await deleteNodes([fileToDelete]);
}
```

---

## Types

### `CloudNode`
Represents a file or folder in the cloud.
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

### `CloudEntry`
A union type for items returned by `getCloud`:
```typescript
type CloudEntry = CloudFolderNode | CloudFileNode;
```
