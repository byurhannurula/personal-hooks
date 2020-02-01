# personal react hooks

## Dark Mode

```jsx
import { useState, useEffect } from 'react'

export const useDarkMode = () => {
  const [theme, setTheme] = useState('light')

  const setNewTheme = newTheme => {
    document.documentElement.setAttribute('data-theme', newTheme)
    localStorage.setItem('theme', newTheme)
    setTheme(newTheme)
  }

  const toggleTheme = () => {
    if (theme === 'light') {
      setNewTheme('dark')
    } else {
      setNewTheme('light')
    }
  }

  const prefersDarkMode = () => {
    const query = window.matchMedia('(prefers-color-scheme: dark)')
    if (query !== 'undefined' && query.matches) {
      return true
    }
    return false
  }

  useEffect(() => {
    const localTheme = localStorage.getItem('theme')
    if (localTheme) {
      setNewTheme(localTheme)
    } else if (prefersDarkMode()) {
      setNewTheme('dark')
    } else {
      setNewTheme('light')
    }
  }, [])

  return [theme, toggleTheme]
}
```