# Linkdirecte

SDK moderne, résilient et 100% tree-shakable pour l'API privée d'EcoleDirecte.

## 🚀 Installation

```bash
bun add linkdirecte
# ou
npm install linkdirecte
```

## 🛠️ Quick Start

```typescript
import { login, getGrades } from "linkdirecte";

// Connexion
const session = await login("mon_identifiant", "mon_mot_de_passe");

if ("securityQuestion" in session && session.type === "securityQuestion") {
  // Handle 2FA
  const success = await session.answer(0);
} else {
  // session is LoginSuccess
  console.log(session.user);
}

// Récupérer les notes
const grades = await getGrades();
console.log(grades);
```

## 📐 Architecture & Principes

- **Zéro Classes Exportées** : Tout est basé sur des fonctions pures pour un tree-shaking optimal.
- **Résilience** : Retries automatiques avec backoff exponentiel, gestion de la concurrence et auto-refresh de session.
- **Transformation** : Les données sont automatiquement nettoyées et les clés traduites en anglais camelCase.
- **Explain Mode** : Passez `{ explain: true }` à n'importe quelle fonction pour obtenir des logs détaillés et la réponse brute.

## 📦 Modules

### Messagerie
```typescript
import { getMessages, sendMessage } from "linkdirecte";

// Récupérer les messages avec leur contenu
const messages = await getMessages({ withContent: true });

// Envoyer un message
await sendMessage({
  subject: "Question",
  content: "Bonjour...",
  destinataires: [...]
});
```

## 🔐 Stockage & Sécurité

Par défaut, le SDK utilise un stockage en mémoire. Pour la persistance avec chiffrement AES-256 :

```typescript
import { configure, fileStorage } from "linkdirecte";

configure({
  storage: fileStorage("./session.json"),
  maxRetries: 5
});
```

## 📡 Mode Offline

Activez la queue offline pour rejouer les mutations quand le réseau revient :

```typescript
configure({ offlineQueue: true });
```

## 🧪 Tests

```bash
bun test
```

## 📄 Licence

AGPL 3.0
