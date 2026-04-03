1.`@/lib/utils.ts`

```ts
import { z } from "zod";

const env = z
  .object({
    VITE_APP_NAME: z.string(),
    VITE_APP_ENV: z.enum(["local", "testing", "production"]),
    VITE_APP_DEBUG: z.coerce.boolean(),
    VITE_APP_URL: z.string().url(),
  })
  .parse(import.meta.env);
```

2. `.env` and `.env.example`

```sh
VITE_APP_NAME="${APP_NAME}"
VITE_APP_ENV="${APP_ENV}"
VITE_APP_DEBUG="${APP_DEBUG}"
VITE_APP_URL="${APP_URL}"
```
