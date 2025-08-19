The `User` component is a declarative way to access the authenticated user object from the Asgardeo authentication context. It uses render props to expose the user data, making it easy to display user information or conditionally render UI based on authentication state.

## Overview

The `User` component automatically retrieves the current user from Asgardeo and passes it to its children as a render prop. If no user is signed in, it renders the provided `fallback` content or nothing by default.

## Usage

Use the `User` component to access and display user information in your React application.

### Basic Usage

Display the user's name and email:

```javascript title="User Example"
import { User } from '@asgardeo/react'

<User fallback={<p>Please sign in</p>}>
  {(user) => (
    <div>
      <h1>Welcome, {user?.displayName}!</h1>
      <p>Email: {user?.email}</p>
    </div>
  )}
</User>
```

!!! info "Note"

    The `user` object will be `null` if no user is signed in. Use the `fallback` prop to show alternative content.

## Props

| Prop       | Type                              | Required | Description                                      |
|------------|-----------------------------------|----------|--------------------------------------------------|
| `children` | `(user: IUser \| null) => ReactNode` | ✅       | Render prop function that receives the user object|
| `fallback` | `ReactNode`                       | ❌       | Content to render when no user is signed in       |

## Notes

- Only supported in browser-based React applications (CSR).
- Use this component to declaratively access the authenticated user object anywhere in your app.
