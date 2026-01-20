# Instance Announcement Integration

Neko Launcher supports displaying announcements for each instance using the `announcementUrl` property in your instance configuration file. You can notify users about news, events, or important updates directly in the launcher.

---

## Overview

This feature allows you to:
* **In-app notifications** – Display news, events, and announcements to players
* **Multi-language support** – Show localized content and images
* **Flexible metadata** – Add custom fields as needed

---

## Configuration

Add `announcementUrl` to your instance configuration file:

```json
{
    "name": "my-instance",
    "announcementUrl": "https://example.com/announcement-instance.json"
}
```

The URL should point to a publicly accessible JSON file containing an array of announcements.

---

## Announcement JSON Format

Each announcement should have the following structure:

| Field      | Type      | Description                                 |
|------------|-----------|---------------------------------------------|
| title      | string    | Announcement title                          |
| category   | string    | 'NOTICE', 'NEWS', or 'EVENT'                |
| link       | string    | URL for more details                        |
| active     | boolean   | Whether the announcement is currently shown |
| date       | string    | ISO 8601 date/time                          |
| metadata   | object    | Additional info such as images, translations, etc. |

### Example

> This example is for demonstration only. Please adjust the data to fit your instance.

```json
[
    {
        "title": "Scheduled System Maintenance",
        "category": "NOTICE",
        "link": "https://status.furimoe.com",
        "active": true,
        "date": "2024-02-10T10:00:00.000Z",
        "metadata": {}
    },
    {
        "title": "Ready to Become Aha?",
        "category": "NEWS",
        "link": "https://www.hoyolab.com/article/43294281",
        "active": true,
        "date": "2026-01-17T09:30:00.000Z",
        "metadata": {
            "th_title": "พร้อมกลายเป็น Aha แล้วหรือยัง?",
            "imageUrl": "https://upload-os-bbs.hoyolab.com/upload/2026/01/16/a5db23dc248ab90458d95a2f94cbbf6a_5621172732061645560.png?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70",
            "th_imageUrl": "https://upload-os-bbs.hoyolab.com/upload/2026/01/16/e62a985320960b34870fc65ef24168eb_2821068148369816818.png?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70"
        }
    },
    {
        "title": "Minato Aqua Christmas Special",
        "category": "NEWS",
        "link": "https://www.youtube.com/watch?v=6bnaBnd4kyU",
        "active": true,
        "date": "2024-12-25T15:00:00.000Z",
        "metadata": {
            "imageUrl": "https://cdn.furimoe.com/images/114758380_p0_master1200.jpg"
        }
    }
]
```

---

## Type Definitions

```typescript
export interface AnnouncementMetadata {
        imageUrl?: string;
        th_imageUrl?: string;
        [key: string]: any;
}

export interface Announcement {
        title: string;
        category: 'NOTICE' | 'NEWS' | 'EVENT';
        metadata: AnnouncementMetadata;
        link: string;
        active: boolean;
        date: Date;
}
```

---

## Best Practices

* Use ISO 8601 format for `date` (e.g., `2026-01-17T09:30:00.000Z`)
* Set `active: true` for visible announcements
* Use `metadata` for localization and images
* Host your JSON file on a reliable CDN or web server
* Test your URL for public accessibility

---

## Troubleshooting

* Ensure your `announcementUrl` is correct and accessible
* Validate your JSON format (use online JSON validators)
* Check image URLs and translations in `metadata`
* Use HTTPS for all resources

---

## See Also

* [Instance Configuration](instance-configuration.md)
* [HTTP Headers](http-headers.md)
* [Back to Documentation Index](README.md)
