The `AsgardeoProvider` is the root context provider component that configures the `@asgardeo/react` and provides authentication context to your entire React application. It must wrap your application to enable authentication features.

## Overview

The `AsgardeoProvider` initializes the Asgardeo authentication client, manages authentication state, and provides context to child components through React Context. It handles token management, user sessions, organization switching, and branding preferences automatically.

## Usage

The `AsgardeoProvider` must be used at the root of your React application to ensure that all components have access to authentication features.

### Basic Usage

A minimal setup involves wrapping your root component with the `AsgardeoProvider` and providing the necessary `clientId` and `baseUrl` props.

```javascript title="src/main.jsx" hl_lines="5 9-12 14"
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import { AsgardeoProvider } from '@asgardeo/react'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <AsgardeoProvider
      clientId="<your-app-client-id>"
      baseUrl="{{content.sdkconfig.baseUrl}}"
    >
      <App />
    </AsgardeoProvider>
  </StrictMode>
)
```

### Advanced Usage

For more advanced configurations, you can provide additional props such as:

- `scopes`: Specifies the OpenID Connect scopes to request during sign-in.
- `afterSignInUrl`: URL to redirect to after successful sign-in.
- `afterSignOutUrl`: URL to redirect to after sign-out.
- `preferences`: Customization options for UI behavior, theming, and internationalization (i18n).
- etc.

For a complete list of props, refer to the [Props section](#props) below.

```javascript title="src/main.jsx"  hl_lines="12-41"
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import { AsgardeoProvider } from '@asgardeo/react'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <AsgardeoProvider
      clientId="<your-app-client-id>"
      baseUrl="{{content.sdkconfig.baseUrl}}"
      scopes="openid profile internal_login internal_organization_create internal_organization_view internal_organization_update internal_organization_delete"
      afterSignInUrl="/dashboard"
      afterSignOutUrl="/home"{% raw %}
      preferences={{
        theme: {
          inheritFromBranding: false,
          mode: 'dark',
          overrides: {
            colors: {
              primary: '#ff0000',
            },
          },
        },
        i18n: {
          language: 'fr-FR',
          fallbackLanguage: 'en-US',
          bundles: {
            'fr-FR': {
              'signin.title': 'Connexion',
              'user.profile.title': 'Profil utilisateur',
              'user.profile.update.generic.error': "Une erreur s'est produite lors de la mise à jour du profil. Veuillez réessayer.",
            },
            'es-ES': {
              'signin.title': 'Iniciar sesión',
              'user.profile.title': 'Perfil de usuario',
              'user.profile.update.generic.error': 'Ocurrió un error al actualizar el perfil. Por favor, inténtalo de nuevo.',
            },
          },
        },
      }}{% endraw %}
    >
      <App />
    </AsgardeoProvider>
  </StrictMode>
)
```

## Props

The `AsgardeoProvider` component accepts the following props:

<!-- markdownlint-disable MD056 -->
| Prop                     | Type                           | Required | Description |
|--------------------------|--------------------------------|----------|-------------|
| `clientId`               | `string`                       | ✅       | Client ID of your application |
| `baseUrl`                | `string`                       | ✅       | The base URL of the Asgardeo tenant (e.g., `https://api.asgardeo.io/t/abc-org`)       |
| `clientSecret`           | `string`                       | ✅       | Client secret of your application |
| `scopes`                 | `string | string[]`            | ❌       | OpenID Connect Scopes to request during the sign-in. Defaults to `'openid profile internal_login'` if not provided. |
| `storage`                | `sessionStorage or localStorage | webWorker`            | ❌       | Storage mechanism to use for session management. Defaults to `sessionStorage` if not provided. |
| `afterSignInUrl`         | `string`                       | ❌       | URL to redirect to after sign-in process is completed. |
| `afterSignUpUrl`         | `string`                       | ❌       | URL to redirect to after sign-up process is completed. |
| `afterSignOutUrl`        | `string`                       | ❌       | URL to redirect to after sign-out process is completed. |
| `tokenValidation`        | `TokenValidation`              | ❌       | Configuration for token validation. |
| `preferences`            | `Preferences`                  | ❌       | Customization options for UI behavior, internationalization (i18n) and styling. |
<!-- markdownlint-enable MD056 -->

---

??? info "TokenValidation"

    The `tokenValidation` prop allows you to configure how ID tokens are validated.

    | Property | Type | Default | Description |
    |----------|------|---------|-------------|
    | `idToken` | `IdTokenValidation` | `{}` | Configuration for ID token validation |

    ??? info "IdTokenValidation"

        | Property | Type | Default | Description |
        |----------|------|---------|-------------|
        | `validate` | `boolean` | `true` | Whether to validate the ID token |
        | `validateIssuer` | `boolean` | `true` | Whether to validate the issuer |
        | `clockTolerance` | `number` | `300` | Allowed clock skew in seconds |

---

??? info "Preferences"

    The `preferences` prop allows you to customize the UI components provided by the SDK.

    ??? info "Theme Preferences (`preferences.theme`)"

        | Property | Type | Default | Description |
        |----------|------|---------|-------------|
        | `inheritFromBranding` | `boolean` | `true` | Whether to inherit theme from Asgardeo organization/application branding |
        | `mode` | `light | dark | system` | `'system'` | Theme mode. `'system'` follows user's OS preference |
        | `overrides` | `ThemeConfig` | `{}` | Custom theme overrides for colors, typography, spacing, etc. |

    ??? info "Internationalization Preferences (`preferences.i18n`)"

        | Property | Type | Default | Description |
        |----------|------|---------|-------------|
        | `language` | `string` | Browser default | Language code for UI text (e.g., `'en-US'`, `'es-ES'`) |
        | `fallbackLanguage` | `string` | `'en-US'` | Fallback language when translations aren't available |
        | `bundles` | `object` | `{}` | Custom translation bundles to override default text |
