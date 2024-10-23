
Services marked as `included by default` come with the `@codingame/monaco-vscode-api` package.

## Base `included by default`

 Service name: `@codingame/monaco-vscode-base-service-override`

- Contains some general-use services that are mandatory to most of the other features

## Monarch

- When textmate and theme service overrides are not used, it allows to restore some standalone features (Token inspection and toggle high contrast commands)

## Host `included by default`

 Service name: `@codingame/monaco-vscode-host-service-override`

- Interaction with the host/browser (shutdown veto, focus/active management, window opening, fullscreen...)

## Extensions `included by default`

 Service name: `@codingame/monaco-vscode-extensions-service-override`

- Support for VSCode extensions.
- A worker configuration can be provided to it:
  - Then, the webworker extension host will be available, allowing to run extensions in a worker which runs in an iframe

## Files `included by default`

 Service name: `@codingame/monaco-vscode-files-service-override`

- It adds the overlay filesystem for `file://` files, but also adds the support for lazy loaded extension files. It adds separate memory user files (e.g. config, keybindings), cache files and log files
- It supports adding overlay filesystems for `file://` files

## QuickAccess `included by default`

 Service name: `@codingame/monaco-vscode-quickaccess-service-override`

- Enables the quickaccess menu in the editor (press F1 or ctrl+shift+p)

## Notifications

 Service name: `@codingame/monaco-vscode-notifications-service-override`

- This services enables vscode notifications you usually find in the bottom right corner

## Dialogs

 Service name: `@codingame/monaco-vscode-dialogs-service-override`

- Enable VSCode modal dialogs. It allows users to select an action to do. Those actions are exposed to the VSCode API. Additionally, this service can be used by the language client to delegate questions to the user

## Model

 Service name: `@codingame/monaco-vscode-model-service-override`

- This service creates and takes care of model references. For example:
  - Create model from filesystem if content is unknown
  - Count references
  - Destroy models when they are no longer used

## Editor

 Service name: `@codingame/monaco-vscode-editor-service-override`

- Enable editor support. This is usually needed when working with the language server protocol. Without enabling the editor service, it will only be able to resolve the currently open model (only internal file references will work)
- Is exclusive with the `views` and `workbench` services. Do not use more than 1 services at the same time

## Views

 Service name: `@codingame/monaco-vscode-views-service-override`

- Enable full views support
- Is exclusive with the `editor` and `workbench` services. Do not use more than 1 service at the same time

## Configuration

 Service name: `@codingame/monaco-vscode-configuration-service-override`

- Allows to change the configuration of not only the editors, but every part of VSCode. The language client for instance uses it to send the requested configuration to the server. The default configuration service already allows to change the configuration. This service overrides makes it rely on a user configuration file (with json schema, overridable by language including all VSCode features)

## Keybindings

 Service name: `@codingame/monaco-vscode-keybindings-service-override`

- Enables platform specific keybindings and make it rely on a user definded keybindings configuration (if available)

## Languages

 Service name: `@codingame/monaco-vscode-languages-service-override`

- Enable language support. It's like the standalone service with 2 differences:
  - It handle the language extension point (getting languages from VSCode extensions)
  - It triggers the `onLanguage:${language}` event (to load VSCode extension listening to those events)

## Textmate

 Service name: `@codingame/monaco-vscode-textmate-service-override`

- Allows to use textmate grammars. Depends on *themes* service. VSCode extensions use textmate grammars exclusively for highlighting. Once this is enabled monarch grammars can no longer be loaded by monaco-editor

## TreeSitter

 Service name: `@codingame/monaco-vscode-treesitter-service-override`

- Experimental service which allows to use treesitter grammars, currently only optionally support typescript.

## Themes

 Service name: `@codingame/monaco-vscode-theme-service-override`

- Allows to use VSCode themes

## Snippets

 Service name: `@codingame/monaco-vscode-snippets-service-override`

- Add snippet extension point (register VSCode extension snippets)

## Debug

 Service name: `@codingame/monaco-vscode-debug-service-override`

- Activate debugging support

## Preferences

 Service name: `@codingame/monaco-vscode-preferences-service-override`

- Allow to read and write preferences

## Output

 Service name: `@codingame/monaco-vscode-output-service-override`

- Output panel support. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Terminal

 Service name: `@codingame/monaco-vscode-terminal-service-override`

- Terminal panel support. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Search

 Service name: `@codingame/monaco-vscode-search-service-override`

- search panel support. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Markers

 Service name: `@codingame/monaco-vscode-markers-service-override`

- It adds the problems panel tab. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## SCM

 Service name: `@codingame/monaco-vscode-scm-service-override`

- It adds the SCM API that can be used to implement source control. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Testing

 Service name: `@codingame/monaco-vscode-testing-service-override`

- It adds the Tests API. *Hint*: It makes more sense to enable it when *Views* or *Workbench* service are used

## Language detection worker

 Service name: `@codingame/monaco-vscode-language-detection-worker-service-override`

- When opening an untitled model or a file without extension or if VSCode is unable to guess the language simply by the file extension or by reading the first line. Then it will use tensorflow in a worker to try to guess the most probable language (only the open source model can be used)

## Storage

 Service name: `@codingame/monaco-vscode-storage-service-override`

- Define your own storage or use the default BrowserStorageService. The storage service is used in many places either as a cache or as a user preference store. For instance:
  - Current loaded theme is stored in there to be loaded faster on start
  - Every panel/view positions are stored in there

## LifeCycle

 Service name: `@codingame/monaco-vscode-lifecycle-service-override`

- Allow other services to veto a page reload (for instance when not all open files are saved)

## Remote agent

 Service name: `@codingame/monaco-vscode-remote-agent-service-override`

- Connect to a remote VSCode agent and have access to:
  - The remote filesystem
  - The remote file search
  - Running terminals
  - Running VSCode extensions (not web-compatible)
  - and probably more?

  Refer to [vscode_server.md](./docs/vscode_server.md) for the server part

## Accessibility

 Service name: `@codingame/monaco-vscode-accessibility-service-override`

- Register accessibility helpers and signals

## Workspace trust

 Service name: `@codingame/monaco-vscode-workspace-trust-service-override`

- Ask user it they trust the current workspace, disable some features if not

## Extension Gallery

 Service name: `@codingame/monaco-vscode-extension-gallery-service-override`

- Support for the VSCode marketplace, it allows to install extensions from the marketplace

## Chat

 Service name: `@codingame/monaco-vscode-chat-service-override`

- Support for chat and inline chat features

## Notebook

 Service name: `@codingame/monaco-vscode-notebook-service-override`

- Support for Jupyter notebooks

## Welcome

 Service name: `@codingame/monaco-vscode-welcome-service-override`

- Support for [viewsWelcome contribution point](https://code.visualstudio.com/api/references/contribution-points#contributes.viewsWelcome). *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Walkthrough

 Service name: `@codingame/monaco-vscode-walkthrough-service-override`

- Getting Started page and support for [walkthrough contribution point](https://code.visualstudio.com/api/references/contribution-points#contributes.walkthroughs). *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## User data profile

 Service name: `@codingame/monaco-vscode-user-data-profile-service-override`

- User profiles support

## User data sync

 Service name: `@codingame/monaco-vscode-user-data-sync-service-override`

- Support for user data sync. ⚠️ It can't really be used as it relies on a [closed source backend from microsoft](https://code.visualstudio.com/docs/editor/settings-sync#_can-i-use-a-different-backend-or-service-for-settings-sync) for the moment ⚠️

## Ai

 Service name: `@codingame/monaco-vscode-ai-service-override`

- Ai support for the ai extension api (RelatedInformation/EmbeddingVector)

## Task

 Service name: `@codingame/monaco-vscode-task-service-override`

- Task management

## Outline

 Service name: `@codingame/monaco-vscode-outline-service-override`

- Support for the outline view. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Timeline

 Service name: `@codingame/monaco-vscode-timeline-service-override`

- Support for the timeline view. *Hint*: It only makes sense to enable it when *Views* or *Workbench* service are used

## Workbench

 Service name: `@codingame/monaco-vscode-workbench-service-override`

- Allows to render the full workbench layout. Is exclusive with the `editor` and `views` service. Do not use more than 1 service at the same time

## Comments

- Enables comments extension api

## Edit-sessions

- Enable cloudchanges

## Emmet

- Enables the `triggerExpansionOnTab` command for the emmet default extension

## Interactive

- Interactive notebooks

## Issue

- Issue reporting

## Multi diff editor

- Multi diff editor support (<https://code.visualstudio.com/updates/v1_85#_multifile-diff-editor>)

## Performance

- Performance monitoring

## Relauncher

- Detects changes that require a reload (like settings change) and prompt the user for it

## Share

- Enables the share extension api

## Survey

- Survey/feedback support

## Update

- Update detection, release notes...

## Localization

- Register callbacks to update the display language from the VSCode UI (either from the `Set Display Language` command or from the extension gallery extension packs)

## Secret Storage

- Storage of secrets for extensions, will store by default in-memory. You can pass a custom implementation as part of the workbench construction options when initializing monaco services (under `secretStorageProvider`)
Additionally, several packages that include the VSCode version of some services (with some glue to make it work with monaco) are published:
