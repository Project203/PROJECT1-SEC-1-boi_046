# PROJECT1-SEC-1-boi_046
## Member
| StudentID | Name | Responsibilities | Percentage
|-----------|------|------------|--------
| 64130500003 | กรพิณธ์ จงศรีวัฒนพร | Sound management functions, background track and effect + game dialog, plots | 20%
| 64130500004 | กษิดิ์เดช พลายเผือก | Core game function, state management, dialog type management, JSON structure | 24%
| 64130500009 | จิรพงศ์ รติวัฒน์ | Ingame management, change dialog, end game function UI and Vue directives | 20%
| 64130500012 | ชนาธิป บุพพัณหสมัย | Game dialog replacing function, send dialog output to user interface,Dynamic in end Scene | 18%
|	64130500018 | ณวรรธน์ ศรีสะอาด | Dynamic image of character, background game and send to user interface, JSON writer | 18%

<h3>รายการฟีเจอร์และคำอธิบาย</h3>
<pre>
- ตัวเกมมี 17 function
- มี variable ทั้ง 7 ตัว และ ref ทั้งหมด 6 
- มีฟีเจอร์หลัก ๆ คือ
  1. gameplay โดยที่เราแบ่งเกมเป็นทั้งหมด 3 state 
      state 1 คือ หน้าแรก 
      state 2 คือ หน้าเนื้อเรื่อง โดยจะแบ่งเป็น 2 ส่วน คือ ส่วนของการเล่นเกม และอีกส่วนคือหน้า subtitles ที่จะเป็นบทบรรยายล้วน ๆ
      state 3 คือ หน้าจบเกม
  2. เปิดปิดเพลง
  3. เลือกช็อยต์ เป็นฟีเจอร์ไว้ตอบกับตัวละคร
  4. ชื่อผู้เล่น สามารถปรับเปลี่ยนได้ตามต้องการ
  5. subtitles เป็นหน้าที่จะบรรยายเหตุการณ์ที่เกิดขึ้น
  6. ending โดยแต่ละคำตอบที่เราตอบจะมีผลต่อฉากจบ
  7. go home เริ่มใหม่
  8. skip ข้ามฉากพูด
  9. change image มีการเปลี่ยนรูปพื้นหลัง เปลี่ยนรูปตัวละคร
  10. ชื่อผู้เล่นในเกมจะเปลี่ยนตามชื่อเราตั้งทั้งในบทพูด ช็อยต์ และบทบรรยาย รวมถึงฉากจบด้วย
  </pre>

<h3>คู่มือ</h3><pre>
- กรอกชื่อ
- กดปุ่ม play now เพื่อเริ่มเล่นเกม
- สามารถเปิดและปิดเพลงได้จากปุ่มซ้ายบนที่อยู่ใกล้สัญลักษณ์โลโก้
- หลังจากเริ่มเกมจะมีบทพูด และอาจมีช็อยต์ให้เราเลือกทางด้านขวาของจอ โดยการกดแต่ละช็อยต์จะมีคะแนนที่แตกต่างกัน จนนำไปสู่ฉากจบในแต่ละแบบ
- มีปุ่ม restart ขวาบนที่เราสามารถเริ่มเล่นเกมใหม่ได้
- หลังจากขึ้นฉากจบเราสามารถกลับมาหน้าแรกโดยการกดปุ่ม retry ที่อยู่ขวาล่างได้
</pre>
