# typr

A clean, minimalist, and modern typing game designed to help you improve your typing speed and accuracy. Built with Vue.js and Vite, typr offers a fast, responsive, and seamless user experience with a distraction-free interface.

## ✨ Features

### Core Typing Features
- 🚀 Real-time WPM and accuracy tracking
- ⚡ Instant feedback on typing errors
- 🎯 Focus mode for distraction-free practice
- 📱 Responsive design for desktop and mobile

### Game Modes
- 📝 **Words Mode**: Practice with common English words
- 💻 **Code Mode**: Type real code snippets in multiple programming languages
  - JavaScript, Python, TypeScript, Java, C++, React, Vue

### Customization
- 🎨 **8 Beautiful Preset Themes**: Default, Dark Blue, Forest, Purple Dream, Ocean, Sunset, Cyberpunk, Light Mode
- 🖌️ **Custom Theme Creator**: Real-time color customization for all UI elements
- 💾 **Theme Persistence**: Your preferences save automatically
- ⏱️ **Flexible Time Options**: 15s, 30s, 1m, 2m, 5m

## Getting Started

### Install dependencies
```sh
npm install
```

### Start the development server
```sh
npm run dev
```

### Build for production
```sh
npm run build
```

## 📈 Development Log

### Phase 1: Foundation (Initial Release)
**Focus**: Core typing functionality and basic UI
- ✅ Basic word generation and typing mechanics
- ✅ Timer implementation with countdown
- ✅ Real-time WPM calculation
- ✅ Cursor positioning and word highlighting
- ✅ Basic error handling and corrections

### Phase 2: Enhanced Experience
**Focus**: Improved accuracy and user experience
- ✅ Added accuracy percentage tracking
- ✅ Switched to keydown events for better responsiveness
- ✅ Fixed cursor alignment issues
- ✅ Improved timer cleanup and memory management
- ✅ Enhanced word generation algorithm

### Phase 3: Branding & Polish
**Focus**: Visual identity and user interface
- ✅ Rebranded from HippoType to **typr**
- ✅ Complete UI/UX overhaul with modern design system
- ✅ Implemented vibrant yellow accent color scheme
- ✅ Added smooth animations and transitions
- ✅ Responsive layout improvements

### Phase 4: Code Mode Revolution 🚀
**Focus**: Programming language support
- ✅ **Multi-language Code Snippets**: Added 7 programming languages
  - JavaScript (async/await, array methods, functions)
  - Python (algorithms, classes, API calls)
  - TypeScript (interfaces, generics, enums)
  - Java (data structures, OOP patterns)
  - C++ (templates, STL algorithms)
  - React (hooks, components, state management)
  - Vue (Composition API, computed properties)
- ✅ **Smart Code Parsing**: Proper handling of special characters, indentation, and syntax
- ✅ **Enhanced Code Display**: Syntax-aware spacing and monospace font optimization
- ✅ **Tab Support**: Intelligent tab-to-space conversion for consistent typing
- ✅ **Language Indicator**: Visual language tags for better context

### Phase 5: Theme Customization System 🎨
**Focus**: Complete visual customization platform

#### 🎯 Preset Theme Collection
- **Default**: Classic dark theme with yellow accents
- **Dark Blue**: Professional blue gradient theme
- **Forest**: Nature-inspired green palette
- **Purple Dream**: Mystical purple and pink tones
- **Ocean**: Calming blue oceanic vibes
- **Sunset**: Warm orange and brown gradients
- **Cyberpunk**: High-contrast neon green matrix style
- **Light Mode**: Clean minimalist light theme

#### 🛠️ Advanced Customization Features
- ✅ **Real-time Color Picker**: Live preview of all theme changes
- ✅ **5-Point Color System**: Background, Primary Text, Secondary Text, Accent, Error
- ✅ **Instant Apply**: No refresh needed, changes apply immediately
- ✅ **localStorage Persistence**: Themes survive browser restarts
- ✅ **One-click Reset**: Easy return to default settings
- ✅ **Visual Theme Cards**: Beautiful preview cards for each preset
- ✅ **Modal Interface**: Intuitive theme customization dialog

#### 🔧 Technical Implementation
- CSS Custom Properties (CSS Variables) for dynamic theming
- Vue.js reactive theme management
- Scoped styling with global theme variables
- Smooth transition animations between theme changes
- Error handling for malformed saved themes

### Phase 6: UI/UX Polish & Bug Fixes
**Focus**: Refined user experience
- ✅ **Fixed Game Over Layout**: Improved WPM/accuracy display formatting
- ✅ **Better Modal Design**: Enhanced theme customization interface
- ✅ **Responsive Theme Grid**: Auto-adjusting layout for different screen sizes
- ✅ **Improved Button Styling**: Consistent theme-aware button design
- ✅ **Enhanced Color Feedback**: Better visual indicators for theme previews

### 🚀 What's Next?
The typing game now features a professional-grade theme system and comprehensive code snippet support. Future development could include:
- Statistics dashboard with historical performance tracking
- Multiplayer racing modes
- Custom text upload functionality
- Achievement system and badges
- Advanced analytics and typing heatmaps
- Sound effects and haptic feedback
- More programming languages and frameworks

### 🛠️ Technical Stack
- **Frontend**: Vue.js 3 with Composition API
- **Build Tool**: Vite for fast development and building
- **Styling**: CSS Custom Properties + Scoped CSS
- **State Management**: Vue.js reactive refs and computed properties
- **Persistence**: localStorage for theme and settings
- **Typography**: Google Fonts (Poppins + Roboto Mono)

---

*typr has evolved from a simple typing test into a comprehensive typing training platform with beautiful themes and real-world code practice. Each phase has focused on both functionality and user experience, resulting in a polished, professional application that rivals commercial typing software.*

## License
MIT
