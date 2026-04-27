# hotel-revenue-stagnation-analysis

โครงการวิเคราะห์รายได้ห้องพักและปัจจัยที่มีผลต่อ Revenue Per Available Room (RevPAR) เพื่อสนับสนุนการวางแผนเชิงกลยุทธ์ด้านราคาและการบริหาร inventory ของโรงแรม "The Azure Stay" ด้วยวิธีการทาง Data Analytics

---

## 1. บทนำและความเป็นมา (Introduction & Background)

ธุรกิจโรงแรมมีการแข่งขันสูงและต้องพึ่งพาข้อมูลเชิงปริมาณในการตัดสินใจด้านราคาและการบริหารห้องพัก โรงแรม "The Azure Stay" เป็นโรงแรมอิสระขนาดกลางที่มีอัตราการเข้าพัก (Occupancy) อยู่ในระดับที่ยอมรับได้ แต่กลับพบว่า Revenue Per Available Room (RevPAR) ต่ำกว่าคู่แข่งในตลาดเดียวกัน สะท้อนถึงปัญหาเชิงโครงสร้างด้านการตั้งราคาและการบริหาร inventory ที่ยังขาดประสิทธิภาพ ส่งผลให้รายได้รวมของโรงแรมอยู่ในภาวะชะงักงัน (Revenue Stagnation)

จากการวิเคราะห์บริบทเบื้องต้น พบว่า pain point หลักของโรงแรมมีสามประเด็น ได้แก่ (1) การกระจายราคาที่ไม่สอดคล้องกับระดับความต้องการของตลาดในแต่ละฤดูกาล (Seasonal Rate Misalignment) (2) การอนุญาตให้ลูกค้าใช้ rate code ที่มีส่วนลดในช่วง High Season แทนที่จะบังคับขายในราคา Rack Rate ส่งผลให้ ADR เฉลี่ยต่ำกว่าศักยภาพที่แท้จริง (Rate Code Dilution) และ (3) พฤติกรรมการเข้าพักระยะสั้นของกลุ่ม Leisure ที่ก่อให้เกิด "คืนฟันหลอ" (Orphan Nights) ซึ่งกดดัน Occupancy โดยรวมให้ต่ำกว่าที่ควรจะเป็น

โครงการนี้จึงมุ่งใช้กระบวนการ Data Analytics วิเคราะห์ปัจจัยเหล่านี้อย่างเป็นระบบผ่านชุดข้อมูลจำลองจาก Property Management System (PMS) และนำผลการวิเคราะห์ไปสนับสนุนการวางแผนเชิงกลยุทธ์ด้านราคาและการเพิ่มประสิทธิภาพรายได้ของโรงแรมในระยะยาว

---

## 2. วัตถุประสงค์ของโครงการ (Research Objectives)

วัตถุประสงค์ของโครงการออกแบบให้สอดคล้องกับหลัก SMART (Specific, Measurable, Achievable, Relevant, Time-bound) เพื่อให้ผลลัพธ์ที่ได้สามารถวัดและประเมินผลได้อย่างเป็นรูปธรรม

1. **วิเคราะห์ปัจจัยที่มีผลต่อ RevPAR** โดยระบุตัวแปรที่มีนัยสำคัญจากชุดข้อมูล 10,000 รายการ เพื่อทำความเข้าใจว่าปัจจัยใดส่งผลต่อรายได้ต่อห้องพักที่มีอยู่มากที่สุด
2. **เปรียบเทียบ ADR และ Occupancy** แยกตาม Booking Channel, Rate Code, Room Type และ Customer Segment เพื่อระบุ segment ที่สร้างรายได้สูงสุดและต่ำสุดในแต่ละมิติ
3. **ทดสอบสมมติฐาน 4 ข้อ** เกี่ยวกับพฤติกรรมการจองและโครงสร้างราคาที่ส่งผลต่อ RevPAR โดยใช้การวิเคราะห์เชิงสถิติและ data visualization
4. **สร้างกรอบการวิเคราะห์ (Analytical Framework)** สำหรับสนับสนุนการตัดสินใจด้าน strategic pricing ที่สามารถนำไปปรับใช้ได้ในบริบทการบริหารโรงแรมจริง
5. **นำเสนอข้อเสนอแนะเชิงนโยบาย (Policy Recommendations)** ที่เชื่อมโยงกับ pain point ทั้งสามประเด็นโดยตรง เพื่อเพิ่ม Net RevPAR ของโรงแรม "The Azure Stay"

---

## 3. คำถามการวิจัยและสมมติฐาน (Research Questions & Hypotheses)

### Research Questions

- ปัจจัยใด (เช่น ช่องทางการจอง, ประเภทราคา, ระยะเวลาเข้าพัก, กลุ่มลูกค้า) มีผลต่อ RevPAR และ total_room_revenue ของโรงแรมมากที่สุด?
- โครงสร้างราคาและเงื่อนไขการจอง (Rate Code & Policy) ในปัจจุบันมีประสิทธิภาพเพียงพอต่อการทำกำไรสูงสุดในช่วง High Season หรือไม่?
- การกระจาย Booking Channel ในปัจจุบันสะท้อนถึงความไม่สมดุลระหว่างต้นทุน commission และรายได้สุทธิที่แท้จริงหรือไม่?
- พฤติกรรมการยกเลิกและ No-Show มีรูปแบบที่สัมพันธ์กับ Booking Lead Time อย่างมีนัยสำคัญหรือไม่?

### สมมติฐาน (Hypotheses)

**H1: OTA Dependency & Net Revenue Leakage**

การจองผ่านช่องทาง OTA (เช่น Expedia, Booking.com) มีสัดส่วนสูงเกินไปในช่วง High Season ทำให้เมื่อหักค่า commission แล้ว Net ADR ของโรงแรมต่ำกว่าการจองผ่านช่องทาง Direct อย่างมีนัยสำคัญ กล่าวคือ แม้ Occupancy จะอยู่ในระดับสูง แต่รายได้สุทธิกลับถูกกัดกินโดยโครงสร้างต้นทุน commission

ตัวแปรที่ใช้พิสูจน์: `channel_id`, `commission_rate`, `total_room_revenue`, `season`

---

**H3: Rate Code Dilution in Peak Demand**

โรงแรมสูญเสียโอกาสในการสร้างรายได้จากการอนุญาตให้ลูกค้าใช้ rate code ที่มีส่วนลด (เช่น AAA Discount, Seasonal Promo) ในปริมาณมากในช่วง High Season แทนที่จะบังคับใช้ราคา Rack Rate ส่งผลให้ ADR เฉลี่ยในช่วงที่ความต้องการสูงสุดต่ำกว่าที่ควรจะเป็น

ตัวแปรที่ใช้พิสูจน์: `season`, `rate_code_id`, ADR, `total_room_revenue`

---

**H4: The "Serial Canceller" Impact on Phantom Inventory**

การจองที่มี Booking Lead Time เกิน 30 วันขึ้นไปมีอัตราการยกเลิก (Cancellation Rate) และ No-Show สูงที่สุด ทำให้เกิดภาวะ "ห้องเต็มหลอก" (Phantom Full House) และโรงแรมเสียโอกาสในการขายห้องให้กับลูกค้าที่พร้อมจ่ายและมีเจตนาเข้าพักจริง

ตัวแปรที่ใช้พิสูจน์: `BLT` (คำนวณจาก `check_in_date − booking_date`), `status` (Confirmed, Cancelled, No-Show)

---

**H5: Short LOS & Orphan Nights**

ลูกค้ากลุ่ม Leisure มี Length of Stay (LOS) เฉลี่ยเพียง 1 คืนในช่วงสุดสัปดาห์ ทำให้เกิด "คืนฟันหลอ" (Orphan Nights) ในวันข้างเคียง (เช่น วันศุกร์หรือวันอาทิตย์ห้องว่าง) ซึ่งกดดัน Occupancy โดยรวมและทำให้ RevPAR ไม่สามารถเติบโตถึงจุดสูงสุดได้

ตัวแปรที่ใช้พิสูจน์: `LOS` (คำนวณจาก `check_out_date − check_in_date`), `segment_id`, `is_weekend`

---

## 4. ชุดข้อมูลและตัวแปรที่ใช้ (Dataset & Features)

### ภาพรวมชุดข้อมูล

| รายการ | รายละเอียด |
| --- | --- |
| จำนวนแถวข้อมูล | 10,000 รายการ |
| จำนวนตัวแปรทั้งหมด | 29 ตัวแปร (รวม 5 ตาราง) |
| แหล่งที่มา | ข้อมูลจำลองจาก Property Management System (PMS) ของโรงแรม "The Azure Stay" |
| ช่วงเวลา | ปี 2025 ครอบคลุม High Season, Low Season และ Shoulder Season |
| รูปแบบโครงสร้าง | Star Schema (1 Fact Table + 4 Dimension Tables) |

---

### Data Dictionary: `fact_bookings`

ตารางหลักของโครงการ บันทึกข้อมูลการจองห้องพักแต่ละรายการ (1 แถว = 1 การจอง, 1 room type)

| Attribute | คำอธิบาย | Data Type | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --- | --- | --- | --- |
| booking_id | รหัสการจอง (Primary Key) | Ordinal | RES-10023 |
| guest_id | รหัสผู้เข้าพัก | Ordinal | G-45821 |
| booking_date | วันที่ทำการจอง | Interval (Date) | 2025-09-15 |
| check_in_date | วันที่เข้าพัก | Interval (Date) | 2025-10-01 |
| check_out_date | วันที่ออกจากที่พัก | Interval (Date) | 2025-10-03 |
| room_type_id | รหัสประเภทห้อง (FK → dim_room_types) | Ordinal | RT_DELUXE |
| rate_code_id | รหัสแผนราคา (FK → dim_rate_codes) | Ordinal | RC_CORP |
| channel_id | รหัสช่องทางการจอง (FK → dim_channels) | Ordinal | CH_EXP |
| segment_id | รหัสกลุ่มลูกค้า (FK → dim_segments) | Ordinal | SEG_BUSINESS |
| status | สถานะการจอง | Nominal | Confirmed, Cancelled, Checked-Out, No-Show |
| total_room_revenue | รายได้จากค่าห้อง (ไม่รวมภาษีและบริการเสริม) | Ratio (Continuous) | 0 – ∞ (เช่น 3,500.00) |
| number_of_rooms | จำนวนห้องที่จองในรายการนี้ | Ratio (Discrete) | 1 – ∞ |
| adults_count | จำนวนผู้ใหญ่ | Ratio (Discrete) | 1 – ∞ |
| children_count | จำนวนเด็ก | Ratio (Discrete) | 0 – ∞ |

---

### Data Dictionary: `dim_room_inventory`

ตารางกำหนดกำลังการผลิต (capacity) ของโรงแรมในแต่ละวัน ใช้คำนวณ RevPAR และ Occupancy

| Attribute | คำอธิบาย | Data Type | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --- | --- | --- | --- |
| date | วันที่ (Primary Key) | Interval (Date) | 2025-10-01 |
| total_capacity | จำนวนห้องทั้งหมดของโรงแรม | Ratio (Discrete) | เช่น 150 |
| rooms_out_of_order | ห้องที่ไม่พร้อมใช้งาน (ซ่อมบำรุง) | Ratio (Discrete) | 0 – total_capacity |
| rooms_available_for_sale | ห้องที่สามารถขายได้จริง | Ratio (Discrete) | total_capacity − rooms_out_of_order |

---

### Data Dictionary: `dim_rate_codes`

ตาราง Lookup สำหรับแผนราคาและกลยุทธ์การตั้งราคาของโรงแรม

| Attribute | คำอธิบาย | Data Type | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --- | --- | --- | --- |
| rate_code_id | รหัสแผนราคา (Primary Key) | Ordinal | RC_CORP |
| rate_name | ชื่อแผนราคา | Nominal | Corporate Flat Rate |
| description | รายละเอียดสิทธิประโยชน์ที่รวมอยู่ในแผนราคา | Nominal (Text) | Includes Breakfast & Wifi |
| is_commissionable | ระบุว่าแผนราคานี้มีการจ่าย commission ให้บุคคลที่สามหรือไม่ | Nominal (Binary) | True / False |

---

### Data Dictionary: `dim_channels`

ตาราง Lookup สำหรับช่องทางการจองและโครงสร้างต้นทุน commission

| Attribute | คำอธิบาย | Data Type | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --- | --- | --- | --- |
| channel_id | รหัสช่องทาง (Primary Key) | Ordinal | CH_EXP |
| channel_name | ชื่อช่องทาง | Nominal | Expedia, Agoda |
| channel_type | ประเภทช่องทาง | Nominal | OTA, Direct, Wholesaler |
| commission_rate | อัตราค่าคอมมิชชั่นที่จ่ายให้ช่องทาง | Ratio (Continuous) | 0 – 1 (เช่น 0.15 = 15%) |

---

### Data Dictionary: `dim_calendar`

ตาราง Date Dimension สำหรับรองรับการวิเคราะห์ตาม Day of Week และฤดูกาล

| Attribute | คำอธิบาย | Data Type | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --- | --- | --- | --- |
| date_key | วันที่ (Primary Key) รูปแบบ YYYY-MM-DD | Interval (Date) | 2025-10-01 |
| day_name | ชื่อวันในสัปดาห์ | Nominal | Monday, Tuesday, …, Sunday |
| is_weekend | ระบุว่าเป็นวันหยุดสุดสัปดาห์หรือไม่ | Nominal (Binary) | True / False |
| is_holiday | ระบุว่าเป็นวันหยุดนักขัตฤกษ์หรือไม่ | Nominal (Binary) | True / False |
| season | ฤดูกาล (กำหนดจากช่วงวันที่ตามเงื่อนไข: High = ต.ค.–ก.พ., Low = พ.ค.–ส.ค., Shoulder = มี.ค.–เม.ย., ก.ย.) | Nominal | High Season, Low Season, Shoulder Season |

---

## 5. ตัววัดและมิติข้อมูล (Measures & Dimensions)

### ตัวแปรเป้าหมาย (Target Variable)

- **`total_room_revenue`** — รายได้รวมจากการขายห้องพักของแต่ละรายการจอง ใช้เป็น base ในการคำนวณ KPI ทั้งหมดของโครงการ

---

### Key Performance Indicators (Derived Measures)

ตัววัดต่อไปนี้ไม่ได้ถูก generate โดยตรงจากข้อมูล แต่ถูก derive ในขั้นตอนการวิเคราะห์โดยใช้สูตรที่ระบุไว้ด้านล่าง

| Measure | คำนิยาม | สูตรการคำนวณ | ช่วงค่า |
| --- | --- | --- | --- |
| **RevPAR** (Revenue Per Available Room) | ตัวชี้วัดความสามารถในการสร้างรายได้จากห้องพักที่มีอยู่ทั้งหมด ไม่ว่าจะขายได้หรือไม่ก็ตาม | `Total Room Revenue ÷ Rooms Available for Sale` | $0 – Max Potential Rate |
| **ADR** (Average Daily Rate) | ราคาเฉลี่ยต่อห้องที่ขายได้จริงในช่วงเวลาที่กำหนด | `Total Room Revenue ÷ Number of Rooms Sold` | $0 – Rack Rate |
| **OCC** (Occupancy Rate) | อัตราส่วนของห้องที่ขายได้ต่อห้องที่มีอยู่ทั้งหมด | `(Rooms Sold ÷ Rooms Available for Sale) × 100` | 0% – 100% |
| **LOS** (Length of Stay) | ระยะเวลาการเข้าพักในหน่วยคืน | `check_out_date − check_in_date` | 1 – 365+ คืน |
| **BLT** (Booking Lead Time) | จำนวนวันระหว่างวันที่จองและวันที่เข้าพัก | `check_in_date − booking_date` | 0 – 365+ วัน |

> **หมายเหตุ:** RevPAR = ADR × OCC สามารถใช้ความสัมพันธ์นี้ verify ความถูกต้องของการคำนวณได้

---

### Key Dimensions (ตัวแปรจำแนกกลุ่ม)

| Dimension | คำนิยาม | ค่าที่เป็นไปได้ |
| --- | --- | --- |
| **Day of Week** | วันที่ห้องถูกเข้าพัก สำคัญสำหรับการวิเคราะห์ weekday vs. weekend | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
| **Booking Channel** | ช่องทางที่ลูกค้าทำการจอง ใช้วิเคราะห์ต้นทุนและรายได้สุทธิต่อช่องทาง | Direct Website, OTA (Expedia, Booking.com), Walk-in, Corporate Agent, GDS |
| **Rate Code** | แผนราคาที่ใช้ในการจอง ใช้วิเคราะห์ Rate Code Dilution | Rack Rate, AAA Discount, Corporate Negotiated, Non-Refundable, Seasonal Promo |
| **Room Type** | ประเภทห้องพัก ใช้วิเคราะห์ ADR variation ตาม product | Standard Queen, Deluxe King, Suite, Ocean View, Accessible |
| **Customer Segment** | กลุ่มลูกค้าแบ่งตามวัตถุประสงค์การเดินทาง | Business, Leisure, Group, Transient, Wholesale |
| **Season** | ฤดูกาลของวันที่เข้าพัก (derived จาก dim_calendar ด้วย conditional logic) | High Season, Low Season, Shoulder Season |

---

### ตัวแปรสำคัญที่ใช้วิเคราะห์ จำแนกตามกลุ่ม (Key Features by Group)

| กลุ่มตัวแปร | ตัวแปร |
| --- | --- |
| กลุ่มวันเวลา (Time Features) | check_in_date, day_name, is_weekend, is_holiday, season |
| กลุ่มการจอง (Booking Characteristics) | LOS, BLT, number_of_rooms, status |
| กลุ่มลูกค้า (Customer Features) | segment_id, adults_count, children_count |
| กลุ่มรายได้และราคา (Pricing & Revenue Drivers) | total_room_revenue, ADR, RevPAR, rate_code_id, is_commissionable |
| ช่องทางการขาย (Channel Features) | channel_id, channel_type, commission_rate |
| ประเภทห้อง (Product Features) | room_type_id |

---

## 6. กระบวนการวิเคราะห์ข้อมูล (Analytical Methodology)

### ภาพรวมกระบวนการ

โครงการนี้ดำเนินการวิเคราะห์ข้อมูลแบบ Exploratory Data Analysis (EDA) ผ่านขั้นตอนหลัก 4 ระยะ ดังนี้

**ระยะที่ 1: Data Preparation & Quality Check**
ตรวจสอบความสมบูรณ์และความถูกต้องของข้อมูล ตรวจหา missing values, outliers และ inconsistent data types ภายใน fact_bookings และ dimension tables ทั้ง 4 ตาราง จากนั้น join ข้อมูลผ่าน foreign key เพื่อเตรียม working dataset สำหรับแต่ละ hypothesis

**ระยะที่ 2: Feature Engineering**
คำนวณตัวแปรที่ derive จากข้อมูลดิบ ได้แก่ ADR (`total_room_revenue ÷ number_of_rooms`), LOS (`check_out_date − check_in_date`), BLT (`check_in_date − booking_date`), Net_ADR (`ADR × (1 − commission_rate)`), Commission_Loss (`ADR − Net_ADR`) และ RevPAR (`Total Room Revenue ÷ Rooms Available for Sale`)

**ระยะที่ 3: Hypothesis Testing & Aggregation**
วิเคราะห์แต่ละ hypothesis โดยใช้ groupby aggregation, pivot table และ comparative analysis แยกตาม dimension ที่กำหนดไว้ในแต่ละสมมติฐาน ระบุ pattern, anomaly และ opportunity cost เชิงตัวเลข

**ระยะที่ 4: Visualization & Reporting**
สร้าง chart visualization ด้วย openpyxl ใน Excel (.xlsx) และสรุปผลเป็น policy recommendation ที่เชื่อมโยงกับ business context ของโรงแรม

---

### เครื่องมือและเทคโนโลยีที่ใช้

| เครื่องมือ | วัตถุประสงค์ |
| --- | --- |
| **Python 3** (pandas, openpyxl) | Data wrangling, EDA, chart generation |
| **Microsoft Excel (.xlsx)** | ผลลัพธ์สุดท้ายในรูป interactive table + chart |
| **Star Schema Model** | โครงสร้างข้อมูลสำหรับ multi-dimensional analysis |

---

## 7. ผลการวิเคราะห์และการตรวจสอบสมมติฐาน (Results & Findings)

---

### H1: OTA Dependency & Net Revenue Leakage — **สมมติฐานได้รับการสนับสนุน**

**ขั้นตอนที่ 1: วิเคราะห์สัดส่วนการจองตามช่องทาง (ทั้งปี)**

จากการจองทั้งหมดที่มีสถานะ Checked-Out จำนวน **7,965 รายการ** พบว่า:

| Channel | จำนวน Booking | สัดส่วน (%) | Commission Rate |
| --- | --- | --- | --- |
| CH_EXP (Expedia) | 2,240 | **28.12%** | 18% |
| CH_DIR (Direct Website) | 2,172 | 27.27% | 0% |
| CH_BDC (Booking.com) | 1,827 | 22.94% | 15% |
| CH_WAL (Walk-in) | 923 | 11.59% | 0% |
| CH_GDS (Corporate Agent) | 803 | 10.08% | 10% |
| **รวม OTA (BDC + EXP)** | **4,067** | **51.06%** | — |
| **รวม Direct (DIR + WAL)** | **3,095** | **38.86%** | — |

> *[แทรกรูปกราฟ: สัดส่วน Booking แต่ละ Channel (Pie/Bar Chart)]*

**ขั้นตอนที่ 2: เปรียบเทียบ Avg ADR vs Avg Net ADR ตาม Channel**

| Channel | Avg ADR | Avg Net ADR | Avg Commission Loss/Booking | % ต่ำกว่า Direct |
| --- | --- | --- | --- | --- |
| CH_DIR | 4,165.10 | 4,165.10 | 0.00 | — (benchmark) |
| CH_WAL | 4,266.75 | 4,266.75 | 0.00 | — |
| CH_GDS | 4,145.41 | 3,730.87 | 414.54 | -10.43% |
| CH_BDC | 4,160.61 | 3,536.52 | 624.09 | -15.09% |
| CH_EXP | 4,153.35 | 3,405.75 | 747.60 | **-18.24%** |

> *[แทรกรูปกราฟ: Avg ADR vs Net ADR by Channel (Clustered Bar)]*

**ขั้นตอนที่ 3: เจาะลึก High Season — ช่วงที่ Demand สูงสุด**

ใน High Season มีการจองทั้งหมด **3,821 รายการ** พบว่า:

| Channel | จำนวน | สัดส่วน (%) | Avg Net ADR | ห่างจาก Direct (%) |
| --- | --- | --- | --- | --- |
| CH_DIR | 1,065 | 27.87% | 4,905.48 | — (benchmark) |
| CH_WAL | 487 | 12.75% | 5,052.54 | +3.00% |
| CH_GDS | 371 | 9.71% | 4,612.87 | -5.97% |
| CH_BDC | 859 | 22.48% | 4,307.32 | **-12.19%** |
| CH_EXP | 1,039 | 27.19% | 4,120.66 | **-16.00%** |
| **OTA รวม** | **1,898** | **49.67%** | — | — |

> *[แทรกรูปกราฟ: Net ADR by Channel — High Season vs Low Season vs Shoulder]*

**สรุป H1:** OTA ครองสัดส่วน 51.06% ของการจองทั้งปี และ 49.67% ใน High Season ทั้งที่เป็นช่วงที่ Direct channel ให้ Net ADR สูงกว่า OTA อยู่ 12–16% ส่งผลให้ Commission Loss สะสมตลอดปี (คำนวณจาก Commission × LOS) อยู่ที่ **8,867,711.52 บาท**

---

### H3: Rate Code Dilution in Peak Demand — **สมมติฐานได้รับการสนับสนุน**

**ขั้นตอนที่ 1: วิเคราะห์สัดส่วน Rate Code ใน High Season**

ใน High Season มีการจองทั้งหมด **3,821 รายการ** พบสัดส่วนการใช้ Rate Code ดังนี้:

| Rate Code | จำนวน Booking | สัดส่วนใน High Season (%) | Avg LOS (คืน) |
| --- | --- | --- | --- |
| Rack Rate | 1,272 | **33.29%** | 2.81 |
| Corporate Flat Rate | 786 | 20.57% | 2.84 |
| Non-Refundable | 779 | 20.39% | 2.70 |
| AAA Discount | 578 | 15.13% | 2.80 |
| Seasonal Promo | 406 | 10.63% | 2.91 |
| **Discount codes รวม** | **2,549** | **66.71%** | — |

> *[แทรกรูปกราฟ: % สัดส่วน Rate Code ใน High Season (Pie/Bar Chart)]*

**ขั้นตอนที่ 2: เปรียบเทียบ Avg ADR ตาม Rate Code × Season**

| Rate Code | High Season | Low Season | Shoulder Season |
| --- | --- | --- | --- |
| Rack Rate | **5,409.32** | 3,400.16 | 4,238.99 |
| Corporate Flat Rate | 5,309.44 | 3,381.97 | 4,164.53 |
| AAA Discount | 4,880.02 | 3,015.31 | 3,893.43 |
| Seasonal Promo | 4,607.44 | 2,784.50 | 3,610.42 |
| Non-Refundable | 4,384.35 | 2,757.15 | 3,187.80 |

> *[แทรกรูปกราฟ: Avg ADR by Rate Code × Season (Clustered Bar)]*

**ขั้นตอนที่ 3: คำนวณ Gap vs Rack Rate และ Revenue Loss ใน High Season**

เทียบ ADR เฉลี่ยของแต่ละ Rate Code กับ Rack Rate (5,409.32 บาท) ในช่วง High Season:

| Rate Code | จำนวน | Avg ADR | ต่ำกว่า Rack (%) | Revenue Loss (บาท) |
| --- | --- | --- | --- | --- |
| Non-Refundable | 779 | 4,384.35 | **-18.95%** | 798,451.63 |
| Seasonal Promo | 406 | 4,607.44 | -14.82% | 325,563.28 |
| AAA Discount | 578 | 4,880.02 | -9.78% | 305,935.40 |
| Corporate Flat Rate | 786 | 5,309.44 | -1.85% | 78,505.68 |
| **รวม** | **2,549** | — | — | **4,238,790.05** |

> *[แทรกรูปกราฟ: Revenue Loss by Rate Code — High Season (Bar Chart)]*

**สรุป H3:** 66.71% ของการจองใน High Season ยังใช้ rate code ที่มีส่วนลด โดย Non-Refundable มี ADR ต่ำกว่า Rack Rate ถึง 18.95% ทั้งที่ควรเป็น premium rate เนื่องจากไม่สามารถยกเลิกได้ รวม Revenue Loss ใน High Season **4,238,790.05 บาท**

---

### H4: The "Serial Canceller" Impact on Phantom Inventory — **สมมติฐานได้รับการสนับสนุน**

**ขั้นตอนที่ 1: แบ่งกลุ่ม Booking ตาม BLT (Booking Lead Time)**

จากการจองทั้งหมด **10,000 รายการ** แบ่งตามจำนวนวันที่จองล่วงหน้าได้ดังนี้:

| BLT Group | จำนวน | สัดส่วน (%) |
| --- | --- | --- |
| Last Minute (0–7 วัน) | 1,181 | 11.81% |
| Short Lead (8–30 วัน) | 4,537 | **45.37%** |
| Long Lead (31–60 วัน) | 4,282 | 42.82% |

> *[แทรกรูปกราฟ: จำนวน Booking แต่ละ BLT Group (Bar Chart)]*

**ขั้นตอนที่ 2: วิเคราะห์ Cancel Rate และ No-Show Rate ตาม BLT Group**

| BLT Group | ยกเลิก | Cancel Rate (%) | No-Show | No-Show Rate (%) | Combined Rate (%) |
| --- | --- | --- | --- | --- | --- |
| Last Minute (0–7 วัน) | 51 | 4.32% | 68 | 5.76% | 10.08% |
| Short Lead (8–30 วัน) | 651 | 14.35% | 230 | 5.07% | 19.42% |
| Long Lead (31–60 วัน) | 757 | **17.68%** | 206 | 4.81% | **22.49%** |

> *[แทรกรูปกราฟ: Cancel Rate vs No-Show Rate by BLT Group (Clustered Bar)]*

**ขั้นตอนที่ 3: สรุปจำนวน Lost Bookings และ Phantom Inventory**

| BLT Group | Lost Bookings (Cancel + No-Show) | % ของกลุ่ม |
| --- | --- | --- |
| Last Minute | 119 | 10.08% |
| Short Lead | 881 | 19.42% |
| Long Lead | **963** | **22.49%** |
| **รวมทั้งหมด** | **1,963** | **19.63% ของ booking ทั้งหมด** |

> *[แทรกรูปกราฟ: จำนวน Lost Bookings by BLT Group (Bar Chart)]*

**สรุป H4:** กลุ่ม Long Lead (31–60 วัน) ซึ่งมีสัดส่วน 42.82% ของ booking ทั้งหมด มี Combined Loss Rate สูงสุดที่ 22.49% สูงกว่า Last Minute ถึง 2.23 เท่า รวม Lost Bookings ทั้งระบบ 1,963 รายการ คิดเป็น 19.63% ของ booking ทั้งปี ก่อให้เกิด Phantom Inventory และ Revenue Loss รวม **22,923,007.91 บาท**

---

### H5: Short LOS & Orphan Nights — **สมมติฐานได้รับการยืนยันบางส่วน**

**ขั้นตอนที่ 1: แบ่งสัดส่วน Booking Weekend vs Weekday**

จากการจองทั้งหมด **7,965 รายการ (Checked-Out)**:

| ช่วงเวลา | จำนวน | สัดส่วน (%) |
| --- | --- | --- |
| Weekday (จ–ศ) | 5,669 | **71.17%** |
| Weekend (ส–อ) | 2,296 | 28.83% |

**ขั้นตอนที่ 2: วิเคราะห์สัดส่วน Weekend Booking ตาม Segment**

| Segment | Weekend Booking | สัดส่วนของ Weekend ทั้งหมด (%) | Avg LOS Weekend | Avg LOS Weekday |
| --- | --- | --- | --- | --- |
| Leisure | 921 | **40.11%** | 3.53 | 3.48 |
| Business | 578 | 25.17% | 1.49 | 1.50 |
| Transient | 447 | 19.47% | 2.05 | 2.01 |
| Group | 230 | 10.02% | 5.09 | 5.03 |
| Wholesale | 120 | 5.23% | 2.96 | 2.99 |

> *[แทรกรูปกราฟ: Avg LOS Weekday vs Weekend by Segment (Clustered Bar)]*

**ขั้นตอนที่ 3: วิเคราะห์ LOS=1 บนวันสุดสัปดาห์ตาม Segment**

ใน Weekend 2,296 รายการ พบ LOS=1 จำนวน **433 รายการ (18.86%)**:

| Segment | Weekend Booking | LOS=1 Count | % LOS=1 ภายใน Segment |
| --- | --- | --- | --- |
| Business | 578 | **295** | **51.04%** |
| Transient | 447 | **138** | **30.87%** |
| Leisure | 921 | 0 | 0.00% |
| Group | 230 | 0 | 0.00% |
| Wholesale | 120 | 0 | 0.00% |
| **รวม** | **2,296** | **433** | **18.86%** |

> *[แทรกรูปกราฟ: % LOS=1 on Weekend by Segment (Bar Chart)]*

**สรุป H5:** สมมติฐานเดิมระบุว่า Leisure เป็นต้นเหตุ แต่ข้อมูลจริงพบว่า **Business (51.04%)** และ **Transient (30.87%)** คือกลุ่มที่มี LOS=1 ในวันสุดสัปดาห์ ขณะที่ Leisure ซึ่งเป็น segment ใหญ่สุดในวันหยุด (40.11%) กลับไม่มี LOS=1 เลย รวม Revenue Loss จาก Orphan Nights **1,797,200.85 บาท**

---

## 8. ข้อเสนอแนะเชิงนโยบาย (Policy Recommendations)

### R1: ปรับโครงสร้าง Booking Channel — ลด OTA Dependency ใน High Season

**ปัญหา:** OTA มีสัดส่วน 51.1% ของ booking ทั้งปี และ 49.7% ใน High Season ทำให้ commission ที่จ่ายออกไปรวม 3.15 ล้านบาท

**ข้อเสนอแนะ:** โรงแรมควรกำหนด Direct Booking Target ที่ 40% ใน High Season โดยใช้กลยุทธ์ "Best Rate Guarantee" บนเว็บไซต์โดยตรง, ลด OTA allotment ในช่วง High Demand, และเสนอสิทธิประโยชน์เพิ่มเติม (breakfast, late checkout) สำหรับการจองผ่าน Direct channel เพื่อเพิ่ม Net ADR โดยไม่ต้องขึ้นราคา rack

---

### R2: บังคับใช้ Rate Fence ใน High Season — จำกัด Discount Rate Codes

**ปัญหา:** Non-Refundable, Seasonal Promo และ AAA Discount ถูกใช้ใน High Season รวม 2,549 bookings ทำให้สูญเสียรายได้ 1.51 ล้านบาทเทียบกับ Rack Rate

**ข้อเสนอแนะ:** กำหนด Blackout Period สำหรับ rate code ที่มีส่วนลดในช่วง High Season (ต.ค.–ก.พ.) โดยเฉพาะ Non-Refundable และ Seasonal Promo ให้โรงแรมเปิดขายเฉพาะ Rack Rate และ Corporate Flat Rate ในช่วงดังกล่าว พร้อมทบทวนเงื่อนไข Non-Refundable ซึ่งควรมี ADR premium สูงกว่า Rack Rate เพื่อ compensate ความเสี่ยงของผู้ซื้อ

---

### R3: นำ Cancellation Policy แบบ Tiered มาใช้สำหรับ Long Lead Bookings

**ปัญหา:** Long Lead (31–60 วัน) มี Combined Loss Rate 22.49% และเป็นแหล่งที่มาหลักของ Phantom Inventory รวมมูลค่า 11.29 ล้านบาท

**ข้อเสนอแนะ:** ออกแบบ cancellation policy แบบขั้นบันไดสำหรับกลุ่ม Long Lead เช่น (1) จองล่วงหน้า 31–60 วัน ต้องชำระ 30% เป็น non-refundable deposit (2) ยกเลิกก่อน 14 วัน คืนเงิน 50% (3) ยกเลิกภายใน 7 วัน ไม่คืนเงิน นอกจากนี้ควรพิจารณา waitlist system สำหรับห้องที่ถูกยกเลิกเพื่อ re-sell ในทันที

---

### R4: กำหนด Minimum Night Policy สำหรับกลุ่ม Business และ Transient ในวันสุดสัปดาห์

**ปัญหา:** Business (51.04%) และ Transient (30.87%) มี LOS=1 ในวันสุดสัปดาห์สูง ก่อให้เกิด Orphan Nights ในวันศุกร์และวันอาทิตย์ กดดัน Occupancy และ RevPAR โดยรวม

**ข้อเสนอแนะ:** กำหนด Minimum 2-Night Stay สำหรับ Business และ Transient segment ในช่วงวันสุดสัปดาห์ (เสาร์–อาทิตย์) ใน High Season พร้อมเสนอ "Weekend Package" ที่รวม breakfast + activity เพื่อ incentivize การเข้าพักแบบ 2 คืนและเพิ่ม ancillary revenue

---

## 9. โครงสร้างไฟล์โปรเจค (Project Structure)

```
hotel-revenue-stagnation-analysis/
│
├── README.md                  # เอกสารโครงการฉบับนี้
│
├── H1.H3,H5.xlsx              # ไฟล์วิเคราะห์หลัก H1, H3, H5
│   ├── h1 / h1.1 / h1.2 / h1.3    → OTA Dependency & Commission Analysis
│   ├── h3 / h3.1 / h3.2 / h3.3    → Rate Code Dilution Analysis
│   ├── h5 / h5.1                    → Short LOS & Orphan Nights Analysis
│   ├── fact_bookings                 → ข้อมูลการจองหลัก (10,000 rows)
│   ├── dim_room_inventory            → Capacity ห้องรายวัน
│   ├── dim_rate_codes                → แผนราคา
│   ├── dim_channels                  → ช่องทางการจองและ commission
│   └── dim_calendar                  → Date dimension (season, day_name, is_weekend)
│
├── H2,H4.xlsx                 # ไฟล์วิเคราะห์ H4
│   ├── H4                           → BLT vs Cancellation/No-Show Rate
│   ├── H2.1 / H2.2 / H2.3 / H2.4   → Day of Week Analysis (ข้อมูลอ้างอิง)
│   └── fact_bookings (4)             → ชุดข้อมูลสำรอง
│
├── H_Charts.xlsx              # Charts สรุปผลการวิเคราะห์ทุก Hypothesis
│   ├── H1                           → ADR vs Net ADR + Commission Loss by Channel
│   ├── H3                           → ADR by Rate Code × Season + Revenue Loss
│   ├── H4                           → Cancellation Rate by BLT Group
│   ├── H5                           → % LOS=1 on Weekend + Avg LOS by Segment
│   ├── RL_H4                        → Revenue Lost from Cancellation & No-Show
│   └── (H2 ถูกนำออกจากขอบเขตโครงการ)
│
├── Data/                      # ข้อมูลต้นทาง (raw data)
└── คำสั่ง.pdf                  # โจทย์และคำสั่งโครงการ DS512-513
```

---

## 10. ข้อมูลผู้จัดทำ (Author)
นาย  ธนวันต์  มานะมงคล  รหัสนิสิต 66102010141  
นาย  จิรัฏฐ์   อู่คงคา     รหัสนิสิต  66102010233
นาย  กษิดิศ  มิตรรวมเสรี  รหัสนิสิต  66102010565

## 11. คำสั่ง (Prompt) ที่ใช้ในการสร้างข้อมูล
สร้าง dataset จำลองการจองโรงแรมขนาด 80 ห้อง ในรูปแบบ Star Schema ประกอบด้วย 5 ไฟล์ CSV
-ไฟล์แรกคือ dim_calendar.csv สร้างข้อมูลวันที่ครอบคลุมปี 2024 ทั้งปี 366 วัน มีคอลัมน์ date_key เป็น DATE รูปแบบ YYYY-MM-DD, day_name เป็นชื่อวันภาษาอังกฤษ, is_weekend เป็น Boolean true ถ้าเป็น Saturday หรือ Sunday, is_holiday เป็น Boolean true ถ้าเป็นวันหยุดนักขัตฤกษ์ประมาณ 15-20 วัน, และ season เป็น "High Season" สำหรับเดือนตุลาคมถึงมีนาคม หรือ "Low Season" สำหรับเมษายนถึงกันยายน

-ไฟล์ที่สองคือ dim_channels.csv มี 5 ช่องทางคือ CH_DIR ชื่อ Direct Website ประเภท Direct ค่า commission 0.00, CH_EXP ชื่อ Expedia ประเภท OTA ค่า commission 0.18, CH_BDC ชื่อ Booking.com ประเภท OTA ค่า commission 0.15, CH_WAL ชื่อ Walk-in ประเภท Direct ค่า commission 0.00, และ CH_GDS ชื่อ Corporate Agent ประเภท GDS ค่า commission 0.10

-ไฟล์ที่สามคือ dim_rate_codes.csv มี 5 rate code คือ RC_RACK ชื่อ Rack Rate เป็น Standard published rate ไม่ commissionable, RC_AAA ชื่อ AAA Discount ลด 10% สำหรับสมาชิก AAA ไม่ commissionable, RC_CORP ชื่อ Corporate Flat Rate รวม Breakfast และ Wifi เป็น commissionable, RC_NRF ชื่อ Non-Refundable ลด 20% ไม่สามารถยกเลิกได้ ไม่ commissionable, และ RC_PROMO ชื่อ Seasonal Promo เป็น Limited time offer ไม่ commissionable

-ไฟล์ที่สี่คือ dim_room_inventory.csv สร้าง 366 แถวสำหรับทุกวันในปี 2024 มีคอลัมน์ date ตรงกับ dim_calendar, total_capacity คงที่ 80 ห้องทุกวัน, rooms_out_of_order สุ่ม 0-3 ห้องโดยส่วนใหญ่เป็น 0, และ rooms_available_for_sale เท่ากับ total_capacity ลบ rooms_out_of_order

-ไฟล์ที่ห้าคือ fact_bookings.csv สร้าง 10,000 แถว มีคอลัมน์ booking_id รูปแบบ RES-10001 ไปเรื่อยๆ, guest_id รูปแบบ GST-XXXX สุ่มจากลูกค้าประมาณ 2,000 รายโดยให้บางคนกลับมาจองซ้ำ, booking_date เป็นวันที่ทำการจองในปี 2024, check_in_date ต้องหลัง booking_date, check_out_date ต้องหลัง check_in_date อย่างน้อย 1 คืน, room_type_id เลือกจาก RT_STQ RT_OCV RT_DLK RT_STE RT_ACC, rate_code_id อ้างอิง dim_rate_codes, channel_id อ้างอิง dim_channels, segment_id เลือกจาก Leisure Corporate Group Government, status เลือกจาก Checked-Out ประมาณ 75% Cancelled ประมาณ 15% และ No-Show ประมาณ 10%, total_room_revenue คำนวณจากราคาห้องคูณจำนวนคืนโดย Cancelled และ No-Show บางส่วนเป็น 0, number_of_rooms ส่วนใหญ่ 1 ห้อง บางส่วน 2-3 ห้องสำหรับ Group, adults_count 1-4 คน, และ children_count 0-2 คน

ราคาห้องพักต่อคืนแยกตามประเภทห้องดังนี้ RT_STQ อยู่ที่ 1,800-2,500 บาท, RT_OCV อยู่ที่ 2,500-4,000 บาท, RT_DLK อยู่ที่ 3,000-5,000 บาท, RT_STE อยู่ที่ 6,000-12,000 บาท, และ RT_ACC อยู่ที่ 1,600-2,200 บาท โดยปรับราคาเพิ่มในช่วง High Season คูณ 1.3-1.5 และ Weekend คูณ 1.1-1.2 ส่วน RC_NRF ลด 20% และ RC_PROMO ลด 15-25%
ข้อมูลควรมีความสมจริง เช่น High Season มี Occupancy สูงกว่า Low Season, Group Segment มักจอง 2-5 ห้องและพักนานกว่า, OTA Channel มีสัดส่วนการจองสูงสุดประมาณ 40%, booking_date ควรกระจายตลอดปีไม่กระจุกตัว และห้อง RT_STE ควรมีจำนวนน้อยกว่าประเภทอื่น
