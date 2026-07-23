# NGX View Builder — Community

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

```bash
npm install ngx-view-builder
```

Requirements:

- Angular 21+ (`@angular/core`, `@angular/common`, `@angular/cdk` as peer dependencies)
- Node.js 20+

The optional Templates plugin (the only officially supported plugin at the moment) installs separately and is version-locked to the core package:

```bash
npm install ngx-view-builder-plugin-templates
```

Full setup, including app config and runtime initialization, is in the [installation guide](https://ngxviewbuilder.io/developers/installation).

### Rendering a view (runtime)

This is genuinely all it takes to render a saved view for end users:

```ts
import { Component } from '@angular/core';
import { NgxViewBuilderRuntime } from 'ngx-view-builder';

@Component({
  selector: 'app-client-form',
  imports: [NgxViewBuilderRuntime],
  template: `<ngx-view-builder-runtime [pageJson]="structure" />`,
})
export class ClientFormComponent {
  structure = /* JSON produced by the builder, loaded from your backend */ {};
}
```

Read data back out with `runtime.getDataSnapshot()`, the `(valueChanged)` output, or by injecting `NgxViewBuilderApiService` and subscribing to `onComplete`. Details in [rendering views](https://ngxviewbuilder.io/developers/runtime-integration).

### Embedding the builder

Letting someone edit a view visually is just as small:

```ts
import { Component } from '@angular/core';
import { BuilderModel, IStructure, NgxViewBuilderBuilder } from 'ngx-view-builder';

@Component({
  selector: 'app-builder-page',
  imports: [NgxViewBuilderBuilder],
  template: `<ngx-view-builder-builder [model]="builderModel" (structureChanged)="onChange($event)" />`,
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
- An expression system for calculated fields, validation, and conditional visibility, including across nested tables and repeaters
- REST and route-based data sources
- An AI assistant built into the builder itself to help compose and extend views
- Angular-native output: standalone components and signals, rendered directly in your app

## License

NGX View Builder is closed source, distributed under a commercial license, not MIT/Apache/GPL. During the public beta (through version 1.0.0) the builder and runtime are free to use, including in production, with no license key required. Commercial licensing starts at 1.0.0; see [pricing](https://ngxviewbuilder.io/pricing) and [licensing terms](https://ngxviewbuilder.io/developers/licensing) for details as they're published.

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
- Pricing & licensing: [ngxviewbuilder.io/pricing](https://ngxviewbuilder.io/pricing)
- npm package: [ngx-view-builder](https://www.npmjs.com/package/ngx-view-builder)
- Issues: use this repository's **Issues** tab
- Discussions: use this repository's **Discussions** tab

Thanks for being part of the NGX View Builder community.
