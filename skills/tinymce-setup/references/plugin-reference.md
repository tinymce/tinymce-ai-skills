# Plugin Reference

> This file is referenced by the main SKILL.md during code generation. Read it before producing the final code snippet.

## Toolbar Button Quick Reference

Use these exact identifiers when building the `toolbar` string:

| Plugin              | Toolbar Button ID(s)                                              |
|---------------------|-------------------------------------------------------------------|
| TinyMCE AI          | `tinymceai-chat`, `tinymceai-quickactions`, `tinymceai-review`    |
| Comments            | `addcomment`                                                      |
| Revision History    | `revisionhistory`                                                 |
| Suggested Edits     | `suggestededits`                                                  |
| Export to PDF       | `exportpdf`                                                       |
| Export to Word      | `exportword`                                                      |
| Import from Word    | `importword`                                                      |
| Accessibility       | `a11ycheck`                                                       |
| Spell Checker Pro   | `spellchecker`                                                    |
| Enhanced Code Editor| `code` (overrides core code plugin)                               |
| Advanced Tables     | `advtablerownumbering`                                            |
| Media Optimizer     | `uploadcare`                                                      |
| Merge Tags          | `mergetags`                                                       |
| Format Painter      | `formatpainter`                                                   |
| Case Change         | `casechange`                                                      |
| Footnotes           | `footnotes`                                                       |
| Table of Contents   | `tableofcontents`                                                 |
| Permanent Pen       | `permanentpen`                                                    |
| Checklist           | `checklist`                                                       |
| Math                | `math`                                                            |

## TinyMCE AI Quick Action Customization

For industry-specific setups, the AI quick actions menu can be tailored using these options:

| Option | Default | Description |
|--------|---------|-------------|
| `tinymceai_quickactions_menu` | All items | Array of quick action groups shown in the menu |
| `tinymceai_quickactions_chat_prompts` | Explain, Summarize, Highlight Key Points | Chat command submenu items |
| `tinymceai_quickactions_change_tone_menu` | Casual, Direct, Friendly, Confident, Professional | Tone options |

These are optional — omit them for the default experience. Only include if the user requests specific AI customization.

## Merge Tags Sample Data

When `mergetags` is included, always provide a realistic `mergetags_list` with nested categories:

```js
mergetags_list: [
  { title: 'Contact', menu: [
    { value: 'Contact.FirstName', title: 'First Name' },
    { value: 'Contact.LastName', title: 'Last Name' },
    { value: 'Contact.Email', title: 'Email' }
  ]},
  { title: 'Company', menu: [
    { value: 'Company.Name', title: 'Company Name' },
    { value: 'Company.Address', title: 'Address' }
  ]}
]
```

Adapt the category names and values to the user's stated industry/use case (e.g., "Patient" for healthcare, "Student" for education, "Customer" for e-commerce).

## Framework Integration Packages

When generating code for frameworks, use the correct integration package:

| Framework   | Package                        | Import                                       |
|-------------|--------------------------------|----------------------------------------------|
| React       | `@tinymce/tinymce-react`       | `import { Editor } from '@tinymce/tinymce-react';` |
| Vue 3       | `@tinymce/tinymce-vue`         | `import Editor from '@tinymce/tinymce-vue';`  |
| Angular     | `@tinymce/tinymce-angular`     | `import { EditorModule } from '@tinymce/tinymce-angular';` |
| Svelte      | `@tinymce/tinymce-svelte`      | `import Editor from '@tinymce/tinymce-svelte';` |
| Blazor      | `TinyMCE.Blazor`               | NuGet package                                |
| Web Component | `@tinymce/tinymce-webcomponent` | `<tinymce-editor>` element                 |

For framework integrations, the Cloud CDN is loaded automatically by the integration package when you provide an `apiKey` prop. No additional dependency installation is needed beyond the integration package itself.

## Environment Variable Patterns

When reading the API key from environment variables, use **only** the pattern that matches the detected build tool. Never mix patterns in the same file — cross-framework fallbacks cause runtime errors (e.g., `process` is not defined in Vite).

| Build Tool   | Env Prefix         | Access Pattern                                  |
|--------------|--------------------|-------------------------------------------------|
| Vite         | `VITE_`            | `import.meta.env.VITE_TINYMCE_API_KEY`          |
| Next.js      | `NEXT_PUBLIC_`     | `process.env.NEXT_PUBLIC_TINYMCE_API_KEY`       |
| CRA          | `REACT_APP_`       | `process.env.REACT_APP_TINYMCE_API_KEY`         |
| Angular CLI  | N/A                | Use `environment.ts` files                       |
| Nuxt         | Runtime config     | `useRuntimeConfig().public.tinymceApiKey`        |
| Plain HTML   | N/A                | Hardcode in script or use a server-rendered variable |

The `.env` file should contain **only** the variable matching the build tool in use. Do not add variables for other frameworks.
