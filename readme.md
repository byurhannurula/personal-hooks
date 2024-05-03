# personal react hooks

## Color Mode

```jsx
import { useState, useEffect } from 'react'

export const useColorMode = () => {
  const [theme, setTheme] = useState(
    window.localStorage.getItem('theme') || 'light',
  )

  const setNewTheme = (newTheme) => {
    setTheme(newTheme)
    window.localStorage.setItem('theme', newTheme)
    document.documentElement.setAttribute('data-theme', newTheme)
  }

  const toggleTheme = () => {
    if (theme === 'dark') {
      setNewTheme('light')
    } else {
      setNewTheme('dark')
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
    const localTheme = window.localStorage.getItem('theme')
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


## Window Resize

```jsx
import { useState, useEffect } from 'react'

export const useResize = () => {
  const [sizes, setSizes] = useState({ width: 0, height: 0 })
  const [isMobile, setMobile] = useState(false)

  const onResize = () => {
    setSizes({ width: window.innerWidth, height: window.innerHeight })
    setMobile(window.matchMedia(`(max-width: 768px)`).matches)
  }

  useEffect(() => {
    if (typeof window === 'undefined') {
      return
    }

    window.addEventListener('resize', onResize)
    return () => {
      window.removeEventListener('resize', onResize)
    }
  }, [])

  return { isMobile, sizes }
}
```
