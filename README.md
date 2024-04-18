## What is the issue?

The [default.js examples](https://nextjs.org/docs/app/api-reference/file-conventions/default#params-optional)
suggest that it is possible to contain `default.js` files deeper than the parallel 
route itself. Such that subfolders can contain a version of default Next displays when 
a fallback is needed. This does not work as proposed in 14.2.0 & 14.2.2 when using a 
route parameter.

## Expected behaviour & issue
The route file-tree looks like this:
```
- @team
-- [teamId]
    default.tsx
    page.tsx

- [teamId]
   page.tsx
-- settings
    page.tsx
```

Both are simultaneously rendered beneath each other in the root `layout.tsx`.

1. We expect following when navigating to `/testId/`: (_passed_)
   1. [x] the content of `@team/[teamId]/page.tsx` is rendered
   2. [x] the content of `[teamId]/page.tsx` is rendered


2. We expect following when navigating to `/testId/settings`: (**_failed_**) 
   1. [x] the content of `[teamId]/settings/page.tsx` is rendered
   2. [ ] the content of `@team/[teamId]/default.tsx` is rendered (**Unexpected**)

By default, neither is rendered from point two, since a 404 is thrown. Only, when 
adding a `default.js` at `@team/` directly, the first is rendered. The more deeply 
located default file is **completely ignored**.

This is unexpected, according to the docs.