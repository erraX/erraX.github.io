---
title: The magic of next.js `headers`
date: 2024-07-05 22:40:15
tags: next.js
---

This article explains how we use `headers()` in the Next.js app router and how it works under the hood.

## `headers()` introduction
Next.js introduced `headers()` from `v13.0.0`.
From the official description:
> The headers function allows you to read the HTTP incoming request headers from a Server Component.

Example:
```ts
import { headers } from 'next/headers'
 
export default function Page() {
  const headersList = headers()

  // You could get headers in server component
  const referer = headersList.get('referer')
 
  return <div>Referer: {referer}</div>
}
```

## How to get incoming request headers without using `headers`?
Image that you need to get `User-agent` header in the component

## Why it is useful ?

## How it works?