The `useAsgardeo` hook provides access to the Asgardeo authentication context in React applications. It allows you to retrieve authentication state, user information, and other context values managed by the `AsgardeoProvider`.

## Overview

`useAsgardeo` is a custom React hook that returns the current value of the Asgardeo context. It must be used within a component tree wrapped by `AsgardeoProvider`. If used outside the provider, it throws an error.

## Usage

Import and use the hook in any functional component to access authentication data:

```typescript
import useAsgardeo from '@asgardeo/react';

const MyComponent = () => {
  const { isSignedIn, user, signIn, signOut } = useAsgardeo();

  return (
    <div>
      {isSignedIn ? (
        <>
          <p>Welcome, {user.displayName}!</p>
          <button onClick={signOut}>Sign Out</button>
        </>
      ) : (
        <button onClick={signIn}>Sign In</button>
      )}
    </div>
  );
};
```

!!! info "Note"
    This hook must be used inside a component rendered by `AsgardeoProvider`. Otherwise, it will throw an error.

## API

The hook returns all properties and methods provided by `AsgardeoContextProps`:

<!-- markdownlint-disable MD056 -->
| Property            | Type                                         | Description                                                      |
|---------------------|----------------------------------------------|------------------------------------------------------------------|
| `isSignedIn`        | `boolean`                                    | Whether the user is currently signed in                          |
| `user`              | `IUser | null`                              | The authenticated user object, or `null` if not signed in        |
| `signIn`            | `() => Promise<void>`                        | Initiates the sign-in flow                                       |
| `signOut`           | `() => Promise<void>`                        | Initiates the sign-out flow                                      |
| `loading`           | `boolean`                                    | Indicates if an authentication operation is in progress          |
| `error`             | `Error | null`                              | The last error encountered during authentication, if any         |
<!-- markdownlint-enable MD056 -->

## Error Handling

If `useAsgardeo` is called outside of an `AsgardeoProvider`, it throws:

```
Error: useAsgardeo must be used within an AsgardeoProvider
```

## Notes

- Use this hook to access authentication state and actions anywhere in your React component tree.
- Always ensure your app is wrapped with `AsgardeoProvider` at the root level.
