# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] 

### Added

#### Multi-Source Intelligence Architecture
- HIBP (Have I Been Pwned) password compromise checker with k-anonymity model
- HIBP breach metadata API integration for 928+ breach records
- ProxyNova credential search (3.2B+ leaked credentials)
- Combined intelligence from multiple breach databases in one CLI

#### HIBP Password Intelligence
- K-anonymity implementation - only sends first 5 chars of SHA-1 hash
- Password compromise checking against 570M+ known breaches
- Shows compromise count (how many times password appears in breaches)
- Privacy-safe - full passwords never transmitted over network
- `checkpw` and `pw` commands for quick password verification

#### HIBP Breach Intelligence
- List all 928+ breaches with sorting by impact (pwn count)
- Filter breaches by domain with `breaches <domain>` command
- Get detailed breach information including dates, data classes, descriptions
- Latest breach tracking with `latest` command
- HTML stripping from breach descriptions for clean CLI display
- Status flags: `[VERIFIED]`, `[SENSITIVE]`, `[SPAM]`, `[MALWARE]`

#### Data Models and Type Safety
- `PasswordCheckResult` dataclass for structured password check results
- `BreachRecord` dataclass with 20+ fields for breach metadata
- `ProxyNovaResult` dataclass for clean search result handling
- `.from_dict()` factory methods for data parsing
- Comprehensive type hints throughout (PEP 484)
- `Optional[T]` for nullable returns
- `Tuple[Optional[T], Optional[str]]` for result/error pairs

#### API Client Architecture
- `HIBPClient` class with persistent session management
- `ProxyNovaClient` class with retry logic and rate limiting
- Automatic retry with exponential backoff (3 attempts max)
- Smart rate limiting with random jitter (0.6s base + 0-1s jitter)
- Configurable timeouts and backoff parameters
- Session cleanup with `.close()` methods

#### Custom Exception Hierarchy
- `BreachPeekError` base exception
- `APIError` for API request failures
- `APITimeoutError` for timeout handling
- `RateLimitError` for rate limit detection

#### Display and Formatting
- `Display` class with separated presentation logic
- Red team aesthetic with ANSI color codes (red, cyan, white, dim)
- Intelligence source statistics display (replaces connectivity checks)
- ASCII banner with GitHub repo credit
- Breach record formatting with metadata and status flags
- Password result formatting with clear warnings
- Credential result tables with email:password pairs

#### Configuration Management
- `Config` class for centralized constants
- Configurable retry logic: MAX_RETRIES=3, exponential backoff base=2.0
- Rate limiting: 0.6s base delay, 1.5s pagination delay
- Request timeout: 15s
- Retry jitter: 0-1s random delay

#### User Experience
- Interactive command shell with `breach@peek »` prompt
- Help system with command reference and examples
- Clear error messages with retry feedback
- Automatic pagination with user control
- OPSEC notes in help text
- Signal handlers for clean shutdown (SIGINT/SIGTERM)
- Resource cleanup in finally blocks

#### CLI Commands
- `search <query>` - Search ProxyNova credential database
- `checkpw <password>` - Check password compromise status
- `pw <password>` - Quick password check alias
- `breaches` - List all HIBP breaches
- `breaches <domain>` - Filter breaches by domain
- `breach <name>` - Get detailed breach information
- `latest` - Show newest breach addition
- `help` - Command reference
- `clear` - Clear screen
- `exit` / `quit` - Exit BreachPeek

#### Pagination Handling
- Automatic retry logic for ProxyNova pagination
- Exponential backoff on 400 errors (2s, 4s, 8s with jitter)
- User feedback during retries with attempt counter
- After 3 failed attempts, offers user choice to continue or stop
- Clear messaging about ProxyNova API limitations

### Changed

#### Architecture
- Complete rewrite from procedural to object-oriented architecture
- Separated concerns: API clients, display logic, CLI controller
- Encapsulated state management in `BreachPeekCLI` class
- Persistent HTTP sessions for better performance
- Signal handlers moved to instance methods

#### Error Handling
- Graceful EOFError handling in pagination loops
- KeyboardInterrupt (Ctrl+C) caught and handled cleanly
- Context-aware error messages
- Try-finally blocks ensure cleanup on exit
- Retry logic for timeouts and transient failures

#### Rate Limiting
- Reduced base delay from previous version for faster requests
- Added pagination-specific delays (1.5s vs 0.6s)
- Random jitter added to all delays for anti-detection
- Separate tracking for first request vs pagination

#### Output
- Static intelligence source statistics instead of API connectivity checks
- No startup API calls for faster launch
- Shows scale of available data (3.2B, 570M, 928+)
- Professional appearance without false negatives from network issues

#### Command Processing
- Default behavior: direct ProxyNova search without "search" prefix
- Shortened command aliases (`pw` for password check)
- Command routing through single `_process_command()` method

### Removed

- API connectivity checks on startup (replaced with static stats)
- Unnecessary API calls for status verification

### Technical Improvements

#### Code Quality
- Full type annotations throughout the codebase
- Immutable dataclasses with field defaults
- Factory methods for safe data parsing
- Separation of concerns (API, display, CLI)
- Short, punchy comments in code
- Proper resource management with context managers

#### Dependencies
- `requests` library for HTTP operations
- `hashlib` for SHA-1 password hashing
- `argparse` for CLI argument parsing
- `signal` for interrupt handling
- `dataclasses` for structured data
- `typing` for type hints

#### CLI Improvements
- Interactive shell with command history
- Subcommand support via argparse
- Help text with examples and OPSEC notes
- Cross-platform signal handling
- Clean exit on EOF and interrupts

#### Documentation
- Module docstring with GitHub link and legal disclaimer
- Comprehensive README with examples and API details
- Known limitations section explaining ProxyNova pagination
- Legal disclaimer and responsible use guidelines
- Platform-specific installation notes

### Known Issues

- ProxyNova API blocks pagination after ~100 results (API limitation, not a bug)
- Tool handles this gracefully with retry logic and user prompts
- HIBP email breach searches require paid API key (not implemented)

## [1.5.0] 

### Added

- ProxyNova credential search with 3.2B+ records
- Interactive CLI with search, help, clear commands
- Basic error handling
- Rate limiting (100 requests/minute)
- Cross-platform support (Windows, Linux, macOS)

### Changed

- Initial public release
- Basic command shell interface

### Technical Details

- Simple procedural architecture
- Basic rate limiting implementation
- Terminal color support for modern terminals
