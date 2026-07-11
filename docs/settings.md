# Settings

The settings module allows managing user account preferences and personal information.

## Functions

### `getSettings`

Fetches the account settings for the currently connected user.

```typescript
function getSettings(options?: {
  raw?: boolean;
  explain?: boolean;
}): Promise<AccountSettings>
```

### `updateSettings`

Updates the user's account information.

```typescript
function updateSettings(
  data: {
    email?: string;
    portable?: string;
    questionSecrete?: string;
    reponse?: string;
    nouveauMotDePasse?: string;
    identifiant?: string;
  },
  options?: { raw?: boolean; explain?: boolean }
): Promise<AccountSettings>
```

> **Note**: When `nouveauMotDePasse` is provided, the SDK automatically handles `confirmationMotDePasse` internally.

### `updateAccessibility`

Enables or disables the visual accessibility parameter in EcoleDirecte.

```typescript
function updateAccessibility(
  enabled: boolean,
  options?: { raw?: boolean; explain?: boolean }
): Promise<{ success: boolean }>
```

#### Example

```typescript
import { updateAccessibility } from "linkdirecte";

await updateAccessibility(true); // Enable visual accessibility
```

---

## Types

### `AccountSettings`
```typescript
interface AccountSettings {
  id: number;
  identifiant: string;
  email: string;
  portable: string;
  questionSecrete: string;
  reponse: string;
  accessToken: string;
  questionsPossibles: string[];
}
```
