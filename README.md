# Personal Portfolio

A modern, interactive portfolio website built with Vue 3 and Vite, featuring a Google-inspired interface with project showcases and dynamic animations.

## ✨ Features

- 🎨 Google-inspired clean interface
- 📂 Interactive tab-based project gallery
- 🖼️ Image lightbox with navigation
- 😺 Animated sleeping cat easter egg
- 📱 Responsive design
- 🎭 Smooth transitions and animations
- 📄 CV download functionality
- 🔍 Search-based navigation

## 🚀 Live Demo

[View Live Demo](https://your-portfolio.vercel.app)

## 🛠️ Tech Stack

- **Frontend:** Vue 3 (Composition API)
- **Build Tool:** Vite
- **Styling:** CSS3 (Scoped Styles)
- **Deployment:** Vercel
- **Containerization:** Docker (optional)

## 📋 Prerequisites

- Node.js 18+ and npm
- Git
- Docker (optional, for containerized deployment)

## 🏃 Quick Start

### Development

1. **Clone the repository**
```bash
   git clone https://github.com/yourusername/portfolio.git
   cd portfolio
```

2. **Install dependencies**
```bash
   npm install
```

3. **Run development server**
```bash
   npm run dev
```

4. **Open browser**
```
   http://localhost:5173
```

### Build for Production
```bash
npm run build
```

Preview production build:
```bash
npm run preview
```

## 🐳 Docker Deployment

### Build and run with Docker
```bash
# Build image
docker build -t portfolio .

# Run container
docker run -p 3000:80 portfolio
```

### Using Docker Compose
```bash
# Start
docker-compose up -d

# Stop
docker-compose down
```

Access at: `http://localhost:3000`

## 📦 Vercel Deployment

1. **Push to GitHub**
```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
```

2. **Deploy to Vercel**
   - Go to [vercel.com](https://vercel.com)
   - Import your GitHub repository
   - Vercel will auto-detect Vue/Vite settings
   - Click "Deploy"

### Deploy via CLI
```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel --prod
```

## 📁 Project Structure
```
portfolio/
├── public/
│   ├── CV_Afwan_Apriansyah_.pdf
│   └── ...
├── src/
│   ├── assets/
│   │   ├── main/        # Main images
│   │   ├── icons/       # Icon files
│   │   ├── links/       # Social media icons
│   │   └── contents/    # Project screenshots
│   ├── components/
│   │   └── Homepage.vue
│   ├── App.vue
│   └── main.js
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── vercel.json
└── vite.config.js
```

## 🎮 Usage

### Search Commands

Type in the search bar and press Enter:

- `download cv` - Download CV/Resume
- `projects` - Open projects modal

### Features

- **Profile Modal:** Click profile picture in top-right
- **Project Gallery:** Navigate between projects using tabs
- **Image Viewer:** Click images for full-screen view with navigation
- **Easter Egg:** Hover over backend repository cards to wake up the sleeping cat!

## 🎨 Credits & Inspiration

- **Tab Overlay Design:** Inspired by [Olivia Ng](https://codepen.io/oliviale/pen/bGWXEWK)
- **Sleeping Cat Animation:** Inspired by [Marcel](https://codepen.io/scjaabkw-the-looper/pen/JoXKvwP)
- **Interface Design:** Google Chrome New Tab

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Afwan Apriansyah**

- LinkedIn: [@afwan-apriansyah](https://www.linkedin.com/in/afwan-apriansyah/)
- GitHub: [@nxstray](https://github.com/nxstray)
- Email: afwanapriansyah@gmail.com

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

## ⭐ Show Your Support

Give a ⭐️ if you like this project!

---

Made with <3 by Afwan Apriansyah