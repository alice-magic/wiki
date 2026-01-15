[Download Neko Launcher](https://launcher.furi.moe/en) | [Furimoe](https://furi.moe)

# Neko Launcher Documentation

Welcome to the **Neko Launcher** documentation. This guide covers the JSON schemas, configuration options, and technical specifications for creating and managing Minecraft instances with Neko Launcher.

---

## Documentation Overview

### Core Schemas

* **[Instance Configuration](instance-configuration.md)** - Complete schema for configuring Minecraft instances, including version settings, metadata, and game arguments
* **[Instance Manifest](instance-manifest.md)** - File manifest schema for defining downloadable resources with integrity verification
* **[Social Links](social-links.md)** - Configure community, development, and store links for your instance

### Integration & Discovery

* **[DNS-based Discovery](dns-discovery.md)** - Auto-discovery configuration using DNS TXT records
* **[HTTP Header Verification](http-headers.md)** - Authentication and verification headers sent by the launcher

---

## Quick Start

1. Create an `instance.json` following the [Instance Configuration Schema](instance-configuration.md)
2. Generate a `manifest.json` with your instance files using the [Manifest Schema](instance-manifest.md)
3. (Optional) Configure [Social Links](social-links.md) for community engagement
4. (Optional) Set up [DNS Discovery](dns-discovery.md) for automatic instance detection

---

## Schema Reference

The main schema is available at:
```text
https://cdn.furimoe.com/schema/neko-launcher.json
```

Include it in your configuration:
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "my-instance",
  ...
}
```

---

## Support & Resources

* [GitHub](https://github.com/alice-magic)
* [Launcher Download](https://launcher.furi.moe/en)
* [Main Website](https://furi.moe)
