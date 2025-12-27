# **LifeLog - Daily Activity & Mood Tracker** 📊

Sebuah aplikasi web untuk mencatat aktivitas harian, mood, dan tingkat energi dengan analisis visual yang informatif.

## 🌐 **Live Demo**
- **Frontend:** [https://lifelog-two.vercel.app/](https://lifelog-two.vercel.app/)
- **Backend API:** [https://lifelog-backend-07v9.onrender.com/](https://lifelog-backend-07v9.onrender.com/)

## 📋 **Fitur Utama**
- ✅ **Registrasi & Login** dengan JWT Authentication
- ✅ **CRUD Aktivitas** (Create, Read, Update, Delete)
- ✅ **Tracking Mood & Energy** (1-5 skala)
- ✅ **Dashboard Analytics** dengan Chart.js
- ✅ **Filter Aktivitas** berdasarkan kategori, mood, dan tanggal
- ✅ **Dark/Light Mode**
- ✅ **Responsive Design** (Mobile & Desktop)
- ✅ **Export Data** (PDF - coming soon)

---

# 🖥️ **BACKEND REPOSITORY**

## 📁 **Struktur Proyek**
```
backend/
├── src/
│   ├── config/
│   │   └── db.js              # Database configuration
│   ├── controllers/
│   │   ├── activity.controller.js
│   │   └── auth.controller.js
│   ├── middlewares/
│   │   └── auth.middleware.js
│   ├── routes/
│   │   ├── activity.routes.js
│   │   └── auth.routes.js
│   └── app.js                 # Main application file
├── .env                       # Environment variables
├── .gitignore
├── package.json
└── README.md
```

## 🚀 **Teknologi Backend**
- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **MySQL** - Database (Aiven Cloud)
- **JWT** - Authentication
- **bcrypt** - Password hashing
- **cors** - Cross-origin resource sharing

## 🔧 **Setup Backend**

### **1. Clone Repository**
```bash
git clone https://github.com/yourusername/lifelog-backend.git
cd lifelog-backend
```

### **2. Install Dependencies**
```bash
npm install
```

### **3. Konfigurasi Environment Variables**
Buat file `.env` di root directory:
```env
# Database Configuration (Aiven MySQL)
DB_HOST=your-aiven-host.aivencloud.com
DB_USER=avnadmin
DB_PASSWORD=your-password
DB_NAME=defaultdb
DB_PORT=12345

# Server Configuration
PORT=3000
NODE_ENV=development

# JWT Secret
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
```

### **4. Setup Database**
```sql
-- Tabel users
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabel activities
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

### **5. Run Development Server**
```bash
npm run dev
```
Server akan berjalan di `http://localhost:3000`

### **6. API Endpoints**

#### **Authentication**
```
POST    /api/auth/register     # Register user
POST    /api/auth/login        # Login user
GET     /api/auth/profile      # Get user profile (protected)
```

#### **Activities**
```
GET     /api/activities        # Get all user activities
GET     /api/activities/summary # Get dashboard summary
GET     /api/activities/analytics # Get analytics data
GET     /api/activities/:id    # Get specific activity
POST    /api/activities        # Create new activity
PUT     /api/activities/:id    # Update activity
DELETE  /api/activities/:id    # Delete activity
```

#### **Health Check**
```
GET     /health                # Check server status
GET     /                      # API documentation
```

## 🌐 **Deploy ke Render**

### **1. Buat Web Service di Render**
1. Login ke [render.com](https://render.com)
2. Klik "New +" → "Web Service"
3. Connect ke GitHub repository
4. Configure:
   - **Name:** `lifelog-backend`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Plan:** Free

### **2. Tambah Environment Variables di Render**
Di dashboard Render, tambahkan semua variabel dari `.env`

### **3. Database Aiven**
1. Buat account di [Aiven](https://console.aiven.io/)
2. Buat MySQL service
3. Dapatkan connection details
4. Update environment variables di Render

### **4. Deploy**
1. Push code ke GitHub
2. Render akan auto-deploy
3. Dapatkan URL production: `https://lifelog-backend-xxxx.onrender.com`

## 🧪 **Testing API**

### **Register User**
```bash
curl -X POST https://lifelog-backend-07v9.onrender.com/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'
```

### **Login**
```bash
curl -X POST https://lifelog-backend-07v9.onrender.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'
```

### **Create Activity**
```bash
curl -X POST https://lifelog-backend-07v9.onrender.com/api/activities \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "title": "Studying Node.js",
    "category": "belajar",
    "mood": "senang",
    "energy": 4,
    "note": "Learned about Express middleware",
    "activity_date": "2024-01-15"
  }'
```

## 📊 **Database Schema**

### **Users Table**
| Column | Type | Description |
|--------|------|-------------|
| id | INT | Primary key |
| name | VARCHAR(100) | User full name |
| email | VARCHAR(100) | Unique email |
| password | VARCHAR(255) | Hashed password |
| created_at | TIMESTAMP | Registration time |

### **Activities Table**
| Column | Type | Description |
|--------|------|-------------|
| id | INT | Primary key |
| user_id | INT | Foreign key to users |
| title | VARCHAR(200) | Activity title |
| category | ENUM | belajar/kerja/olahraga/santai/lainnya |
| mood | ENUM | senang/netral/capek/sedih/stres |
| energy | INT | 1-5 scale |
| note | TEXT | Optional notes |
| activity_date | DATE | Date of activity |
| created_at | TIMESTAMP | Creation time |

## 🔐 **Security Features**
- ✅ **Password hashing** dengan bcrypt
- ✅ **JWT authentication** untuk protected routes
- ✅ **CORS configuration** untuk security
- ✅ **Input validation** di semua endpoints
- ✅ **SQL injection prevention** dengan parameterized queries
- ✅ **Environment variables** untuk sensitive data

## 🐛 **Troubleshooting**

### **Database Connection Error**
```bash
# Test database connection
node -e "
const mysql = require('mysql2');
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  port: process.env.DB_PORT,
  ssl: { rejectUnauthorized: false }
});
pool.getConnection((err, conn) => {
  if (err) console.error('Error:', err.message);
  else { console.log('Connected!'); conn.release(); }
});
"
```

### **Port Already in Use**
```bash
# Linux/Mac
lsof -i :3000
kill -9 <PID>

# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### **Missing Dependencies**
```bash
# Clear npm cache and reinstall
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

---

# 🎨 **FRONTEND REPOSITORY**

## 📁 **Struktur Proyek**
```
frontend/
├── index.html          # Landing page
├── login.html          # Login page
├── register.html       # Register page
├── dashboard.html      # Main dashboard (All-in-one)
├── js/
│   ├── api.js          # API communication
│   ├── auth.js         # Authentication functions
│   ├── dashboard.js    # Dashboard logic
│   └── theme.js        # Theme management
├── assets/             # Images, icons
└── README.md
```

## 🚀 **Teknologi Frontend**
- **HTML5** - Markup structure
- **Tailwind CSS** - Utility-first CSS framework
- **JavaScript (ES6+)** - Client-side logic
- **Chart.js** - Data visualization
- **Font Awesome** - Icons
- **Google Fonts** - Typography

## 🔧 **Setup Frontend**

### **1. Clone Repository**
```bash
git clone https://github.com/yourusername/lifelog-frontend.git
cd lifelog-frontend
```

### **2. Local Development**
#### **Option A: VS Code Live Server**
1. Install "Live Server" extension
2. Right-click `index.html` → "Open with Live Server"
3. Browser akan terbuka di `http://localhost:5500`

#### **Option B: Python HTTP Server**
```bash
# Python 3
python -m http.server 5500

# Atau dengan PHP
php -S localhost:5500
```

#### **Option C: Node.js HTTP Server**
```bash
npm install -g live-server
live-server --port=5500
```

### **3. Konfigurasi API URL**
Edit file `js/api.js`:
```javascript
// Untuk development (localhost)
const API_BASE_URL = 'http://localhost:3000/api';

// Untuk production (Render)
const API_BASE_URL = 'https://lifelog-backend-07v9.onrender.com/api';
```

## 🎨 **UI Features**

### **1. Landing Page (`index.html`)**
- Hero section dengan CTA
- Features showcase
- How-it-works section
- Responsive navigation

### **2. Authentication Pages**
- **Login Page** (`login.html`): Form login dengan validation
- **Register Page** (`register.html`): Form registrasi dengan password confirmation

### **3. Dashboard (`dashboard.html`) - ALL-IN-ONE**
- **Tab Navigation**: Dashboard, Activities, Analytics, Profile, Settings
- **Real-time Stats**: Today's activities, mood, energy, productivity
- **Activity Form**: Quick add dengan kategori, mood, energy level
- **Activities List**: Today's activities dengan edit/delete
- **Charts**: Weekly mood, category distribution, analytics
- **Profile Management**: Update profile, change password
- **Settings**: Dark mode, notifications, data privacy

## 📱 **Responsive Breakpoints**
- **Mobile**: `< 640px` (100% width, stacked layout)
- **Tablet**: `640px - 1024px` (2-column layout)
- **Desktop**: `> 1024px` (3-column layout, sidebar navigation)

## 🌈 **Color Palette**
| Color | Hex | Usage |
|-------|-----|-------|
| Primary | `#3b82f6` | Buttons, links, active states |
| Secondary | `#8b5cf6` | Accents, charts |
| Success | `#10b981` | Positive indicators |
| Warning | `#f59e0b` | Warnings, energy level 3 |
| Danger | `#ef4444` | Delete buttons, errors |
| Dark | `#1f2937` | Text, dark mode |
| Light | `#f3f4f6` | Background, cards |

## 🚀 **Deploy ke Vercel**

### **1. Connect ke Vercel**
1. Login ke [vercel.com](https://vercel.com)
2. Klik "Add New..." → "Project"
3. Import GitHub repository

### **2. Configure Project**
- **Framework Preset**: Static
- **Build Command**: (kosongkan - static site)
- **Output Directory**: (kosongkan)
- **Install Command**: (kosongkan)

### **3. Environment Variables**
Tidak diperlukan untuk frontend static

### **4. Deploy**
1. Klik "Deploy"
2. Vercel akan memberikan URL: `https://lifelog-two.vercel.app`

### **5. Custom Domain (Optional)**
1. Go to Project → Settings → Domains
2. Add your custom domain
3. Configure DNS records

## 🔗 **Frontend-Backend Integration**

### **API Communication Flow**
```javascript
// 1. User login → get JWT token
POST /api/auth/login → token

// 2. Store token in localStorage
localStorage.setItem('token', token);

// 3. Use token for subsequent requests
fetch('/api/activities', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});

// 4. Auto-redirect if token expired
if (response.status === 401) {
  localStorage.removeItem('token');
  window.location.href = '/login.html';
}
```

### **State Management**
```javascript
// User state in localStorage
{
  "token": "jwt_token_here",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  }
}

// Cached activities for offline use
localStorage.setItem('user_activities', JSON.stringify(activities));
```

## 📊 **Chart Implementations**

### **1. Weekly Mood Chart**
```javascript
new Chart(ctx, {
  type: 'line',
  data: {
    labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    datasets: [{
      label: 'Energy Level',
      data: [3, 4, 3, 5, 4, 3, 4],
      borderColor: 'rgb(99, 102, 241)'
    }]
  }
});
```

### **2. Category Distribution**
```javascript
new Chart(ctx, {
  type: 'doughnut',
  data: {
    labels: ['Study', 'Work', 'Exercise', 'Relax', 'Other'],
    datasets: [{
      data: [25, 30, 15, 20, 10],
      backgroundColor: [
        'rgb(139, 92, 246)',
        'rgb(59, 130, 246)',
        'rgb(16, 185, 129)',
        'rgb(245, 158, 11)',
        'rgb(107, 114, 128)'
      ]
    }]
  }
});
```

## 🎯 **User Flow**

### **Registration Flow**
```
Landing Page → Register → Auto Login → Dashboard
```

### **Login Flow**
```
Login Page → Authentication → Dashboard
```

### **Activity Flow**
```
Dashboard → Add Activity → View in List → Edit/Delete
```

### **Analytics Flow**
```
Dashboard → Analytics Tab → View Charts & Insights
```

## 🛠 **Development Workflow**

### **1. Setup Development Environment**
```bash
# Clone both repos
git clone https://github.com/yourusername/lifelog-backend.git
git clone https://github.com/yourusername/lifelog-frontend.git

# Run backend
cd lifelog-backend
npm run dev

# Run frontend
cd ../lifelog-frontend
live-server --port=5500
```

### **2. Testing Endpoints**
Gunakan **Postman** atau **curl** untuk test API:
```bash
# Health check
curl https://lifelog-backend-07v9.onrender.com/health

# Get activities (with auth)
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://lifelog-backend-07v9.onrender.com/api/activities
```

### **3. Debugging Tips**
```javascript
// Enable debug logging
console.log('API Response:', response);

// Check localStorage
console.log('Token:', localStorage.getItem('token'));
console.log('User:', localStorage.getItem('user'));

// Monitor network requests
// Browser DevTools → Network tab
```

## 📈 **Performance Optimization**

### **Frontend**
- ✅ **Lazy loading** untuk charts
- ✅ **Local caching** untuk activities
- ✅ **Minified assets** di production
- ✅ **CDN usage** untuk libraries

### **Backend**
- ✅ **Connection pooling** untuk database
- ✅ **Indexed columns** untuk faster queries
- ✅ **Compressed responses** (gzip)
- ✅ **CORS preflight caching**

## 🔒 **Security Best Practices**

### **Frontend**
- ✅ **Input validation** di client-side
- ✅ **XSS prevention** dengan textContent (bukan innerHTML)
- ✅ **Secure token storage** di localStorage
- ✅ **HTTPS enforcement** di production

### **Backend**
- ✅ **SQL injection prevention** dengan parameterized queries
- ✅ **Rate limiting** (coming soon)
- ✅ **Helmet.js** untuk security headers
- ✅ **Input sanitization**

## 📱 **Browser Compatibility**
- ✅ Chrome 60+
- ✅ Firefox 55+
- ✅ Safari 11+
- ✅ Edge 79+
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

## 🤝 **Contributing**

### **Backend Contributions**
1. Fork repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

### **Frontend Contributions**
1. Fork repository
2. Create feature branch
3. Test UI changes
4. Commit changes
5. Create Pull Request

### **Coding Standards**
```javascript
// Backend: ES6 modules, async/await, error handling
exports.getActivities = async (req, res) => {
  try {
    const data = await db.query('SELECT * FROM activities');
    res.json({ success: true, data });
  } catch (error) {
    res.status(500).json({ success: false, message: error.message });
  }
};

// Frontend: ES6+, template literals, modular functions
async function loadDashboard() {
  try {
    const response = await fetch('/api/activities');
    const data = await response.json();
    updateUI(data);
  } catch (error) {
    showError(error.message);
  }
}
```

## 📄 **License**
MIT License - see LICENSE file

## 🙏 **Acknowledgements**
- **Tailwind CSS** for amazing utility classes
- **Chart.js** for beautiful data visualization
- **Aiven** for free MySQL hosting
- **Render** for free backend hosting
- **Vercel** for free frontend hosting

## 📞 **Support**
- **Issues**: [GitHub Issues](https://github.com/yourusername/lifelog/issues)
- **Email**: your-email@example.com
- **Documentation**: [Wiki](https://github.com/yourusername/lifelog/wiki)

---

## 🎯 **Quick Start Summary**

### **For Users**
1. Visit [https://lifelog-two.vercel.app/](https://lifelog-two.vercel.app/)
2. Register new account
3. Start logging activities
4. View analytics in dashboard

### **For Developers**
```bash
# Backend
git clone https://github.com/yourusername/lifelog-backend.git
cd lifelog-backend
npm install
npm run dev

# Frontend
git clone https://github.com/yourusername/lifelog-frontend.git
cd lifelog-frontend
live-server --port=5500
```

### **For Deployment**
1. **Backend**: Connect GitHub repo to Render
2. **Frontend**: Connect GitHub repo to Vercel
3. **Database**: Create Aiven MySQL service
4. **Environment**: Set variables in both platforms

---

**Happy Tracking!** 📈✨

*"Track your daily journey, understand your patterns, improve your life."*
