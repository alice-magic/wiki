# MCP server

Neko Launcher เปิด **MCP (Model Context Protocol) server** บนเครื่องได้ เพื่อให้ AI client อย่าง Claude Code, Claude Desktop, Cursor หรือ Codex ดูและเปิด instance อ่าน log จัดการม็อด และสร้าง instance ในเครื่องให้คุณได้

## เปิดใช้งาน

1. ไปที่ **Settings → Developer → MCP server** แล้วเปิดสวิตช์ ค่าเริ่มต้นฟังที่ `127.0.0.1`
2. ตั้ง **access key** หากเครื่องอื่นเข้าถึงพอร์ตนี้ได้
3. ส่วน **Connect a client** ในหน้าเดียวกันมีคำสั่งหรือ config สำหรับแต่ละ client วิธีที่เร็วที่สุดคือใช้ CLI:

```bash
npx @neko-launcher/cli setup claude-code
```

**Settings → Developer → MCP tools** แสดงเครื่องมือทั้งหมดที่เซิร์ฟเวอร์มี ค้นหาได้ และดึงรายการจากเซิร์ฟเวอร์โดยตรง

## เครื่องมือ

ตอนนี้ลันเชอร์มี **76 เครื่องมือ** คำอธิบายด้านล่างเป็นข้อความเดียวกับที่ AI client เห็น (ภาษาอังกฤษ) เครื่องมือที่ขึ้นต้นด้วย *DESTRUCTIVE* จะลบข้อมูล client ที่ดีจะถามคุณก่อน

| Tool | What it does |
|---|---|
| `list_instances` | List every instance known to the launcher, installed or not. |
| `get_instance` | Full stored record for one instance, by name or unique id. |
| `get_settings` | The launcher's settings as stored in setting.json. The MCP access key is not returned: `mcp.hasKey` says whether one is set. |
| `get_system_info` | Launcher version, platform, arch and the on-disk paths it uses. |
| `list_instance_files` | List files in an installed instance. `path` is relative to the                  instance root and defaults to the top level. |
| `get_game_logs` | Buffered game output (stdout/stderr + launcher notes) per instance, oldest first, up to 2000 lines each. Omit `instance` for every instance. This is what the launcher's own log window shows. |
| `read_log` | Read the tail of the launcher log. `lines` defaults to 200. |
| `upsert_instance` | Create or update an instance record. Omit uniqueId to create a new one. |
| `delete_instance` | Delete an instance record by unique id. |
| `kill_instance` | Stop a running instance. |
| `list_accounts` | Signed-in accounts (uuid and name only, never tokens). |
| `get_installed_instances` | Instances with files on disk, with their stored records. |
| `get_local_instances` | Instances added by the user (custom / imported). |
| `get_local_instance` | One local instance record. |
| `get_instance_sizes` | Disk usage per installed instance, in bytes. |
| `fetch_api_instances` | The public instance catalogue from the Neko API. |
| `fetch_api_instance` | One catalogue instance by name. |
| `fetch_api_instance_install` | Install manifest (files, loader, version) for a catalogue instance. |
| `get_custom_instances` | Resolve a custom server address into instances the account can join. |
| `remove_custom_instance` | Remove a custom instance from the launcher list (files stay). |
| `update_instances_order` | Reorder the home list. |
| `force_refresh_instance_srv` | Re-resolve an instance's SRV record, bypassing the cache. |
| `remove_state_downloaded` | Forget that an instance is installed so the next launch re-verifies every file. |
| `delete_instance_files` | DESTRUCTIVE: delete an instance's game files from disk. |
| `delete_instance_entry` | DESTRUCTIVE: delete one file or folder inside an instance. |
| `focus_minecraft` | Bring a running instance's game window to the front. |
| `save_settings` | Write top-level keys into setting.json; keys not given are kept. The `mcp` block is ignored: this server is configured in the launcher's settings screen. |
| `update_settings` | Merge top-level keys into setting.json (e.g. {"ram": 8192}) and return the result. The `mcp` block is ignored, and reported with `hasKey` instead of the key. |
| `clear_game_logs` | Clear buffered game output. Omit `instance` for all. |
| `save_game_logs` | Write an instance's buffered game output to a file; returns the path. |
| `detect_java` | Find installed Java runtimes of a major version (8, 17, 21…). |
| `test_java` | Probe a java executable and report its version/vendor. |
| `install_java` | Download and install a Zulu JDK of the given major version; returns its path. |
| `get_update_channel` | Current updater channel. |
| `set_update_channel` | Switch the updater channel. |
| `check_update` | Ask the updater whether a newer launcher is published. |
| `start_update_download` | Download and install the pending launcher update. |
| `add_offline_account` | Add an offline (no Microsoft) account. |
| `start_device_login` | Start the Microsoft device-code sign-in; returns the code and URL the user must visit. |
| `remove_account` | Sign an account out. |
| `import_mrpack` | Import a Modrinth .mrpack from a local path. |
| `open_instance_folder` | Reveal an instance folder in the OS file manager. |
| `open_launcher_data_dir` | Reveal the launcher data directory. |
| `open_launcher_log_dir` | Reveal the launcher log directory. |
| `app_info` | Launcher build info. |
| `get_hardware_info` | CPU, RAM, GPU and OS summary. |
| `resolve_srv_record` | Resolve a Minecraft SRV record for a domain. |
| `modrinth_search` | Search Modrinth. |
| `modrinth_project_versions` | Versions of a Modrinth project compatible with a game version and loader. |
| `modrinth_install_version` | Install a Modrinth version file into an instance (mods/, resourcepacks/ or shaderpacks/ by kind). |
| `modrinth_list_mods` | Files of one content kind in an instance (mods by default). |
| `modrinth_remove_mod` | Delete a content file from an instance. |
| `list_game_storage` | Shared game versions and Java runtimes on disk, with sizes. |
| `delete_game_version` | DESTRUCTIVE: delete a shared game version folder. |
| `delete_java_runtime` | DESTRUCTIVE: delete an installed Java runtime. |
| `local_minecraft_versions` | Minecraft versions a local instance can use (releases and snapshots). |
| `local_loader_versions` | Loader builds for a Minecraft version, newest first. |
| `local_instance_create` | Create a local instance in the background (game and loader are pre-installed). Returns a jobId; poll local_jobs. |
| `modpack_preview` | Read a Modrinth .mrpack or CurseForge .zip on disk without installing it. |
| `modpack_import` | Import a .mrpack or CurseForge .zip as a local instance in the background. Returns a jobId; poll local_jobs. |
| `modpack_search` | Search modpacks on Modrinth, CurseForge, FTB or ATLauncher (popular when the query is empty). 20 per page. |
| `modpack_versions` | Versions of a modpack, newest first. |
| `modpack_install` | Install a modpack version as a local instance in the background. Returns a jobId; poll local_jobs. |
| `local_jobs` | Background installs: running ones with their latest progress, and finished ones with their result or error. |
| `local_job_cancel` | Cancel a running background install (its half-written folder is removed). |
| `local_instance_rename` | Rename a local instance. |
| `local_instance_delete` | DESTRUCTIVE: delete a local instance and its whole folder (worlds included). |
| `local_instance_settings` | A local instance's settings (neko-instance.json). |
| `local_instance_update_settings` | Change a local instance's settings. patch fields: displayName, description, mcVersion, loader {type, build}, ramMaxMb, jvmArgs[], gameArgs[] (only the ones given change). |
| `local_content_list` | Files of one kind in a local instance. |
| `local_content_set_enabled` | Turn a mod or pack on or off (renames to/from .disabled). |
| `local_content_delete` | DESTRUCTIVE: delete a file (or world) from a local instance. |
| `create_instance_shortcut` | Create a desktop shortcut that launches an instance. |
| `dev_paths` | Every path the launcher uses, spelled physically. |
| `dev_clear_caches` | Drop in-memory caches (DNS, server status, manifests). |
| `mcp_status` | This MCP server's config and bound address. `hasKey` says whether an access key is set; the key itself is never returned. |

## งานเบื้องหลัง

`local_instance_create`, `modpack_import` และ `modpack_install` ส่ง `jobId` กลับทันทีแล้วทำงานต่อในเบื้องหลัง (การติดตั้งอาจใช้เวลาหลายนาที) ใช้ `local_jobs` ดูความคืบหน้าและผลลัพธ์ หรือ `local_job_cancel` เพื่อยกเลิก หน้าต่างลันเชอร์จะแสดงงานเหล่านี้เหมือนงานที่เริ่มจากในลันเชอร์เอง

## สิ่งที่ไม่เปิดให้ใช้

การจัดการหน้าต่าง หน้าต่างเลือกไฟล์ของระบบ การล็อกอิน Microsoft ผ่านเบราว์เซอร์ access token ของ Microsoft และตัว access key จะไม่เปิดให้ใช้ผ่าน MCP
