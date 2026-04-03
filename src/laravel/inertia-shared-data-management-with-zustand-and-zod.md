# inertia shared data management with zustand and zod

![bear](./bear.jpg)

## requiered

- [`react-starter-kit`](https://packagist.org/packages/laravel/react-starter-kit)
- [`@inertiajs/core`](https://www.npmjs.com/package/@inertiajs/core)
- [`zod`](https://www.npmjs.com/package/zod)
- [`zustand`](https://www.npmjs.com/package/zustand)

## implementation

### app/Http/Middleware/HandleInertiaRequests.php

```php
<?php

declare(strict_types=1);

namespace App\Http\Middleware;

use Illuminate\Foundation\Inspiring;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use Illuminate\Support\Facades\Session;
use Inertia\Middleware;
use Tighten\Ziggy\Ziggy;

class HandleInertiaRequests extends Middleware
{
    // ...

    public function share(Request $request): array
    {
        // ...

        return [
            // ...

            'common' => [
                'user' => $this->user($request),
                'session' => [
                    'id' => Session::id(),
                    // ip_address
                    // user_agent
                    // payload
                    // last_activity
                ],
                'route' => $this->route($request),
            ],
        ];
    }

    /**
     * @return array<string, mixed>
     */
    private function route(Request $request): array
    {
        $route = Route::current();

        return [
            'name' => $route->getName(),
            'path' => $route->uri(),
            // middleware
            // parameters
        ];
    }

    /**
     * @return array<string, mixed>|null
     */
    private function user(Request $request): ?array
    {
        $user = $request->user();

        if (is_null($user)) {
            return null;
        }

        return [
            'id' => $user->id,
            'name' => $user->name,
            'email' => $user->email,
            'email_verified' => isset($user->email_verified_at),
            'created_at' => $user->created_at,
            'updated_at' => $user->updated_at,
        ];
    }
}
```

### stores/common.ts

```ts
import { z } from 'zod';
import { create } from 'zustand';

const Route = z.object({
    name: z.string().nullable(),
    path: z.string().nullable()
});
const Session = z.object({
    id: z.string().nullable()
});
const User = z
    .object({
        id: z.number(),
        name: z.string(),
        email: z.string().email(),
        email_verified: z.boolean(),
        created_at: z.string().datetime(),
        updated_at: z.string().datetime()
    })
    .nullable();


const Storable = z.object({
    session: Session,
    route: Route,
    user: User
});
const Updatable = z.discriminatedUnion('name', [
    z.object({ name: z.literal('route'), value: Route }),
    z.object({ name: z.literal('session'), value: Session }),
    z.object({ name: z.literal('user'), value: User })
]);
const Deletable = z.enum(['session', 'route', 'user']);


const Common = z.object({
    route: Route.extend({
        named: z.function().args(z.string()).returns(z.boolean())
    }),
    session: Session,
    user: User,
    store: z.function(z.tuple([Storable]), z.void()),
    update: z.function(z.tuple([Updatable]), z.void()),
    destroy: z.function(z.tuple([Deletable.optional()], z.void()))
});

// or useCommonStore
const common = create<z.infer<typeof Common>>((set, get) => ({
    route: {
        name: null,
        path: null,
        named: (value) => {
            const { route } = get();
            return route.name === value;
        }
    },
    session: {
        id: null
    },
    user: null,
    store: ({ route, user }) =>
        set((state) => ({
            route: {
                ...state.route,
                name: route.name,
                path: route.path
            },
            session: { id: state.session.id },
            user: user
        })),
    update: ({ name, value }) => {
        if (name === 'route') {
            set((state) => ({
                route: {
                    ...state.route,
                    name: value.name,
                    path: value.path
                }
            }));
        } else if (name === 'session') {
            set({ session: { id: value.id } });
        } else if (name === 'user') {
            set({ user: value });
        }
    },
    destroy: (name) => {
        if (name === undefined) {
            return set((state) => ({
                route: {
                    ...state.route,
                    name: null,
                    path: null
                },
                session: {
                    id: null
                },
                user: null
            }));
        }

        const value = Deletable.parse(name); // z.ZodError

        if (value === 'route') {
            return set((state) => ({ route: { ...state.route, name: null, path: null } }));
        } else if (value === 'session') {
            return set({ session: { id: null } });
        } else if (value === 'user') {
            return set({ user: null });
        }
    }
}));

export { common, Storable };
```

### .env and .env.example

```sh
# ...

VITE_APP_NAME="${APP_NAME}"
VITE_APP_DEBUG=${APP_DEBUG}
VITE_APP_URL="${APP_URL}"
```

### resources/js/env.ts

```ts
import { z } from 'zod';

const env = z
    .object({
        VITE_APP_NAME: z.string(),
        VITE_APP_DEBUG: z.coerce.boolean(),
        // VITE_APP_URL: z.string().url()
    })
    .parse(import.meta.env);

export { env };
```

### resources/js/app.tsx

```ts
import './../css/app.css';

import { router } from '@inertiajs/core';
import { resolvePageComponent } from 'laravel-vite-plugin/inertia-helpers';
import { createRoot } from 'react-dom/client';
import { env } from './env';
import { initializeTheme } from './hooks/use-appearance';
import { common, Storable } from './stores/common';

const { log, info } = console;
const { VITE_APP_NAME, VITE_APP_DEBUG } = env;

router.on('navigate', ({ detail }) => {
    if (VITE_APP_DEBUG) info('navigate', detail);

    const { data, success, error } = Storable.safeParse(detail.page.props.common);
    if (success) common.getState().store(data);
    else {
        if (VITE_APP_DEBUG) console.error('navigate', error);
    }
});

createInertiaApp({
    title: (title) => `${title} - ${VITE_APP_NAME}`,
    resolve: (name) => resolvePageComponent(`./pages/${name}.tsx`, import.meta.glob('./pages/**/*.tsx')),
    setup: ({ el, App, props }) => {
        if (VITE_APP_DEBUG) info('createInertiaApp', props.initialPage);

        const { data, success, error } = Storable.safeParse(props.initialPage.props.common);

        if (success) common.getState().store(data);
        else {
            if (VITE_APP_DEBUG) console.error('createInertiaApp', error);
        }

        createRoot(el).render(<App {...props} />);
    },
    progress: {
        color: '#4B5563'
    }
});

// This will set light / dark mode on load...
initializeTheme();
```

### resources/js/welcome.tsx

```tsx
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { common } from '@/stores/common';

export default () => {
    const route = common((state) => state.route);
    const session = common((state) => state.session);
    const user = common((state) => state.user);

    const update = common((state) => state.update);

    return (
        <div className="mx-auto w-full max-w-md space-y-6 p-4">
            <form onSubmit={(event) => event.preventDefault()}>
                <div className="space-y-2">
                    <Label htmlFor="name">Name</Label>
                    <Input
                        type="text"
                        id="name"
                        autoFocus={true}
                        onChange={(event) => {
                            // authenticated
                            if (user) update({ name: 'user', value: { ...user, name: event.target.value } });
                        }}
                    />
                </div>
            </form>

            <ul className="space-y-6">
                {[route, session, user].map((value, index) => (
                    <li>
                        <pre key={index} className="bg-secondary break-all whitespace-pre-wrap">
                            {JSON.stringify(value, undefined, 4)}
                        </pre>
                    </li>
                ))}
            </ul>
        </div>
    );
};
```
