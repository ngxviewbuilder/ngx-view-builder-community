# NGX View Builder - Community

This repository is the community home for **[NGX View Builder](https://ngxviewbuilder.io)**, a visual builder for Angular applications. If you landed here from a search engine or a link and aren't sure what the project actually is, this page is for you. If you're already a user, this is also where bug reports, feature requests, and general discussion happen.

## What is NGX View Builder?

NGX View Builder lets you design forms, views, dashboards, and data tables for an Angular app by dragging and dropping elements on a canvas instead of hand-coding every screen. The output isn't a black box: it's plain JSON. A lightweight Angular runtime takes that JSON and renders it as real, native Angular components (standalone components, signals, no iframe, no separate rendering engine).

Because the view lives as JSON, updating it doesn't require a new deploy. Someone who isn't a developer, a PM, a business analyst, a support engineer, can change a form or dashboard after launch and the app picks it up.

Typical uses:

- Internal tools and admin panels that change shape often
- Forms with conditional logic, validation, and calculated fields
- Dashboards and data tables backed by REST APIs, with server-side paging
- Multi-step flows where non-developers own the content after the initial build

It's built specifically for Angular, not a generic embeddable widget, so it integrates with the router, HttpClient, and your existing component tree.

## Quick start

NGX View Builder ships as scoped packages on npm. The old unscoped `ngx-view-builder` and `ngx-view-builder-plugin-templates` packages stopped at 0.6.0 and are not updated any more.

| Package | You need it when | License |
| --- | --- | --- |
| `@ngxviewbuilder/runtime` | Your app renders saved views | Free, no key |
| `@ngxviewbuilder/designer` | Your app also hosts the visual builder | Commercial |
| `@ngxviewbuilder/plugin-templates` | You want reusable HTML templates in the builder (optional) | Commercial |

Rendering views only:

```bash
npm install @ngxviewbuilder/runtime
```

Hosting the builder as well:

```bash
npm install @ngxviewbuilder/runtime @ngxviewbuilder/designer
```

Requirements:

- Angular 22+ (`@angular/core`, `@angular/common`, `@angular/cdk` as peer dependencies; the designer also needs `@angular/forms`)
- Node.js 22.22+ (or 24.15+ / 26+)

Import the global stylesheet once, for example in `styles.css`:

```css
@import '@ngxviewbuilder/runtime/styles/index.css';
```

Full setup, including app config and runtime initialization, is in the [installation guide](https://ngxviewbuilder.io/developers/installation).

### Rendering a view (runtime)

```ts
import { Component, signal } from '@angular/core';
import { IStructure, NgxViewBuilderRuntime } from '@ngxviewbuilder/runtime';

@Component({
  selector: 'app-client-form',
  imports: [NgxViewBuilderRuntime],
  template: `<ngx-view-builder-runtime [pageJson]="structure()" />`,
})
export class ClientFormComponent {
  structure = signal<IStructure>(/* JSON produced by the builder, loaded from your backend */);
}
```

Read data back out with `runtime.getDataSnapshot()`, the `(valueChanged)` output, or by injecting `NgxViewBuilderApiService`. Details in [rendering views](https://ngxviewbuilder.io/developers/runtime-integration).

### Embedding the builder

```ts
import { Component } from '@angular/core';
import { BuilderModel, IStructure, NgxViewBuilderDesigner } from '@ngxviewbuilder/designer';

@Component({
  selector: 'app-builder-page',
  imports: [NgxViewBuilderDesigner],
  template: `<ngx-view-builder-designer
    [model]="builderModel"
    (structureChanged)="onChange($event)"
  />`,
})
export class BuilderPageComponent {
  builderModel = new BuilderModel();

  onChange(structure: IStructure): void {
    // persist wherever you like, e.g. your own backend
  }
}
```

`BuilderModel` holds the structure being edited (`setJson`/`getJson`); `structureChanged` fires on every edit, so autosave is a one-liner. Details in [embedding the builder](https://ngxviewbuilder.io/developers/builder-integration).

## Key capabilities

- 55+ built-in elements: inputs, tables, charts, KPIs, layout, and more
- An expression system for calculated fields, validation, and conditional visibility, including across nested tables and repeaters, with a visual rule builder for non-programmers
- REST, WebSocket and route-based data sources
- A stable `data-testid` on every control, for Playwright and Cypress ([E2E testing](https://ngxviewbuilder.io/developers/e2e-testing))
- Angular-native output: standalone components and signals, rendered directly in your app

## AI access (MCP)

The builder has **no AI chat and no AI model inside it**. AI works from the outside: your own AI client (Claude, ChatGPT, Cursor, Codex or your own agent) connects to the builder over [MCP](https://modelcontextprotocol.io) and drives it while you watch the canvas. The builder exposes a command API that reads the view and applies changes, and you see each element arrive on the canvas as the agent builds. AI access comes with a designer license. See [AI access](https://ngxviewbuilder.io/developers/ai-command-api).

## License

NGX View Builder is closed source, distributed under a commercial license, not MIT/Apache/GPL.

**The runtime, the part that renders views in your app, is free, always, with no license key and no watermark, whether you're on 1.0.0 or a pre-1.0 beta build.** Only the visual builder itself becomes a paid, licensed product starting at version 1.0.0. Right now, during the public beta, the builder is also free to use, including in production. See [pricing](https://ngxviewbuilder.io/#pricing) and [licensing terms](https://ngxviewbuilder.io/developers/licensing) for details as they're published.

## Community

This repository doesn't contain the source code. It exists so the community has a place to connect and shape the project.

### Found a bug?

Check open issues first, then open a new one describing what you were trying to do, what you expected, what actually happened, and steps to reproduce if you have them.

### Have an idea?

Open an issue with the `enhancement` label and describe the functionality and why it'd help.

### Want to discuss something?

Use the **Discussions** tab for anything that isn't a specific bug or feature request, questions, use cases, feedback on the roadmap.

### Code of conduct

Be respectful. Constructive feedback and a friendly tone are what make a community worth showing up to.

## Links

- Website: [ngxviewbuilder.io](https://ngxviewbuilder.io)
- Installation guide: [ngxviewbuilder.io/developers/installation](https://ngxviewbuilder.io/developers/installation)
- Pricing & licensing: [ngxviewbuilder.io/#pricing](https://ngxviewbuilder.io/#pricing)
- npm packages: [@ngxviewbuilder/runtime](https://www.npmjs.com/package/@ngxviewbuilder/runtime), [@ngxviewbuilder/designer](https://www.npmjs.com/package/@ngxviewbuilder/designer), [@ngxviewbuilder/plugin-templates](https://www.npmjs.com/package/@ngxviewbuilder/plugin-templates)
- Contact: [info@heydelabs.com](mailto:info@heydelabs.com) (Heyde Labs, MB)
- Issues: use this repository's **Issues** tab
- Discussions: use this repository's **Discussions** tab

Thanks for being part of the NGX View Builder community.
