# Study Streak Battle

Ung dung thi dau hoc bai giua 3 nguoi ban. Ai cuoi bang mua ca phe!

## Cach cai dat

### Buoc 1: Tao Firebase Project

1. Vao [Firebase Console](https://console.firebase.google.com)
2. Nhan **Add project** (hoac **Tao du an**)
3. Dat ten project (VD: `study-streak-battle`)
4. Tat Google Analytics (khong can thiet) → **Create project**

### Buoc 2: Lay Firebase Config

1. Trong Firebase Console, nhan bieu tuong **</>** (Web) de them web app
2. Dat ten app (VD: `study-streak`)
3. **Khong can** tick Firebase Hosting
4. Nhan **Register app**
5. Copy doan `firebaseConfig` hien ra — se can dung o Buoc 5

### Buoc 3: Bat Authentication

1. Trong Firebase Console → menu trai → **Authentication**
2. Nhan **Get started**
3. Tab **Sign-in method** → bat **Email/Password**
4. Nhan **Save**

### Buoc 4: Tao Firestore Database

1. Menu trai → **Firestore Database**
2. Nhan **Create database**
3. Chon **Start in production mode**
4. Chon location gan nhat: `asia-southeast1` (Singapore)
5. Nhan **Create**

Sau khi tao xong, vao tab **Rules** va thay toan bo noi dung bang noi dung file `firestore.rules` trong project nay. Nhan **Publish**.

### Buoc 5: Cap nhat Firebase Config trong code

Mo file `index.html`, tim doan `FIREBASE_CONFIG` (gan dong 479) va thay cac gia tri `YOUR_...` bang config ban lay o Buoc 2:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "AIzaSy...",
  authDomain: "study-streak-battle.firebaseapp.com",
  projectId: "study-streak-battle",
  storageBucket: "study-streak-battle.firebasestorage.app",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### Buoc 6: Deploy len GitHub Pages

1. Tao repo moi tren GitHub (co the public hoac private)
2. Push code len:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/study-streak-battle.git
   git push -u origin main
   ```
3. Vao repo tren GitHub → **Settings** → **Pages**
4. Source: chon **Deploy from a branch**
5. Branch: chon **main** / **root** → **Save**
6. Doi 1-2 phut, trang se co tai: `https://YOUR_USERNAME.github.io/study-streak-battle/`

### Buoc 7: Moi ban be

Gui link trang web cho 2 nguoi ban. Ho vao trang → nhan **Dang ky** → tao tai khoan → bat dau thi dau!

---

## Tinh nang

### Dong ho hoc bai
- Countdown timer voi nut Bat dau / Tam dung / Ket thuc
- Chon nhanh 30p / 60p / 90p / 120p hoac nhap so phut tuy y
- Timer het gio → tu dong ghi nhan phien hoc
- Ket thuc som (>= 1 phut) → van ghi nhan thoi gian thuc te

### He thong tinh diem

| Hanh dong | Diem |
|-----------|------|
| Moi section (30 phut hoc) | +10 |
| Cam ket khong mo MXH | +5 / section |
| Phut le cuoi ngay | (phut ÷ 30) × 10 (ty le) |

Vi du: hoc 75 phut = 2 section (20 diem) + 15p le (5 diem) = 25 diem co so. Neu cam ket MXH thi cong them 2 × 5 = 10, tong = 35.

### Chuoi ngay (Streak)

Hoc moi ngay de xay dung chuoi lien tiep. Moi moc dat duoc se nhan bonus 1 lan:

| Ngay | Bonus |
|------|-------|
| 2 | +5 |
| 5 | +15 |
| 7 | +30 |
| 14 | +60 |
| 30 | +120 |
| 40 | +200 |
| 50 | +350 |
| 60 | +550 |
| Sau 60 | +20/ngay |

**Streak Recovery**: Bo lo **dung 1 ngay** → co the chuoc lai streak bang cach tra 1.2× bonus moc gan nhat. Bo 2+ ngay → mat streak va mat tat ca cac moc da dat.

### Cot moc sections

Tong sections tich luy (khong reset khi het vong):

| Tong | Bonus |
|------|-------|
| 1 | +5 |
| 10 | +30 |
| 25 | +60 |
| 50 | +120 |
| 75 | +180 |
| 100 | +300 |
| 150 | +450 |
| 200 | +700 |

### Vong dau

- Mac dinh 5 ngay / vong
- Cuoi vong, ca 3 nguoi bo phieu chon thoi luong vong tiep theo
- Quyet dinh: da so thang (2+ chon giong nhau). Neu 3 phieu khac nhau → lay trung vi
- Tuy chon: 3 / 5 / 7 / 10 / 14 / 21 / 30 ngay
- Bang xep hang co 2 che do: **Vong nay** (chi tinh diem trong vong hien tai) va **Tich luy** (tong tat ca)
- Nguoi cuoi bang xep hang mua ca phe cho nguoi dung dau!

---

## Luu y bao mat

- Firebase config trong source code la cong khai — dieu nay binh thuong va duoc Firebase thiet ke nhu vay
- Bao mat duoc xu ly boi Firestore Security Rules (file `firestore.rules`)
- Moi nguoi dung chi co the sua du lieu cua chinh minh
- Cai dat vong dau (`settings/round`) cho phep tat ca nguoi dung da dang nhap doc/ghi (de bo phieu)

## Cau truc du lieu Firestore

```
users/{userId}
  ├── displayName, email, totalMinutes, totalSections
  ├── currentStreak, longestStreak, lastStudyDate
  ├── streakMs[], sectionMs[], bonusPts, roundVote
  └── days/{YYYY-MM-DD}
        ├── date, minutes, sessionCount, sections
        ├── basePoints, mxhPoints, bonusToday, dayPoints
        ├── sessionLog[], mxhPledge
        └── recoveryDeduction, recoveryDay, streakDaily60

settings/round
  ├── number, startDate, endDate, duration
  ├── status ("active" | "voting")
  └── votes { userId: days }
```

## Cong nghe su dung

- HTML / CSS / JavaScript (vanilla, single file)
- Firebase Authentication (dang nhap email)
- Cloud Firestore (luu tru du lieu, real-time sync)
- GitHub Pages (hosting)
- Press Start 2P font (retro pixel-art design)
- Web Audio API (thong bao khi het gio)
