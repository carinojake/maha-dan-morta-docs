# รายงานผลการทดสอบอัตโนมัติและการเล่นตรวจรับ (TEST_REPORT)

เอกสารฉบับนี้สรุปผลการทดสอบ **ก่อนเริ่มแก้ไข (Baseline)** เทียบกับ **หลังแก้ไขครบงาน A–E** พร้อมจำแนกสาเหตุของชุดทดสอบที่เคยล้มเหลว 2 รายการ และบันทึกผลการทดลองเล่นจริงในฉากต้นแบบและลานบอส

---

## 1. สรุปผลการรันชุดทดสอบอัตโนมัติ (Before vs After)

- **เอนจินที่ใช้ทดสอบ:** Godot `4.7.2.stable.official.ed1daf0bf` (Apple Silicon M1)
- **คำสั่งรันรวม:** `./tools/run_qa.sh`
- **ผลก่อนแก้ (Baseline):** `9 PASS / 2 FAIL` (และพบ ObjectDB/OggPacketSequence resource leak ตอนปิดโปรเซสในโหมด headless)
- **ผลหลังแก้ (Final):** **`12 PASS / 0 FAIL` (รวมชุดทดสอบใหม่ `test_quality_mission_e2e.gd`) และ `0 ObjectDB Leaks`**

| ลำดับ | ชุดทดสอบ (`tests/*.gd`) | ผลก่อนแก้ | ผลหลังแก้ | ขอบเขตที่ตรวจรับ |
|---:|---|---|---|---|
| 1 | [test_ui_layout_regression.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_ui_layout_regression.gd) | ✅ PASS | ✅ PASS | ขอบเขต HUD, ป้ายห้อง, แถบบอส, แบนเนอร์ และเมนูขวาบนในกรอบ `1280x720` |
| 2 | [test_ipad_touch_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_ipad_touch_e2e.gd) | ❌ **FAIL** | ✅ **PASS** | ปุ่มทัชมือซ้ายข้างเดียวบนจอ iPad `1280x960` (`x <= 600, 400 <= y <= 950`), โหมดสลับ 2 มือขวา, จอยลอย และ Z-order หน้าคัมภีร์ |
| 3 | [test_bai_yueling_cutin_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_bai_yueling_cutin_e2e.gd) | ✅ PASS | ✅ PASS | คัตอินท่าไม้ตายครบ 7 ตัวละคร, หยุดเวลาขณะเล่นคลิป, กดข้ามได้ และจำโหมดคัตอิน |
| 4 | [test_combat_depth_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_combat_depth_e2e.gd) | ✅ PASS | ✅ PASS | Perfect Dodge (`+30 ปราณ` + คริ 100%), คอมโบแรงก์ `D ➔ SSS`, ปฏิกิริยาธาตุครบ 6 คู่ |
| 5 | [test_dark_jade_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_dark_jade_e2e.gd) | ❌ **FAIL** | ✅ **PASS** | โครงสร้าง 10 ห้องในบทที่ 1–2, ห้องฝึกสอน Ch1 Room 0 (ศัตรู 1 ตัว), ห้องปกติ (ศัตรู 3 ตัว), สกิล Q/E/R, สลับตัวละคร และโถงบอส |
| 6 | [test_7heroes_costumes_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_7heroes_costumes_e2e.gd) | ✅ PASS | ✅ PASS | สไปรต์ชีตเดิน 4 ทิศ (`1200x320`) + โจมตี/สกิล 4 เฟรม (`1600x320`) ครบ 7 ตัวละคร + ชุดทางเลือก + ฮีลสำรองของเฮยซานเหล่ากุ่ย |
| 7 | [test_ch3_and_meta_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_ch3_and_meta_e2e.gd) | ✅ PASS | ✅ PASS | ประตูกั้นห้องครบ 9 บาน, กับดักใบเลื่อย, ระบบผสานปราณสลับตัว และการอัปเกรดถาวร |
| 8 | [test_100_floors_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_100_floors_e2e.gd) | ✅ PASS | ✅ PASS | ความต่อเนื่องครบ 10 บท × 10 ห้อง = 100 ชั้น (รวมบอสและมินิบอสทุกบท) |
| 9 | [test_p1_progression_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_p1_progression_e2e.gd) | ✅ PASS | ✅ PASS | พรเซียน 12 แบบ, หลอม/สวมใส่/ตีบวกอุปกรณ์, รหัสย้ายเซฟ `MDM1:`, ความเร็ว `1.5×`, หน้าสถานะตัวละคร + พรีวิวค่าพลัง |
| 10 | [test_hd2d_prototype_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_hd2d_prototype_e2e.gd) | ✅ PASS | ✅ PASS | ระบบแสงเงา Normal Map (`CanvasTexture`), ไฟห้องคัดแยก (`PointLight2D`), และพอร์ตเทรต HD-2D |
| 11 | [test_p3_polish_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_p3_polish_e2e.gd) | ✅ PASS | ✅ PASS | ไอเทมครบ 15 ชิ้น, โบนัสเซ็ต 4 ภพ, ตีบวก `+5`, ภารกิจความสำเร็จ, ระบบฝึกยุทธ์ 3 ขั้น |
| 12 | [test_quality_mission_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_quality_mission_e2e.gd) | *(สร้างใหม่)* | ✅ **PASS** | ท่าเดินไป๋เยว่หลิง, AI ศัตรู 8 เฟรม + บอส 10 เฟรม + Anti-stacking, ลบวงเตือนทันทีเมื่อบอสตาย, ลบป้าย `"⛩️ ทางออก →"` เมื่อข้ามห้อง, กฎอุปกรณ์ 1 ชิ้นต่อ 1 ตัวละคร, รักษาสัดส่วน HP/SP, โบนัส CDR/CRIT, กันปุ่มลั่นหลังบอสชนะ และจำโหมดออโต้/ลดเอฟเฟกต์ข้ามบท |

---

## 2. การจำแนกสาเหตุของชุดทดสอบที่เคยล้มเหลว 2 รายการ

### 2.1 `tests/test_ipad_touch_e2e.gd` — จำแนกเป็น **บั๊กของเกมจากการแก้ปุ่มทัชรอบก่อน (Game Bug)**
- **อาการก่อนแก้:** `FAIL: iPad left-hand button out of thumb zone: Q 18 -> [P: (1032.0, 880.0), S: (64.0, 64.0)]`
- **สาเหตุจริงจากโค้ด:** ใน [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) กำหนดความหมายของ `touch_mirror == false` คือโหมด **"มือซ้ายข้างเดียว"** และ `touch_mirror == true` คือโหมด **"สลับ 2 มือ (ขวา)"** แต่ใน [mobile_controls_landscape.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/mobile_controls_landscape.gd) กลับสลับเงื่อนไข `if not _mirrored:` ไปวางปุ่มสกิลไว้ฝั่งขวา (`x = 1032`) และวางปุ่ม `📜 คัมภีร์` ไว้กลางค่อนขวา (`x = 690`, `end.x = 786 > 600`) ทำให้โหมดมือซ้ายข้างเดียวใช้งานด้วยนิ้วโป้งซ้ายบน iPad ไม่ได้จริง
- **วิธีแก้:** จัดตำแหน่งใน `layout_for_viewport()` ของ [mobile_controls_landscape.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/mobile_controls_landscape.gd) ให้ตรงตามสเปกจริง: เมื่อ `not _mirrored` (โหมดมือซ้ายข้างเดียว) ปุ่มโจมตี สกิล หลบ ออโต้ ความเร็ว และคัมภีร์ทั้งหมดอยู่ในโซนนิ้วโป้งซ้าย (`x = 18..578 <= 600, y = 766..908 <= 950`) และเมื่อกดสลับโหมด 2 มือ (`_mirrored = true`) ปุ่มโจมตี/สกิลจะย้ายไปฝั่งขวา (`x >= 1032`) พร้อมเพิ่ม assertion ตรวจครบทั้งสองโหมดโดยไม่ลดความเข้มงวดของเทสต์เดิม

### 2.2 `tests/test_dark_jade_e2e.gd` — จำแนกเป็น **ชุดทดสอบเก่าที่ยังไม่อัปเดตตามระบบห้องฝึกยุทธ์ P3 (Outdated Test)**
- **อาการก่อนแก้:** `Assertion failed: Ch1 Room 0 should spawn 3 enemies`
- **สาเหตุจริงจากโค้ด:** ใน [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) บรรทัด `_spawn_room_wave()` จงใจเกิดศัตรู `cultist` เพียง **1 ตัว** เฉพาะใน **บทที่ 1 ห้องแรก (`chapter == 1 and current_room == 0`)** เพื่อให้ผู้เล่นใหม่ฝึกหลบพริบตา–แปะธาตุ–สลับตัว 3 ขั้นตอนโดยไม่ถูกรุม ส่วนห้องปกติอื่นทั้งหมดเกิดศัตรู **3 ตัว** ตามเดิม แต่ไฟล์ `test_dark_jade_e2e.gd` เขียนตั้งแต่ก่อนมีระบบ Interactive Tutorial
- **วิธีแก้:** ปรับ `test_dark_jade_e2e.gd` ให้ตรวจยืนยันทั้งกฎห้องฝึกสอน (`Ch1 Room 0 == 1 enemy`) **และ** ตรวจยืนยันห้องปกติที่ไม่ใช่ห้องฝึกสอน (`Ch2 Room 0 == 3 enemies`) ครบทั้งสองกรณี

### 2.3 การกำจัด Resource Leak ตอนปิดเกมในโหมด Headless (Environment / Audio Hygiene)
- **อาการก่อนแก้:** ตอนจบเทสต์มีคำเตือน `ObjectDB instances leaked at exit` จาก `AudioStreamWAV` / `OggPacketSequence` เพราะตัวขับเสียง Dummy ใน `--headless` ไม่เดินเข็มเวลาเสียงจนจบ
- **วิธีแก้:** เพิ่มเงื่อนไขตรวจ `DisplayServer.get_name() != "headless"` ก่อนสั่ง `.play()` ใน [sfx_synth.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/sfx_synth.gd) และ [cutin_overlay.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/cutin_overlay.gd) ทำให้ทุกเทสต์จบคลีน `0 leaks` 100%

---

## 3. บันทึกผลการทดลองเล่นตรวจรับ 3 โหมดควบคุม + โหมดลดเอฟเฟกต์

ได้จำลองและรันผ่านฉากต้นแบบ **"ป่าไผ่หมอกพิษ"** (ห้องที่ 1–2) ต่อเนื่องถึง **โถงบอส Furnace Guardian** (ห้องที่ 10) ครบทั้ง 4 รูปแบบการใช้งาน:

1. **โหมดคีย์บอร์ดมาตรฐาน (`WASD + F + Q/E/R + Space`):**
   - ไป๋เยว่หลิงเดินครบ 8 ทิศ (ซ้าย ขวา ขึ้น ลง และแนวทแยง) จังหวะก้าวสัมพันธ์กับระยะทางจริง หยุดเดินแล้วเท้าวางนิ่งบนเฟรมยืนตรงทันที
   - ศัตรู `อสูรเงาคลั่ง` แยกสล็อตล้อมรอบตัวละคร ไม่ซ้อนทับกันเป็นก้อนเดียว มีจังหวะง้างดาบพร้อมกรวยแดงเตือนก่อนฟันจริง
   - เมื่อเคลียร์ห้อง โอสถหยกและป้าย `"⛩️ ทางออก →"` ปรากฏหน้าซุ้มประตูขวา และหายไปทันทีเมื่อก้าวเข้าสู่ห้องที่ 2
2. **โหมดโจมตีอัตโนมัติ (`[C] ออโต้ ON`) + ข้ามบท:**
   - เปิดโหมดออโต้แล้วเปลี่ยนบทผ่านหอคัมภีร์ ยืนยันว่าสถานะ `auto_attack = true` ยังคงเปิดต่อเนื่อง ไม่หลุดปิดเองเหมือนก่อนแก้
   - หลังสังหารบอสใหญ่ ระบบล็อกปุ่มชั่วคราว `0.85 วินาที` และตัดการโฟกัสปุ่มเริ่มใหม่ด้วย `Space` ป้องกันการกดสกิล/หลบค้างแล้วเผลอข้ามหน้าสรุปผลชั้น 100 ได้เด็ดขาด
3. **โหมดปุ่มทัชมือซ้ายข้างเดียว & โหมดสลับ 2 มือ (`📱 ทัช`):**
   - จอยลอยตอบสนองลื่นไหล โหมดออโต้ช่วยเดินเก็บโอสถหยกและพามุ่งหน้าสู่ทางออกเมื่อเคลียร์ห้องเสร็จ
4. **โหมดลดเอฟเฟกต์ (`✨ เอฟเฟกต์: ลดลง`) & ลดสั่นจอ (`📳 สั่นจอ: เบา / ปิด`):**
   - ความทึบของเอฟเฟกต์พื้นและจำนวนใบไผ่/สะเก็ดไฟลดลงครึ่งหนึ่ง วงเตือนสีแดงของบอสและตัวเลขดาเมจอ่านง่ายชัดเจน ไม่แสบตาและประหยัดทรัพยากรเครื่อง Mac M1

---

## 4. ผลตรวจผลกระทบต่อเวอร์ชันมือถือ (`maha-dan-morta-mobile/`)

จากการตรวจสอบโครงสร้างจริงใน `/Users/jake/Downloads/rocg/maha-dan-morta-mobile/` ([project.godot](file:///Users/jake/Downloads/rocg/maha-dan-morta-mobile/project.godot) และ [mobile_controls.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta-mobile/scripts/mobile_controls.gd)) โดย **ไม่คัดลอกโค้ดข้ามโปรเจกต์แบบเหมารวม**:
- **ความต่างสำคัญของเวอร์ชันมือถือ:** ใช้จอแนวตั้ง `540x880` (`Portrait`), มีแถบปุ่มล่าง `dock` 4 ปุ่ม (`ออโต้ / ความเร็ว / คัมภีร์ / คู่มือ`), แถบสลับตัวละครแนวตั้งซ้ายบน `hero_slot_col`, และ `CharacterStatusUI` รันในโหมด `is_portrait = true`
- **สิ่งที่นำไปใช้ต่อได้ทันทีเมื่อพอร์ตลงมือถือในเฟสถัดไป:**
  1. ไฟล์สไปรต์ที่ซ่อมแล้วทั้งหมด (`assets/heroes/bai_yueling/**/walk_*.png`, `assets/monsters/shadow_cultist_sheet.png`, `assets/monsters/boss_ch1_sheet.png`, `assets/env/prop_*.png`, `assets/monsters/shadow_hound.png`)
  2. โมดูลข้อมูลและคำนวณสมดุล ([equip_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/equip_data.gd), [hero_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/hero_data.gd), [vfx_studio.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/vfx_studio.gd), [save_manager.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/save_manager.gd))
- **สิ่งที่ต้องปรับ Layout แยกเฉพาะมือถือเมื่อพอร์ต:**
  1. หน้าคลังศาสตราวุธ (`Tab 3`) ใน `codex_ui.gd` ของมือถือต้องเรียงการ์ดช่องสวมใส่ 3 ช่องเป็นแนวตั้ง (`VBoxContainer`) หรือแถวละ 1–2 ช่องให้พอดีความกว้าง `540 px` (ห้ามใช้ความกว้าง `860 px` ของจอแนวนอน)
  2. ห้ามทับ `mobile_controls.gd` ของมือถือด้วย `mobile_controls_landscape.gd` เพราะตำแหน่ง `dock_y` และ `hero_slot_col` ออกแบบเฉพาะจอแนวตั้ง

---

## 5. การยืนยันความปลอดภัยของไฟล์เซฟจริงของผู้ใช้

ตรวจสอบค่า SHA-256 ของไฟล์เซฟจริง `/Users/jake/Library/Application Support/Godot/app_userdata/มหาแดนพันภพ- ศึกผนึกกระบี่สะบั้นสวรรค์/save_data.json` คงเดิมที่ `6f706b27cede05d834d47542937a87826845f6283338e43c401e577ac17052a1` ไม่มีการเขียนทับหรือแก้ไขใด ๆ ทั้งสิ้น
