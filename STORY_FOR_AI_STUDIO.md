# เรื่องราวทั้งหมดของ "มหาแดนพันภพ" → เตรียมทำเวอร์ชัน Google AI Studio (Apps)

> รวบรวมจาก `TECHNICAL_HANDOFF_REPORT.md`, ประวัติแชททั้งหมด (23–24 พ.ย. 2026) และโค้ดในโฟลเดอร์ `rocg/`
> ใช้เป็น "บรีฟ" วางใน AI Studio Build ได้เลย (ดูหัวข้อ 7)

## 1. เราสร้างอะไรไว้

เกม Action-RPG / Dungeon Crawler สไตล์เซียนจีน **"มหาแดนพันภพ: ศึกผนึกกระบี่สะบั้นสวรรค์"** (ธีม Maha Dan Panpaph) สร้างด้วย **Godot 4 (GDScript)** เกือบทั้งหมดสร้างฉาก/UI ด้วยโค้ด (`dark_jade_dungeon.gd`) ส่งออกเป็น **WebGL2 + PWA**

| โปรเจกต์ | บทบาท | Viewport | สถานะ QA |
|---|---|---|---|
| `maha-dan-morta` | Desktop / iPad แนวนอน | 1280×720 | 7/9 PASS (พังอยู่ 2 เทส) |
| `maha-dan-morta-mobile` ("มหาแดน gogo M") | มือถือแนวตั้ง | 540×880 | 9/9 PASS |

โปรเจกต์อื่นในโฟลเดอร์ (`japan-rpg`, `yulgang-rpg`, `online-prototype`, `immortals-inspired-demo`, `2d`, `concepts`) เป็นงานทดลอง/ต้นแบบก่อนหน้า

## 2. ไทม์ไลน์

| วันที่ | เหตุการณ์ |
|---|---|
| 2 พ.ย. | วาง Canon: เอกสาร AI Handoff v1.2, Character Bible (ไป๋เยว่หลิง, เฉินซี, เฮ่ยซานเหล่ากุ่ย) |
| 4–6 พ.ย. | Concept art, ชุดรายงานตรวจรับ (Gate G1, AC Coverage, Canon Compliance, Asset Verification), `audit_dashboard.html` |
| 6 พ.ย. | ชีตสไปรต์ตัวละคร ไป๋เยว่หลิง / ซ่งเหริน / เยี่ยซางเยี่ย |
| 17–21 พ.ย. | ต้นแบบ RPG หลายแบบ → สรุปมาที่ Godot `maha-dan-morta` และแยก spin-off มือถือ |
| 23 พ.ย. | น้องฟ้าทดสอบเล่น 3 รอบ → พบการเดินแข็ง, ปุ่ม `[C]` ชน, กล้อง/ขอบดำ → แก้ P0–P2 ได้ความพึงพอใจ 93% → ทดสอบ iPad (HTTPS tunnel, เลย์เอาต์ Two-Thumb) |
| 23 พ.ย. ค่ำ | ฉากที่ 2 ขึ้นไปตัวละครไม่เดิน → แก้ |
| 24 พ.ย. เช้า | ทำ Mission Quality Improvement งาน A–E (ดูข้อ 3), เทส 12/12 PASS, เขียน `BALANCE_REPORT/TEST_REPORT/CHANGELOG/VISUAL_REVIEW` |
| 24 พ.ย. 10:02 | เขียน `TECHNICAL_HANDOFF_REPORT.md` |

## 3. สิ่งที่ทำลงไป (ตามหมวด)

**ตัวละคร & ข้อมูล**
- 7 ฮีโร่ (`bai_yueling, ye_shangye, song_ren, chen_xi, yu_han, zhao_yan, hei_shan`) ชุดละ 2 คอสตูม, สกิล Q/E/R
- 10 บท × 10 ห้อง = 100 ชั้น, ม็อบ/มินิบอส/บอสต่อบท, ฮาซาร์ด (fire_pillar, poison_mist, blood_swords)
- อุปกรณ์ 15 ชิ้น (+1..+5), Set Bonus 4 ภพ (มนุษย์/เซียน/มาร/เทพ), 12 Boons, Achievements, Tutorial

**ระบบต่อสู้**
- Perfect Dodge (หน้าต่าง 0.22s, สโลว์ 0.35x, +30 SP, คริ ×1.8)
- Combo Rank D→SSS (โบนัสดาเมจสูงสุด +35%)
- 4 ธาตุ + 6 ปฏิกิริยา (melt, overload, superconduct, burning ฯลฯ)
- Ultimate Cut-in ครบ 7/7 ฮีโร่ (สไปรต์ชีต 6×5 เฟรม + เสียง) 3 โหมด full/card/off
- VFX 4 เลเยอร์แบบ procedural, SFX สังเคราะห์ PCM 18 เสียง + BGM

**แก้บั๊ก/คุณภาพ (Root-Cause)**
- ท่าเดินไป๋เยว่หลิง: สร้างชีตใหม่ 8 ทิศ ยกเลิก shear, แก้ขาสั้น 8px, ก้าวผูกระยะจริง 28px/จังหวะ, Squash&Stretch, ฝุ่นใต้เท้า
- ศัตรู `อสูรเงาคลั่ง` (8 เฟรม) และบอส `Furnace Guardian` (10 เฟรม) แยกสถานะ Idle→Move→Windup→Attack→Recovery→Hurt→Death
- กล้อง Look-ahead + Drag margin, ขอบดำหาย
- แก้ปุ่มชน: `[C]` = ออโต้, `[K]` = สถานะ, `Esc` ปิดหน้าต่างได้
- ศัตรูรุมซ้อน/อ่านยาก → ปรับสมดุลและความชัดเจน (BALANCE_REPORT)

**มือถือ/iPad**
- ปัญหา: iOS ต้อง HTTPS (SharedArrayBuffer) → ใช้ Cloudflare Tunnel (`play_ipad_wifi.sh` พอร์ต 8089, `play_iphone_wifi.sh` พอร์ต 8088)
- แก้ hardcode สเกล 1280px, ปลดตัวกรอง touch, เลย์เอาต์ Two-Thumb (จอยซ้าย / ปุ่มสกิลขวา), โหมดมือซ้าย
- เซฟข้ามเครื่องด้วยรหัส `MDM1:` (Base64)

**QA / Pipeline**
- `tools/run_qa.sh` (headless Godot), `tools/export_web.sh`, ชุดสคริปต์ Python สร้างสไปรต์/คัตอิน/ฉาก

## 4. ปัญหาค้าง
1. `test_ipad_touch_e2e` FAIL: ปุ่ม Q มือซ้ายอยู่นอก thumb zone
2. `test_dark_jade_e2e` FAIL: Ch1 Room 0 ไม่ spawn ศัตรู 3 ตัว (น่าจะเพราะ Pre-battle Lobby)
3. `dark_jade_dungeon.gd` ไฟล์ก้อนเดียว 4,647 บรรทัด (ควรแยกโมดูล)
4. ObjectDB leak 8–10 instance ตอนปิดเทส
5. ต้อง re-export web build หลังแก้

## 5. ข้อควรรู้ก่อนย้ายไป AI Studio

AI Studio Apps (Build) สร้างเว็บแอป **React/TypeScript** (ไฟล์ขนาดเล็ก, เรียก Gemini API ได้) → **วาง Godot export (~84–99 MB, ต้อง COOP/COEP) ลงไปตรงๆ ไม่เหมาะ** ทางเลือก:

| ทางเลือก | ข้อดี | ข้อเสีย |
|---|---|---|
| **A. เขียนใหม่เป็น TS + Canvas/Phaser (แนะนำ)** | เบา, รันได้ทุกที่, ต่อ Gemini ได้ง่าย | ต้องรีเมค logic; ใช้ข้อมูลใน Data Layer ย้ายง่าย |
| B. Godot export แบบ single-thread แล้วโฮสต์ภายนอก (Cloudflare Pages) ใส่ iframe | ได้เกมเดิม | ใหญ่, ขึ้นกับโฮสต์ภายนอก |
| C. AI Studio ทำเฉพาะ "ส่วนเสริม AI" (NPC สนทนา, สร้างเควส/เนื้อเรื่อง, Character Bible Q&A) | ใช้จุดแข็ง Gemini | ไม่ใช่ตัวเกมเต็ม |

ของที่ย้ายได้ง่าย: ตารางข้อมูล `hero_data / chapter_data / equip_data / monster_data` (แปลงเป็น JSON/TS), สูตร Combat, สไปรต์ PNG, เสียง/เพลง
ข้อจำกัด: asset หนักมาก (heroes 28–38 MB) ต้องบีบ/ลดจำนวนฮีโร่ในเวอร์ชันแรก

## 6. แผนทำเวอร์ชัน AI Studio (MVP)
1. ฮีโร่ 3 ตัวเริ่มต้น (ไป๋เยว่หลิง, เยี่ยซางเยี่ย, ซ่งเหริน) + บท 1 (10 ห้อง) + บอส 1
2. ระบบ: เดิน/โจมตี/สกิล Q E R/หลบสมบูรณ์/คอมโบ Rank/ธาตุ
3. Touch Two-Thumb แนวตั้ง (อิง `mobile_controls.gd`)
4. ฟีเจอร์ Gemini: NPC บทสนทนา, เล่าเรื่องบอส, ปรับความยากตามสไตล์ผู้เล่น
5. เซฟ localStorage + รหัส `MDM1:` ที่เข้ากันได้กับเกม Godot

## 7. พรอมต์เริ่มต้นสำหรับ AI Studio Build (คัดลอกไปวางได้)

```text
สร้างเว็บเกม Action-RPG แนวตั้งสำหรับมือถือ ชื่อ "มหาแดนพันภพ" ด้วย React + TypeScript + HTML Canvas
ธีมเซียนจีน/เซียนน้ำแข็ง ภาษาไทย
- ฮีโร่เริ่มต้น 3 ตัว: ไป๋เยว่หลิง (ดาบ/น้ำแข็ง), เยี่ยซางเยี่ย (จอมมาร), ซ่งเหริน (นักปรุงยันต์)
- ควบคุม: จอยสติ๊กซ้าย, ปุ่มตี/Q/E/R/หลบ ขวา (มือซ้ายสลับได้)
- ต่อสู้: Perfect Dodge (0.22s แล้วสโลว์โมชัน +30 SP), Combo Rank D→SSS, ธาตุ 4 + ปฏิกิริยา 6
- ด่าน: 10 ห้องต่อบท, ม็อบ 2 ชนิด, มินิบอส, บอสท้ายบท
- เก็บ Spirit Jade อัปเกรด HP/SP/ATK, ของสวมใส่ 3 ช่อง
- ใช้ Gemini API สร้างบทสนทนา NPC และเล่าเรื่องก่อนบอส
- เซฟ localStorage และ export/import รหัสเซฟ
```

## 8. ไฟล์อ้างอิง
- [TECHNICAL_HANDOFF_REPORT.md](file:///Users/jake/Downloads/rocg/TECHNICAL_HANDOFF_REPORT.md) — สถาปัตยกรรมเต็ม
- [Maha_Dan_Panpaph_AI_Handoff_v1.2.md](file:///Users/jake/Downloads/rocg/Maha_Dan_Panpaph_AI_Handoff_v1.2.md) — Canon/โลกเกม
- [hero_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/hero_data.gd), [chapter_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/chapter_data.gd), [equip_data.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/equip_data.gd), [combat_system.gd](file:///Users/jake/Downloads/rocg/maha-dan-morta/scripts/combat_system.gd) — ข้อมูลที่ย้ายไป TS
