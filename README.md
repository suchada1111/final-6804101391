# Final Exam – Software Testing with Playwright

- **ชื่อ-นามสกุล / รหัสนักศึกษา:** สุชาดา บุบผา / 6804101391
- **ระบบที่ทดสอบ:** https://www.saucedemo.com/ (Playwright Test + TypeScript)

## วิธีรัน
```bash
npm install
npx playwright install chromium
npx playwright test
npx playwright show-report
```

## โครงสร้างการทดสอบ
| รหัส | ไฟล์ | ระดับ |
|---|---|---|
| UT-01 | tests/unit/UT-01.spec.ts | UI Component (Login validation, 3 test cases) |
| UT-02 | tests/unit/UT-02.spec.ts | UI Component (ปุ่ม Add to cart/Remove + Badge) |
| IT-01 | tests/integration/IT-01.spec.ts | Top-Down + Stub |
| IT-02 | tests/integration/IT-02.spec.ts | Bottom-Up + Driver |
| ST-01 | tests/system/ST-01.spec.ts | System: ซื้อสินค้า 2 ชิ้น |
| ST-02 | tests/system/ST-02.spec.ts | System: Checkout validation & recovery |

## IT-01: Top-Down Integration (Stub)
- **Real:** หน้า Login ของ SauceDemo และการ redirect ไป `inventory.html` (ทดสอบด้วยเบราว์เซอร์จริง)
- **Stub:** หน้า Inventory ถูกแทนที่ด้วย HTML จำลองผ่าน `page.setContent()` ซึ่งแสดงข้อความ
  `Mock Inventory Ready` และสินค้าจำลอง `Mock Backpack`, `Mock Bike Light`
- โมดูลระดับบน (Login) ถูกทดสอบก่อน ส่วนโมดูลระดับล่าง (Inventory) ถูกแทนด้วย Stub
  เป็นการจำลอง DOM ระดับ Browser ไม่ใช่การแทน Internal Module ของ SauceDemo

## IT-02: Bottom-Up Integration (Driver)
- **Driver:** ฟังก์ชัน `driverOpenInventory()` เพิ่ม Cookie `session-username=standard_user`
  ที่โดเมน `www.saucedemo.com` ด้วย `BrowserContext.addCookies()` แล้วเปิด `inventory.html` โดยตรง
  โดยไม่ผ่านฟอร์ม Login ทำหน้าที่เป็นตัวเรียก/เตรียมสถานะให้โมดูลระดับล่าง
- **Real:** หน้า Inventory (สินค้า 6 รายการ), การเพิ่มสินค้า, Cart Badge และหน้า Cart ของ SauceDemo จริง

## ผลการทดสอบ
ดู `reports/test-summary.md`, `playwright-report/index.html` และ `reports/results.json`