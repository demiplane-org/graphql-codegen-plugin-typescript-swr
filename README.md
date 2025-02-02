# graphql-codegen-plugin-typescript-swr <!-- omit in toc -->

## WARNING <!-- omit in toc -->

This is a fork of [graphql-codegen-plugin-typescript-swr](https://github.com/croutonn/graphql-codegen-plugin-typescript-swr).  It also merges in the fork [graphql-codegen-plugin-typescript-swr-ramiel](https://github.com/ramiel/graphql-codegen-plugin-typescript-swr).  ANd brings everything up to date.  Instead of waiting for these features to be available in the original package in any form. This repo was created.

## Install

`npm install graphql-codegen-plugin-typescript-swr-ramiel`

## Known bugs

`autogenSWRKey: false` is not supported. At the moment you always have to pass `true`

# New features

## Skip queries

You can skip a request

```ts
const result = sdk.useMyQuery(
  {
    eventId: eventId as string,
  },
  {
    // if this is true, the query is not executed
    // if this value becomes false, the query is executed
    skip: isConditionToSkipMet,
    // ...normal options goes here
  }
)
```

## Customize auto generated key

Even if the key is autogenerated, it can be overwritten)

```ts
import type { Key } from 'swr'

//Let you create a function to override the generated key
const customKey = useCallback(
  (key: Key) => {
    if (admin) return [...key, 'admin']

    return key
  },
  [admin]
)

const result = sdk.useMyQuery(
  {
    eventId: eventId as string,
  },
  {
    // pass the callback to override the key
    customKey: customKey,
    // ...normal options goes here
  }
)
```

## Loading status

Loading state is now part of the result.  
**note** that loading is always false if the query is skipped

```ts
const { loading, data, error } = sdk.useMyQuery({
  eventId: eventId as string,
})
```

## Prebound key generator

You can generate a new key with a different set of variables. This is useful when you need to
prefetch data

```ts
import { mutate } from 'swr'

const { loading, data, error, genKey } = sdk.useMyQuery({
  page: 1,
})

useEffect(() => {
  mutate(
    // genKey is typesafe and let you generate a new key including the name of the query, without the need
    // of specifing it
    genKey({ page: 2 }),
    prefetchedData,
    {
      revalidate: false,
      populateCache: true,
    }
  )
})
```
