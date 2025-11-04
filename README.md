# Test Manifests

Example manifest files for testing Shoehorn's catalog functionality.

## Directory Structure

```
test-manifests/
├── .shoehorn/           # Shoehorn native manifests
│   ├── admin-dashboard.yml
│   ├── analytics-service.yml
│   ├── auth-service.yml
│   ├── data-pipeline.yml
│   ├── frontend-app.yml
│   ├── kafka-events.yml
│   ├── ml-recommendation-service.yml
│   ├── mobile-app.yml
│   ├── postgres-db.yml
│   └── user-api.yml
│
└── backstage/           # Backstage catalog-info.yaml examples
    ├── admin-dashboard-catalog-info.yaml
    ├── platform-team-catalog-info.yaml
    ├── postgres-db-catalog-info.yaml
    ├── user-api-catalog-info.yaml
    ├── user-management-system-catalog-info.yaml
    └── user-service-catalog-info.yaml
```

## Manifest Formats

This repository contains examples of both manifest formats supported by Shoehorn:

### Shoehorn Manifests (`.shoehorn/*.yml`)

Native Shoehorn manifest format with simplified schema:

```yaml
schemaVersion: 1
service:
  id: user-api
  name: User Management API
  type: api
owner:
  - type: team
    id: platform-team
lifecycle: production
tags:
  - api
  - golang
links:
  - name: Repository
    url: https://github.com/company/user-api
interfaces:
  http:
    baseUrl: https://api.company.com/users
    openapi: ./api/openapi.yaml
relations:
  - type: depends_on
    target: database:postgres-db
```

### Backstage Manifests (`catalog-info.yaml`)

Standard Backstage catalog format - automatically converted by Shoehorn:

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: user-api
  title: User Management API
  tags:
    - api
    - golang
spec:
  type: service
  lifecycle: production
  owner: platform-team
  dependsOn:
    - resource:postgres-db
```

## Using These Manifests

### With Shoehorn

Shoehorn automatically detects and converts both formats:

```bash
# Validate a Shoehorn manifest
shoehorn validate .shoehorn/user-api.yml

# Validate a Backstage manifest
shoehorn validate backstage/user-service-catalog-info.yaml

# Convert Backstage to Shoehorn
shoehorn convert backstage/user-service-catalog-info.yaml

# Convert all Backstage manifests
shoehorn convert backstage/ -r --to shoehorn
```

### Manual Conversion

Use the Shoehorn bridge to convert between formats:

```go
import "github.com/imbabamba/shoehorn/internal/manifest"

parser := manifest.NewParser()
bridge := manifest.NewBackstageBridge()

// Parse any format (auto-detects)
manifest, _, err := parser.ParseAndValidate(yamlData)

// Convert to Backstage
backstage, err := bridge.ConvertToBackstage(manifest)
```

## Features Demonstrated

### Shoehorn Manifests

- ✅ Multiple service types (api, service, frontend, database)
- ✅ Multiple owners per service
- ✅ Rich interface definitions (HTTP, gRPC)
- ✅ Detailed relations with notes
- ✅ Team extras (Slack channels, custom resources)
- ✅ Lifecycle stages
- ✅ Tags and links

### Backstage Manifests

- ✅ Standard Backstage kinds (Component, API, Resource, Group, System)
- ✅ Metadata with annotations
- ✅ Relations (dependsOn, consumesApis, providesApis)
- ✅ Links with icons
- ✅ API definitions (OpenAPI, gRPC, AsyncAPI)
- ✅ System and domain organization

## Testing

These manifests are used for:

1. **Unit Tests**: Validation and parsing tests
2. **Integration Tests**: Catalog ingestion and search
3. **Manual Testing**: UI and API testing
4. **Documentation**: Examples and tutorials

## Documentation

- [Shoehorn Manifest Specification](../shoehorn/specs/entities/manifest/manifest-spec.md)
- [Backstage Bridge Documentation](../shoehorn/docs/backstage-bridge.md)
- [Catalog Structure](../shoehorn/specs/catalog/catalog-structure.md)

## Contributing

To add new test manifests:

1. Create manifests in the appropriate directory
2. Follow naming conventions (`kebab-case.yml`)
3. Include comprehensive metadata
4. Document any special features or edge cases

## License

MIT License - See [LICENSE](LICENSE) for details
