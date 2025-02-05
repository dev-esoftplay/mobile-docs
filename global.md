# useGlobalState Hook Documentation

```
import useGlobalState from './path/to/hook';
```

## Basic Usage
```javascript
const globalState = useGlobalState(initialValue, options);
```

## Core Methods

### `useState()`
```javascript
const [state, setState, getCurrent] = globalState.useState();
```

### `get()`
```javascript
const value = globalState.get('nested.property');
```

### `set()`
```javascript
globalState.set(newValue);
globalState.set(prev => ({ ...prev, update }));
```

### `reset()`
```javascript
globalState.reset();
```

## Configuration Options
```typescript
interface useGlobalOption {
  persistKey?: string;
  inFile?: boolean;
  inFastStorage?: boolean;
  useAutoSync?: {
    url: string;
    post: (item: any) => Object;
    delayInMs?: number;
    isSyncing?: (status: boolean) => void;
  };
  isUserData?: boolean;
}
```

## Advanced Features

### Persisted State
```javascript
const auth = useGlobalState(null, {
  persistKey: 'user-auth',
  inFastStorage: true
});
```

### Auto-Sync
```javascript
const todos = useGlobalState([], {
  useAutoSync: {
    url: '/api/todos',
    post: items => ({ data: items })
  }
});
```

### Component Binding
```javascript
const ConnectedComponent = globalState.connect({
  selector: state => ({ items: state.list }),
  render: ({ items }) => <List data={items} />
});
```

## Storage Providers
| Option          | Package                              |
|-----------------|--------------------------------------|
| Default         | @react-native-async-storage/async-storage |
| `inFile: true`  | esoftplay/storage                    |
| `inFastStorage: true` | esoftplay/mmkv          |

## Requirements
```bash
npm install react-fast-compare @react-native-async-storage/async-storage
```

---
```

**To create the file:**  
1. Copy all content between the horizontal lines (including code blocks)
2. Create new file in your project
3. Paste content
4. Save as `useGlobalState.md`

**Command Line Method (Mac/Linux):**
```bash
# Create and open file
touch useGlobalState.md && open useGlobalState.md
# Paste content and save
```