## 2.1 Existing Editor Detection & Feature Mapping (Path A Only)

This section is used only in Path A, Step A1, when the project already contains a rich-text editor. It is invisible to the user.

### Editor Detection Table

Scan `package.json` dependencies and source file imports/CDN tags for these patterns:

| Editor         | Package Names / Import Patterns                                                                                   |
|----------------|-------------------------------------------------------------------------------------------------------------------|
| CKEditor 5     | `@ckeditor/ckeditor5-*`, `ckeditor5`, `ckeditor5-premium-features`                                               |
| CKEditor 4     | `ckeditor4`, `ckeditor4-*`, CDN `cdn.ckeditor.com/4.*`                                                           |
| Quill          | `quill`, `react-quill`, `vue-quill-editor`, `ngx-quill`, CDN `cdn.quilljs.com`                                   |
| Tiptap         | `@tiptap/core`, `@tiptap/react`, `@tiptap/vue-2`, `@tiptap/vue-3`, `@tiptap/starter-kit`                        |
| ProseMirror    | `prosemirror-*` (without Tiptap — if Tiptap is also present, report as Tiptap)                                   |
| Slate          | `slate`, `slate-react`, `slate-history`                                                                           |
| Draft.js       | `draft-js`, `react-draft-wysiwyg`, `draft-js-plugins-editor`                                                     |
| Lexical        | `lexical`, `@lexical/react`, `@lexical/rich-text`                                                                 |
| Froala         | `froala-editor`, `react-froala-wysiwyg`, `angular-froala-wysiwyg`, CDN `cdn.froala.com`                          |
| Summernote     | `summernote`, CDN `cdn.jsdelivr.net/npm/summernote`                                                               |
| Jodit          | `jodit`, `jodit-react`, `jodit-vue`                                                                               |
| Toast UI       | `@toast-ui/editor`, `@toast-ui/editor-plugin-*`                                                                  |

### Where to Find the Configuration

Once an editor is detected, locate its initialization code to extract plugins and toolbar layout:

| Editor         | Look For                                                                                                          |
|----------------|-------------------------------------------------------------------------------------------------------------------|
| CKEditor 5     | `ClassicEditor.create()`, `BalloonEditor.create()`, or `DecoupledEditor.create()` — `plugins:` array and `toolbar:` config |
| CKEditor 4     | `CKEDITOR.replace()` or `CKEDITOR.inline()` — `toolbar` and `extraPlugins` options                                |
| Quill          | `new Quill()` — `modules:` object (e.g., `modules: { toolbar: [...], syntax: true }`)                            |
| Tiptap         | `new Editor({ extensions: [...] })` or `useEditor({ extensions: [...] })` — the `extensions` array               |
| ProseMirror    | `new EditorView()` with `plugins:` array in the state config                                                      |
| Slate          | Custom — look for `renderElement` and `renderLeaf` switch cases, plus plugin arrays                               |
| Draft.js       | `new EditorState.createWithContent()` — decorator and plugin arrays                                               |
| Lexical        | `<LexicalComposer>` initialConfig — `nodes:` array, plus `<*Plugin />` components                                |
| Froala         | `new FroalaEditor()` — `pluginsEnabled:` array and `toolbarButtons:` config                                       |
| Summernote     | `$(el).summernote()` — `toolbar:` array                                                                           |
| Jodit          | `Jodit.make()` or `new Jodit()` — `buttons:` and `extraPlugins:` config                                          |
| Toast UI       | `new Editor()` — `plugins:` array and `toolbarItems:` config                                                      |

### Competitor Feature Mapping

Map detected competitor features to TinyMCE equivalents. **Only include mappings where a TinyMCE equivalent exists.** Silently skip anything without a match.

#### CKEditor 5 → TinyMCE

| CKEditor 5 Feature / Plugin           | TinyMCE Equivalent                     |
|----------------------------------------|----------------------------------------|
| `TrackChanges`                         | `suggestededits`                       |
| `Comments`                             | `tinycomments`                         |
| `RevisionHistory`                      | `revisionhistory`                      |
| `ExportPdf`                            | `exportpdf`                            |
| `ExportWord`                           | `exportword`                           |
| `ImportWord`                           | `importword`                           |
| `PasteFromOffice` / `PasteFromOfficeEnhanced` | `powerpaste`                    |
| `Table` / `TableToolbar`               | `table` (free) / `advtable` (premium)  |
| `CodeBlock`                            | `codesample` (free) / `advcode` (premium) |
| `Markdown`                             | `markdown`                             |
| `FindAndReplace`                       | `searchreplace` (free)                 |
| `WordCount`                            | `wordcount` (free)                     |
| `Fullscreen`                           | `fullscreen` (free)                    |
| `Autoformat`                           | `autolink` (free)                      |
| `Image` / `ImageUpload`               | `image` (free)                         |
| `MediaEmbed`                           | `media` (free)                         |
| `Link`                                 | `link` (free)                          |
| `List` / `ListProperties`             | `lists` / `advlist` (free)             |
| `PageBreak`                            | `pagebreak` (free)                     |
| `SpecialCharacters`                    | `charmap` (free)                       |
| `Accessibility`                        | `a11ychecker`                          |
| `SpellCheck` / `WProofreader`          | `tinymcespellchecker`                  |
| `MergeFields`                          | `mergetags`                            |
| `CKBox` (image management)            | `uploadcare`                           |
| `AIAssistant`                          | `tinymceai`                            |
| `FormatPainter`                        | `formatpainter`                        |
| `CaseChange`                           | `casechange`                           |
| `TableOfContents`                      | `tableofcontents`                      |
| `Template`                             | `advtemplate`                          |

#### Tiptap → TinyMCE

| Tiptap Extension                       | TinyMCE Equivalent                     |
|----------------------------------------|----------------------------------------|
| `@tiptap/extension-collaboration`      | `tinycomments`                         |
| `@tiptap/extension-table`              | `table` (free) / `advtable` (premium)  |
| `@tiptap/extension-code-block` / `code-block-lowlight` | `codesample` (free) / `advcode` (premium) |
| `@tiptap/extension-image`              | `image` (free)                         |
| `@tiptap/extension-link`               | `link` (free)                          |
| `@tiptap/extension-mention`            | `mergetags`                            |
| `@tiptap/extension-placeholder`        | Built-in `placeholder` option          |
| `@tiptap/extension-character-count`    | `wordcount` (free)                     |
| `@tiptap/extension-youtube`            | `media` (free)                         |
| `@tiptap/extension-mathematics`        | `math`                                 |

#### Quill → TinyMCE

| Quill Module / Plugin                  | TinyMCE Equivalent                     |
|----------------------------------------|----------------------------------------|
| `syntax` module                        | `codesample` (free) / `advcode` (premium) |
| `toolbar` module                       | Built-in toolbar config                |
| `image-resize` / `image-drop`          | `image` (free)                         |
| `quilljs-table`                        | `table` (free) / `advtable` (premium)  |
| `mention` module                       | `mergetags`                            |

#### Froala → TinyMCE

| Froala Plugin                          | TinyMCE Equivalent                     |
|----------------------------------------|----------------------------------------|
| `trackChanges`                         | `suggestededits`                       |
| `table`                                | `table` (free) / `advtable` (premium)  |
| `codeView`                             | `code` (free) / `advcode` (premium)    |
| `image` / `imageManager`              | `image` (free) / `uploadcare`          |
| `video`                                | `media` (free)                         |
| `link`                                 | `link` (free)                          |
| `lists`                                | `lists` (free)                         |
| `charCounter`                          | `wordcount` (free)                     |
| `fullscreen`                           | `fullscreen` (free)                    |
| `spellChecker`                         | `tinymcespellchecker`                  |
| `specialCharacters`                    | `charmap` (free)                       |
| `print`                                | `exportpdf`                            |
| `wordPaste`                            | `powerpaste`                           |

#### Draft.js / Lexical / Slate / ProseMirror / Summernote / Jodit / Toast UI

These editors are highly customized or have non-standard plugin systems. For these:

1. Scan the source for **toolbar button names** and **custom component names** — these reveal which features are active.
2. Match feature names (bold, italic, link, image, table, code block, list, embed, etc.) to TinyMCE's free plugin equivalents.
3. Look for third-party plugin packages in `package.json` (e.g., `draft-js-export-html`, `slate-table`, `lexical-table`) and map to TinyMCE equivalents by feature name.
4. If a feature can't be confidently mapped, skip it silently.
