# SPECKY(1) - Specky Package Manager

## NAME

spm - Specky Package Manager for component specifications

## SYNOPSIS

`spm [command] [options]`

## DESCRIPTION

Specky Package Manager (spm) is a command-line tool for creating, sharing, and downloading component specifications. It helps developers define, manage, and reuse component specifications across projects.

## COMMANDS

### Project Initialization

#### `spm app <project-name>`

Initialize a new Specky project.

**Options:**
- None documented

**Example:**
```bash
spm app my-first-project
```


### Component Management

#### `spm comp <component-name>`

Create a new component specification in the components directory.

**Options:**
- None documented

**Example:**
```bash
spm comp user-component
```

#### `spm validate [component-name]`

Validate component specifications against the Specky schema.

**Options:**
- `--strict`: Enforce additional best practices and style guidelines
- `--fix`: Attempt to automatically fix common issues
- `--focus=<file>`: Validate a specific file (e.g., component.md)

**Examples:**
```bash
spm validate
spm validate user-component
spm validate --strict
spm validate --fix
spm validate --focus=component.md
```

### Dependency Management

#### `spm install [specification] [options]`

Install specifications from the Specky registry.

**Options:**
- `--save`, `-S`: Add the specification to dependencies (default)
- `--no-save`: Install without adding to dependencies
- `--exact`, `-E`: Install the exact version (without ^ or ~ version range)
- `@<version>`: Install a specific version (e.g., `spm install product-component@1.2.0`)

**Examples:**
```bash
spm install product-component
spm install product-component@1.2.0
spm install product-component shopping-cart-component user-component
spm install product-component --save --exact
spm install # Install all dependencies from spec.json
```

#### `spm uninstall <specification>`

Remove a specification from your project.

**Options:**
- None documented

**Example:**
```bash
spm uninstall product-component
```

#### `spm update [specification]`

Update specifications to newer versions.

**Options:**
- `--depth=<number>`: Limit the depth of dependencies to update
- `--latest`: Update to the latest versions, regardless of version constraints

**Examples:**
```bash
spm update
spm update user-component
spm update user-component product-component
spm update --depth=0
spm update --latest
```

#### `spm outdated`

Check for outdated specifications.

**Options:**
- `--json`: Output in JSON format

**Examples:**
```bash
spm outdated
spm outdated --json > outdated.json
```

#### `spm list`

List installed specifications.

**Options:**
- `--depth=<number>`: Limit the depth of dependencies to display
- `--search=<term>`: Search among locally installed specifications

**Examples:**
```bash
spm list
spm list --depth=0
spm list --search=auth
```

### Registry Interaction

#### `spm search <term> [options]`

Search for specifications in the Specky registry.

**Options:**
- `--tag=<tag>`: Filter by tag
- `--author=<author>`: Filter by author
- `--sort=<criteria>`: Sort results by relevance (default), downloads, recent, or rating
- `--limit=<number>`: Limit the number of results

**Examples:**
```bash
spm search user authentication
spm search authentication --tag=security
spm search --author=auth-experts
spm search authentication --sort=downloads
spm search authentication --limit=5
spm search "user AND authentication NOT oauth"
spm search name:auth description:secure
```

#### `spm info <specification>`

Get detailed information about a specification.

**Options:**
- None documented

**Example:**
```bash
spm info user-auth-component
```

#### `spm related <specification>`

Find specifications related to the specified one.

**Options:**
- None documented

**Example:**
```bash
spm related user-auth-component
```

#### `spm examples <specification>`

Find example implementations of a specification.

**Options:**
- None documented

**Example:**
```bash
spm examples user-auth-component
```

#### `spm browse`

Open the Specky registry in your browser.

**Options:**
- None documented

**Example:**
```bash
spm browse
```

### Visualization

#### `spm viz [options]`

Visualize component relationships.

**Options:**
- `--format=<format>`: Output format (svg, png, json, dot)
- `--output=<file>`: Save to file instead of displaying in browser
- `--depth=<number>`: Limit the depth of dependencies to visualize
- `--focus=<component>`: Center visualization on a specific component
- `--highlight=<type>`: Highlight specific types of relationships (direct-dependencies, indirect-dependencies, dependents, conflicts)
- `--theme=<theme>`: Visual theme (light, dark, colorblind, minimal)
- `--analyze=<aspect>`: Analyze specific aspects (conflicts, impact, unused, circular)
- `--component=<name>`: Used with --analyze=impact to specify the component to analyze
- `--group=<criteria>`: Group related components

**Examples:**
```bash
spm viz
spm viz --format=svg
spm viz --format=png
spm viz --format=json
spm viz --format=dot
spm viz --depth=2
spm viz --focus=user-component
spm viz --highlight=direct-dependencies
spm viz --theme=dark
spm viz --analyze=conflicts
spm viz --analyze=impact --component=auth-component
spm viz --analyze=unused
spm viz --format=svg --output=component-diagram.svg
```

### Configuration

#### `spm config <action> [key] [value]`

Manage Specky configuration settings.

**Actions:**
- `list`: Show current configuration
- `set`: Set a configuration value
- `delete`: Remove a configuration value
- `reset`: Reset to default values

**Options:**
- `--location=<location>`: Specify where to set the configuration (user or project)
- `--profile=<profile>`: Use or create a configuration profile

**Examples:**
```bash
spm config list
spm config set registry https://registry.specky.dev
spm config set registry https://registry.specky.dev --location=project
spm config set registry https://registry.specky.dev --location=user
spm config delete registry
spm config set init.author.name "Your Name"
spm config set init.author.email "your.email@example.com"
spm config set init.author.url "https://your-website.com"
spm config set init.license "MIT"
spm config set init.version "0.1.0"
spm config set proxy http://proxy.example.com:8080
spm config set https-proxy http://proxy.example.com:8080
spm config set cache /path/to/custom/cache
spm config set cache-max 86400
spm config set --profile=work registry https://work-registry.example.com
spm config set strict-validation true
spm config set validators.datamodel true
spm config set validators.features false
spm config set viz.format svg
spm config set viz.theme dark
spm config set viz.depth 2
spm config set audit true
spm config set min-rating 3
spm config set update-notifier true
```

#### `spm cache <action>`

Manage the Specky cache.

**Actions:**
- `clean`: Clear the cache

**Example:**
```bash
spm cache clean
```

### Publishing

#### `spm login`

Authenticate with the Specky registry.

**Options:**
- None documented

**Example:**
```bash
spm login
```

#### `spm publish [options]`

Publish specifications to the Specky registry.

**Options:**
- `--access=<level>`: Set access level (public or restricted)
- `--tag=<tag>`: Publish with a specific tag (latest, beta, alpha, next)
- `--dry-run`: Show what would be published without actually publishing
- `--scope=<scope>`: Publish to an organization

**Examples:**
```bash
spm publish
spm publish --access=restricted
spm publish --tag=beta
spm publish --dry-run
spm publish --access=public --scope=@your-org
```

#### `spm deprecate <specification> <message>`

Mark a specification as deprecated.

**Options:**
- None documented

**Example:**
```bash
spm deprecate user-component "Use auth-component instead"
```

#### `spm unpublish <specification>`

Remove a specification from the registry.

**Options:**
- None documented

**Example:**
```bash
spm unpublish user-component
```

### Development Tools

#### `spm link [specification]`

Create symbolic links between projects for local development.

**Options:**
- None documented

**Examples:**
```bash
# In the directory of the specification you want to link
spm link

# In the project where you want to use the linked specification
spm link specification-name
```

#### `spm run <script>`

Run custom scripts defined in spec.json.

**Options:**
- None documented

**Examples:**
```bash
spm run validate
spm run test
spm run docs
```

## ENVIRONMENT VARIABLES

- `SPECKY_REGISTRY`: Registry URL
- `SPECKY_AUTH_TOKEN`: Authentication token
- `SPECKY_PROXY`: HTTP proxy
- `SPECKY_HTTPS_PROXY`: HTTPS proxy
- `SPECKY_CACHE`: Cache directory
- `SPECKY_COLOR`: Enable/disable color output (true/false)

## FILES

- `spec.json`: Component metadata and dependencies
- `component.md`: Component description
- `datamodel.json`: Data model definition
- `features/`: Directory for Gherkin feature specifications
- `.speckyrc`: Project-specific configuration
- `~/.speckyrc` or `~/.config/specky/config`: User configuration

## SEE ALSO

For more detailed information on specific topics:

- Component Specifications
- Specky Commands
- Best Practices for creating specifications

## AUTHOR

The Specky Team

## REPORTING BUGS

Report bugs to the issue tracker at https://github.com/specky/issues