---
tags: ["tanstack-start", "turso", "upstash", "typescript", "react", "drizzle"]
title: Secret Santa and new tech fun
description: Building a Secret Santa app with TanStack Start, Turso, and Upstash - exploring modern full-stack development
pubDatetime: 2025-11-29
---

It all began when my friends and I decided to do our annual Secret Santa. But now that we're all adults, having all of us in a room necessitates a lot of scheduling, which none of us can do. We therefore decided to conduct our Secret Santa drawings online. __We were all too lazy to look for an app or website to accomplish it, so I thought I could waste my Sunday afternoon creating one.__ Why not?

Additionally, I intended to use vibe coding to create a whole application. I've been using **Cursor** for a long time, but I've mostly used it for small adjustments or just delegating tedious tasks to it. I wanted to build an entire app vibe coded, to see how it feels and pretend it won't take developers job in a few years.

There were also some technologies I wanted to use:

1. **Tanstack Start + Form**: I honestly love pretty much anything Tanstack people do. They rule, so I wanted to give a shot.
2. **Turso**: SQLite is underrated, and Turso seems to know it.
3. **Upstash**: Managing Redis is not fun, and I wanted to complicate things with rate limiter.

## App Workflow

My goal was to create a basic application.  No registration, no nothing.  A room is created by one person, and others join by a link.  Everyone receives mail when the room creator presses the draw.  Done.

I required three pages:
1. Homepage: Simple information + room creation form.
2. Room Admin Page: This page allows the room administrator to check who has entered and initiate draw.
3. Participant Page: A straightforward form for joining.  Nothing noteworthy.


## How to vibe?

I have detailed these in my prompt. Mentioned the technologies I wanted to use, the design system (Shadcn, big surprise.), overall application logic etc. 

I used Cursor in Plan mode. Additionally, I have been strongly advocating the use of this mode for any task requiring the modification of multiple files. It lets you see how the agent will tackle the issue and modify its strategy before it starts altering files excessively.

It was surprisingly good. It scaffolded the application quite sufficiently, followed my instructions. But I soon discovered that you need a good Cursor rules or AGENTS.md. I would include the following in my instructions, which at first required me to make several course corrections:

1. Emphasize the usage of theme, cohesion in the design: Otherwise it starts introducing new colors, shades to make it look nicer. It does, but at what cost...
2. Best practices and maintainability: If not mentioned, I noticed instead of creating components it tends to copy paste in a lot of places or build god-components.
3. Security: This one is obvious I believe. Never trust it, read the code.

## Tech

I mentioned a few tech I have used. Here's how they fared

### Tanstack Start

Really good. To be honest, it is pretty much just Tanstack Router with a server added on top. Tanstack Router was already amazing with first-class Typescript support, dev tools, search params handling, etc. Tanstack Start feels like it is added capability on top, which doesn't require you to write directives like `use server`, but rather just use regular Javascript with specific, typesafe functions.

For example, instead of dealing with RSC, `use server;`, specific rules, adding a server function is simple and intuitive:

```js
export const createRoom = createServerFn({ method: "POST" })
	.middleware([rateLimitMiddleware])
	.inputValidator(createRoomSchema)
	.handler(async (ctx) => {
     ...
     ...
    })
```

Then you just call the function you created in your app. Very, very easy to use. Love it, and will continue to use it. Looking forward to v1.

### Tanstack Form

This one was alright.  I didn't have any negative things to say about it, but I didn't think there was much of a benefit to using it over React Hook Form, which I am more familiar with.  It wasn't too bad, but I don't think I have enough reasons to switch because it takes some getting used to.  Because I am more accustomed to React Hook Form, I would continue to use it unless it became problematic or was abandoned.

### Turso + Drizzle

incredibly simple to use and set up.  Because Turso is "almost" SQLite, you can work locally and it is very performant.  Drizzle has been excellent.  I can assess the production performance if the website receives a lot of traffic, but I have no reason to doubt it.

### Upstash

Once more, it was very simple to set up and operate.  I appreciate that you can use the Redis client or their SDK if you're lazy and don't mind a little vendor lock-in.  Just to see how it functions, I put in place a basic rate-limiter, and it does.  Easy.

## Conclusion

Overall, this took an afternoon to construct and perhaps an hour to refine and record.  I'm pleased with the outcome. 

Vibe coding was enjoyable and is now a great tool for toy projects and proof-of-concepts.  It wasn't worthwhile for that a year ago.  It was having trouble even with a small number of files, but now it can produce a fully functional application, albeit with a few bugs and security and maintainability problems.  But there has been progress, and things will improve.

All of the technologies I used were enjoyable, fulfilled their intended purpose, and I had no regrets about using any of them.

Overall, it was a pleasant day.

## Links & Resources

- [Live App](https://santa.buncolakc.om)
- [GitHub Repository](https://github.com/bun-colak-creolytix-dev/secret-santa)
- [TanStack Start Docs](https://tanstack.com/start)
- [Turso Docs](https://docs.turso.tech)
- [Upstash Redis Docs](https://docs.upstash.com/redis)
