# Neko Launcher Documentation

Welcome to the **Neko Launcher** documentation. This guide covers the JSON schemas, configuration options, and technical details for creating and managing Minecraft instances with Neko Launcher.

---

## Table of Contents

### Core Schemas & Configuration

* **[Instance Configuration](instance-configuration.md)** – Complete schema for configuring Minecraft instances, including version, metadata, and game arguments
* **[Instance Manifest](instance-manifest.md)** – Manifest schema for defining downloadable files and integrity verification
* **[Social Links](social-links.md)** – Configure community, development, and store links

### Integration & Discovery

* **[DNS-based Discovery](dns-discovery.md)** – Auto-discovery setup using DNS TXT records
* **[HTTP Header Verification](http-headers.md)** – Authentication and verification via HTTP headers

---

## Quick Start

1. Create an `instance.json` using the [Instance Configuration Schema](instance-configuration.md)
2. Create a `manifest.json` for your instance files using the [Manifest Schema](instance-manifest.md)
3. (Optional) Set up [Social Links](social-links.md) for community features
4. (Optional) Configure [DNS Discovery](dns-discovery.md) for automatic instance detection

---

## Schema Reference

The main schema is available at:
```text
https://cdn.furimoe.com/schema/neko-launcher.json
```

Example usage in your config file:
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "my-instance",
  ...
}
```

---

## Instance Announcement

Display announcements on your instance page by setting the `announcementUrl` property in your config. The URL should point to a JSON file containing an array of announcements.

Example configuration:
```json
{
  "name": "my-instance",
  "announcementUrl": "https://example.com/announcement.json"
}
```

Example announcement JSON structure:
```json
{
  "title": "Maintenance Notice",
  "category": "NOTICE", // NOTICE | NEWS | EVENT
  "metadata": {
    "imageUrl": "https://example.com/image.png",
    "th_imageUrl": "https://example.com/image-th.png"
  },
  "link": "https://example.com/details",
  "active": true,
  "date": "2026-01-20T12:00:00Z"
}
```

> This is a sample format. Please adjust the data to fit your instance.

---

## Support & Resources

* [GitHub](https://github.com/alice-magic)
* [Launcher Download](https://launcher.furi.moe/en)
* [Main Website](https://furi.moe)
