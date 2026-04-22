# hotel-revenue-stagnation-analysis
โครงการวิเคราะห์รายได้ห้องพักและปัจจัยที่มีผลต่อ Revenue Per Available Room (RevPAR) เพื่อสนับสนุนการวางแผนเชิงกลยุทธ์ด้านราคาและการบริหาร inventory ของโรงแรมด้วยวิธีการทาง Data Analytics”
# 1. บทนำและความเป็นมา (Introduction & Background)
ธุรกิจโรงแรมมีการแข่งขันสูงและต้องอาศัยข้อมูลเพื่อสนับสนุนการตัดสินใจด้านราคาและการบริหารห้องพัก แม้โรงแรมจะมีอัตราการเข้าพักอยู่ในระดับที่ดี แต่ยังพบว่า Revenue Per Available Room (RevPAR) ต่ำกว่าคู่แข่ง สะท้อนถึงปัญหาการตั้งราคาและการบริหาร inventory ที่ไม่มีประสิทธิภาพ ส่งผลให้รายได้ชะงักงัน (Revenue Stagnation)
ดังนั้น โครงการนี้จึงมุ่งใช้ Data Analytics วิเคราะห์ปัจจัยที่ส่งผลต่อ RevPAR เช่น อัตราการเข้าพัก ราคาห้องพัก ช่องทางการจอง และพฤติกรรมลูกค้า เพื่อสนับสนุนการวางแผนเชิงกลยุทธ์ด้านราคาและเพิ่มประสิทธิภาพการสร้างรายได้ของโรงแรม
# 2. วัตถุประสงค์ของโครงการ (Research Objectives)
- วิเคราะห์ปัจจัยที่มีผลต่อ RevPAR
- วิเคราะห์ ADR, Occupancy, LOS, BLT
- วิเคราะห์ Booking Channel, Rate Code, Room Type, Customer Segment
- สร้างกรอบการวิเคราะห์เพื่อสนับสนุน strategic pricing
- เสนอแนวทางเพิ่มรายได้และ optimize inventory
# 3. คำถามการวิจัยและสมมติฐาน (Research Questions & Hypothesis)
  Research Questions
- ปัจจัยใด (เช่น ช่องทางการจอง, ประเภทราคา, ระยะเวลาเข้าพัก) มีผลต่อการเติบโตของรายได้ (Revenue) และ RevPAR ของโรงแรมมากที่สุด?
- พฤติกรรมการจองของลูกค้า (เช่น ระยะเวลาการจองล่วงหน้า, การยกเลิก, การเข้าพักในวันหยุด/วันธรรมดา) มีอิทธิพลต่อรายได้สุทธิ (Net ADR) และอัตราการเข้าพัก (Occupancy) หรือไม่?
- ใช้การวิเคราะห์ข้อมูลเชิงสำรวจ (EDA) เพื่อค้นหาต้นตอของปัญหารายได้คงที่ (Revenue Stagnation) และสร้าง Business Insights สำหรับการวางแผนเชิงกลยุทธ์
- โครงสร้างราคาและเงื่อนไขการจอง (Rate Code & Policy) ในปัจจุบัน มีประสิทธิภาพเพียงพอต่อการทำกำไรสูงสุดในช่วงเวลาที่ความต้องการสูง (High Season) หรือไม่?
- ผลลัพธ์จากการวิเคราะห์สามารถนำไปใช้สนับสนุนการตัดสินใจด้านกลยุทธ์การตั้งราคา (Pricing Strategy), การบริหารช่องทางการจัดจำหน่าย (Distribution Channel), และนโยบายการจอง (Booking Policy) ได้อย่างไร?
# 4. ชุดข้อมูลและตัวแปรที่ใช้ (Dataset & Features)
- จำนวนแถวข้อมูล: 10,000
- จำนวนตัวแปรทั้งหมด: 29
- แหล่งที่มาของข้อมูล:
# Data Dictionary: fact_bookings
| Attribute          | คำอธิบาย                                  | Data Type          | ช่วงค่าที่ถูกต้อง / ตัวอย่าง               |
| ------------------ | ----------------------------------------- | ------------------ | ------------------------------------------ |
| booking_id         | รหัสการจอง (Primary Key)                  | Ordinal            | RES-10023                                  |
| guest_id           | รหัสผู้เข้าพัก                            | Ordinal            | G-45821                                    |
| booking_date       | วันที่ทำการจอง                            | Interval (Date)    | 2025-09-15                                 |
| check_in_date      | วันที่เข้าพัก                             | Interval (Date)    | 2025-10-01                                 |
| check_out_date     | วันที่ออกจากที่พัก                        | Interval (Date)    | 2025-10-03                                 |
| room_type_id       | รหัสประเภทห้อง (FK)                       | Ordinal            | RT_DELUXE                                  |
| rate_code_id       | รหัสแผนราคา (FK)                          | Ordinal            | RC_CORP                                    |
| channel_id         | รหัสช่องทางการจอง (FK)                    | Ordinal            | CH_EXP                                     |
| segment_id         | รหัสกลุ่มลูกค้า (FK)                      | Ordinal            | SEG_BUSINESS                               |
| status             | สถานะการจอง                               | Nominal            | Confirmed, Cancelled, Checked-Out, No-Show |
| total_room_revenue | รายได้จากค่าห้อง (ไม่รวมภาษี/บริการเสริม) | Ratio (Continuous) | 0 – ∞ (เช่น 3500.00)                       |
| number_of_rooms    | จำนวนห้องที่จอง                           | Ratio (Discrete)   | 1 – ∞                                      |
| adults_count       | จำนวนผู้ใหญ่                              | Ratio (Discrete)   | 1 – ∞                                      |
| children_count     | จำนวนเด็ก                                 | Ratio (Discrete)   | 0 – ∞                                      |
# Data Dictionary: dim_room_inventory
| Attribute                | คำอธิบาย                  | Data Type        | ช่วงค่าที่ถูกต้อง / ตัวอย่าง        |
| ------------------------ | ------------------------- | ---------------- | ----------------------------------- |
| date                     | วันที่ (Primary Key)      | Interval (Date)  | 2025-10-01                          |
| total_capacity           | จำนวนห้องทั้งหมดของโรงแรม | Ratio (Discrete) | เช่น 150                            |
| rooms_out_of_order       | ห้องที่ไม่พร้อมใช้งาน     | Ratio (Discrete) | 0 – total_capacity                  |
| rooms_available_for_sale | ห้องที่สามารถขายได้       | Ratio (Discrete) | total_capacity - rooms_out_of_order |
# Data Dictionary: dim_rate_codes
| Attribute         | คำอธิบาย                  | Data Type        | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| ----------------- | ------------------------- | ---------------- | ---------------------------- |
| rate_name         | ชื่อแผนราคา               | Nominal          | Corporate Flat Rate          |
| description       | รายละเอียดแผนราคา         | Nominal (Text)   | Includes Breakfast & Wifi    |
| is_commissionable | มีค่าคอมมิชชั่นหรือไม่    | Nominal (Binary) | True / False                 |
# Data Dictionary: dim_channels
| Attribute       | คำอธิบาย                  | Data Type          | ช่วงค่าที่ถูกต้อง / ตัวอย่าง |
| --------------- | ------------------------- | ------------------ | ---------------------------- |
| channel_name    | ชื่อช่องทาง               | Nominal            | Expedia, Agoda               |
| channel_type    | ประเภทช่องทาง             | Nominal            | OTA, Direct, Wholesaler      |
| commission_rate | อัตราค่าคอมมิชชั่น        | Ratio (Continuous) | 0 – 1 (เช่น 0.15)            |
# Data Dictionary: dim_calendar
| Attribute  | คำอธิบาย                     | Data Type        | ช่วงค่าที่ถูกต้อง / ตัวอย่าง             |
| ---------- | ---------------------------- | ---------------- | ---------------------------------------- |
| date_key   | วันที่ (Primary Key)         | Interval (Date)  | 2025-10-01                               |
| day_name   | ชื่อวัน                      | Nominal          | Monday, Tuesday                          |
| is_weekend | เป็นวันหยุดสุดสัปดาห์หรือไม่ | Nominal (Binary) | True / False                             |
| is_holiday | เป็นวันหยุดนักขัตฤกษ์หรือไม่ | Nominal (Binary) | True / False                             |
| season     | ฤดูกาล                       | Nominal          | High Season, Low Season, Shoulder Season |
# ตัวแปรเป้าหมาย (Target Variable)
- total_room_revenue
# ตัวแปรสำคัญที่ใช้วิเคราะห์ (Key Features)
- กลุ่มวันเวลา (Time Features)
- กลุ่มการจอง (Booking Characteristics)
- กลุ่มลูกค้า (Customer Features)
- กลุ่มรายได้และราคา (Pricing & Revenue Drivers)
- ช่องทางการขาย (Channel Features)
- ประเภทห้อง (Product Features)
