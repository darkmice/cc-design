# Platform Tool Reference

This reference documents the tools available in the Claude Artifacts design environment. Read this when you need specifics on tool parameters and behavior.

## File Operations

| Tool | Purpose |
|---|---|
| `read_file` | Read file contents (up to 2000 lines; use offset/limit to paginate) |
| `write_file` | Write content to a file (creates or overwrites). Pass `asset: "<name>"` for user-facing deliverables |
| `list_files` | List files/directories (up to 200 results per call) |
| `copy_files` | Copy files/folders recursively; supports cross-project via `/projects/<id>/<path>` |
| `delete_file` | Delete files or folders recursively |
| `str_replace_edit` | Edit files by replacing unique strings. Always prefer over write_file for edits |

## Preview & Screenshots

| Tool | Purpose |
|---|---|
| `show_html` | Open HTML in YOUR preview iframe (not user's pane) |
| `show_to_user` | Open file in user's tab bar to direct their attention |
| `done` | End-of-turn: open file for user + return console errors. Must call before `fork_verifier_agent` |
| `save_screenshot` | Take screenshots (disk or in-memory PNG blobs for PPTX export) |
| `multi_screenshot` | Multiple screenshots with JS snippets between captures (max 12 steps) |
| `view_image` | Load and see image contents (auto-resized to 1000px) |
| `image_metadata` | Get image dimensions, format, transparency info |

## Verification & JS Execution

| Tool | Purpose |
|---|---|
| `fork_verifier_agent` | Background subagent that checks output in its own iframe. Silent on pass |
| `eval_js_user_view` | Execute JS in user's preview (only when you need their live state) |
| `screenshot_user_view` | Screenshot user's preview (only for live media/stream states) |
| `get_webview_logs` | Get console logs from your preview |

## Scripting & Export

| Tool | Purpose |
|---|---|
| `run_script` | Async JS sandbox for batch operations, image manipulation, programmatic file generation |
| `copy_starter_component` | Drop ready-made scaffolds (device frames, deck shells, animation engines) |
| `gen_pptx` | Export deck to .pptx (editable or screenshots mode) |
| `super_inline_html` | Bundle HTML + assets into single self-contained file |
| `open_for_print` | Open HTML for Print-to-PDF |
| `invoke_skill` | Load a built-in skill's instructions by name |

## Cross-Project Access

Prefix paths with `/projects/<projectId>/` to read from another project (read-only). Parse project URLs: `.../p/<projectId>?file=<encodedPath>`.

## Claude API from HTML

HTML artifacts can call Claude without SDK or API key:

```html
<script>
(async () => {
  const text = await window.claude.complete("Summarize this: ...");
  // or with messages array:
  const text2 = await window.claude.complete({
    messages: [{ role: 'user', content: '...' }],
  });
})();
</script>
```

Uses `claude-haiku-4-5` with 1024-token output cap. Rate-limited per user.
