![Statements](https://img.shields.io/badge/statements-97.68%25-brightgreen.svg?style=flat)
![Branches](https://img.shields.io/badge/branches-89.87%25-yellow.svg?style=flat)
![Functions](https://img.shields.io/badge/functions-96.55%25-brightgreen.svg?style=flat)
![Lines](https://img.shields.io/badge/lines-97.68%25-brightgreen.svg?style=flat)

# Girouette (Community Edition)

> Elegant decorator-based routing for AdonisJS v6

## Notice

Girouette was originally developed by its author, who has since discontinued maintaining the package. This version is a community-maintained republication, intended to ensure Girouette remains accessible and updated for users. All original author credits and changes have been preserved.

## Introduction

Girouette provides a beautiful, fluent API for defining your AdonisJS routes using decorators. By bringing route definitions closer to your controller methods, Girouette makes your application's routing more intuitive and maintainable.

## Installation

You can install Girouette via the AdonisJS CLI:

```bash
node ace add @softwarecitadel/girouette
```

## Basic Routing

After installation, you can start using decorators to define your routes.

By convention, Girouette scans all files in the `./app` folder ending with `_controller.ts`.

```typescript
import {
  Get, Post, Put, Patch, Delete, Any,
  Middleware, ResourceMiddleware, GroupMiddleware,
  Resource, Where, Group, GroupDomain,
} from '@softwarecitadel/girouette'
import { HttpContext } from '@adonisjs/core/http'

export default class UsersController {
  @Get('/users')
  async index({ response }: HttpContext) {}

  @Post('/users')
  async store({ request }: HttpContext) {}
}
```

## Route Groups

Use decorators for grouping routes:

```typescript
import { Group, GroupMiddleware, GroupDomain } from '@softwarecitadel/girouette'
import { middleware } from '#start/kernel'

@Group({ name: 'admin', prefix: '/admin' })
@GroupMiddleware([middleware.auth()])
@GroupDomain('admin.example.com')
export default class AdminController {
  @Get('/dashboard', 'admin.dashboard')
  async index() {}
}
```

## Route Middleware

Protect routes using middleware:

```typescript
import { Get, Middleware } from '@softwarecitadel/girouette'
import { middleware } from '#start/kernel'

export default class UsersController {
  @Get('/profile')
  @Middleware([middleware.auth()])
  async show({ auth }: HttpContext) {}
}
```

## Resource Controllers

Define RESTful resource routes easily:

```typescript
import { Resource } from '@softwarecitadel/girouette'

@Resource('/posts', 'posts')
export default class PostsController {
  async index() {}
  async create() {}
  async store() {}
  async show() {}
  async edit() {}
  async update() {}
  async destroy() {}
}
```

## Route Constraints

Add constraints to route parameters:

```typescript
import { Get, Where } from '@softwarecitadel/girouette'

export default class PostsController {
  @Get('/posts/:slug')
  @Where('slug', /^[a-z0-9-]+$/)
  async show({ params }: HttpContext) {}
}
```

## Available Decorators

### HTTP Methods

* `@Get`, `@Post`, `@Put`, `@Patch`, `@Delete`, `@Any`

### Route Configuration

* `@Group`, `@GroupDomain`, `@GroupMiddleware`, `@Middleware`, `@Resource`, `@ResourceMiddleware`, `@Where`

## Advanced Usage

Combine decorators for complex routing setups:

```typescript
import { Get, Middleware, Where, Group } from '@softwarecitadel/girouette'
import { middleware } from '#start/kernel'

@Group('/api')
export default class ArticlesController {
  @Get('/articles/:id')
  @Middleware([middleware.auth()])
  @Where('id', /^\d+$/)
  async show({ params }: HttpContext) {}
}
```

Apply middleware selectively on resource actions:

```typescript
import { Resource, ResourceMiddleware } from '@softwarecitadel/girouette'
import { middleware } from '#start/kernel'

@Resource('/admin/posts', 'admin.posts')
@ResourceMiddleware(['store', 'update', 'destroy'], [middleware.auth()])
export default class AdminPostsController {}
```

## License

Girouette is open-sourced software licensed under the [MIT license](./LICENSE.md). All original author contributions remain intact and credited.
