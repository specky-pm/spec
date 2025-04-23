# spec.json Specification

The `spec.json` file is the heart of every Specky component specification. It contains metadata about the component, defines its dependencies, and configures how Specky should handle it.

## Purpose

- Provides metadata about the component
- Defines dependencies on other components
- Configures component-specific settings
- Serves as the package manifest

## Structure

The `spec.json` file is a JSON document with the following key fields:

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | String | The name of the component specification. Must be unique within the Specky registry. Must be lowercase and can contain hyphens and underscores. |
| `version` | String | The version of the component specification, following semantic versioning (MAJOR.MINOR.PATCH). |
| `description` | String | A brief description of what the component does. |

### Author Information

| Field | Type | Description |
|-------|------|-------------|
| `author` | String or Object | Information about the author of the component specification. Can be a simple string or a more detailed object with name, email, and URL. |
| `contributors` | Array | A list of people who have contributed to the component specification. |

### License Information

| Field | Type | Description |
|-------|------|-------------|
| `license` | String | The license under which the component specification is distributed. Uses SPDX license identifiers. |

### Categorization

| Field | Type | Description |
|-------|------|-------------|
| `keywords` | Array of Strings | Tags that help others discover the component specification when searching the registry. |

### Dependencies

| Field | Type | Description |
|-------|------|-------------|
| `dependencies` | Object | Other component specifications that this component depends on. Keys are names, values are version ranges. |
| `devDependencies` | Object | Dependencies needed only during development. |
| `peerDependencies` | Object | Dependencies that the consumer must provide. |
| `optionalDependencies` | Object | Dependencies that enhance the component but aren't required. |

### Repository Information

| Field | Type | Description |
|-------|------|-------------|
| `repository` | String or Object | Information about where the component's source code is hosted. |
| `homepage` | String | URL to the component's homepage or documentation. |
| `bugs` | String or Object | Information about where to report issues. |

### Component Configuration

| Field | Type | Description |
|-------|------|-------------|
| `main` | String | The primary entry point to the component specification (default: `component.md`). |
| `files` | Array of Strings | Files to include when publishing the component specification. |
| `specky` | Object | Specky-specific configuration options. |

### Publishing Configuration

| Field | Type | Description |
|-------|------|-------------|
| `private` | Boolean | If true, prevents the component from being published. |
| `publishConfig` | Object | Configuration options used when publishing to the registry. |
| `engines` | Object | Specifies which versions of Specky the component is compatible with. |

### Scripts

| Field | Type | Description |
|-------|------|-------------|
| `scripts` | Object | Custom scripts that can be run with `spm run <script-name>`. |

## Example

```json
{
  "name": "user-component",
  "version": "1.2.0",
  "description": "A comprehensive user management component",
  "author": {
    "name": "Jane Developer",
    "email": "jane@example.com",
    "url": "https://janedeveloper.com"
  },
  "license": "MIT",
  "keywords": ["user", "authentication", "profile"],
  "dependencies": {
    "email-validator": "^1.0.0",
    "password-strength": "~2.1.0"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/username/user-component.git"
  },
  "specky": {
    "validators": {
      "strict": true,
      "datamodel": true
    }
  }
}
```

## Additional Files

### README.md (Optional)

While not required, it's good practice to include a README.md file in your specification with:

- An overview of the component
- Examples of how to use the specification
- Any special considerations
- Contact information for questions or contributions

### CHANGELOG.md (Optional)

A file that documents the changes made in each version of the component specification:

- Version number and release date
- New features added
- Bugs fixed
- Breaking changes
- Deprecations

## Relationships Between Files

The files in a Specky component package are interrelated:

- `spec.json` provides the metadata and identifies the component
- `component.md` describes the functional requirements referenced in `spec.json`
- `datamodel.json` defines the data structures mentioned in `component.md`
- Files in the `features/` directory provide test scenarios for the functionality described in `component.md`

Together, these files provide a comprehensive specification of a component that can be implemented in any programming language or framework while ensuring it meets the functional requirements.