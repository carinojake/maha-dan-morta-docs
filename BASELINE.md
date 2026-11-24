# BASELINE REPORT — MAHA DAN MORTA QUALITY IMPROVEMENT

**วันที่บันทึกสถานะตั้งต้น:** 2026-11-24  
**ผู้ตรวจสอบ:** Lead Godot Developer / Technical Artist / Game QA  
**โปรเจกต์หลัก:** `/Users/jake/Downloads/rocg/maha-dan-morta`  
**โปรเจกต์มือถือ (ตรวจผลกระทบ):** `/Users/jake/Downloads/rocg/maha-dan-morta-mobile`

---

## 1. สภาพแวดล้อมและเครื่องมือจริง (Verified Environment)

- **เครื่อง:** MacBook Air (Apple Silicon M1) · macOS (`zsh`)
- **Godot Engine:** `/opt/homebrew/bin/godot` — `4.7.2.stable.official.ed1daf0bf`
- **Export Templates ที่ติดตั้งจริง:** `~/Library/Application Support/Godot/export_templates/4.7.2.stable/`
  - มีครบทั้ง `web_nothreads_release.zip` (10.2 MB), `web_nothreads_debug.zip`, `macos.zip`, `windows_*`, `linux_*`
- **Python Environment สำหรับจัดการ Asset/QA:** `/Users/Shared/my_ai_project/venv/bin/python3` (มี `Pillow`, `numpy`)
- **เอกสารคำแนะนำในโปรเจกต์:**
  - ไม่พบไฟล์ชื่อ `AGENTS.md` ภายใน `/Users/jake/Downloads/rocg/` (มีกฎปลั๊กอินภายนอกและเอกสาร [`MAHA_DAN_PANPAPH_AI_START_HERE.txt`](file:///Users/jake/Downloads/rocg/MAHA_DAN_PANPAPH_AI_START_HERE.txt), [`TECHNICAL_HANDOFF_REPORT.md`](file:///Users/jake/Downloads/rocg/TECHNICAL_HANDOFF_REPORT.md), Character Bibles ของไป๋เยว่หลิง เฉินซี และเฮยซานเหล่ากุ่ย)
  - ตรวจสอบ Canon C-001 ถึง C-022 และ Character Bible ยืนยันชัดเจนว่า:
    - **ไป๋เยว่หลิง (`bai_yueling`):** เซียนหญิง ชุดผ้าไหมขาวหิมะขลิบเงิน ผ้าคาดเอวหยกคราม กระบี่บินผลึกหยก ไอเกล็ดหิมะ (C-008)
    - **เฮยซานเหล่ากุ่ย (`hei_shan`):** **ผู้หญิง** — องค์หญิงแห่งแดนเซียน พี่สาวแท้ๆ ของไป๋เยว่หลิง มีจิ้งจอกขาวเซียนพิทักษ์ ชุดฮั่นฝูขาว-แดงชาดจักรพรรดิ-ทองคำ

---

## 2. สถานะ Git, ไฟล์ค้าง และการสำรองข้อมูล (Backup & Working Tree)

### 2.1 สถานะ Git ก่อนเริ่มงาน
- **Branch (`maha-dan-morta`):** `main...origin/main [ahead 7]`
- **HEAD Commit (`maha-dan-morta`):** `1013476 fix(p2): fix _rebuild_party_hud call and mobile codex sheet height`
- **HEAD Commit (`maha-dan-morta-mobile`):** `1961e2f` (บน `main`)

### 2.2 การสำรองข้อมูลครบชุดก่อนแก้ไข (Zero-Overwrite Safeguard)
ได้สำรองสถานะไฟล์ทั้งหมดรวมไฟล์ Modified, Untracked, `/tmp/rocg-polish-baseline/` และไฟล์เซฟจริงของผู้เล่นไว้เรียบร้อยก่อนรันเทสต์หรือแก้ไฟล์ใดๆ:
- **สำเนาโปรเจกต์และ Git Patch:** `/Users/jake/.gemini/antigravity/brain/2c2a760f-992e-4227-ab58-78b849405519/scratch/baseline_backup/`
  - `maha-dan-morta/` (119 MB รวมไฟล์ untracked ทั้งหมด)
  - `maha-dan-morta-mobile/` (76 MB)
  - `rocg-polish-baseline/` (สำเนา 3 ไฟล์จาก `/tmp/rocg-polish-baseline`: `codex_ui.gd`, `dark_jade_dungeon.gd`, `vfx_studio.gd`)
  - `git_status_main.txt` และ `git_diff_main.patch`
- **สำเนาเซฟจริงของผู้เล่น (`user://save_data.json`):**
  - สำรองไว้ที่ `/Users/jake/.gemini/antigravity/brain/2c2a760f-992e-4227-ab58-78b849405519/scratch/user_saves_backup/`
  - ค่า SHA-256 ของเซฟจริง (`มหาแดนพันภพ- ศึกผนึกกระบี่สะบั้นสวรรค์/save_data.json`): `6f706b27cede05d834d47542937a87826845f6283338e43c401e577ac17052a1` (ผ่านครบ 10 บท, หยกวิญญาณ 13,572, อุปกรณ์ครบ 15 ชิ้น +5 — ยืนยันไม่ถูกแก้ไขหลังรันเทสต์ตั้งต้น)

### 2.3 รายการไฟล์ที่มีงานแก้ค้างก่อนเริ่ม (Uncommitted & Codex Polish Edits)
1. **งานแก้ชุดล่าสุดจาก Codex (เทียบกับ `/tmp/rocg-polish-baseline/`):**
   - [`scripts/dark_jade_dungeon.gd`](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd):
     - ผูก `walk_timer` กับอัตราส่วนความเร็วจริง (`player.velocity.length() / data["speed"]`)
     - ตั้ง `target_z = 0.0` และปิดการยืด/เอียง `player_sprite.scale` / `rotation` ตอนเดิน
     - เพิ่ม `gait` โยกตัวเบาๆ ให้ศัตรู และเอียง `-12 deg` ช่วง `windup`
     - เปลี่ยน `z_index` ของวงเตือนบอส (`outer`) จาก `-4` เป็น `8`
     - เพิ่ม `Label` `"ทางออก →"` แบบ `add_child(exit_hint)` ลงบน Root Scene โดยไม่ได้เก็บเข้า `room_props` หรือล้างเมื่อเปลี่ยนห้อง (ทำให้ค้างข้ามห้องจริงตามที่รายงาน)
   - [`scripts/vfx_studio.gd`](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/vfx_studio.gd):
     - ตั้ง `root.modulate.a = 0.48` แบบเหมาเข่งใน 10 ฟังก์ชันหลัก (ทำให้ทั้งฟันปกติ ท่าไม้ตาย และวงพื้นจางเท่ากันหมด ขาดลำดับความสำคัญของภาพ)
   - [`scripts/codex_ui.gd`](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/codex_ui.gd):
     - ขยาย `font_size` รายการอุปกรณ์จาก `11` เป็น `13` และเปลี่ยนปุ่มของที่สวมจาก `"✓ ใส่อยู่"` เป็น `"ถอดออก"`
2. **ไฟล์ Modified & Untracked อื่นๆ ที่มีอยู่ก่อนหน้า (รักษาไว้ทั้งหมด):**
   - สไปรต์ 4 ทิศ (`walk_down`, `walk_up`, `walk_side`, `attack_4f`, `skill_4f`) ของทั้ง 7 ฮีโร่ และชุด `alt` / `alt_snow` ของไป๋เยว่หลิง
   - ระบบเกราะ + Set Bonus 4 ภพภูมิ (`equip_data.gd`, `character_status_ui.gd`, `save_manager.gd`, `test_p3_polish_e2e.gd`)
   - สไปรต์ฉาก HD + Normal maps (`assets/env/*_n.png`) และซีน `scenes/test_hd2d_prototype.tscn`

---

## 3. ผลทดสอบตั้งต้นก่อนแก้ไข (Baseline Headless Test Results)

รันแยกทีละชุดด้วย `godot --headless --path . -s res://tests/<test>.gd`:

| ชุดทดสอบ | สถานะก่อนแก้ | เวลา/Exit | รายละเอียดและการจำแนกสาเหตุจริง |
| :--- | :---: | :---: | :--- |
| `test_ui_layout_regression.gd` | ✅ PASS | `exit=0` | HUD ทั้งหมดอยู่ในกรอบ `1280x720` ไม่ซ้อนทับ |
| `test_ipad_touch_e2e.gd` | ❌ **FAIL** | `exit=1` | `FAIL: iPad left-hand button out of thumb zone: Q 18 -> [P: (1032.0, 880.0)]` |
| `test_bai_yueling_cutin_e2e.gd` | ✅ PASS | `exit=0` | ผ่านครบ 7 ฮีโร่ และโหมด `full` / `boss` / `skip` |
| `test_combat_depth_e2e.gd` | ✅ PASS | `exit=0` | Perfect Dodge, Combo D->SSS, 6 Elemental Reactions ผ่าน |
| `test_dark_jade_e2e.gd` | ❌ **FAIL** | `SCRIPT ERROR` | `Assertion failed: Ch1 Room 0 should spawn 3 enemies` (บรรทัด 24) |
| `test_7heroes_costumes_e2e.gd` | ✅ PASS | `exit=0` | 7 ฮีโร่ × 2 ชุด + สลับตัว + ฮีล ผ่าน |
| `test_ch3_and_meta_e2e.gd` | ✅ PASS | `exit=0` | Ch3 10 ห้อง + Synergy + Meta ผ่าน |
| `test_100_floors_e2e.gd` | ✅ PASS | `exit=0` | ตรวจครบ 10 บท 100 ชั้น |
| `test_p1_progression_e2e.gd` | ✅ PASS | `exit=0` | 12 Boons + Gear +1..+5 + Stat Preview + 1.5x + Fake 2.5D ผ่าน |
| `test_p3_polish_e2e.gd` | ✅ PASS | `exit=0` | 15 Gear + 4-Realm Set Bonus + Achievements + Tutorial + Touch Mirror ผ่าน |
| `test_hd2d_prototype_e2e.gd` | ✅ PASS | `exit=0` | HD-2D Prototype ผ่าน |

### การจำแนกปัญหาทดสอบตามข้อกำหนดข้อ 5 (Root-Cause Classification)

1. **`test_ipad_touch_e2e.gd` (`ปุ่ม Q นอกพื้นที่นิ้วโป้ง`):**
   - **จำแนก:** **ลูกผสมระหว่าง (ก) ชุดทดสอบล้าสมัยหลังเปลี่ยนดีฟอลต์เป็นมือขวา และ (ข) บั๊ก Layout โหมดมือซ้ายจริง**
   - **หลักฐาน:** เดิม `mobile_controls_landscape.gd` (commit `1f59859`) วางปุ่มทั้งหมดทางซ้ายเป็นค่าเริ่มต้น ต่อมางาน P3 เพิ่มโหมดสองมือมาตรฐาน (`_mirrored = false`: จอยซ้าย ปุ่มขวา `x=1032`) และโหมดสลับมือซ้าย (`_mirrored = true` / `touch_mirror`) แต่ (1) เทสต์ไม่ได้เปิด `touch_mirror = true` ก่อนเช็กโซนมือซ้าย และ (2) แม้เปิด `_mirrored = true` แล้ว ปุ่ม Utility (`btn_auto`, `btn_speed`, `btn_codex`) ใน `mobile_controls_landscape.gd` บรรทัด 97–99 ยังวางค้างกลางจอ (`btn_codex` จบที่ `x = 786 > 600`) ซึ่งหลุดพ้นระยะนิ้วโป้งซ้ายบน iPad `1280x960` จริง
2. **`test_dark_jade_e2e.gd` (`จำนวนศัตรูห้องแรกไม่ตรง`):**
   - **จำแนก:** **ชุดทดสอบล้าสมัยจากการปรับดีไซน์ห้องฝึกสอน (Tutorial Room)**
   - **หลักฐาน:** ใน `dark_jade_dungeon.gd` บรรทัด 1507–1511 ห้องแรกสุดของบทที่ 1 (`chapter == 1 and current_room == 0 and not boss_rush_mode`) ถูกปรับให้สปอว์นศัตรูฝึกซ้อม `1` ตัว (`cultist` HP 150) เพื่อรองรับ Interactive Tutorial ในขณะที่ห้องปกติ (`current_room in [1, 2, 3]` หรือบทอื่น) ยังคงสปอว์น `3` ตัว แต่ `test_dark_jade_e2e.gd` บรรทัด 24 ยังเช็ก `enemies.size() == 3` ที่ Ch1 Room 0
3. **`cutin_mode=skip` และ `test_p1_progression_e2e` (จำนวน starter gear):**
   - **จำแนก:** **ได้รับการแก้ใน working tree ก่อนหน้าแล้ว** (`save_manager.gd` บรรทัด 101–105, 162 และ `equip_data.gd` `STARTER_GEAR` 3 ชิ้นสอดคล้องกับ `test_p1_progression_e2e.gd` ปัจจุบัน) จึงผ่านในการรันตั้งต้นรอบนี้
4. **ObjectDB leak ตอนปิดเทสต์ (4–18 instances + 2–4 resources):**
   - **จำแนก:** **บั๊กการคืนทรัพยากรเสียงในเกม/เทสต์จริง**
   - **หลักฐานจาก `--verbose`:** `Leaked instance: AudioStreamWAV / AudioStreamOggVorbis / AudioStreamPlaybackWAV / OggPacketSequence` และ `Resource still in use: res://music/310-world-map-loop.ogg` เกิดจากแคชเสียงและ `AudioStreamPlayer` ที่ยังเล่นค้างขณะเรียก `SceneTree.quit(0)` ทันทีโดยไม่หยุดและล้าง stream ใน `_exit_tree()`

---

## 4. ข้อค้นพบเบื้องต้นด้านภาพและเกมเพลย์ (Pre-Fix Code & Asset Inspection)

1. **สาเหตุที่ไป๋เยว่หลิงเดินเหมือนเขย่ง (งาน A):**
   - ตรวจพบใน `tools/enhance_walk_stride_motion.py` ที่รันทับ `walk_side.png`, `walk_down.png`, `walk_up.png`:
     - ใน `walk_side.png`: ฟังก์ชัน `shear_side_frame()` ยกลำตัวบนขึ้น `dy = -2px` และยกปลายเท้าทั้งสองข้างลอยขึ้นจากพื้น `dy = -4px` ทั้งในเฟรม 0 และเฟรม 2 ขณะที่เฟรม 1 อยู่ระดับปกติ ทำให้ลำดับเฟรม `[0, 1, 2, 1]` ตัวลอยขึ้น–ลงทุกก้าวเหมือนเดินเขย่ง และบิดชายกระโปรงทั้งผืนไปหน้า–หลังแทนการก้าวเท้าสลับซ้าย–ขวา
     - ใน `walk_down.png` / `walk_up.png`: ฟังก์ชัน `lift_front_back_leg()` ตัดครึ่งภาพตามแนวตั้งที่ `x = 200` แล้วยกซีกซ้าย/ขวาของชายกระโปรงขึ้น `8px` จนขอบชุดขาดเป็นขั้นบันไดและดูเขย่ง
2. **ป้ายทางออกค้างข้ามห้อง & โหมดตีอัตโนมัติ / บทที่ 100 (งาน B–D):**
   - `exit_hint` ถูก `add_child()` ลง Root โดยไม่อยู่ในรายการล้างเมื่อเข้าห้องใหม่
   - ต้องตรวจการรีเซ็ต `auto_attack_enabled` ตอนเปลี่ยนบท และการรับปุ่มสกิลซ้ำขณะแสดงหน้าสรุปหลังจบชั้น 100
3. **สวมอุปกรณ์ซ้ำหลายตัวละคร (งาน E):**
   - ในเซฟจริงพบ `golden_dragon_armor`, `heaven_halberd`, `immortal_pill` ถูกสวมให้ฮีโร่ 6 คนพร้อมกันจากกติกาเดิมที่ไม่เช็กการถือครองข้ามตัวละคร จะต้องตรวจกติกาและระวังไม่ทำให้เซฟเดิมเสียหาย
