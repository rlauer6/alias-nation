# NAME

show-aliases.pl - A categorized viewer for Bash aliases

# SYNOPSIS

    show-aliases.pl [options] [category]

    # Show all aliases
    show-aliases.pl

    # Show only Docker aliases
    show-aliases.pl -C Docker

    # Include the 'Junk Drawer' stashed aliases
    show-aliases.pl --local

[screenshot](https://github.com/rlauer6/alias-nation/blob/main/screenshot.png)

# OPTIONS

- **--help, -h**

    Display this help documentation.

- **--category, -C**

    Filter the output by a specific category found in `~/.bash_aliases`.

- **--color, --no-color, -c**

    Enable or disable ANSI colorized output. Defaults to enabled.

- **--local, -l**

    Include `~/.local-aliases` (the "Junk Drawer") in the output.

- **--random, -r**

    Select and display a single "Alias o' the Day" for periodic review.

- **--width, -w**

    Set the manual word-wrap width for the command column. Default is 100.

See `perldoc show-aliases.pl` for more details.

# DESCRIPTION

This script parses standard Bash startup files (`.bashrc`,
`.profile`, and `.bash_aliases`) to display aliases in a scannable
ASCII table.

If aliases are organized under headings (e.g., `# Docker`) in
`.bash_aliases`, they will be grouped accordingly in the output.

# SHELL INTEGRATION

To use the "Junk Drawer" workflow, add the following function to your 
`~/.bashrc` or `~/.functions` file:

    stash() {
        local name="$1"
        shift
        local cmd="$*"

        if [[ -z "$name" || -z "$cmd" ]]; then
            echo "Usage: stash <name> <command>" >&2
            return 1
        fi

        # 1. Enable for the current shell immediately
        alias "$name"="$cmd"

        # 2. Append to the junk drawer as a comment
        {
            echo "# Added: $(date +%Y-%m-%d) in $PWD"
            echo "# alias $name='$cmd'"
            echo ""
        } >> "$HOME/.local-aliases"

        echo "Nugget '$name' active and stashed in ~/.local-aliases"
    }

# THE JUNK DRAWER WORKFLOW

The "Junk Drawer" (`~/.local-aliases`) handles project-specific
commands that are too repetitive to type but too specialized for the
permanent toolbox (`~/.bash_aliases`) that are automatically loaded
when enter a new shell.

By using the `stash` function, you create a "Desire Path" of automation 
that can be reviewed later. Use `show-aliases.pl --local` 
to view these stashed nuggets.

# AUTHOR

Rob Lauer - <rlauer@treasurersbriefcase.com>

# SEE ALSO

[Text::ASCIITable](https://metacpan.org/pod/Text%3A%3AASCIITable), [Term::ANSIColor](https://metacpan.org/pod/Term%3A%3AANSIColor), [Text::Wrap](https://metacpan.org/pod/Text%3A%3AWrap)
