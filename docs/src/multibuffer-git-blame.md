# Multibuffer Git Blame

The Zed editor now supports git blame functionality in multibuffers. This feature allows you to see git blame information when working with multiple files simultaneously, such as when viewing search results across your codebase or viewing diagnostics from multiple files.

## What is Multibuffer Git Blame?

Multibuffer git blame extends the existing single-file git blame feature to work with multibuffers. A multibuffer is a view that combines excerpts from multiple files into a single editor, commonly used for:

- Search results across multiple files
- Diagnostic views showing errors/warnings from multiple files
- Custom workflows that combine content from different files

## How It Works

When you enable git blame in a multibuffer view, the editor will:

1. **Automatically detect multiple buffers**: The system recognizes when you're working with a multibuffer instead of a single file
2. **Generate blame data per file**: Each file in the multibuffer gets its own git blame information
3. **Display contextual blame info**: The blame gutter and inline blame show information relevant to each specific file and line
4. **Handle mixed repositories**: If files come from different git repositories, each gets blame data from its respective repository

## Usage

The multibuffer git blame feature uses the same commands and UI as single-file git blame:

### Toggle Git Blame Gutter
- **Command**: `editor: toggle git blame`
- **Default Keybinding**: None (can be configured)
- Shows commit information in the left gutter for each line

### Toggle Inline Git Blame
- **Command**: `editor: toggle git blame inline`
- **Default Keybinding**: None (can be configured)
- Shows commit information inline after the current line

### Open Git Blame Commit
- **Command**: `editor: open git blame commit`
- **Default Keybinding**: None (can be configured)
- Opens detailed information about the commit for the current line

## Features

### Gutter Display
The git blame gutter shows:
- Author name (abbreviated if necessary)
- Commit SHA (shortened)
- Relative timestamp (e.g., "2 hours ago")
- Color-coded by commit for visual grouping

### Inline Display
The inline git blame shows:
- Brief commit information after the current line
- Appears after a configurable delay
- Only shows when the line has focus

### Commit Details
Clicking on blame information or using the "open git blame commit" command shows:
- Full commit message
- Author and committer information
- Commit timestamp
- Link to the commit in your git hosting provider (if configured)

## Implementation Details

The multibuffer git blame feature is implemented using:

- **GitMultiBufferBlame**: A new structure that manages blame data for multiple buffers
- **Per-buffer blame tracking**: Each buffer in the multibuffer maintains its own blame information
- **Efficient rendering**: Only visible lines are processed for blame information
- **Event-driven updates**: Blame data is automatically updated when files change

## Limitations

- Git blame is only available for files that are part of a git repository
- Very large files may have delayed blame generation
- Blame information requires the file to be saved (unsaved changes won't show blame data)
- Performance may be impacted when working with many large files simultaneously

## Configuration

Multibuffer git blame respects the same settings as single-file git blame:

```json
{
  "git": {
    "inline_blame_enabled": true,
    "inline_blame_delay": 600
  }
}
```

- `inline_blame_enabled`: Enable inline blame by default
- `inline_blame_delay`: Delay in milliseconds before showing inline blame (null for no delay)

## Troubleshooting

If git blame isn't working in multibuffers:

1. **Check file status**: Ensure files are saved and part of a git repository
2. **Verify git availability**: Make sure git is installed and accessible
3. **Check repository health**: Ensure the git repository isn't corrupted
4. **Performance considerations**: Large multibuffers may take time to generate blame data

## Technical Implementation

The multibuffer git blame feature extends the existing `GitBlame` implementation with:

- `GitMultiBufferBlame`: Manages blame data for multiple buffers
- `BufferBlameData`: Per-buffer blame information storage
- Enhanced rendering logic that determines which buffer each line belongs to
- Automatic buffer detection and blame generation

This ensures that the multibuffer experience is seamless and consistent with single-file git blame functionality.