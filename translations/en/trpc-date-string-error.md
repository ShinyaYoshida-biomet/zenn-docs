---
title: "Solving the Issue of Prisma's Date Type Returning as String in tRPC with SuperJSON"
emoji: "🔥"
type: "tech" # tech: technical article / idea: idea article
topics: ["trpc", "Prisma", "React.js", "TypeScript", "Next.js"]
published: true
---

# Background

The versions of various packages required to reproduce the issue are as follows:

- Next.js 15.0 series
- Prisma 5.x series
- tRPC 10.x series

When trying to incorporate data fetched via tRPC into a useState state in the frontend, the following error occurred:

```tsx
import { useState, useEffect } from 'react';
import { User } from '@prisma/client';
import { trpc } from '@/server/trpc/trpcClient';
// ...existing code...

const Page = () => {
  const [users, setUsers] = useState<User[]>();
  const { data } = trpc.getUsers.useQuery();
  useEffect(() => {
    if (data) {
      setUsers(data);
    }
  }, [setUsers, data]);

  return <div>{users && users[0].name}</div>;
};

export default Page;
```

```text
        Types of property 'created_at' are incompatible.
          Type 'string | null' is not assignable to type 'Date | null'.
            Type 'string' is not assignable to type 'Date'.ts(2345)
```

# Solution

The solution was discussed in this community:
https://discord-questions.trpc.io/m/1070455735533703269
In conclusion, setting the transformer to superjson on both the server and client sides resolved the issue.

The [official tRPC documentation](https://trpc.io/docs/server/data-transformers) also describes the process of setting the transformer to superjson.
Below is a reproduction of the official documentation.

## Server Side
```ts
import { initTRPC } from '@trpc/server';
import superjson from 'superjson';
export const t = initTRPC.create({
  transformer: superjson,
});
```

## Client Side
(In the case of Next.js)
```ts
import { createTRPCNext } from '@trpc/next';
import type { AppRouter } from '~/server/routers/_app';
import superjson from 'superjson';
// ...
export const trpc = createTRPCNext<AppRouter>({
  config({ ctx }) {
    return {
      transformer: superjson, // <-- Add here
    };
  },
  // ...
});
```

In my case, I referred to the [tRPC provider page](https://trpc.io/docs/v10/client/react/setup#4-add-trpc-providers) and set it up as follows:

```ts
export function App() {
  const [queryClient] = useState(() => new QueryClient());
  const [trpcClient] = useState(() =>
    trpc.createClient({
      transformer: superjson, // <-- Add here
      links: [
        httpBatchLink({
          url: 'http://localhost:3000/trpc',
          async headers() {
            return {
              authorization: getAuthCookie(),
            };
          },
        }),
      ],
    }),
  );
  return (
    <trpc.Provider client={trpcClient} queryClient={queryClient}>
      <QueryClientProvider client={queryClient}>
        {/* Your app here */}
      </QueryClientProvider>
    </trpc.Provider>
  );
}
```

# What Was the Problem?

tRPC uses standard JSON serialization by default to achieve type-safe API communication.
However, JSON serialization struggles with special types like dates, maps, and sets, making it difficult to accurately pass data such as Date types.

By introducing superjson, Date types can be correctly serialized and deserialized.
Although I haven't tried it, customizing the transformer should yield similar results.
