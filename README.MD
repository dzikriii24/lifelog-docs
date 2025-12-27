# 📚 **Lifelog - Daily Activity & Mood Tracker**

Sebuah aplikasi web untuk mencatat aktivitas harian, mood, dan tingkat energi. Dibangun dengan arsitektur client-server menggunakan **Node.js + Express** untuk backend dan **Vanilla JavaScript + Tailwind CSS** untuk frontend.

![Lifelog Dashboard](https://img.shields.io/badge/Status-Live-brightgreen) ![Backend](https://img.shields.io/badge/Backend-Node.js-green) ![Frontend](https://img.shields.io/badge/Frontend-Vanilla%20JS-yellow) ![Database](https://img.shields.io/badge/Database-MySQL-blue)

## 🎯 **Fitur Utama**

- ✅ **Autentikasi User** (Register & Login dengan JWT)
- ✅ **CRUD Aktivitas Harian** dengan kategori, mood, dan energi
- ✅ **Dashboard Analytics** dengan chart dan statistik
- ✅ **Filter & Pencarian** aktivitas
- ✅ **Responsive Design** (Mobile & Desktop)
- ✅ **Dark/Light Mode**
- ✅ **Proteksi Route** berdasarkan autentikasi

## 🏗️ **Arsitektur Sistem**

```
┌─────────────────┐     HTTP/REST     ┌─────────────────┐     SQL Query     ┌─────────────────┐
│   Frontend      │ ◄───────────────► │   Backend API   │ ◄───────────────► │   MySQL DB      │
│   (Vercel)      │                   │   (Render)      │                   │   (Aiven)       │
└─────────────────┘                   └─────────────────┘                   └─────────────────┘
```

---

# 🔧 **BACKEND REPOSITORY**

**Repository:** [https://github.com/dzikriii24/lifelog-backend](https://github.com/dzikriii24/lifelog-backend)

## 🚀 **Live API Endpoint**
```
https://lifelog-backend-07v9.onrender.com
```

## 📁 **Struktur Proyek Backend**
```
backend/
├── src/
│   ├── config/
│   │   └── db.js              # Konfigurasi database
│   ├── controllers/
│   │   ├── activity.controller.js
│   │   └── auth.controller.js
│   ├── middlewares/
│   │   └── auth.middleware.js # JWT authentication
│   ├── routes/
│   │   ├── activity.routes.js
│   │   └── auth.routes.js
│   └── app.js                 # Entry point
├── package.json
├── .env.example
└── README.md
```

## 🛠️ **Teknologi Backend**

- **Runtime:** Node.js v18+
- **Framework:** Express.js
- **Database:** MySQL (Aiven Cloud)
- **Authentication:** JWT (JSON Web Token)
- **Security:** bcrypt, CORS, dotenv
- **Package Manager:** npm

## 📦 **Dependencies Backend**

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mysql2": "^3.6.0",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "bcrypt": "^5.1.1",
    "jsonwebtoken": "^9.0.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

## 🔌 **API Endpoints**

### **Authentication**
```
POST   /api/auth/register      # Register user baru
POST   /api/auth/login         # Login user
GET    /api/auth/profile       # Get user profile (protected)
GET    /api/check-auth         # Check authentication status
```

### **Activities** (semua endpoints protected)
```
GET    /api/activities         # Get semua aktivitas user
GET    /api/activities/summary # Get dashboard summary
GET    /api/activities/:id     # Get aktivitas by ID
POST   /api/activities         # Create aktivitas baru
PUT    /api/activities/:id     # Update aktivitas
DELETE /api/activities/:id     # Delete aktivitas
GET    /api/activities/analytics # Get data analytics
```

### **Health Check**
```
GET    /health                 # Health check endpoint
GET    /                       # API documentation
```

## 🗄️ **Database Schema**

### **Tabel: users**
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **Tabel: activities**
```sql
CREATE TABLE activities (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    title VARCHAR(200) NOT NULL,
    category ENUM('belajar', 'kerja', 'olahraga', 'santai', 'lainnya') NOT NULL,
    mood ENUM('senang', 'netral', 'capek', 'sedih', 'stres') NOT NULL,
    energy INT CHECK (energy >= 1 AND energy <= 5),
    note TEXT,
    activity_date DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

## ⚙️ **Setup Backend Lokal**

### **1. Clone Repository**
```bash
git clone https://github.com/dzikriii24/lifelog-backend.git
cd lifelog-backend
```

### **2. Install Dependencies**
```bash
npm install
```

### **3. Setup Environment Variables**
Buat file `.env` berdasarkan `.env.example`:
```env
# Database Configuration (Aiven MySQL)
DB_HOST=your-host.aivencloud.com
DB_USER=avnadmin
DB_PASSWORD=your-password
DB_NAME=defaultdb
DB_PORT=your-port

# Server Configuration
PORT=3000
NODE_ENV=development

# JWT Secret
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
```

### **4. Setup Database Aiven**
1. Buat akun di [Aiven.io](https://aiven.io)
2. Buat MySQL service baru
3. Dapatkan connection details
4. Import database schema yang tersedia

### **5. Run Server**
```bash
# Development mode
npm run dev

# Production mode
npm start
```

### **6. Test API**
```bash
# Health check
curl https://lifelog-backend-07v9.onrender.com/health

# Register user
curl -X POST https://lifelog-backend-07v9.onrender.com/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'

# Login
curl -X POST https://lifelog-backend-07v9.onrender.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

## 🚢 **Deployment ke Render**

### **1. Push ke GitHub**
```bash
git add .
git commit -m "Setup backend"
git push origin main
```

### **2. Deploy ke Render**
1. Login ke [Render](https://render.com)
2. Klik "New +" → "Web Service"
3. Connect ke GitHub repository
4. Configure:
   - **Name:** `lifelog-backend`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Environment Variables:** Tambahkan semua variabel dari `.env`

### **3. Setup Database di Aiven**
1. Buat MySQL service di Aiven
2. Dapatkan connection string
3. Update environment variables di Render dengan data Aiven

---

# 🎨 **FRONTEND REPOSITORY**

**Repository:** [https://github.com/dzikriii24/lifelog-frontend](https://github.com/dzikriii24/lifelog-frontend)

## 🌐 **Live Website**
```
https://lifelog-two.vercel.app
```

## 📁 **Struktur Proyek Frontend**
```
frontend/
├── index.html          # Landing page
├── login.html          # Login page
├── register.html       # Register page
├── dashboard.html      # Main dashboard (all-in-one)
├── js/
│   ├── api.js          # API communication
│   ├── auth.js         # Authentication functions
│   ├── dashboard.js    # Dashboard logic (all features)
│   └── theme.js        # Dark/light theme
├── assets/             # Images, icons, etc.
└── README.md
```

## 🎨 **Teknologi Frontend**

- **HTML5** - Semantic markup
- **Tailwind CSS** - Utility-first CSS framework
- **Vanilla JavaScript** - No frameworks
- **Chart.js** - Data visualization
- **Font Awesome** - Icons
- **Google Fonts** - Typography

## 📦 **Dependencies Frontend (CDN)**

```html
<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<!-- Chart.js -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

## 🎯 **Fitur Frontend**

### **1. Landing Page**
- Hero section dengan CTA
- Features showcase
- How it works section
- Responsive design

### **2. Authentication Pages**
- Register dengan validasi
- Login dengan JWT token
- Auto-redirect setelah login
- Error handling

### **3. Dashboard (All-in-One)**
- **Tab Navigation:** Dashboard, Activities, Analytics, Profile
- **Quick Stats:** Today's activities, mood, energy, productivity
- **Activity Form:** Add new activities with category, mood, energy
- **Activity List:** Today's activities with CRUD operations
- **Charts:** Weekly mood, category distribution, monthly trend
- **Filters:** Date range, category, mood filters
- **Profile Management:** Update profile, change password
- **Settings:** Theme, notifications, data preferences

### **4. Analytics Dashboard**
- Weekly mood trends
- Activity distribution by category
- Monthly activity trends
- Mood distribution pie chart
- Peak activity hours
- Most productive day
- Weekly summary

## 🚀 **Setup Frontend Lokal**

### **1. Clone Repository**
```bash
git clone https://github.com/dzikriii24/lifelog-frontend.git
cd lifelog-frontend
```

### **2. Setup API Base URL**
Edit `js/api.js`:
```javascript
// Untuk development lokal
const API_BASE_URL = 'http://localhost:3000/api';

// Untuk production (gunakan ini jika deploy)
// const API_BASE_URL = 'https://lifelog-backend-07v9.onrender.com/api';
```

### **3. Run Local Server**
Gunakan Live Server extension di VS Code atau:

```bash
# Python simple server
python -m http.server 8000

# Atau dengan Node.js http-server
npx http-server
```

### **4. Access di Browser**
```
http://localhost:8000
```

## 🚢 **Deployment ke Vercel**

### **1. Push ke GitHub**
```bash
git add .
git commit -m "Deploy frontend"
git push origin main
```

### **2. Deploy ke Vercel**
1. Login ke [Vercel](https://vercel.com)
2. Klik "New Project"
3. Import GitHub repository
4. Configure:
   - **Framework Preset:** Static
   - **Build Command:** (kosongkan)
   - **Output Directory:** `.`
   - **Environment Variables:** Tidak perlu untuk static site

### **3. Update API Base URL**
Setelah deploy, update `API_BASE_URL` di `js/api.js` ke URL backend production.

---

# 🔗 **KONEKSI FRONTEND-BACKEND**

## **Configuration**

### **Frontend (api.js)**
```javascript
const API_BASE_URL = 'https://lifelog-backend-07v9.onrender.com/api';
```

### **Backend (CORS Configuration)**
```javascript
app.use(cors({
  origin: ['https://lifelog-two.vercel.app', 'http://localhost:8000'],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

## 🔐 **Authentication Flow**

1. **Register** → POST `/api/auth/register`
2. **Login** → POST `/api/auth/login` → dapat JWT token
3. **Store token** di localStorage
4. **Setiap request** include token di Authorization header
5. **Logout** → remove token dari localStorage

```javascript
// Contoh request dengan token
fetch('https://lifelog-backend-07v9.onrender.com/api/activities', {
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  }
});
```

---

# 📊 **DATA FLOW & FEATURES**

## **Activity Data Structure**
```javascript
{
  "title": "Belajar Node.js",
  "category": "belajar", // belajar/kerja/olahraga/santai/lainnya
  "mood": "senang",      // senang/netral/capek/sedih/stres
  "energy": 4,           // 1-5
  "note": "Membuat REST API",
  "activity_date": "2024-01-15"
}
```

## **Dashboard Features**

### **1. Real-time Statistics**
- Total activities today
- Dominant mood
- Average energy level
- Productivity score

### **2. Interactive Charts**
- Weekly mood trend (line chart)
- Category distribution (doughnut chart)
- Monthly activity trend (bar chart)
- Mood distribution (pie chart)

### **3. CRUD Operations**
- **Create:** Add new activities
- **Read:** View activities with filters
- **Update:** Edit existing activities
- **Delete:** Remove activities

### **4. Filtering & Searching**
- Filter by date range
- Filter by category
- Filter by mood
- Sort by date/energy

---

# 🐛 **TROUBLESHOOTING**

## **Common Issues**

### **1. CORS Error**
```
Access to fetch at 'https://backend.com' from origin 'https://frontend.com' 
has been blocked by CORS policy
```
**Solution:** Update CORS origin di backend untuk include frontend URL.

### **2. JWT Token Expired**
```
HTTP 401: Unauthorized
```
**Solution:** Auto-logout user dan redirect ke login page.

### **3. Database Connection Error**
```
Error: connect ECONNREFUSED
```
**Solution:** Cek database credentials dan pastikan service aktif.

### **4. Chart.js Error**
```
Canvas is already in use. Chart with ID '0' must be destroyed
```
**Solution:** Pastikan chart di-destroy sebelum membuat yang baru.

## **Development Tips**

1. **Gunakan browser DevTools** untuk debugging
2. **Cek Network tab** untuk request/response
3. **Monitor console errors**
4. **Test API dengan Postman/curl**
5. **Clear localStorage** jika ada auth issues

---

# 📈 **PROJECT ROADMAP**

## **Completed**
- [x] Basic CRUD operations
- [x] User authentication
- [x] Dashboard with charts
- [x] Responsive design
- [x] Deployment to Render & Vercel

## **Future Enhancements**
- [ ] Push notifications
- [ ] Data export (PDF/Excel)
- [ ] Mobile app (React Native)
- [ ] Social features
- [ ] Advanced analytics
- [ ] Multi-language support

---

# 👥 **CONTRIBUTING**

## **Cara Kontribusi**

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

## **Development Workflow**

```bash
# Backend
cd backend
npm install
npm run dev

# Frontend
cd frontend
# Use Live Server or any static server
```

---

# 📞 **SUPPORT**

## **Contact Developer**
- **GitHub:** [@dzikriii24](https://github.com/dzikriii24)
- **Project Issues:** Gunakan GitHub Issues

## **API Documentation**
Live API docs tersedia di: `https://lifelog-backend-07v9.onrender.com`

## **Demo Credentials**
```
Email: zkk@gmail.com
Password: zkk@gmail.com
```

---

# 📜 **LICENSE**

MIT License - lihat [LICENSE](LICENSE) file untuk detail.

---

# 🙏 **ACKNOWLEDGMENTS**

- **Tailwind CSS** untuk utility classes yang luar biasa
- **Chart.js** untuk visualisasi data yang mudah
- **Aiven** untuk database cloud gratis
- **Render & Vercel** untuk hosting gratis
- **Font Awesome** untuk icon set lengkap

---

**⭐ Jika project ini membantu, jangan lupa beri star di GitHub!**
