# Final Exam: Software Testing ด้วย Playwright

ระบบที่ทดสอบ: https://www.saucedemo.com/ | เครื่องมือ: Playwright Test (TypeScript)

## วิธีรัน
```bash
npm install
npx playwright install chromium
npx playwright test
npx playwright show-report
```
รายงาน: `playwright-report/index.html` และ `reports/results.json`

## โครงสร้างการทดสอบ
| รหัส | ไฟล์ | เนื้อหา |
|---|---|---|
| UT-01 | tests/unit/UT-01.spec.ts | Login Form Validation (3 test cases) |
| UT-02 | tests/unit/UT-02.spec.ts | สถานะปุ่ม Add to cart / Remove และ Cart Badge |
| IT-01 | tests/integration/IT-01.spec.ts | Top-Down ด้วย Stub |
| IT-02 | tests/integration/IT-02.spec.ts | Bottom-Up ด้วย Driver |
| ST-01 | tests/system/ST-01.spec.ts | ซื้อสินค้า 2 รายการตั้งแต่ต้นจนจบ |
| ST-02 | tests/system/ST-02.spec.ts | Checkout Validation และการแก้ไขจนสำเร็จ |

> บทที่ 1 เป็น UI Component/Functional Testing แบบ Black-box ผ่าน Browser ไม่ใช่ Unit Testing ของฟังก์ชันภายใน เพราะไม่สามารถเข้าถึง Source Code ของ SauceDemo

## IT-01: Top-Down Integration (Real vs Stub)
- **Real:** หน้า Login ของ SauceDemo (กรอกข้อมูล, กด Login, ตรวจสอบ URL `inventory.html`)
- **Stub:** หน้า Inventory ที่สร้างด้วย `page.setContent()` แสดง `Mock Inventory Ready` และสินค้าจำลอง `Mock Backpack`, `Mock Bike Light`
- Stub เป็นการจำลอง DOM ระดับ Browser หลัง Login จริง ไม่ได้แทน Internal Module ของ SauceDemo
- แนวคิด Top-Down: ทดสอบโมดูลระดับบน (Login) ก่อน โดยใช้ Stub แทนโมดูลระดับล่างที่ยังไม่นำมาเชื่อมต่อ (Inventory)

## IT-02: Bottom-Up Integration (Driver vs Real)
- **Driver:** ฟังก์ชัน `driverOpenInventory()` ทำหน้าที่แทนโมดูล Login โดยตั้ง Cookie `session-username=standard_user` ผ่าน `BrowserContext.addCookies()` แล้วเปิด `inventory.html` โดยตรง ไม่ผ่าน Login Form
- **Real:** หน้า Inventory (สินค้า 6 รายการ), การเพิ่มสินค้าลง Cart, Cart Badge และหน้า Shopping Cart ของ SauceDemo
- แนวคิด Bottom-Up: ทดสอบโมดูลระดับล่าง (Inventory/Cart) ก่อน โดยใช้ Driver เรียกใช้งานแทนโมดูลระดับบน
