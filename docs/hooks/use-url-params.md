---
sidebar_position: 1
---

# Use Url Params

:::danger Take care

You will need Typescript to use this hook

:::

Create a file at `hooks/use-url-params.ts`:

```jsx title="hooks/use-url-params.ts"
import { useSearchParams } from 'react-router-dom';
import { useMemo, useCallback } from 'react';

type UrlParamValue = string | number | boolean | null | undefined;

export const useUrlParams = <T extends Record<string, UrlParamValue>>(
  defaultParams: T
): [T, (updates: Partial<T>) => void] => {
  const [searchParams, setSearchParams] = useSearchParams();

  // 1. GET: Map URL search params back to the expected type T, applying defaults.
  const params: T = useMemo(() => {
    const state = {} as T;

    for (const key in defaultParams) {
      const defaultValue = defaultParams[key];
      const urlValue = searchParams.get(key);

      // Map the URL value back to the desired type or use the default
      let finalValue: UrlParamValue;

      if (urlValue === null) {
        // If not in URL, use the default value
        finalValue = defaultValue;
      } else if (typeof defaultValue === 'number') {
        // Handle number conversion
        finalValue = Number(urlValue) || (defaultValue as number);
      } else if (typeof defaultValue === 'boolean') {
        // Handle boolean conversion ('true'/'false')
        finalValue = urlValue === 'true';
      } else {
        // Default to string (or null if the default was null)
        finalValue = urlValue;
      }

      state[key as keyof T] = finalValue as T[keyof T];
    }

    return state;
  }, [searchParams, defaultParams]);

  // 2. SET: Function to update parameters, accepting partial updates of type T.
  const setParams = useCallback(
    (updates: Partial<T>) => {
      const newSearchParams = new URLSearchParams(searchParams);

      for (const key in updates) {
        const newValue = updates[key as keyof T];

        if (newValue === null || newValue === undefined) {
          // Remove the parameter
          newSearchParams.delete(key);
        } else {
          // Convert value to a string for the URL
          newSearchParams.set(key, String(newValue));
        }
      }

      setSearchParams(newSearchParams, { replace: true });
    },
    [searchParams, setSearchParams]
  );

  return [params, setParams];
};
```

## Use this hook in your project

Create a file at `page.tsx`:

```jsx title="page.tsx"
import { useUrlParams } from "@/hooks/use-url-params";

type Params = {
  page: number | null,
};

export default function MyReactPage() {
  const [params, setParams] = useUrlParams < Params > { page: 1 };
  return (
    <Layout>
      <h1>My React page</h1>
      <p>This is a React page</p>
      <button onClick={() => setParams({ page: params.page + 1 })}>Next</button>
    </Layout>
  );
}
```

### 📝 Short Summary of `useUrlParams` Hook

The custom **`useUrlParams`** hook is a robust, type-safe utility built on top of React Router DOM's `useSearchParams`. Its primary function is to simplify the management, reading, and writing of **URL query parameters** in a React application.

---

#### Key Capabilities

- **Type Safety (TypeScript):** It uses **generics** and requires a `defaultParams` object to enforce a strict type contract (T) for the parameters, eliminating common JavaScript errors related to missing or incorrectly typed URL values.
- **Automatic Hydration:** It automatically converts the string values found in the URL back into their proper **JavaScript types** (`string`, `number`, `boolean`), based on the type of the provided default value.
- **Default Value Fallback:** It ensures your component always receives a **complete object** of parameters by using the provided defaults for any parameters not currently present in the URL.
- **Simplified Updates:** It returns a setter function that accepts a **partial object** of updates. Passing `null` or `undefined` for a key automatically **removes** that parameter from the URL for a cleaner state.
- **History Management:** Updates are performed using the **`replace`** option in history, preventing the browser's back button from logging every single parameter change.

Essentially, it turns messy, string-only URL data into a clean, typed JavaScript object that is easy to read and update.
