---
title: "tRPCでPrismaのDate型がstringで帰ってくる問題とsuperjsonによる解決策"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["trpc", "Prisma", "React.js", "TypeScript", "Next.js"]
published: true
---


# 背景

## 技術スタック
- Next.js 15.0系
- Prisma 5.系統
- trpc 10.系統

trpcで取得したdataをfrontendで受け取ってuseStateのstateとして取り込もうとした際に下記のエラーが生じた。

```tsx
import { useState, useEffect } from 'react';
import { User } from '@prisma/client';
import { trpc } from '@/server/trpc/trpcClient';
// (中略)

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

# 解決策

基本的にこのコミュニティで議論されている通りでした。
https://discord-questions.trpc.io/m/1070455735533703269
結論としてはServer側とClient側との両方でtransformerにsuperjsonを設定することで解決しました。


[trpcの公式ドキュメント](https://trpc.io/docs/server/data-transformers)でも、
transformerにsuperjsonを設定するプロセスが記載されています。
以下は公式ドキュメントの転載です。

## Server側
```ts
import { initTRPC } from '@trpc/server';
import superjson from 'superjson';
export const t = initTRPC.create({
  transformer: superjson,
});
```

## Client側
(Next.jsの場合)
```ts
import { createTRPCNext } from '@trpc/next';
import type { AppRouter } from '~/server/routers/_app';
import superjson from 'superjson';
// [...]
export const trpc = createTRPCNext<AppRouter>({
  config({ ctx }) {
    return {
      transformer: superjson, // <--　ここに追加
    };
  },
  // [...]
});
```

私の場合、[trpcのprovider](https://trpc.io/docs/v10/client/react/setup#4-add-trpc-providers)のpageを参考にして、以下のように設定しました。

```ts
export function App() {
  const [queryClient] = useState(() => new QueryClient());
  const [trpcClient] = useState(() =>
    trpc.createClient({
      transformer: superjson, // <--　ここに追加
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



# 何が問題だったの？

tRPCは基本的に型安全なAPI通信を実現するために、デフォルトでは標準のJSONシリアライズを使用します。
しかし、困ったことにJSONシリアライズでは日付やマップ、セットなどの特殊な型を扱うのが難しく、Date型などで正確なデータの受け渡しができない場合があるようです。

そこで、superjsonを導入することで、Date型も正しくシリアライズ／デシリアライズできるようになるということでした。
試しておりませんが、transformerをカスタマイズしても同様の効果が得られるようです。