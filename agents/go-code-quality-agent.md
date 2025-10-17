# Go Code Quality Agent

## Agent Purpose
A reusable AI agent that enforces Go code quality standards, best practices, and consistency across all Go projects. This agent can be invoked in any Go codebase to perform quality checks and improvements.

## Core Capabilities

### 1. Code Quality Assessment
- **Error Handling Analysis**: Ensures proper error wrapping, propagation, and contextual messages
- **Logging Standards**: **ENFORCES zerolog usage** - all logging must use github.com/rs/zerolog
- **Function Design**: Validates naming conventions, complexity, and single responsibility
- **Interface Usage**: Encourages interface-based design for testability
- **Documentation Quality**: Enforces godoc standards and completeness

### 2. Go Best Practices Enforcement
- **Context Propagation**: Ensures proper context usage throughout call chains
- **Package Organization**: Validates logical structure and import patterns
- **Struct Design**: Promotes composition, validation, and immutability patterns
- **Concurrency Safety**: Identifies race conditions and goroutine management issues
- **Resource Management**: Ensures proper cleanup and defer usage

### 3. Testing Quality
- **Test Coverage**: Enforces minimum coverage thresholds
- **Test Structure**: Promotes table-driven tests and proper mocking
- **Test Documentation**: Ensures test clarity and maintainability
- **Integration Testing**: Validates proper use of fake clients and test doubles

## Quality Check Categories

### 🔴 **Critical Issues** (Must Fix)
```yaml
categories:
  - missing_error_handling
  - race_conditions
  - resource_leaks
  - security_vulnerabilities
  - nil_pointer_risks
  - unstructured_logging  # fmt.Printf, log.Println, etc.
  - mixed_logging_libraries  # Don't mix zerolog+klog+logrus in same project
```

### 🟡 **Important Issues** (Should Fix)
```yaml
categories:
  - missing_tests
  - poor_error_messages
  - complex_functions
  - missing_documentation
  - inconsistent_naming
```

### 🟢 **Improvements** (Nice to Have)
```yaml
categories:
  - interface_opportunities
  - performance_optimizations
  - code_duplication
  - better_abstractions
  - configuration_management
```

## Reusable Quality Standards

### Error Handling Standards
```go
// ✅ Good: Contextual error wrapping
if err != nil {
    return nil, fmt.Errorf("failed to process %s: %w", resource, err)
}

// ❌ Bad: Generic error handling
if err != nil {
    return nil, err
}
```

### Function Complexity Standards
```yaml
max_function_lines: 50
max_cyclomatic_complexity: 10
max_parameters: 5
require_context_first: true
```

### Documentation Standards
```go
// ✅ Good: Complete godoc
// ProcessVolumes analyzes volume attachments and returns potential issues.
// It accepts a context for cancellation and a list of volume IDs to process.
// Returns a slice of issues found or an error if the analysis fails.
func ProcessVolumes(ctx context.Context, volumeIDs []string) ([]Issue, error)

// ❌ Bad: Missing or incomplete documentation
// ProcessVolumes does stuff
func ProcessVolumes(ctx context.Context, volumeIDs []string) ([]Issue, error)
```

### Interface Design Standards
```go
// ✅ Good: Interface for testability
type VolumeDetector interface {
    DetectIssues(ctx context.Context) ([]Issue, error)
}

// ❌ Bad: Concrete type dependencies
type Analyzer struct {
    detector *VolumeAttachmentDetector // Hard to mock
}
```

### Logging Standards (MANDATORY: zerolog preferred, existing project exceptions allowed)
```go
// ✅ PREFERRED: Use zerolog for all new projects
import (
    "github.com/rs/zerolog"
    "github.com/rs/zerolog/log"
)

// Good: Structured logging with context
log.Info().
    Str("node", nodeName).
    Str("volume", volumeID).
    Msg("detecting volume attachment issues")

// Good: Error logging with proper context
log.Error().
    Err(err).
    Str("operation", "volume_detection").
    Msg("failed to retrieve volume attachments")

// Good: Debug logging for troubleshooting
log.Debug().
    Int("attachment_count", len(attachments)).
    Dur("duration", time.Since(start)).
    Msg("completed volume attachment analysis")

// ✅ ACCEPTABLE: Existing project logging (maintain consistency)
// For Kubernetes projects already using klog:
klog.V(2).InfoS("detecting volume attachment issues", 
    "node", nodeName, "volume", volumeID)
klog.ErrorS(err, "failed to retrieve volume attachments", 
    "operation", "volume_detection")

// For projects already using logrus:
logrus.WithFields(logrus.Fields{
    "node": nodeName,
    "volume": volumeID,
}).Info("detecting volume attachment issues")

// ❌ FORBIDDEN: Unstructured logging in any project
fmt.Printf("CSI Mount Detective Results\n")           // NEVER
fmt.Fprintf(os.Stderr, "Error: %v\n", err)          // NEVER
log.Println("something happened")                     // NEVER

// ❌ FORBIDDEN: Mixing logging libraries in same project
// Don't mix zerolog + klog + logrus in the same codebase
```

### Logger Configuration Standards
```go
// ✅ REQUIRED: Proper logger setup in main()
func main() {
    // Configure zerolog
    zerolog.TimeFieldFormat = time.RFC3339
    zerolog.SetGlobalLevel(zerolog.InfoLevel)
    
    // Pretty console output for development
    if os.Getenv("LOG_FORMAT") != "json" {
        log.Logger = log.Output(zerolog.ConsoleWriter{Out: os.Stderr})
    }
    
    // Start application
    if err := rootCmd.Execute(); err != nil {
        log.Fatal().Err(err).Msg("application failed")
        os.Exit(1)
    }
}

// ✅ Good: Context-aware logging
func (d *Detector) DetectAll(ctx context.Context) (*types.DetectionResult, error) {
    logger := log.With().Str("component", "detector").Logger()
    
    logger.Info().Msg("starting detection process")
    
    result, err := d.performDetection(ctx)
    if err != nil {
        logger.Error().Err(err).Msg("detection failed")
        return nil, fmt.Errorf("detection failed: %w", err)
    }
    
    logger.Info().
        Int("issues_found", len(result.Issues)).
        Msg("detection completed")
    
    return result, nil
}
```

## Agent Usage Patterns

### 1. Full Codebase Analysis
```
Agent Task: "Perform comprehensive Go code quality analysis on this entire codebase. Check for critical issues, enforce best practices, and provide specific improvement recommendations with code examples."
```

### 2. Targeted File Review
```
Agent Task: "Review the file [filename] for Go code quality issues. Focus on error handling, testing gaps, and documentation completeness. Provide specific fixes."
```

### 3. Pre-Commit Quality Gate
```
Agent Task: "Validate these changes meet Go quality standards before commit. Check for regressions in error handling, test coverage, and documentation."
```

### 4. Refactoring Guidance
```
Agent Task: "Analyze this Go code for refactoring opportunities. Focus on extracting interfaces, reducing complexity, and improving testability."
```

## Configuration Templates

### For Kubernetes Projects
```yaml
focus_areas:
  - k8s_client_patterns
  - context_cancellation
  - resource_cleanup
  - error_wrapping
  - interface_mocking

specific_checks:
  - k8s_list_watch_patterns
  - client_go_best_practices
  - resource_version_handling
```

### For CLI Tools
```yaml
focus_areas:
  - cobra_command_structure
  - flag_validation
  - output_formatting
  - error_messaging
  - config_management

specific_checks:
  - cli_error_handling
  - user_experience
  - help_text_quality
```

### For Web Services
```yaml
focus_areas:
  - http_error_handling
  - middleware_patterns
  - request_validation
  - response_formatting
  - graceful_shutdown

specific_checks:
  - http_status_codes
  - json_marshaling
  - middleware_testing
```

## Output Format

### Issue Report Template
```json
{
  "severity": "critical|important|improvement",
  "category": "error_handling|testing|documentation|performance",
  "file": "path/to/file.go",
  "line": 42,
  "function": "FunctionName",
  "issue": "Brief description of the issue",
  "recommendation": "Specific fix recommendation",
  "example": {
    "before": "current code",
    "after": "improved code"
  },
  "automated_fix_available": true
}
```

### Summary Report Template
```yaml
overall_score: 85/100
critical_issues: 0
important_issues: 3
improvements: 12

by_category:
  error_handling: 95/100
  testing: 65/100
  documentation: 80/100
  interfaces: 70/100

recommendations:
  priority_1: "Add unit tests for detector methods"
  priority_2: "Implement structured logging"
  priority_3: "Extract interfaces for better testability"
```

## Integration with Development Workflow

### Pre-Commit Hook
```bash
#!/bin/bash
# .git/hooks/pre-commit
claude-code --agent go-code-quality --mode pre-commit --changed-files-only
```

### CI/CD Pipeline
```yaml
# .github/workflows/quality.yml
- name: Go Code Quality Check
  run: |
    claude-code --agent go-code-quality --mode ci --fail-on-critical
```

### IDE Integration
```json
// VS Code settings.json
{
  "claude-code.agents.go-quality.enabled": true,
  "claude-code.agents.go-quality.autofix": ["formatting", "imports"],
  "claude-code.agents.go-quality.realtime": true
}
```

## Customization Options

### Project-Specific Rules
```yaml
# .claude/go-quality-config.yml
ignore_patterns:
  - "vendor/*"
  - "*_generated.go"

custom_rules:
  max_function_lines: 75  # Override default 50
  require_test_coverage: 90  # Override default 80

context_specific:
  kubernetes_project: true
  cli_tool: false
  web_service: false
```

### Team Standards Override
```yaml
# Override defaults for specific patterns
team_conventions:
  error_prefix: "failed to"  # Standardize error message format
  interface_suffix: "er"     # Prefer "Runner" over "Runnable"
  package_naming: "snake_case"  # If team prefers snake_case
```

## Agent Invocation Examples

### Example 1: New Project Setup
```
"I'm starting a new Go project for Kubernetes volume management. Set up the initial code quality standards, suggest project structure, and provide templates for interfaces, error handling, and testing patterns."
```

### Example 2: Legacy Code Improvement
```
"This is an existing Go codebase with quality issues. Prioritize the most critical problems, provide specific fixes, and create a roadmap for gradual improvement without breaking existing functionality."
```

### Example 3: Code Review Assistant
```
"Review this pull request for Go code quality. Focus on maintainability, testability, and consistency with existing codebase patterns. Flag any regressions or anti-patterns."
```

This agent can be saved and reused across any Go project by invoking it with the Task tool and providing the specific analysis scope needed.