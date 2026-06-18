# The Configuration File

Each [notebook](../notes/notebook.md) contains a configuration file used to customize your experience with `zk`. This file is located at `.zk/config.toml` and uses the [TOML format](https://github.com/toml-lang/toml). It is composed of several optional sections:

* `[notebook]` configures the [default notebook](config-notebook.md)
* `[note]` sets the [note creation rules](config-note.md)
* `[extra]` contains free [user variables](config-extra.md) which can be expanded in templates
* `[group]` defines [note groups](config-group.md) with custom rules
* `[format]` configures the [note format settings](../notes/note-format.md), such as Markdown options
* `[tool]` customizes interaction with external programs such as:
    * [your default editor](tool-editor.md)
    * [your default shell](tool-shell.md)
    * [your default pager](tool-pager.md)
    * [`fzf`](tool-fzf.md)
* `[lsp]` setups the [Language Server Protocol settings](config-lsp.md) for [editors integration](../tips/editors-integration.md)
* `[filter]` declares your [named filters](config-filter.md)
* `[alias]` holds your [command aliases](config-alias.md)

## Global configuration file

You can also create a global configuration file to share aliases and settings across several notebooks. The default location is `~/.config/zk/config.toml`. You can override the location of the directory by setting `ZK_CONFIG_DIR`, or follow the XDG Base Directory specification by setting [`XDG_CONFIG_HOME`](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html).

Notebook configuration files will inherit the settings defined in the global configuration file. You can also share templates by storing them under a `templates/` subdirectory, e.g. `~/.config/zk/templates/` or `$ZK_CONFIG_DIR/templates/`.

## Complete example

Here's an example of a complete configuration file:

```toml
# NOTEBOOK SETTINGS
[notebook]
# Only applicable for the global config file
dir = "~/notebook"

# NOTE SETTINGS
#
# Defines the default options used when generating new notes.
[note]

# Language used when writing notes.
# This is used to generate slugs or with date formats.
language = "en"

# The default title used for new note, if no `--title` flag is provided.
default-title = "Untitled"

# Template used to generate a note's filename, without extension.
filename = "{{id}}"

# The file extension used for the notes.
extension = "md"

# Template used to generate a note's content.
# If not an absolute path or "~/unix/path", it's relative to .zk/templates/
template = "default.md"

# Path globs ignored while indexing existing notes.
exclude = [
    "drafts/*",
    "log.md"
]

# Configure random ID generation.

# The charset used for random IDs. You can use:
#   * letters: only letters from a to z.
#   * numbers: 0 to 9
#   * alphanum: letters + numbers
#   * hex: hexadecimal, from a to f and 0 to 9
#   * custom string: will use any character from the provided value
id-charset = "alphanum"

# Length of the generated IDs.
id-length = 4

# Letter case for the random IDs, among lower, upper or mixed.
id-case = "lower"


# EXTRA VARIABLES
#
# A dictionary of variables you can use for any custom values when generating
# new notes. They are accessible in templates with {{extra.<key>}}
[extra]

key = "value"


# GROUP OVERRIDES
#
# You can override global settings from [note] and [extra] for a particular
# group of notes by declaring a [group."<name>"] section.
#
# Specify the list of directories which will automatically belong to the group
# with the optional `paths` property.
#
# Omitting `paths` is equivalent to providing a single path equal to the name of
# the group. This can be useful to quickly declare a group by the name of the
# directory it applies to.

[group."<NAME>"]
paths = ["<DIR1>", "<DIR2>"]

[group."<NAME>".note]
filename = "{{format-date now}}"

[group."<NAME>".extra]
key = "value"


# MARKDOWN SETTINGS
[format.markdown]

# Format used to generate links between notes.
# Either "wiki", "markdown" or a custom template. Default is "markdown".
link-format = "wiki"

# Indicates whether a link's path will be percent-encoded.
# Defaults to true for "markdown" format and false for "wiki" format.
link-encode-path = true

# Indicates whether a link's path file extension will be removed.
# Defaults to true.
link-drop-extension = true

# Enable support for #hashtags.
hashtags = true

# Enable support for :colon:separated:tags:.
colon-tags = false

# Enable support for Bear's #multi-word tags#
# Hashtags must be enabled for multi-word tags to work.
multiword-tags = false


# EXTERNAL TOOLS
[tool]

# Default editor used to open notes. When not set, the EDITOR or VISUAL
# environment variables are used.
editor = "vim"

# Pager used to scroll through long output. If you want to disable paging
# altogether, set it to an empty string "".
pager = "less -FIRX"

# Command used to preview a note during interactive fzf mode.
# Set it to an empty string "" to disable preview.

# bat is a great tool to render Markdown document with syntax highlighting.
# https://github.com/sharkdp/bat
fzf-preview = "bat -p --color always {-1}"


# LSP
#
# Configure basic editor integration for LSP-compatible editors.
# See https://zk-org.github.io/zk/tips/editors-integration.html
[lsp]

[lsp.diagnostics]
# Each diagnostic can have for value: none, hint, info, warning, error

# Report titles of wiki-links as hints.
wiki-title = "hint"

# Warn for dead links between notes.
dead-link = "error"

# Warn when a note links to itself.
self-link = "warning"

# Report missing backlinks
missing-backlink = { level = "hint", position = "bottom" }


[lsp.completion]
# Customize the completion pop-up of your LSP client.

# Show the note title in the completion pop-up, or fallback on its path if empty.
note-label = "{{title-or-path}}"

# Filter out the completion pop-up using the note title or its path.
note-filter-text = "{{title}} {{path}}"

# Show the note filename without extension as detail.
note-detail = "{{filename-stem}}"


# NAMED FILTERS
#
# A named filter is a set of note filtering options used frequently together.
[filter]

# Matches the notes created the last two weeks. For example:
#    $ zk list recents --limit 15
#    $ zk edit recents --interactive
recents = "--sort created- --created-after 'last two weeks'"


# COMMAND ALIASES
#
# Aliases are user commands called with `zk <alias> [<flags>] [<args>]`.
# The alias will be executed with `$SHELL -c`
[alias]
# Here are a few aliases to get you started.

# Shortcut to a command.
ls = "zk list $@"

# Default flags for an existing command.
list = "zk list --quiet $@"

# Edit the last modified note.
editlast = "zk edit --limit 1 --sort modified- $@"

# Edit notes selected interactively among the notes created in the last two weeks.
recent = "zk edit --sort created- --created-after 'last two weeks' --interactive"

# Print paths separated with colons for the notes found with given arguments.
path = "zk list --quiet --format {{path}} --delimiter , $@"

# Show a random note.
lucky = "zk list --quiet --format full --sort random --limit 1"

# Returns the Git history for the notes found with the given arguments.
hist = "zk list --format path --delimiter0 --quiet $@ | xargs -t -0 git log --patch --"

# Edit this configuration file.
conf = '$EDITOR "$ZK_NOTEBOOK_DIR/.zk/config.toml"'
```
