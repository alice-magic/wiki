# Instance Manifest Schema

The instance manifest defines all files required to run a Minecraft instance, including mods, resource packs, configurations, and other assets.

* **type**: `array`
* **description**: List of downloadable files with integrity data

---

## Manifest Structure

The manifest is a JSON array containing file objects:

```json
[
  {
    "path": "mods/example-mod.jar",
    "url": "https://cdn.example.com/mod.jar",
    "size": 1234567,
    "hash": "abc123def456..."
  }
]
```

---

## Item Fields

| Field  | Type         | Required | Description                                       |
| ------ | ------------ | -------- | ------------------------------------------------- |
| `path` | string       | ✓        | Relative file path in instance directory          |
| `url`  | string (URI) | ✓        | Download URL                                      |
| `size` | integer      | ✓        | File size in bytes                                |
| `hash` | string       | ✓        | SHA-1 hash for integrity verification             |

---

## Path Guidelines

The `path` field determines where files are placed within the instance:

### Common Directories

* `mods/` - Mod JAR files
* `resourcepacks/` - Resource packs
* `shaderpacks/` - Shader packs
* `config/` - Mod configuration files
* `kubejs/` - KubeJS scripts
* `defaultconfigs/` - Default mod configurations
* `scripts/` - CraftTweaker or other scripting

### Path Rules

* Use forward slashes `/` (not backslashes)
* Paths are relative to instance root
* No leading slash
* Preserve original filenames where possible

---

## Hash Verification

The `hash` field uses **SHA-1** for file integrity:

* Ensures downloaded files match expected content
* Prevents corruption during download
* Verifies file authenticity

### Generate SHA-1 Hash

**Linux/macOS:**
```bash
sha1sum file.jar
```

**Windows (PowerShell):**
```powershell
Get-FileHash -Algorithm SHA1 file.jar
```

**Node.js:**
```javascript
const crypto = require('crypto');
const fs = require('fs');

const hash = crypto.createHash('sha1');
hash.update(fs.readFileSync('file.jar'));
// console.log(hash.digest('hex'));
```

---

## Complete Example

```json
[
  {
    "path": "mods/iris-fabric-1.9.2+mc1.21.8.jar",
    "url": "https://cdn.modrinth.com/data/YL57xq9U/versions/x2f4KxP0/iris-fabric-1.9.2%2Bmc1.21.8.jar",
    "size": 2741900,
    "hash": "4964121fd2d75eebec77dd8b723bc381952fe43a"
  },
  {
    "path": "mods/sodium-fabric-0.6.5+mc1.21.8.jar",
    "url": "https://cdn.modrinth.com/data/AANobbMI/versions/b2fzXn3C/sodium-fabric-0.6.5%2Bmc1.21.8.jar",
    "size": 891234,
    "hash": "9876543210abcdef9876543210abcdef98765432"
  },
  {
    "path": "resourcepacks/Faithful 64x - Release 10.zip",
    "url": "https://cdn.modrinth.com/data/r4GILswZ/versions/5T6GekBK/Faithful%2064x%20-%20Release%2010.zip",
    "size": 18001871,
    "hash": "00413cf363e708c93a11f87dd425f0a164f3c1fb"
  },
  {
    "path": "config/iris.properties",
    "url": "https://cdn.example.com/config/iris.properties",
    "size": 2048,
    "hash": "abcdef1234567890abcdef1234567890abcdef12"
  }
]
```

---

## Best Practices

### File Organization

* Group related files (all mods together, configs together)
* Maintain consistent naming conventions
* Use descriptive filenames

### URL Management

* Use CDNs for better download speeds
* Consider URL encoding for special characters
* Ensure URLs are publicly accessible
* Test all URLs before deployment

### Version Control

* Update manifest when adding/removing files
* Document changes in version control
* Test manifest before release

### Performance

* Minimize total file count where possible
* Combine small files when appropriate
* Use compressed formats (ZIP, JAR)
* Consider file size impact on download time

---

## Validation

Before deployment, verify your manifest:

1. **Check all URLs** - Ensure files are downloadable
2. **Verify hashes** - Confirm SHA-1 matches actual files
3. **Validate paths** - Check for correct directory structure
4. **Test sizes** - Ensure size values are accurate
5. **JSON syntax** - Validate JSON formatting

---

## See Also

* [Instance Configuration](instance-configuration.md) - Main instance configuration schema
* [HTTP Headers](http-headers.md) - Authentication headers for download requests
* [Back to Documentation Index](README.md)
