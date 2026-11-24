# บันทึกการแก้ไขและยกระดับคุณภาพเกม (CHANGELOG)

เอกสารฉบับนี้สรุปรายการแก้ไขทั้งหมดในรูปแบบ **ปัญหา ➔ สาเหตุจริงที่ยืนยันจากโค้ด/สไปรต์ ➔ วิธีแก้ที่ต้นตอ ➔ ไฟล์ที่แก้ไข** โดยรักษางานค้างและประวัติ Git เดิมครบถ้วน 100%

---

## 1. งาน A — แก้ท่าเดินไป๋เยว่หลิงครบ 8 ทิศ และซิงก์จังหวะเท้า

| หัวข้อ | รายละเอียด |
|---|---|
| **ปัญหา** | ไป๋เยว่หลิงเดินขวาเหมือนเขย่ง/ลอยตัว เดินหน้า-ถอยหลังเท้าไม่สลับซ้ายขวา และชุดคลุมหิมะ (`alt_snow`) ตอนหยุดยืนค้างท่าก้าวขา |
| **สาเหตุจริง** | 1) สคริปต์เก่า `enhance_walk_stride_motion.py` เคยบิดแกนภาพ (`shear ±10 px`) และยกตัวละครขึ้น `3–6 px` ในเฟรมเดิน<br>2) `walk_side.png` ชุดปกติ คอลัมน์ 2 วาดสั้นกว่าคอลัมน์อื่น `8 px` และปลายเท้าหลังจิกแบบเขย่ง<br>3) `walk_down.png` ซ้ำขาข้างเดิมทั้งคอลัมน์ 0 และ 2<br>4) ชุด `alt_snow` เรียงลำดับเฟรมเป็น `[Idle, StrideA, StrideB]` ต่างจากชุดปกติ `[StrideA, Idle, StrideB]` ทำให้โค้ดที่เรียกเฟรม `1` ตอนหยุดยืนไปดึงภาพก้าวขามาแสดง<br>5) ตัวแปร `_prev_c` ในโค้ดถูกใช้ซ้ำทับกันระหว่างปุ่ม `[C]` (ออโต้) กับ `[K]` (สถานะ) |
| **วิธีแก้** | 1) สร้างสคริปต์ [rebuild_bai_yueling_walk.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/rebuild_bai_yueling_walk.py) สร้างชีต `1200x320` ใหม่จากต้นฉบับสะอาด ล็อกแกนศีรษะ `X = 200` และระนาบฝ่าเท้า `Y = 282` เท่ากันทุกเฟรม แก้สัดส่วนขาหลังคอลัมน์ 2 สลับก้าวซ้าย-ขวาจริงใน `walk_down` และจัดเรียงเฟรมชุด `alt` / `alt_snow` ให้ตรงมาตรฐาน `[StrideA, NeutralIdle, StrideB]`<br>2) ปรับลอจิกเดินใน [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) ให้เปลี่ยนเฟรมตามระยะทางเดินจริง (`WALK_PHASE_DIST = 28.0 px`), ออกตัวก้าวทันทีในเฟรมแรกที่ขยับ, เพิ่ม Hysteresis มุมทแยง `45°` ใช้ `walk_side` ไม่กระพริบสลับทิศ และแยกตัวแปร `_prev_k` ออกจาก `_prev_c` |
| **ไฟล์ที่แก้ไข** | [rebuild_bai_yueling_walk.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/rebuild_bai_yueling_walk.py), `assets/heroes/bai_yueling/**/walk_*.png`, `stride_0.png`, [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd), [qa_capture_walk.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/qa_capture_walk.gd) |

---

## 2. งาน B — ศัตรู (`อสูรเงาคลั่ง`) และบอส (`Furnace Guardian`) ที่มีชีวิต

| หัวข้อ | รายละเอียด |
|---|---|
| **ปัญหา** | ศัตรูและบอสใช้ภาพนิ่งเฟรมเดียวขยับโยกเล็กน้อย วิ่งเข้ามารุมซ้อนทับตัวละครจนอ่านท่าไม่ออก บอสมีแพตเทิร์นน้อย และวงเตือนบางจังหวะค้างเมื่อบอสตายหรือเปิดคัตอิน |
| **สาเหตุจริง** | 1) `shadow_cultist.png` และ `boss_ch1.png` มีเพียง 1 เฟรมนิ่ง และ `shadow_cultist.png` มีรูสีขาวติดอยู่ใต้รักแร้ซ้าย (รวมถึง `shadow_hound.png` ใต้ขาหน้า)<br>2) AI ศัตรูเดินพุ่งเข้าหาพิกัดผู้เล่นตรง ๆ โดยไม่มีรัศมีเว้นระยะหรือสล็อตทิศทาง และเช็กดาเมจจากระยะห่างรอบตัวแทนทิศทางฟันจริง<br>3) วงเตือนเดิมผูกกับ `SceneTreeTween` อิสระ จึงไม่หยุดตามคัตอินและไม่ถูกลบเมื่อผู้ร่ายตาย |
| **วิธีแก้** | 1) สร้างสคริปต์ [build_living_monsters.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/build_living_monsters.py) ผลิตสไปรต์ชีต [shadow_cultist_sheet.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/monsters/shadow_cultist_sheet.png) (`8 เฟรม`: `Idle, MoveA, MoveB, Windup, Attack, Recovery, Hurt, Death`) และ [boss_ch1_sheet.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/monsters/boss_ch1_sheet.png) (`10 เฟรม` รวมท่าง้างทุบ ปล่อยคลื่น พักหอบเปิดช่องว่าง เจ็บ และล้มตาย) พร้อมลบรูขาวใน `shadow_cultist` และ `shadow_hound`<br>2) สร้างระบบ **Anti-Stacking Compass Slots** และระยะยืนประชิดขั้นต่ำ พร้อมกรวยเตือนทิศฟัน `SlashCone` ที่เช็กดาเมจตรงตามมุมฟันและจังหวะปล่อยดาบจริง<br>3) อัปเกรด **Furnace Guardian** ให้มี **3 รูปแบบโจมตีชัดเจน** (ทุบหลุมหลอมเหลว, พ่นคลื่นอัคคี 3➔5 แฉกพร้อมเส้นทางเตือนบนพื้น, เสาเพลิงผุดหลายจุดตามด้วยช่วงพักร้อนจัด `⚡ ช่องว่าง!` นาน `1.55s` รับดาเมจแรงขึ้น `+25%`), เปลี่ยนเฟส 2 ที่ `HP <= 50%` อย่างเห็นได้ชัด และย้ายระบบวงเตือนทั้งหมดเข้า `_process_telegraphs(delta)` ที่หยุดเวลาตามคัตอิน/พอร์ซ และลบวงเตือนทิ้งทันทีเมื่อศัตรูตาย |
| **ไฟล์ที่แก้ไข** | [build_living_monsters.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/build_living_monsters.py), [shadow_cultist_sheet.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/monsters/shadow_cultist_sheet.png), [boss_ch1_sheet.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/monsters/boss_ch1_sheet.png), [shadow_hound.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/monsters/shadow_hound.png), [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) |

---

## 3. งาน C — ลำดับชั้น VFX, ความรู้สึกตอนฟันโดน, ตัวเลขไม่บังจอ, ตัวเลือกภาพ และกันปุ่มลั่นหลังชั้น 100

| หัวข้อ | รายละเอียด |
|---|---|
| **ปัญหา** | เอฟเฟกต์วงเวทย์และตัวเลขดาเมจซ้อนทับบังตัวละครกับวงเตือนบอส โหมดออโต้หลุดเมื่อเปลี่ยนบท และหลังชนะบอสชั้น 100 มีโอกาสกดสกิล/หลบค้างจนข้ามหน้าสรุปผลทันที |
| **สาเหตุจริง** | 1) งานแก้ค้างก่อนหน้าคูณ `0.48` เท่ากันทุกโหนดใน `vfx_studio.gd` ทำให้คมดาบจางลงแต่วงเวทย์พื้นยังค้างนาน `1.2s` และวาดทับวงเตือน<br>2) `_spawn_floating_text` สุ่มพิกัดแคบ ๆ เหนือหัวจุดเดียวโดยไม่จำกัดจำนวนป้าย<br>3) `auto_attack_enabled` ไม่ได้บันทึกลง `save_data`<br>4) `_trigger_victory()` สั่ง `restart_btn.grab_focus()` ทำให้การกด `Space` (ปุ่มหลบ) ไปกดปุ่มเริ่มใหม่ทันทีโดยไม่มีช่วงหน่วงกันปุ่มลั่น |
| **วิธีแก้** | 1) จัดลำดับชั้น `_mount_role()` ใน [vfx_studio.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/vfx_studio.gd) แยกบทบาท `floor` (`alpha 0.34, z_index=1` ต่ำกว่าวงเตือนบอส `z_index=6`), `slash` (`0.74`), `impact` (`0.88`), `ult_fx` (`0.64`, ลดเวลาค้างเหลือ `0.48–0.65s`) พร้อมเพดานจำกัดจำนวน VFX บนเครื่อง Mac M1<br>2) แยก Hit Impact 3 ระดับ (`เบา / คริติคอล / ท่าหนัก-สวนช่องว่าง`) พร้อมเสียงสังเคราะห์ใหม่ `"heavy_hit"` ใน [sfx_synth.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/sfx_synth.gd)<br>3) จัดป้ายตัวเลขลอยแบบพัดสลับฟันปลา 5 ช่อง จำกัดสูงสุด 10 ป้าย<br>4) เพิ่มปุ่มปรับ **เอฟเฟกต์ (`ปกติ / ลดลง`)** และ **สั่นจอ (`ปกติ / เบา / ปิด`)** ในหอคัมภีร์ บันทึกทั้ง `fx_reduced`, `shake_mode`, และ `auto_attack` ลง [save_manager.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/save_manager.gd)<br>5) เพิ่ม `end_input_guard_timer = 0.85s` หลังจบฉากและตัด `grab_focus()` เพื่อกันปุ่มสกิล/หลบเผลอข้ามหน้าสรุปผลชั้น 100 |
| **ไฟล์ที่แก้ไข** | [vfx_studio.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/vfx_studio.gd), [sfx_synth.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/sfx_synth.gd), [cutin_overlay.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/cutin_overlay.gd), [save_manager.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/save_manager.gd), [codex_ui.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/codex_ui.gd), [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) |

---

## 4. งาน D — ยกระดับฉากต้นแบบ `"ป่าไผ่หมอกพิษ"` และโถงบอส `Furnace Guardian`

| หัวข้อ | รายละเอียด |
|---|---|
| **ปัญหา** | พื้นกับขอบห้องกลืนกัน กอไผ่มีรูพื้นหลังสีขาวและจุดหมุน Y-sort อยู่กลางลำต้นจนตัวละครเดินทับยอดไผ่ เสาในห้องบอสบังสนาม และป้าย `"ทางออก →"` ค้างอยู่ในห้องเก่าเมื่อเดินเข้าห้องถัดไป |
| **สาเหตุจริง** | 1) `prop_bamboo_clump.png`, `prop_peach_tree.png`, และ `prop_brazier.png` มีพิกเซลพื้นหลังสีขาวตกค้างในช่องปิดระหว่างใบ/เปลวไฟ<br>2) `prop_bamboo_clump` ตั้ง `offset = Vector2(0, -88)` ทั้งที่ภาพสูง `220 px` และวางไว้ที่ `y = 7.1` ทำให้ล้ำเข้ามาในเลนเดิน<br>3) `_check_room_progression()` สร้าง `Label` `"ทางออก →"` ด้วย `add_child()` โดยไม่เก็บตัวแปรอ้างอิงไว้ลบตอนเปลี่ยนห้อง |
| **วิธีแก้** | 1) สร้างสคริปต์ [clean_env_props.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/clean_env_props.py) ลบรูพื้นหลังสีขาวในสไปรต์ฉากทั้ง 3 ชิ้นจนคมชัด<br>2) เพิ่มเส้นขอบหินหยกแบ่งเขตพื้นเดิน แผ่นหินนำสายตา ซุ้มเสามังกรทางออก โอสถหยกเรืองแสงหลายชั้น เงาไผ่ฉากหน้า-ภูเขา 2 ระนาบฉากหลัง ใบไผ่ปลิว หมอกพิษลอย และสะเก็ดไฟลอยขึ้นในโถงเตาหลอมบอส (พร้อมย้ายเสามังกรทั้งหมดไว้ผนังบน ไม่บังลานสู้ 100%)<br>3) เก็บป้าย `"⛩️ ทางออก →"` ไว้ใน `active_exit_hint` และสั่ง `_clear_exit_hint()` ทันทีเมื่อข้ามห้องหรือวาร์ปห้อง<br>4) ปรับ `cam_look_ahead` และ `Camera2D` smoothing ให้นุ่มนวล มั่นคงขณะอ่านวงเตือน |
| **ไฟล์ที่แก้ไข** | [clean_env_props.py](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/clean_env_props.py), [prop_bamboo_clump.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/env/prop_bamboo_clump.png), [prop_peach_tree.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/env/prop_peach_tree.png), [prop_brazier.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/assets/env/prop_brazier.png), [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd) |

---

## 5. งาน E & งานซ่อม QA — ยกระดับ UI อุปกรณ์, กฎสวมใส่, สมดุล และชุดทดสอบ

| หัวข้อ | รายละเอียด |
|---|---|
| **ปัญหา** | หน้าอุปกรณ์เดิมไม่แสดงการ์ดช่องสวมใส่ครบ 3 ช่อง ไม่แสดงเงื่อนไขเซ็ตก่อนใส่ครบ ไม่แยกปุ่มดูรายละเอียดออกจากปุ่มสวมใส่ อุปกรณ์ 1 ชิ้นถูกสวมซ้ำหลายตัวละคร โบนัส CRIT/CDR ตกหล่นในระบบสู้จริง อวี่หานเก่งโดดเมื่อใส่เซ็ตเทพ +5 และชุดทดสอบ QA เดิมตก 2 ชุด |
| **สาเหตุจริง** | ดูรายละเอียดเชิงลึกและตารางตัวเลขใน [BALANCE_REPORT.md](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/BALANCE_REPORT.md) และ [TEST_REPORT.md](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/TEST_REPORT.md) |
| **วิธีแก้** | 1) ยกระดับหน้าคลังศาสตราวุธ (`Tab 3`) และหน้าสถานะตัวละคร (`Tab 5`) ให้มีแถบเลือกตัวละครเป้าหมายชัดเจน การ์ดช่องสวมใส่ครบ 3 ช่อง (`อาวุธ / เกราะ / เครื่องราง`) กล่องเงื่อนไขโบนัสเซ็ต (`0/3..3/3`) แผง `🔍 รายละเอียด & เทียบค่าพลัง ก่อน➔หลัง` ตัวอย่างต้นทุนและผลอัปเกรดขั้นถัดไป และปุ่มแยกชัด (`ดูรายละเอียด / อัปเกรด / สวมใส่ / ถอดออก / ย้ายมาสวม`)<br>2) บังคับกฎ **1 ชิ้นต่อ 1 ตัวละคร**, รักษาสัดส่วน `HP/SP` เมื่อค่าสูงสุดเปลี่ยน, ผูกโบนัส `crit` และ `cd_red` เข้ากับการโจมตีและสกิลจริง และปรับสมดุลเซ็ตท้ายเกม + อวี่หานตามข้อมูลวัดผล<br>3) แก้เลย์เอาต์ทัชมือซ้ายใน [mobile_controls_landscape.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/mobile_controls_landscape.gd) และปรับปรุงชุดทดสอบจนผ่านครบ `12/12 PASS` |
| **ไฟล์ที่แก้ไข** | [equip_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/equip_data.gd), [hero_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/hero_data.gd), [codex_ui.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/codex_ui.gd), [character_status_ui.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/character_status_ui.gd), [mobile_controls_landscape.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/mobile_controls_landscape.gd), [dark_jade_dungeon.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/dark_jade_dungeon.gd), [test_ipad_touch_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_ipad_touch_e2e.gd), [test_dark_jade_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_dark_jade_e2e.gd), [test_7heroes_costumes_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_7heroes_costumes_e2e.gd), [test_quality_mission_e2e.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/tests/test_quality_mission_e2e.gd), [run_qa.sh](file:///Users/jake/Downloads/rocg/maha-dan-morta/tools/run_qa.sh) |
