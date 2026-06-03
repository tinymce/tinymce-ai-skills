# Troubleshooting Quick Reference

> This file is referenced by the main SKILL.md when the user reports an issue after setup.

If the user reports an issue after setup, check these common problems:

| Symptom                                      | Likely Cause                                        | Fix                                                    |
|----------------------------------------------|-----------------------------------------------------|--------------------------------------------------------|
| Editor loads but no premium plugins           | Missing or invalid API key                          | Verify key in Tiny Dashboard; check browser console    |
| `tinymceai` plugin shows no UI               | Missing `tinymceai_token_provider` callback         | Add the trial token provider (Section 3 of SKILL.md)   |
| Comments plugin does nothing                  | Missing `tinycomments_mode`                         | Add `tinycomments_mode: 'embedded'`                    |
| Revision History button disabled              | Missing `revisionhistory_fetch`                     | Add `revisionhistory_fetch: () => Promise.resolve([])` |
| Merge Tags button hidden                      | Missing `mergetags_list`                            | Add `mergetags_list` array with sample tags            |
| "This domain is not registered" warning       | Domain not whitelisted in Tiny Dashboard            | Add domain at tiny.cloud/my-account/integrate          |
| Console error: `Failed to load plugin`        | Plugin not available on current plan                | Check plan at tiny.cloud or remove the plugin          |
| Editor doesn't appear at all                  | `selector` doesn't match any element on the page    | Verify the CSS selector matches an existing `<textarea>` or `<div>` |
| Image toolbar/menu conflicts or duplicates    | `uploadcare` and `image` both included              | Remove `image` and `editimage` from plugins when using `uploadcare` |
