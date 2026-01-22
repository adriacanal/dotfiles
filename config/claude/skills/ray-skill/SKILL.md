---
name: ray
description: Use when user says "send to Ray," "show in Ray," "debug in Ray," "log to Ray," "display in Ray," or wants to visualize data, debug output, or show diagrams in the Ray desktop application.
metadata:
  author: Spatie
  tags:
    - debugging
    - logging
    - visualization
    - ray
---

# Ray Skill

## Overview

Ray is Spatie's desktop debugging application for developers. This skill covers two ways to interact with Ray:

1. **MCP Tools** - Direct AI-to-Ray communication without writing code
2. **PHP API** - The `ray()` helper function and Ray class methods for use in PHP code

---

# Part 1: MCP Tools

Use these tools for direct communication with Ray - no PHP code needed.

### Core Concepts

**projectName and hostname**: Most Ray MCP tools require these parameters to target the correct Ray window.

- **projectName**: Identifies which project's Ray window to send output to
- **hostname**: The machine where Ray is running (defaults to `murzemacbook.local`)

**Always call `get_ray_windows` first** to discover available windows and their parameters.

## Discovery Tools

### get_ray_windows

Get all active Ray windows with their projectName and hostname.

**Always call this first** before using other Ray tools.

```
mcp__ray-app__get_ray_windows
```

### get_ray_window_logs

Read existing logs from a Ray window in a paginated manner.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| limit | No | 100 | Number of logs to return |
| offset | No | 0 | Starting position for pagination |

### get_ray_theme

Get the current Ray theme and color palette. Use this before generating styled HTML to match Ray's active theme.

Returns: theme name, dark/light mode, font family, and semantic colors for text, backgrounds, links, status indicators, tables, and code blocks.

## Output Tools

### send_custom_html_output

Display fully formatted HTML in Ray. This is the primary tool for rich output.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| html | Yes | - | Valid HTML content (CSS supported, no JavaScript) |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| llmName | No | - | Name of the LLM for attribution |
| appendToLogId | No | - | UUID to append as iteration to existing log (creates carousel) |

**Tips:**
- Use `get_ray_theme` first to style HTML to match Ray's theme
- JavaScript is not allowed, but all CSS is supported
- Use `appendToLogId` to show iterative updates to the same log entry

### send_custom_markdown_output

Display formatted Markdown content with full formatting support.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| markdown | Yes | - | Markdown content |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| llmName | No | - | Name of the LLM for attribution |

Supports: headings, lists, code blocks, tables, and more.

### send_mermaid_diagram

Display Mermaid diagrams for visualizations.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| mermaid | Yes | - | Valid Mermaid diagram code (without code fences) |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| llmName | No | - | Name of the LLM for attribution |

**Supported diagram types:**
- Flowcharts: `graph TD; A-->B;`
- Sequence diagrams
- Class diagrams
- State diagrams
- ER diagrams
- Gantt charts
- Pie charts

### send_ray_log

Send basic text or value logging.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| values | Yes | - | Array of strings or numbers to log |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| clipboardData | No | - | Data to copy when user clicks copy button |

### send_ray_custom_log

Send a custom log with a label and HTML content.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| content | Yes | - | HTML content to display |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| label | No | - | Label for the log entry |
| llmName | No | - | Name of the LLM (fallback for label) |

### send_ray_json_log

Display JSON with syntax highlighting and formatting.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| value | Yes | - | Valid JSON string |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

### send_ray_json_string_log

Display JSON as a plain string without parsing or formatting.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| value | Yes | - | JSON string to display |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

### send_ray_table_log

Display data in a formatted table.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| values | Yes | - | Object with keys as row labels, values as row content |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |
| label | No | - | Label for the table |
| llmName | No | - | Name of the LLM (fallback for label) |

## Organization Tools

### send_ray_separator

Add a visual divider between log entries.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

### send_ray_new_screen

Clear the current view and start a fresh screen.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| name | Yes | - | Name for the new screen |
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

### send_ray_clear_all

Remove all current log entries.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

## Notification & Control Tools

### send_ray_notification

Send an OS-level notification to the user. Does not require projectName or hostname.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| value | Yes | - | Notification message |
| llmName | Yes | - | Name of the LLM sending notification |

### send_ray_confetti

Shoot confetti animation in Ray. Use to celebrate completed tasks.

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| projectName | Yes | - | The project name |
| hostname | No | murzemacbook.local | The hostname |

### send_ray_show_app

Bring all Ray windows to the foreground. No parameters required.

### send_ray_hide_app

Hide all Ray windows. No parameters required.

## Best Practices

1. **Always start with discovery**: Call `get_ray_windows` to find the correct projectName and hostname before sending any output.

2. **Match Ray's theme**: Call `get_ray_theme` before generating styled HTML output to ensure your content matches the user's Ray theme.

3. **Organize with screens**: Use `send_ray_new_screen` to create logical sections when sending multiple related outputs.

4. **Use separators**: Add `send_ray_separator` between distinct pieces of information.

5. **Choose the right tool**:
   - Simple values: `send_ray_log`
   - Structured data: `send_ray_json_log` or `send_ray_table_log`
   - Rich formatting: `send_custom_markdown_output`
   - Complex layouts: `send_custom_html_output`
   - Visualizations: `send_mermaid_diagram`

6. **Iterative updates**: Use `appendToLogId` with `send_custom_html_output` to show progress on long-running tasks.

7. **Notify completion**: Use `send_ray_notification` for important events and `send_ray_confetti` for celebrations.

## Quick Reference

| I need to... | Use this tool |
|--------------|---------------|
| Find Ray windows | `get_ray_windows` |
| Read existing logs | `get_ray_window_logs` |
| Get theme colors | `get_ray_theme` |
| Log simple values | `send_ray_log` |
| Show JSON data | `send_ray_json_log` |
| Display a table | `send_ray_table_log` |
| Show Markdown | `send_custom_markdown_output` |
| Show rich HTML | `send_custom_html_output` |
| Draw a diagram | `send_mermaid_diagram` |
| Add a divider | `send_ray_separator` |
| Start fresh screen | `send_ray_new_screen` |
| Clear everything | `send_ray_clear_all` |
| Send notification | `send_ray_notification` |
| Celebrate | `send_ray_confetti` |
| Show Ray | `send_ray_show_app` |
| Hide Ray | `send_ray_hide_app` |

---

# Part 2: Direct HTTP API

Send data directly to Ray by making HTTP requests to its local server. This is what the `ray()` PHP function does under the hood.

## Connection Details

| Setting | Default | Environment Variable |
|---------|---------|---------------------|
| Host | `localhost` | `RAY_HOST` |
| Port | `23517` | `RAY_PORT` |
| URL | `http://localhost:23517/` | - |

## Request Format

**Method:** POST
**Content-Type:** `application/json`
**User-Agent:** `Ray 1.0`

### Basic Request Structure

```json
{
  "uuid": "unique-identifier-for-this-ray-instance",
  "payloads": [
    {
      "type": "log",
      "content": { },
      "origin": {
        "file": "/path/to/file.php",
        "line_number": 42,
        "hostname": "my-machine"
      }
    }
  ],
  "meta": {
    "ray_package_version": "1.0.0"
  }
}
```

### Fields

| Field | Type | Description |
|-------|------|-------------|
| `uuid` | string | Unique identifier for this Ray instance. Reuse the same UUID to update an existing entry. |
| `payloads` | array | Array of payload objects to send |
| `meta` | object | Optional metadata (ray_package_version, project_name, php_version) |

### Origin Object

Every payload includes origin information:

```json
{
  "file": "/Users/dev/project/app/Controller.php",
  "line_number": 42,
  "hostname": "dev-machine"
}
```

## Payload Types

### Log (Send Values)

```json
{
  "type": "log",
  "content": {
    "values": ["Hello World", 42, {"key": "value"}]
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Custom (HTML/Text Content)

```json
{
  "type": "custom",
  "content": {
    "content": "<h1>HTML Content</h1><p>With formatting</p>",
    "label": "My Label"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Table

```json
{
  "type": "table",
  "content": {
    "values": {"name": "John", "email": "john@example.com", "age": 30},
    "label": "User Data"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Color

Set the color of the preceding log entry:

```json
{
  "type": "color",
  "content": {
    "color": "green"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

**Available colors:** `green`, `orange`, `red`, `purple`, `blue`, `gray`

### Screen Color

Set the background color of the screen:

```json
{
  "type": "screen_color",
  "content": {
    "color": "green"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Label

Add a label to the entry:

```json
{
  "type": "label",
  "content": {
    "label": "Important"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Size

Set the size of the entry:

```json
{
  "type": "size",
  "content": {
    "size": "lg"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

**Available sizes:** `sm`, `lg`

### Notify (Desktop Notification)

```json
{
  "type": "notify",
  "content": {
    "value": "Task completed!"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### New Screen

```json
{
  "type": "new_screen",
  "content": {
    "name": "Debug Session"
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

### Measure (Timing)

```json
{
  "type": "measure",
  "content": {
    "name": "my-timer",
    "is_new_timer": true,
    "total_time": 0,
    "time_since_last_call": 0,
    "max_memory_usage_during_total_time": 0,
    "max_memory_usage_since_last_call": 0
  },
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

For subsequent measurements, set `is_new_timer: false` and provide actual timing values.

### Simple Payloads (No Content)

These payloads only need a `type` and empty `content`:

```json
{
  "type": "separator",
  "content": {},
  "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
}
```

| Type | Purpose |
|------|---------|
| `separator` | Add visual divider |
| `clear_all` | Clear all entries |
| `hide` | Hide this entry |
| `remove` | Remove this entry |
| `confetti` | Show confetti animation |
| `show_app` | Bring Ray to foreground |
| `hide_app` | Hide Ray window |

## Combining Multiple Payloads

Send multiple payloads in one request. Use the same `uuid` to apply modifiers (color, label, size) to a log entry:

```json
{
  "uuid": "abc-123",
  "payloads": [
    {
      "type": "log",
      "content": { "values": ["Important message"] },
      "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
    },
    {
      "type": "color",
      "content": { "color": "red" },
      "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
    },
    {
      "type": "label",
      "content": { "label": "ERROR" },
      "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
    },
    {
      "type": "size",
      "content": { "size": "lg" },
      "origin": { "file": "test.php", "line_number": 1, "hostname": "localhost" }
    }
  ],
  "meta": {}
}
```

## Example: Complete Request

Send a green, labeled log message:

```bash
curl -X POST http://localhost:23517/ \
  -H "Content-Type: application/json" \
  -H "User-Agent: Ray 1.0" \
  -d '{
    "uuid": "my-unique-id-123",
    "payloads": [
      {
        "type": "log",
        "content": {
          "values": ["User logged in", {"user_id": 42, "name": "John"}]
        },
        "origin": {
          "file": "/app/AuthController.php",
          "line_number": 55,
          "hostname": "dev-server"
        }
      },
      {
        "type": "color",
        "content": { "color": "green" },
        "origin": { "file": "/app/AuthController.php", "line_number": 55, "hostname": "dev-server" }
      },
      {
        "type": "label",
        "content": { "label": "Auth" },
        "origin": { "file": "/app/AuthController.php", "line_number": 55, "hostname": "dev-server" }
      }
    ],
    "meta": {
      "project_name": "my-app"
    }
  }'
```

## Availability Check

Before sending data, you can check if Ray is running:

```
GET http://localhost:23517/_availability_check
```

Ray responds with HTTP 404 when available (the endpoint doesn't exist, but the server is running).

## Payload Type Reference

| Type | Content Fields | Purpose |
|------|----------------|---------|
| `log` | `values` (array) | Send values to Ray |
| `custom` | `content`, `label` | HTML or text content |
| `table` | `values`, `label` | Display as table |
| `color` | `color` | Set entry color |
| `screen_color` | `color` | Set screen background |
| `label` | `label` | Add label to entry |
| `size` | `size` | Set entry size (sm/lg) |
| `notify` | `value` | Desktop notification |
| `new_screen` | `name` | Create new screen |
| `measure` | `name`, `is_new_timer`, timing fields | Performance timing |
| `separator` | (empty) | Visual divider |
| `clear_all` | (empty) | Clear all entries |
| `hide` | (empty) | Hide entry |
| `remove` | (empty) | Remove entry |
| `confetti` | (empty) | Confetti animation |
| `show_app` | (empty) | Show Ray window |
| `hide_app` | (empty) | Hide Ray window |
