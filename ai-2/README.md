# User Management System

A responsive Vue 3 + TypeScript application that displays and manages user data from the JSONPlaceholder API.

## Features

- **User List Display**: Clean table layout showing user information
- **Interactive Modal**: Click any user to view detailed information
- **User Management**: Delete users from the list (client-side only)
- **Responsive Design**: Works seamlessly on desktop and mobile devices
- **Modern UI/UX**: Clean interface with smooth animations and transitions

## Tech Stack

- Vue 3 with Composition API
- TypeScript
- SCSS for styling
- Axios for API calls
- Vite for build tooling

## Getting Started

### Prerequisites

- Node.js (v20 or higher)
- npm

### Installation

1. Navigate to the project directory:
```bash
cd ai-2
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

## Project Structure

```
ai-2/
├── src/
│   ├── components/
│   │   ├── UserTable.vue    # Table component for displaying users
│   │   └── UserModal.vue    # Modal component for user details
│   ├── types/
│   │   └── User.ts          # TypeScript interfaces
│   └── App.vue              # Main application component
├── package.json
└── README.md
```

## API

The application fetches user data from:
- https://jsonplaceholder.typicode.com/users

## Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run type-check` - Run TypeScript type checking
- `npm run lint` - Run linting

## Features in Detail

### User Table
- Displays user information in a clean, organized table format
- Shows name, email, address, phone, website, and company
- Hover effects for better interactivity
- Click on any row to view detailed information

### User Modal
- Comprehensive user information display
- Map link integration using geo coordinates
- Smooth animations for opening/closing
- Click outside or use the close button to dismiss

### Responsive Design
- Mobile-first approach
- Table scrolls horizontally on smaller screens
- Modal adapts to screen size
- Touch-friendly interface
