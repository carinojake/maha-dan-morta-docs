# คู่มือตรวจรับคุณภาพภาพและแอนิเมชัน (VISUAL_REVIEW)

เอกสารฉบับนี้รวบรวมภาพหน้าจอก่อน–หลังแก้ คลิปวิดีโอเปรียบเทียบ และจุดสังเกตสำหรับเจ้าของโครงการในการตรวจรับงาน **A–E**

---

## 1. สารบัญคลิปวิดีโอสาธิต (`docs/quality_improvement/clips/`)

| คลิปวิดีโอ | สิ่งที่ต้องสังเกตในการตรวจรับ |
|---|---|
| [walk_bai_yueling_before.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/walk_bai_yueling_before.mp4) | **ก่อนแก้:** ไป๋เยว่หลิงเดินขวาปลายเท้าหลังเขย่งและตัวสั้นลงในจังหวะสลับเฟรม เดินลงซ้ำขาเดิม และชุดคลุมหิมะตอนหยุดยืนค้างท่าก้าวขา |
| [walk_bai_yueling_after.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/walk_bai_yueling_after.mp4) | **หลังแก้:** เดินครบ 4 ทิศหลัก + แนวทแยงทั้งชุดปกติและชุดคลุมหิมะ ฝ่าเท้าติดพื้นสมจริง ความเร็วเดินสัมพันธ์จังหวะก้าว และหยุดปุ๊บยืนตรงทันที |
| [walk_bai_yueling_before_vs_after.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/walk_bai_yueling_before_vs_after.mp4) | **เทียบซ้าย–ขวาเฟรมต่อเฟรม** ด้วยฉาก มุมกล้อง และความเร็วเดียวกัน |
| [enemy_cultist_states.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/enemy_cultist_states.mp4) | **งาน B (ศัตรูพื้นฐาน):** `อสูรเงาคลั่ง` แสดงครบสถานะ `Idle ➔ Move ➔ Windup (ง้างดาบ+กรวยแดง) ➔ Attack ➔ Recovery ➔ Hurt ➔ Death` และการกระจายตำแหน่งไม่กองซ้อนทับตัวละคร |
| [boss_furnace_guardian_phases.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/boss_furnace_guardian_phases.mp4) | **งาน B (บอส Furnace Guardian):** 3 ท่าโจมตี (ทุบหลุมหลอมเหลว, พ่นคลื่นไฟพร้อมเส้นเตือน, เสาเพลิงผุดหลายจุด), การเปลี่ยนร่าง **เฟส 2 (`HP <= 50%`)**, ช่วงพักหอบ **`⚡ ช่องว่าง!`** เปิดให้สวนกลับ และวงเตือนหายทันทีเมื่อบอสตาย |
| [vfx_and_prototype_arena.mp4](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/clips/vfx_and_prototype_arena.mp4) | **งาน C & D:** เอฟเฟกต์ฟันคมชัดโดยวงเวทย์พื้นไม่บังวงเตือนบอส ตัวเลขดาเมจกระจายไม่ทับกัน ซุ้มประตูทางออก + ป้าย `"⛩️ ทางออก →"` ที่หายทันทีเมื่อเข้าห้องใหม่ และโหมดลดเอฟเฟกต์ |

---

## 2. เปรียบเทียบภาพฉากต้นแบบก่อน–หลังแก้ (`docs/quality_improvement/screenshots/`)

### 2.1 ห้องต้นแบบ `"ป่าไผ่หมอกพิษ"` (ก่อนแก้ vs หลังแก้)
- **ก่อนแก้:** [room0_before.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room0_before.png)
  - กอไผ่มีรูสามเหลี่ยมสีขาวค้างระหว่างใบ จุดหมุน Y-sort อยู่กลางกอทำให้ตัวละครเดินทับยอดไผ่ พื้นกับขอบกำแพงกลืนกัน และทางออกขวาเป็นเพียงเส้นบาง ๆ
- **หลังแก้ (ขณะต่อสู้):** [room0_combat_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room0_combat_after.png)
  - กอไผ่คมชัดไร้รูสีขาว แยกเลเยอร์หลัง (ภูเขา 2 ระนาบ + หมอกหุบเขา) กลาง (แผ่นหินนำสายตา ขอบหินหยก โคมไฟอุ่นตัดหมอกพิษเย็น) และหน้า (เงาไผ่ขอบเหวล่าง + ใบไผ่ปลิวเบา ๆ) โดยพร็อพทั้งหมดอยู่นอกเลนต่อสู้ ไม่บังตัวละครหรือวงเตือน
- **หลังแก้ (เมื่อเคลียร์ห้อง & ทางออก):** [room0_cleared_exit_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room0_cleared_exit_after.png)
  - โอสถหยกมีวงแสงออร่าลอยขึ้นลง ซุ้มเสามังกรทางออกเปิดแสงสีเขียวมรกต พร้อมป้าย `"⛩️ ทางออก →"` ชัดเจน
- **หลังก้าวเข้าห้องถัดไป (ตรวจการลบป้ายค้าง & Anti-Stacking):** [room1_anti_stack_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room1_anti_stack_after.png)
  - ป้าย `"⛩️ ทางออก →"` ของห้องเก่าถูกลบทิ้งสะอาด ศัตรูทั้ง 3 ตัว (`อสูรเงาคลั่ง`, `หมาป่าเงาอสูร` ที่ลบรูขาวใต้ขาแล้ว, `หุ่นศิลาหยก`) ยืนล้อมคนละมุมอ่านท่าง้างโจมตีได้ชัดเจน

### 2.2 ทางเลือกมุมกล้องซูมใกล้ขึ้นเล็กน้อย (สำหรับเจ้าของโครงการพิจารณา)
- **มุมมาตรฐานปัจจุบัน (`Zoom 1.18×`):** [room0_combat_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room0_combat_after.png) — เห็นขอบเขตสนามทั้งห้องและซุ้มประตูทางออกชัดเจน เหมาะกับการหลบกระสุนและอ่านวงเตือนบอส
- **มุมซูมเน้นรายละเอียดตัวละคร (`Zoom 1.26×`):** [room0_camera_126x.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/room0_camera_126x.png) — เห็นลวดลายชุดและท่าฟันใหญ่ขึ้น ~7% โดยยังคงเป็น 2D Top-down/2.5D เดิม

### 2.3 โถงบอสใหญ่ `Furnace Guardian` (ห้องที่ 10)
- **ภาพตรวจรับ:** [boss_furnace_arena_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/boss_furnace_arena_after.png)
  - พื้นหินชนวนเผาไฟตัดขอบทองแดง ค่ายกลยันต์เตาหลอมกลางห้อง กระถางเพลิง 4 มุม เสามังกรเรียงเฉพาะผนังด้านบนไม่บังสนามสู้ สะเก็ดไฟลอยขึ้น และวงเตือนพายุ 8 ทิศมีป้าย `"🟢 เขตปลอดภัย!"` อ่านเข้าใจในเสี้ยววินาที

---

## 3. ภาพตรวจรับ UI อุปกรณ์หลังยกระดับ (งาน E)

- **คลังศาสตราวุธ (`Tab 3`):** [equip_tab3_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/equip_tab3_after.png)
  - แสดงชัดว่ากำลังจัดอุปกรณ์ให้ตัวละครใด สลับตัวละครได้ทันที แสดงการ์ดช่องสวมใส่ครบ 3 ช่อง (`ช่องอาวุธ / ช่องเกราะ / ช่องเครื่องราง`) พร้อมปุ่ม `ดู` และ `ถอดออก`, แถบเงื่อนไขโบนัสเซ็ต (`X/3 ชิ้น`), กรอบ `🔍 รายละเอียด & เทียบค่าพลัง ก่อน➔หลัง` พร้อมตัวอย่างต้นทุนและผลอัปเกรดขั้นถัดไป และปุ่มแยกชัดไม่ล้นกรอบ `1280x720`
- **หน้าสถานะตัวละคร (`Tab 5`):** [equip_tab5_after.png](file:///Users/jake/Downloads/rocg/maha-dan-morta/docs/quality_improvement/screenshots/equip_tab5_after.png)
  - แสดงค่าพลังครบ (`HP, ปราณ, ATK, CRIT, ลดดาเมจ, SPD`) พร้อมตัวเลขเปรียบเทียบสีเขียว `▲ +` / สีแดง `▼ -` แบบเรียลไทม์ ปุ่ม `ดู` แยกต่างหาก และป้าย `(ย้าย: อวี่หาน)` เมื่อไอเทมสวมอยู่ที่ตัวละครอื่นตามกฎ 1 ชิ้นต่อ 1 ตัวละคร
