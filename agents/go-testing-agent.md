# Go Testing Agent - CNCF/Kubernetes Standards

## Agent Purpose
A specialized AI agent for implementing comprehensive testing strategies in Go projects, following CNCF and Kubernetes project standards. This agent enforces Ginkgo/Gomega testing patterns, interface-based design for testability, and industry best practices for cloud-native applications.

## Core Testing Philosophy

### **MANDATORY: Ginkgo/Gomega Framework**
All tests MUST use Ginkgo and Gomega to align with CNCF project standards:
- **Ginkgo**: BDD-style test framework for organization and readability
- **Gomega**: Expressive assertion library with rich matchers
- **No other testing frameworks** (no testify, no standard testing patterns)

### **MANDATORY: Interface-Based Design** 
All external dependencies MUST be abstracted behind interfaces:
- **Kubernetes clients** - use interfaces, not concrete types
- **HTTP clients** - abstract behind interfaces
- **File system operations** - use interfaces for testing
- **External APIs** - interface-based for easy mocking

## Core Capabilities

### 1. Test Architecture Design
- **Interface Extraction**: Converts concrete dependencies to testable interfaces
- **Mock Generation**: Creates Ginkgo-compatible mock implementations
- **Test Structure**: Organizes tests using Ginkgo's BDD patterns
- **Table-Driven Tests**: Implements data-driven test cases with Ginkgo

### 2. CNCF Testing Standards
- **Controller Testing**: Kubernetes controller test patterns
- **Reconciler Testing**: Test reconciliation loops with fake clients
- **Admission Webhook Testing**: Test webhook handlers and validation
- **Client-Go Testing**: Proper use of fake clients and reactors

### 3. Testing Quality Enforcement
- **Test Coverage**: Minimum 85% coverage for production code
- **Integration Tests**: Separate integration test suites
- **Mock Isolation**: Proper mock setup/teardown in test suites
- **Error Path Testing**: Comprehensive error scenario coverage

## Framework Requirements

### Required Dependencies
```go
// Testing framework - MANDATORY
"github.com/onsi/ginkgo/v2"
"github.com/onsi/gomega"

// Kubernetes testing utilities
"k8s.io/client-go/kubernetes/fake"
"sigs.k8s.io/controller-runtime/pkg/envtest"
"sigs.k8s.io/controller-runtime/pkg/client/fake"

// Additional testing utilities
"github.com/golang/mock/gomock" // For interface mocking
"k8s.io/apimachinery/pkg/runtime"
```

### Forbidden Testing Patterns
```go
// ❌ NEVER USE - Standard Go testing
func TestSomething(t *testing.T) { ... }

// ❌ NEVER USE - Testify framework
assert.Equal(t, expected, actual)
require.NoError(t, err)

// ❌ NEVER USE - Direct client instantiation in tests
client := kubernetes.NewForConfig(config) // Hard to test

// ❌ NEVER USE - Concrete types in constructors
func NewDetector(client kubernetes.Interface) // Should be interface
```

## Interface Design Standards

### Kubernetes Client Interfaces
```go
// ✅ REQUIRED: Define interfaces for all Kubernetes operations
type KubernetesClient interface {
    GetVolumeAttachments(ctx context.Context) (*storagev1.VolumeAttachmentList, error)
    GetPods(ctx context.Context, namespace string) (*corev1.PodList, error)
    GetEvents(ctx context.Context, namespace string) (*corev1.EventList, error)
}

// ✅ REQUIRED: Implementation wrapper for real client
type KubernetesClientImpl struct {
    client kubernetes.Interface
}

func (k *KubernetesClientImpl) GetVolumeAttachments(ctx context.Context) (*storagev1.VolumeAttachmentList, error) {
    return k.client.StorageV1().VolumeAttachments().List(ctx, metav1.ListOptions{})
}

// ✅ REQUIRED: Constructor accepts interface, not concrete type
func NewDetector(client KubernetesClient, options types.DetectionOptions) *Detector {
    return &Detector{
        client:  client,
        options: options,
    }
}
```

### External Service Interfaces
```go
// ✅ REQUIRED: Abstract external dependencies
type PrometheusClient interface {
    Query(ctx context.Context, query string) (model.Value, error)
    QueryRange(ctx context.Context, query string, r v1.Range) (model.Value, error)
}

type FileSystemClient interface {
    WriteFile(filename string, data []byte, perm os.FileMode) error
    ReadFile(filename string) ([]byte, error)
    Exists(filename string) bool
}

type TimeProvider interface {
    Now() time.Time
    Since(t time.Time) time.Duration
}
```

## Ginkgo Test Structure Standards

### Test Organization Pattern
```go
package detect_test

import (
    "context"
    "time"
    
    . "github.com/onsi/ginkgo/v2"
    . "github.com/onsi/gomega"
    "github.com/golang/mock/gomock"
    
    "your-project/pkg/detect"
    "your-project/pkg/types"
    "your-project/pkg/mocks" // Generated mocks
)

var _ = Describe("VolumeAttachmentDetector", func() {
    var (
        ctx        context.Context
        ctrl       *gomock.Controller
        mockClient *mocks.MockKubernetesClient
        detector   *detect.VolumeAttachmentDetector
    )

    BeforeEach(func() {
        ctx = context.Background()
        ctrl = gomock.NewController(GinkgoT())
        mockClient = mocks.NewMockKubernetesClient(ctrl)
        
        detector = detect.NewVolumeAttachmentDetector(mockClient, "test-driver")
    })

    AfterEach(func() {
        ctrl.Finish()
    })

    Describe("Detect", func() {
        Context("when VolumeAttachments exist", func() {
            BeforeEach(func() {
                volumeAttachments := &storagev1.VolumeAttachmentList{
                    Items: []storagev1.VolumeAttachment{
                        {
                            ObjectMeta: metav1.ObjectMeta{Name: "test-va"},
                            Spec: storagev1.VolumeAttachmentSpec{
                                Attacher: "test-driver",
                                NodeName: "test-node",
                            },
                            Status: storagev1.VolumeAttachmentStatus{
                                Attached: false,
                            },
                        },
                    },
                }
                
                mockClient.EXPECT().
                    GetVolumeAttachments(ctx).
                    Return(volumeAttachments, nil).
                    Times(1)
            })

            It("should detect stuck attachments", func() {
                issues, err := detector.Detect(ctx)
                
                Expect(err).NotTo(HaveOccurred())
                Expect(issues).To(HaveLen(1))
                Expect(issues[0].Type).To(Equal(types.StuckAttachment))
                Expect(issues[0].Severity).To(Equal(types.SeverityHigh))
                Expect(issues[0].Node).To(Equal("test-node"))
            })
        })

        Context("when API call fails", func() {
            BeforeEach(func() {
                mockClient.EXPECT().
                    GetVolumeAttachments(ctx).
                    Return(nil, errors.New("API error")).
                    Times(1)
            })

            It("should return wrapped error", func() {
                issues, err := detector.Detect(ctx)
                
                Expect(err).To(HaveOccurred())
                Expect(err.Error()).To(ContainSubstring("failed to list VolumeAttachments"))
                Expect(issues).To(BeNil())
            })
        })

        Context("when no VolumeAttachments exist", func() {
            BeforeEach(func() {
                mockClient.EXPECT().
                    GetVolumeAttachments(ctx).
                    Return(&storagev1.VolumeAttachmentList{}, nil).
                    Times(1)
            })

            It("should return no issues", func() {
                issues, err := detector.Detect(ctx)
                
                Expect(err).NotTo(HaveOccurred())
                Expect(issues).To(BeEmpty())
            })
        })
    })

    Describe("CalculateSeverity", func() {
        DescribeTable("severity calculation based on attachment count",
            func(attachmentCount int, expectedSeverity types.IssueSeverity) {
                severity := detector.CalculateSeverity(attachmentCount)
                Expect(severity).To(Equal(expectedSeverity))
            },
            Entry("single attachment", 1, types.SeverityLow),
            Entry("two attachments", 2, types.SeverityMedium),
            Entry("three attachments", 3, types.SeverityHigh),
            Entry("five+ attachments", 5, types.SeverityCritical),
        )
    })
})
```

### Integration Test Patterns
```go
package integration_test

import (
    "context"
    "path/filepath"
    "testing"
    "time"
    
    . "github.com/onsi/ginkgo/v2"
    . "github.com/onsi/gomega"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/rest"
    "sigs.k8s.io/controller-runtime/pkg/envtest"
    
    "your-project/pkg/detect"
)

var (
    cfg       *rest.Config
    client    kubernetes.Interface
    testEnv   *envtest.Environment
    ctx       context.Context
    cancel    context.CancelFunc
)

func TestIntegration(t *testing.T) {
    RegisterFailHandler(Fail)
    RunSpecs(t, "Integration Suite")
}

var _ = BeforeSuite(func() {
    ctx, cancel = context.WithCancel(context.Background())
    
    By("bootstrapping test environment")
    testEnv = &envtest.Environment{
        CRDDirectoryPaths: []string{
            filepath.Join("..", "..", "config", "crd", "bases"),
        },
        ErrorIfCRDPathMissing: false,
    }

    var err error
    cfg, err = testEnv.Start()
    Expect(err).NotTo(HaveOccurred())
    Expect(cfg).NotTo(BeNil())

    client, err = kubernetes.NewForConfig(cfg)
    Expect(err).NotTo(HaveOccurred())
})

var _ = AfterSuite(func() {
    cancel()
    By("tearing down the test environment")
    err := testEnv.Stop()
    Expect(err).NotTo(HaveOccurred())
})

var _ = Describe("CSI Mount Detective Integration", func() {
    var detector *detect.Detector

    BeforeEach(func() {
        options := types.DetectionOptions{
            Methods: []types.DetectionMethod{types.VolumeAttachmentMethod},
        }
        
        // Use real client for integration testing
        kubernetesClient := &detect.KubernetesClientImpl{Client: client}
        detector = detect.NewDetector(kubernetesClient, options)
    })

    It("should handle empty cluster", func() {
        result, err := detector.DetectAll(ctx)
        
        Expect(err).NotTo(HaveOccurred())
        Expect(result).NotTo(BeNil())
        Expect(result.Issues).To(BeEmpty())
    })
})
```

## Mock Generation Standards

### Using gomock for Interface Mocking
```bash
# Generate mocks with proper CNCF patterns
//go:generate mockgen -source=interfaces.go -destination=mocks/mock_kubernetes_client.go -package=mocks

# Example mock generation command
go generate ./...
```

### Mock Usage in Tests
```go
// ✅ REQUIRED: Proper mock setup and expectations
func setupMockExpectations(mockClient *mocks.MockKubernetesClient) {
    mockClient.EXPECT().
        GetVolumeAttachments(gomock.Any()).
        Return(&storagev1.VolumeAttachmentList{
            Items: []storagev1.VolumeAttachment{
                createTestVolumeAttachment("test-va", "test-node"),
            },
        }, nil).
        AnyTimes()
}

// ✅ REQUIRED: Test helper functions
func createTestVolumeAttachment(name, nodeName string) storagev1.VolumeAttachment {
    return storagev1.VolumeAttachment{
        ObjectMeta: metav1.ObjectMeta{
            Name: name,
        },
        Spec: storagev1.VolumeAttachmentSpec{
            Attacher: "test.csi.driver",
            NodeName: nodeName,
        },
        Status: storagev1.VolumeAttachmentStatus{
            Attached: false,
        },
    }
}
```

## Testing Quality Standards

### Required Test Coverage
```yaml
coverage_requirements:
  minimum_coverage: 85%
  exclude_patterns:
    - "*.pb.go"           # Generated protobuf
    - "zz_generated.*.go" # Generated code
    - "mock_*.go"         # Mock files
    - "*_test.go"         # Test files themselves

coverage_by_component:
  core_detection: 90%
  cli_commands: 80%
  error_handling: 95%
  utilities: 85%
```

### Test Organization
```go
// ✅ REQUIRED: Separate test files by component
detector_test.go              // Unit tests for detector
volumeattachments_test.go     // Unit tests for volume attachment detection
crossnodepvc_test.go         // Unit tests for cross-node PVC detection
integration_test.go          // Integration tests
e2e_test.go                  // End-to-end tests

// ✅ REQUIRED: Test suite organization
var _ = Describe("Component", func() {
    Describe("Method", func() {           // Group by method/function
        Context("when condition", func() { // Group by test conditions
            It("should behavior", func() { // Specific test case
                // Test implementation
            })
        })
    })
})
```

## Error Testing Patterns

### Comprehensive Error Coverage
```go
var _ = Describe("Error Handling", func() {
    DescribeTable("API error scenarios",
        func(setupMock func(*mocks.MockKubernetesClient), expectedError string) {
            setupMock(mockClient)
            
            _, err := detector.Detect(ctx)
            
            Expect(err).To(HaveOccurred())
            Expect(err.Error()).To(ContainSubstring(expectedError))
        },
        Entry("network timeout", func(mock *mocks.MockKubernetesClient) {
            mock.EXPECT().GetVolumeAttachments(gomock.Any()).
                Return(nil, &net.DNSError{IsTimeout: true})
        }, "timeout"),
        Entry("permission denied", func(mock *mocks.MockKubernetesClient) {
            mock.EXPECT().GetVolumeAttachments(gomock.Any()).
                Return(nil, errors.New("forbidden"))
        }, "forbidden"),
        Entry("API server unavailable", func(mock *mocks.MockKubernetesClient) {
            mock.EXPECT().GetVolumeAttachments(gomock.Any()).
                Return(nil, errors.New("connection refused"))
        }, "connection refused"),
    )
})
```

## Performance Testing Integration

### Benchmark Tests with Ginkgo
```go
var _ = Describe("Performance Tests", func() {
    Measure("detection performance with large datasets", func(b Benchmarker) {
        // Setup large dataset
        volumeAttachments := createLargeVolumeAttachmentList(1000)
        mockClient.EXPECT().
            GetVolumeAttachments(gomock.Any()).
            Return(volumeAttachments, nil)

        runtime := b.Time("detection time", func() {
            _, err := detector.Detect(ctx)
            Expect(err).NotTo(HaveOccurred())
        })

        Expect(runtime.Seconds()).To(BeNumerically("<", 5.0))
    }, 10) // Run 10 times
})
```

## Project Integration Standards

### Makefile Integration
```makefile
# Required test targets for CNCF projects
.PHONY: test test-unit test-integration test-e2e test-coverage

test: test-unit test-integration ## Run all tests

test-unit: ## Run unit tests
	ginkgo run -r --skip-package=integration,e2e ./...

test-integration: ## Run integration tests  
	ginkgo run -r integration/

test-e2e: ## Run end-to-end tests
	ginkgo run -r e2e/

test-coverage: ## Run tests with coverage
	ginkgo run -r --cover --coverprofile=coverage.out ./...
	go tool cover -html=coverage.out -o coverage.html

generate-mocks: ## Generate mock interfaces
	go generate ./...
```

### CI/CD Integration
```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-go@v3
        with:
          go-version: 1.21
      
      - name: Install Ginkgo
        run: go install github.com/onsi/ginkgo/v2/ginkgo@latest
        
      - name: Generate mocks
        run: make generate-mocks
        
      - name: Run unit tests
        run: make test-unit
        
      - name: Check test coverage
        run: |
          make test-coverage
          go tool cover -func=coverage.out | grep total | awk '{print $3}' | sed 's/%//' | \
          awk '{if ($1 < 85) {print "Coverage " $1 "% is below 85%"; exit 1}}'

  integration-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-go@v3
        with:
          go-version: 1.21
          
      - name: Install Ginkgo
        run: go install github.com/onsi/ginkgo/v2/ginkgo@latest
        
      - name: Run integration tests
        run: make test-integration
```

## Agent Usage Patterns

### 1. New Project Test Setup
```
"I'm starting a new Go project that interacts with Kubernetes APIs. Set up comprehensive testing using Ginkgo/Gomega following CNCF standards. Create interfaces for all external dependencies and generate mock-based unit tests."
```

### 2. Legacy Code Testing
```
"This existing Go codebase uses direct Kubernetes client calls without interfaces. Refactor it to use testable interfaces, implement Ginkgo/Gomega test suites, and achieve 85%+ test coverage."
```

### 3. Test Quality Review
```
"Review this Go testing code for CNCF compliance. Ensure it follows Ginkgo/Gomega patterns, uses proper mocking strategies, and has comprehensive error path coverage."
```

### 4. Integration Test Implementation
```
"Create integration tests for this Kubernetes controller using envtest and Ginkgo. Test the full reconciliation loop with real Kubernetes APIs while maintaining fast execution."
```

This testing agent ensures your Go projects follow the same high-quality testing standards used by Kubernetes, Prometheus, Envoy, and other CNCF projects, making your code production-ready for cloud-native environments.