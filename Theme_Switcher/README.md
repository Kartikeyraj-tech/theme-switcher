
## Getting Started

### 1. Create the project

```bash
npm create vite@latest my-app -- --template react
cd my-app
```

### 2. Install Tailwind CSS v4

```bash
npm install -D tailwindcss @tailwindcss/vite
```

### 3. Configure Vite

`vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),
  ],
})
```

### 4. Set up the CSS

`src/index.css`:

```css
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));

@layer base {
  body {
    background-image: url('https://media.istockphoto.com/id/672054060/photo/underwater-scene-with-bubbles-and-sunbeams-3d-illustration.jpg?s=612x612&w=0&k=20&c=nYO68AWp8QZpfrSV9QKRxb5EcXK8k7aDswiV1y0BOak=');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    background-repeat: no-repeat;
    min-height: 100vh;
  }
}
```

### 5. Import the CSS

`src/main.jsx`:

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

### 6. Run the project

```bash
npm run dev
```

Open the URL shown in the terminal (usually `http://localhost:5173`).

## Components

### `Card.jsx`

A product card with:
- A background image with a dark overlay for readable text
- A product image, title, 5-star rating, price, and button
- Responsive width (`max-w-lg`) and a fixed-height image (`h-96 object-cover`)

### `ThemeToggle.jsx`

A button that adds or removes the `dark` class on `<html>`. It saves the choice in `localStorage` and falls back to the OS preference on first load.

```jsx
import { useEffect, useState } from 'react'

export default function ThemeToggle() {
  const [dark, setDark] = useState(() => {
    const saved = localStorage.getItem('theme')
    if (saved) return saved === 'dark'
    return window.matchMedia('(prefers-color-scheme: dark)').matches
  })

  useEffect(() => {
    document.documentElement.classList.toggle('dark', dark)
    localStorage.setItem('theme', dark ? 'dark' : 'light')
  }, [dark])

  return (
    <button
      onClick={() => setDark(d => !d)}
      className="px-4 py-2 rounded bg-gray-200 text-gray-900 dark:bg-gray-700 dark:text-white"
    >
      {dark ? 'Light mode' : 'Dark mode'}
    </button>
  )
}
```

### `App.jsx`

Renders the card and the theme toggle.

## Dark Mode

Dark mode uses the class-based variant defined in `index.css`:

```css
@custom-variant dark (&:where(.dark, .dark *));
```

Any element with the `dark:` prefix (for example `dark:bg-gray-900`) gets its styles when `<html>` has the `dark` class.

## Customization

- **Card size:** Change `max-w-lg` on the card's outer `div` (for example `max-w-md`, `max-w-xl`) or set a fixed width like `w-[500px]`.
- **Image height:** Change `h-96` on the card image to `h-72`, `h-[400px]`, and so on.
- **Background image:** Replace the URL in `index.css` with your own image URL, or place a file in `public/` and use `url('/bg.jpg')`.
- **Overlay:** Change `bg-black/30` to adjust the darkness of the card overlay, or remove the overlay `div` entirely.

## Notes

- `localStorage` works in a normal Vite app. It does not work in some preview environments, such as Claude artifacts, so use `useState` alone there.
- Remote images from Unsplash and iStock are used as examples. Check their licenses before using them in production, and consider hosting your own images in `public/`.
- The `@custom-variant` line is required for class-based dark mode in Tailwind v4. Do not remove it if you use the `ThemeToggle`.

## Scripts

| Command           | Description                    |
| ----------------- | ------------------------------ |
| `npm run dev`     | Start the development server   |
| `npm run build`   | Build for production           |
| `npm run preview` | Preview the production build   |

## License

MIT
