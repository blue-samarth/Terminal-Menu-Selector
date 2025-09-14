# Universal Menu Selector - Complete Documentation

## Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [Architecture](#architecture)
4. [Terminal Compatibility](#terminal-compatibility)
5. [Installation](#installation)
6. [Usage Guide](#usage-guide)
7. [API Reference](#api-reference)
8. [Menu Types](#menu-types)
9. [Environment Detection](#environment-detection)
10. [Color System](#color-system)
11. [Error Handling](#error-handling)
12. [Examples](#examples)
13. [Integration Patterns](#integration-patterns)
14. [Troubleshooting](#troubleshooting)
15. [Best Practices](#best-practices)

## Overview

The Universal Menu Selector (`menu_selector.sh`) is a sophisticated Bash library that provides interactive menu functionality across different terminal environments. It automatically detects terminal capabilities and adapts its behavior to provide the best possible user experience.

### Key Characteristics
- **Universal Compatibility**: Works on macOS Terminal, iTerm2, WSL, Linux terminals, Git Bash, and more
- **Intelligent Detection**: Automatically detects terminal capabilities and chooses optimal display method
- **Zero Dependencies**: Pure Bash implementation with no external requirements
- **Color Support**: Comprehensive color handling with fallbacks for limited terminals
- **Multiple Interface Types**: Advanced, simple, numbered, and macOS-optimized menu types

### Version Information
- **Author**: blue-Samarth
- **Version**: 3.4
- **License**: MIT
- **File Size**: 637 lines of code

## Features

### Core Features
1. **Adaptive Menu Types**
   - Advanced menu with real-time highlighting
   - Simple menu with clear screen updates
   - Numbered menu for maximum compatibility
   - macOS Terminal optimized menu

2. **Terminal Capability Detection**
   - ANSI color support detection
   - Cursor movement capability detection
   - Platform-specific optimizations
   - Interactive terminal detection

3. **Color and Formatting System**
   - 8-color ANSI palette support
   - Text formatting (bold, underline, reverse)
   - Automatic fallback for non-color terminals
   - Platform-specific color handling

4. **Input Handling**
   - Arrow key navigation
   - WASD key navigation
   - Enter/Return selection
   - Graceful cancellation (q/Q/Ctrl+C)

5. **Display Options**
   - Separate display text and return values
   - Dynamic option highlighting
   - Status messages and feedback
   - Clear menu cleanup after selection

## Architecture

### Component Structure
```
menu_selector.sh
├── Environment Detection
│   ├── detect_terminal_capabilities()
│   └── Platform detection logic
├── Color System
│   ├── setup_colors()
│   └── Formatting functions
├── Cursor Management
│   ├── cursor_up()
│   ├── cursor_clear_line()
│   ├── hide_cursor()
│   └── show_cursor()
├── Menu Implementations
│   ├── advanced_menu_selector()
│   ├── simple_menu_selector()
│   ├── numbered_menu_selector()
│   └── macos_menu_selector()
├── Main Interface
│   └── menu_selector()
├── Utility Functions
│   ├── print_header()
│   ├── print_info()
│   ├── print_success()
│   ├── print_warning()
│   ├── print_error()
│   ├── print_guide()
│   └── print_prompt()
└── Testing
    └── test_universal_menu()
```

### Design Principles
1. **Progressive Enhancement**: Start with basic functionality and enhance based on capabilities
2. **Graceful Degradation**: Fall back to simpler methods when advanced features aren't available
3. **Zero Assumptions**: Don't assume terminal capabilities without detection
4. **Clean Interfaces**: Consistent API regardless of underlying implementation

## Terminal Compatibility

### Supported Platforms
| Platform | Status | Notes |
|----------|--------|-------|
| macOS Terminal | ✅ Full Support | Optimized menu with enhanced colors |
| iTerm2 | ✅ Full Support | Advanced menu with all features |
| WSL (Windows Subsystem for Linux) | ✅ Full Support | Fixed color handling in v3.4 |
| Linux Terminals (xterm, gnome-terminal, etc.) | ✅ Full Support | Advanced or simple menu based on capabilities |
| Git Bash (Windows) | ✅ Full Support | Usually falls back to numbered menu |
| Windows PowerShell | ⚠️ Limited | Numbered menu only |
| tmux/screen | ✅ Full Support | Detected and handled appropriately |

### Capability Matrix
| Feature | Advanced | Simple | Numbered | macOS |
|---------|----------|--------|----------|-------|
| Arrow Key Navigation | ✅ | ✅ | ❌ | ✅ |
| Real-time Highlighting | ✅ | ✅ | ❌ | ✅ |
| Color Support | ✅ | ✅ | ✅ | ✅ |
| No Screen Clear | ✅ | ❌ | ✅ | ❌ |
| WASD Navigation | ❌ | ✅ | ❌ | ✅ |

## Installation

### Method 1: Direct Download
```bash
# Download and install globally
sudo curl -s -o /usr/local/bin/menu_selector \
  https://raw.githubusercontent.com/blue-samarth/Terminal-Menu-Selector/master/menu_selector.sh
sudo chmod +x /usr/local/bin/menu_selector
```

### Method 2: Source in Script
```bash
# Download and source in your script
MENU_TOOL="./menu_selector.sh"
MENU_URL="https://raw.githubusercontent.com/blue-samarth/Terminal-Menu-Selector/master/menu_selector.sh"

if [ ! -f "$MENU_TOOL" ]; then
  curl -s -o "$MENU_TOOL" "$MENU_URL"
  chmod +x "$MENU_TOOL"
fi

source "$MENU_TOOL"
```

### Method 3: Local Copy
```bash
# Copy to your project directory
cp menu_selector.sh /path/to/your/project/
source ./menu_selector.sh
```

## Usage Guide

### Basic Usage Pattern
```bash
#!/bin/bash
source ./menu_selector.sh

# Define your variable to store the result
result=""

# Call the menu selector
menu_selector "Choose an option:" result \
  "Option 1" \
  "Option 2" \
  "Option 3"

# Use the result
echo "You selected: $result"
```

### Advanced Usage with Custom Return Values
```bash
#!/bin/bash
source ./menu_selector.sh

result=""

# Display options with custom return values
menu_selector "Choose your programming language:" result \
  "Python - Great for data science" \
  "JavaScript - Web development" \
  "Go - System programming" \
  -- \
  "python" \
  "javascript" \
  "golang"

echo "Selected language: $result"
```

## API Reference

### Primary Function

#### `menu_selector(prompt, output_var, ...options)`
The main function that provides universal menu selection capability.

**Parameters:**
- `prompt` (string): The question or instruction to display
- `output_var` (string): Variable name to store the selected value
- `...options` (strings): Menu options to display

**Separator Usage:**
- Use `--` to separate display options from return values
- If no `--` is provided, display options are used as return values

**Return Codes:**
- `0`: Successful selection
- `130`: User cancelled (q/Q/Ctrl+C)
- `1`: Error (no options provided, mismatched arrays)

### Environment Detection

#### `detect_terminal_capabilities()`
Detects and returns terminal capabilities as space-separated keywords.

**Returns:**
- `interactive`: Terminal supports interactive input
- `ansi`: ANSI color codes supported
- `tput_colors`: tput color commands available
- `cursor_movement`: Cursor positioning supported
- `read_timeout`: Read command supports timeout
- `macos`/`linux`/`wsl`/`windows_bash`: Platform detection
- `apple_terminal`/`iterm2`/`tmux`: Terminal app detection

#### `setup_colors(capabilities)`
Sets up color variables based on detected capabilities.

**Global Variables Set:**
- `RED`, `GREEN`, `YELLOW`, `BLUE`, `MAGENTA`, `CYAN`, `WHITE`
- `BOLD`, `UNDERLINE`, `REVERSE`, `RESET`, `NC`

### Menu Implementations

#### `advanced_menu_selector(prompt, output_var, ...options)`
Flicker-free menu with real-time highlighting. Best for capable terminals.

**Features:**
- No screen clearing
- Arrow key navigation
- Real-time visual feedback
- Cursor hiding during operation

#### `simple_menu_selector(prompt, output_var, ...options)`
Clear-screen based menu with highlighting. Good fallback option.

**Features:**
- Full screen clear between updates
- Arrow key + WASD navigation
- Bold highlighting
- Works on most terminals

#### `numbered_menu_selector(prompt, output_var, ...options)`
Most compatible option using numbered choices.

**Features:**
- No special key handling required
- Numeric input only
- Works in any terminal
- Highest compatibility

#### `macos_menu_selector(prompt, output_var, ...options)`
Optimized specifically for macOS Terminal with enhanced color support.

**Features:**
- Forced ANSI color codes
- macOS-specific optimizations
- Enhanced visual feedback
- Arrows and WASD navigation

### Utility Functions

#### Print Functions
All print functions automatically detect terminal capabilities and apply appropriate colors.

```bash
print_header "Header Text"      # Cyan bold header with borders
print_info "Info message"       # Blue [INFO] prefix
print_success "Success message" # Green [SUCCESS] prefix
print_warning "Warning message" # Yellow [WARNING] prefix
print_error "Error message"     # Red [ERROR] prefix
print_guide "Guide text"        # Blue bold text
print_prompt "Prompt text"      # Cyan bold text
```

### Cursor Management

#### `cursor_up(lines, capabilities)`
Move cursor up by specified number of lines.

#### `cursor_clear_line(capabilities)`
Clear current line from cursor position to end.

#### `hide_cursor(capabilities)` / `show_cursor(capabilities)`
Hide or show the terminal cursor.

## Menu Types

### 1. Advanced Menu (Default for Capable Terminals)
```
Choose your favorite programming language:

 > Python - Great for data science 
   JavaScript - Web development king
   Bash - System administration
   Go - Modern system programming
   Rust - Memory safe performance
```

**Characteristics:**
- Real-time highlighting with green background
- No screen flickering
- Arrow key navigation
- Smooth user experience

**Best For:**
- Modern terminals
- Interactive applications
- Professional scripts

### 2. Simple Menu (Fallback Option)
```
Choose your favorite programming language:

 >> Python - Great for data science <<
    JavaScript - Web development king
    Bash - System administration
    Go - Modern system programming
    Rust - Memory safe performance

Use w/s or arrow keys to move, Enter to select, q to quit
```

**Characteristics:**
- Full screen clear between updates
- Bold highlighting with arrows
- Arrow keys + WASD navigation
- Clear instructions

**Best For:**
- Terminals with limited capabilities
- When flicker-free operation isn't critical

### 3. Numbered Menu (Maximum Compatibility)
```
Choose your favorite programming language:

1. Python - Great for data science
2. JavaScript - Web development king
3. Bash - System administration
4. Go - Modern system programming
5. Rust - Memory safe performance

Enter choice (1-5) or q to quit: 
```

**Characteristics:**
- No special key handling
- Numeric input only
- Works everywhere
- Familiar interface

**Best For:**
- Basic terminals
- Scripts run in minimal environments
- When arrow key support is uncertain

### 4. macOS Optimized Menu
```
Choose your favorite programming language:

 >> Python - Great for data science <<
   JavaScript - Web development king
   Bash - System administration
   Go - Modern system programming
   Rust - Memory safe performance

Use ↑/↓ arrow keys or w/s to move, Enter to select, q to quit
```

**Characteristics:**
- Forced ANSI color codes
- Unicode arrow symbols in instructions
- macOS Terminal specific optimizations
- Enhanced visual feedback

**Best For:**
- macOS Terminal specifically
- When colors appear washed out in standard mode

## Environment Detection

### Detection Process
The script uses a multi-layer detection system:

1. **Interactive Terminal Detection**
   ```bash
   [[ -t 1 ]]  # Check if stdout is a terminal
   ```

2. **ANSI Support Detection**
   ```bash
   [[ $TERM == *"color"* ]] || [[ $TERM == "xterm"* ]] || [[ $TERM == "screen"* ]]
   ```

3. **tput Capability Detection**
   ```bash
   command -v tput >/dev/null 2>&1 && tput colors >/dev/null 2>&1
   (( $(tput colors 2>/dev/null || echo 0) >= 8 ))
   ```

4. **Platform Detection**
   ```bash
   case "$(uname -s)" in
     Darwin) capabilities+="macos " ;;
     Linux) 
       if grep -qi microsoft /proc/version 2>/dev/null; then
         capabilities+="wsl "
       else
         capabilities+="linux "
       fi
       ;;
     MINGW*|MSYS*|CYGWIN*) capabilities+="windows_bash " ;;
   esac
   ```

5. **Terminal Application Detection**
   ```bash
   if [[ $TERM_PROGRAM == "Apple_Terminal" ]]; then
     capabilities+="apple_terminal "
   elif [[ $TERM_PROGRAM == "iTerm.app" ]]; then
     capabilities+="iterm2 "
   fi
   ```

### Capability-Based Selection Logic
```bash
if [[ $capabilities == *"wsl"* ]]; then
    # WSL: prefer advanced menu
    advanced_menu_selector "$@"
elif [[ $capabilities == *"macos"* ]] && [[ $capabilities == *"apple_terminal"* ]]; then
    # macOS Terminal: use optimized selector
    macos_menu_selector "$@"
elif [[ $capabilities == *"cursor_movement"* ]] && [[ $capabilities == *"read_timeout"* ]]; then
    # Full capabilities: use advanced menu
    advanced_menu_selector "$@"
elif [[ $capabilities == *"interactive"* ]] && command -v clear >/dev/null 2>&1; then
    # Interactive with clear: use simple menu
    simple_menu_selector "$@"
else
    # Fallback: numbered menu
    numbered_menu_selector "$@"
fi
```

## Color System

### Color Variables
The script sets up the following color variables:

#### Basic Colors
- `RED`: Error messages, warnings
- `GREEN`: Success messages, selection highlighting
- `YELLOW`: Warnings, input prompts
- `BLUE`: Information messages, guides
- `MAGENTA`: Special highlighting
- `CYAN`: Headers, prompts
- `WHITE`: General text

#### Text Formatting
- `BOLD`: Emphasis, headers
- `UNDERLINE`: Links, important text
- `REVERSE`: Inverted colors
- `RESET`/`NC`: Reset to default

### Color Detection Priority
1. **tput colors** (most reliable)
   ```bash
   RED=$(tput setaf 1 2>/dev/null)
   GREEN=$(tput setaf 2 2>/dev/null)
   # ... etc
   ```

2. **ANSI escape codes** (widely compatible)
   ```bash
   RED='\033[0;31m'
   GREEN='\033[0;32m'
   # ... etc
   ```

3. **No colors** (fallback)
   ```bash
   RED="" GREEN="" YELLOW=""  # Empty strings
   ```

### WSL Color Fix
Version 3.4 specifically addresses WSL color rendering issues:

```bash
# Enhanced ANSI fallback for WSL and other terminals
if [[ $capabilities == *"ansi"* ]] || [[ $capabilities == *"wsl"* ]] || 
   [[ $TERM == *"color"* ]] || [[ $COLORTERM == "truecolor" ]]; then
    # Use ANSI codes with explicit WSL support
    RED='\033[0;31m'
    # ... etc
fi
```

## Error Handling

### Input Validation
```bash
# Check for empty options
if (( count == 0 )); then
    echo "Error: No options provided" >&2
    return 1
fi

# Check array length matching
if (( ${#display_options[@]} != ${#return_values[@]} )); then
    echo "Error: Mismatched display and return value arrays" >&2
    return 1
fi
```

### Signal Handling
```bash
# Cleanup on exit or interruption
trap 'tput cnorm 2>/dev/null; stty echo 2>/dev/null' EXIT INT TERM
```

### User Cancellation
```bash
# Handle various cancellation methods
elif [[ $key == $'\003' || $key == "q" || $key == "Q" ]]; then
    # Cleanup and exit
    tput cnorm 2>/dev/null
    stty echo 2>/dev/null
    trap - EXIT INT TERM
    echo "Selection cancelled" >&2
    return 130
fi
```

### Graceful Degradation
- If advanced features fail, falls back to simpler methods
- If colors aren't available, continues without them
- If cursor control fails, uses alternatives

## Examples

### Example 1: Basic Menu
```bash
#!/bin/bash
source ./menu_selector.sh

echo "Welcome to the Service Manager!"

service=""
menu_selector "What would you like to do?" service \
  "Start Service" \
  "Stop Service" \
  "Restart Service" \
  "Check Status"

case $service in
  "Start Service")
    echo "Starting service..."
    ;;
  "Stop Service")
    echo "Stopping service..."
    ;;
  "Restart Service")
    echo "Restarting service..."
    ;;
  "Check Status")
    echo "Checking service status..."
    ;;
esac
```

### Example 2: Configuration Menu with Values
```bash
#!/bin/bash
source ./menu_selector.sh

print_header "Database Configuration"

db_type=""
menu_selector "Choose database type:" db_type \
  "MySQL - Relational database" \
  "PostgreSQL - Advanced relational" \
  "MongoDB - Document database" \
  "Redis - Key-value store" \
  -- \
  "mysql" \
  "postgresql" \
  "mongodb" \
  "redis"

print_success "Selected database: $db_type"

# Configure based on selection
case $db_type in
  "mysql")
    DB_PORT=3306
    DB_IMAGE="mysql:8.0"
    ;;
  "postgresql")
    DB_PORT=5432
    DB_IMAGE="postgres:13"
    ;;
  "mongodb")
    DB_PORT=27017
    DB_IMAGE="mongo:4.4"
    ;;
  "redis")
    DB_PORT=6379
    DB_IMAGE="redis:6.2"
    ;;
esac

print_info "Port: $DB_PORT"
print_info "Docker Image: $DB_IMAGE"
```

### Example 3: Multi-Level Menu System
```bash
#!/bin/bash
source ./menu_selector.sh

main_menu() {
    while true; do
        print_header "Main Menu"
        
        choice=""
        menu_selector "Choose a category:" choice \
          "System Administration" \
          "Development Tools" \
          "Network Configuration" \
          "Exit" \
          -- \
          "sysadmin" \
          "devtools" \
          "network" \
          "exit"
        
        case $choice in
          "sysadmin") system_menu ;;
          "devtools") dev_menu ;;
          "network") network_menu ;;
          "exit") break ;;
        esac
    done
}

system_menu() {
    print_header "System Administration"
    
    action=""
    menu_selector "Choose an action:" action \
      "View System Info" \
      "Manage Services" \
      "Check Disk Usage" \
      "Back to Main Menu" \
      -- \
      "sysinfo" \
      "services" \
      "disk" \
      "back"
    
    case $action in
      "sysinfo") 
        print_info "System Information:"
        uname -a
        ;;
      "services")
        print_info "Listing services..."
        systemctl list-units --type=service
        ;;
      "disk")
        print_info "Disk usage:"
        df -h
        ;;
      "back") return ;;
    esac
    
    read -p "Press Enter to continue..."
}

# Start the application
main_menu
```

### Example 4: Error Handling
```bash
#!/bin/bash
source ./menu_selector.sh

safe_menu_selection() {
    local prompt="$1"
    local var_name="$2"
    shift 2
    
    if ! menu_selector "$prompt" "$var_name" "$@"; then
        case $? in
          130)
            print_warning "Operation cancelled by user"
            return 130
            ;;
          1)
            print_error "Menu configuration error"
            return 1
            ;;
          *)
            print_error "Unknown error occurred"
            return 1
            ;;
        esac
    fi
    
    return 0
}

# Usage with error handling
result=""
if safe_menu_selection "Choose an option:" result "Option 1" "Option 2" "Option 3"; then
    print_success "Selected: $result"
else
    exit_code=$?
    print_error "Menu selection failed"
    exit $exit_code
fi
```

### Example 5: Integration with External Scripts
```bash
#!/bin/bash
# vpc_creation_script.sh

source ./menu_selector.sh

print_header "VPC Configuration Wizard"

# Step 1: Choose VPC size
vpc_size=""
menu_selector "Choose VPC size:" vpc_size \
  "/16 - Enterprise (65,534 IPs)" \
  "/20 - Production (4,094 IPs)" \
  "/24 - Standard (254 IPs)" \
  "/28 - Development (14 IPs)" \
  -- \
  "16" "20" "24" "28"

print_info "Selected VPC size: /$vpc_size"

# Step 2: Choose region
region=""
menu_selector "Choose AWS region:" region \
  "US East (N. Virginia)" \
  "US West (Oregon)" \
  "Europe (Ireland)" \
  "Asia Pacific (Tokyo)" \
  -- \
  "us-east-1" \
  "us-west-2" \
  "eu-west-1" \
  "ap-northeast-1"

print_info "Selected region: $region"

# Step 3: Confirm and create
confirm=""
menu_selector "Create VPC with these settings?" confirm \
  "Yes, create VPC" \
  "No, cancel" \
  -- \
  "yes" "no"

if [[ $confirm == "yes" ]]; then
    print_success "Creating VPC..."
    # Call Terraform or AWS CLI here
    terraform apply -var="vpc_cidr=10.0.0.0/$vpc_size" -var="region=$region"
else
    print_warning "VPC creation cancelled"
fi
```

## Integration Patterns

### Pattern 1: Library Integration
```bash
#!/bin/bash
# my_script.sh

# Check if menu_selector is available
if ! command -v menu_selector >/dev/null 2>&1; then
    # Download if not available
    MENU_TOOL="./menu_selector.sh"
    MENU_URL="https://raw.githubusercontent.com/blue-samarth/vpc-aws-module-samarth/master/modules/menu_selector.sh"
    
    echo "Downloading menu_selector..."
    curl -s -o "$MENU_TOOL" "$MENU_URL"
    chmod +x "$MENU_TOOL"
    source "$MENU_TOOL"
else
    # Use system-wide installation
    source menu_selector
fi

# Use menu_selector functions
result=""
menu_selector "Choose option:" result "Option 1" "Option 2"
```

### Pattern 2: Configuration File Integration
```bash
#!/bin/bash
# config_generator.sh

source ./menu_selector.sh

# Read configuration options from file
declare -A CONFIG_OPTIONS
while IFS='=' read -r key value; do
    CONFIG_OPTIONS["$key"]="$value"
done < config_options.txt

# Convert to arrays for menu_selector
display_options=()
return_values=()
for key in "${!CONFIG_OPTIONS[@]}"; do
    display_options+=("${CONFIG_OPTIONS[$key]}")
    return_values+=("$key")
done

# Show menu
selected=""
menu_selector "Choose configuration:" selected \
    "${display_options[@]}" -- "${return_values[@]}"

echo "Selected configuration: $selected"
```

### Pattern 3: Pipeline Integration
```bash
#!/bin/bash
# pipeline_menu.sh

source ./menu_selector.sh

# Function to get user choices and pass to next stage
get_deployment_config() {
    local env=""
    menu_selector "Choose environment:" env \
        "Development" "Staging" "Production" \
        -- "dev" "staging" "prod"
    
    local region=""
    menu_selector "Choose region:" region \
        "US East" "US West" "Europe" \
        -- "us-east-1" "us-west-2" "eu-west-1"
    
    # Output configuration for next pipeline stage
    cat > deployment_config.json <<EOF
{
    "environment": "$env",
    "region": "$region",
    "timestamp": "$(date -u +%Y-%m-%dT%H:%M:%SZ)"
}
EOF
    
    print_success "Configuration saved to deployment_config.json"
}

get_deployment_config
```

## Troubleshooting

### Common Issues

#### 1. Colors Not Displaying
**Symptoms:** No colors or broken color codes visible
**Causes:**
- Terminal doesn't support ANSI colors
- `TERM` variable not set correctly
- Color capability detection failed

**Solutions:**
```bash
# Check TERM variable
echo "TERM: $TERM"

# Force color mode
export TERM=xterm-256color

# Test color support
tput colors

# Manual color test
echo -e "\033[31mRed text\033[0m"
```

#### 2. Arrow Keys Not Working
**Symptoms:** Arrow keys print characters instead of navigating
**Causes:**
- Terminal doesn't support escape sequences
- Input buffering issues
- Terminal emulation problems

**Solutions:**
```bash
# Test arrow key detection
read -rsn1 key; echo "First: '$key'"
if [[ $key == $'\033' ]]; then
    read -rsn2 key; echo "Sequence: '$key'"
fi

# Force simple menu mode
FORCE_SIMPLE_MENU=1 menu_selector "Test:" result "A" "B" "C"

# Use WASD keys instead
# (Available in simple and macOS menus)
```

#### 3. Menu Not Displaying Correctly
**Symptoms:** Overlapping text, cursor issues
**Causes:**
- Terminal size issues
- Cursor control problems
- Screen clearing failures

**Solutions:**
```bash
# Check terminal size
echo "Columns: $COLUMNS, Lines: $LINES"
tput cols; tput lines

# Test cursor control
tput cup 10 10; echo "Test"
tput cuu 1; echo "Should overwrite"

# Force numbered menu
menu_selector() { numbered_menu_selector "$@"; }
```

#### 4. WSL-Specific Issues
**Symptoms:** Colors appear wrong, input lag
**Causes:**
- WSL terminal emulation quirks
- Windows Terminal compatibility

**Solutions:**
```bash
# Force WSL mode
export WSL_DISTRO_NAME="Ubuntu"  # or your distro

# Use Windows Terminal
# - Better color support
# - Improved key handling

# Check WSL version
wsl --version
```

### Debug Mode
```bash
#!/bin/bash
# Enable debugging

set -x  # Enable command tracing

source ./menu_selector.sh

# Test capability detection
echo "=== Capability Detection ==="
capabilities=$(detect_terminal_capabilities)
echo "Detected: $capabilities"

# Test color setup
echo "=== Color Setup ==="
setup_colors "$capabilities"
echo -e "RED: ${RED}Red text${RESET}"
echo -e "GREEN: ${GREEN}Green text${RESET}"

# Test menu with debug info
echo "=== Menu Test ==="
result=""
menu_selector "Debug test:" result "Option 1" "Option 2"
echo "Result: $result"

set +x  # Disable tracing
```

### Performance Profiling
```bash
#!/bin/bash
# Profile menu performance

time_test() {
    local start_time=$(date +%s.%N)
    
    result=""
    menu_selector "Performance test:" result \
        "Option 1" "Option 2" "Option 3" "Option 4" "Option 5"
    
    local end_time=$(date +%s.%N)
    local duration=$(echo "$end_time - $start_time" | bc)
    echo "Menu operation took: ${duration} seconds"
}

time_test
```

## Best Practices

### 1. Script Integration
```bash
#!/bin/bash
# Good: Check for menu_selector availability
if [[ ! -f "./menu_selector.sh" ]]; then
    echo "Error: menu_selector.sh not found" >&2
    exit 1
fi

source ./menu_selector.sh

# Good: Validate menu result
result=""
if menu_selector "Choose option:" result "A" "B" "C"; then
    case $result in
        "A") handle_option_a ;;
        "B") handle_option_b ;;
        "C") handle_option_c ;;
        *) 
            echo "Unexpected result: $result" >&2
            exit 1
            ;;
    esac
else
    echo "Menu selection failed or cancelled" >&2
    exit $?
fi
```

### 2. Option Design
```bash
# Good: Clear, descriptive options
menu_selector "Choose deployment environment:" env \
    "Development - Local testing environment" \
    "Staging - Pre-production testing" \
    "Production - Live environment" \
    -- \
    "dev" "staging" "prod"

# Avoid: Ambiguous options
menu_selector "Choose:" env "Dev" "Stage" "Prod"  # Too brief
```

### 3. Error Handling
```bash
# Good: Comprehensive error handling
safe_menu() {
    local prompt="$1" var_name="$2"
    shift 2
    
    if [[ $# -eq 0 ]]; then
        print_error "No menu options provided"
        return 1
    fi
    
    if ! menu_selector "$prompt" "$var_name" "$@"; then
        case $? in
            130) print_warning "Cancelled by user"; return 130 ;;
            1)   print_error "Menu configuration error"; return 1 ;;
            *)   print_error "Unknown menu error"; return 1 ;;
        esac
    fi
    
    return 0
}
```

### 4. User Experience
```bash
# Good: Provide context and guidance
print_header "Database Configuration"
print_info "Choose the database type for your application"
print_guide "Consider your data structure and scalability needs"

result=""
menu_selector "Select database type:" result \
    "PostgreSQL - Full SQL support, best for complex queries" \
    "MySQL - Reliable, widely supported, good for web apps" \
    "MongoDB - NoSQL, flexible schema, good for JSON data" \
    "Redis - In-memory, very fast, good for caching"

print_success "Selected: $result"
print_info "Configuring $result database..."
```

### 5. Maintainable Code
```bash
# Good: Use functions for repeated patterns
show_environment_menu() {
    local selected_env=""
    menu_selector "Choose environment:" selected_env \
        "Development" "Staging" "Production" \
        -- "dev" "staging" "prod"
    echo "$selected_env"
}

show_region_menu() {
    local selected_region=""
    menu_selector "Choose region:" selected_region \
        "US East (Virginia)" "US West (Oregon)" "Europe (Ireland)" \
        -- "us-east-1" "us-west-2" "eu-west-1"
    echo "$selected_region"
}

# Usage
env=$(show_environment_menu)
region=$(show_region_menu)
```

### 6. Testing
```bash
# Good: Test different scenarios
test_menu_functionality() {
    echo "Testing menu functionality..."
    
    # Test normal operation
    result=""
    if echo "2" | menu_selector "Test menu:" result "Option 1" "Option 2"; then
        echo "✓ Normal operation works"
    else
        echo "✗ Normal operation failed"
    fi
    
    # Test cancellation
    if echo "q" | menu_selector "Test menu:" result "Option 1" "Option 2"; then
        echo "✗ Cancellation not handled"
    else
        echo "✓ Cancellation works"
    fi
    
    # Test with no options
    if menu_selector "Test menu:" result; then
        echo "✗ Empty options not handled"
    else
        echo "✓ Empty options handled"
    fi
}
```

### 7. Documentation in Scripts
```bash
#!/bin/bash
#=============================================================================
# VPC Setup Script
#=============================================================================
# This script uses menu_selector.sh to guide users through VPC configuration
# 
# Dependencies:
#   - menu_selector.sh (auto-downloaded if missing)
#   - terraform (for infrastructure creation)
#   - aws cli (for AWS operations)
#
# Usage:
#   ./setup_vpc.sh
#
# Environment Variables:
#   FORCE_SIMPLE_MENU - Force simple menu mode
#   AWS_PROFILE - AWS profile to use
#=============================================================================

source ./menu_selector.sh

print_header "VPC Configuration Wizard"
print_info "This wizard will guide you through creating a new VPC"
print_guide "You can cancel at any time by pressing 'q'"
```

---

## Conclusion

The Universal Menu Selector is a powerful and flexible tool for creating interactive command-line interfaces. Its intelligent detection system and multiple interface modes make it suitable for a wide range of applications, from simple configuration scripts to complex multi-stage wizards.

The key to successful integration is understanding your target environment and users, then leveraging the appropriate features while maintaining good error handling and user experience practices.

For the latest updates and examples, refer to the source repository and test the functionality in your specific environment before deploying to production systems.