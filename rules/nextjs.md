---
name: "Next.js Rules"
description: "Rules for AI agents on writing clean, maintainable, and performant Next.js code"
category: "Web Framework"
icon: "nextjs.svg"
version: "1.0"
globs:
  - "**/*.tsx"
  - "**/*.ts"
  - "**/*.jsx"
  - "**/*.js"
  - "**/app/**"
  - "**/pages/**"
---

- Use the App Router (app directory) for new projects
- Organize code with clear separation of concerns:

```
app/
├── layout.tsx          # Root layout
├── page.tsx            # Home page
├── globals.css         # Global styles
├── (auth)/             # Route group
│   ├── login/
│   │   └── page.tsx
│   └── register/
│       └── page.tsx
├── dashboard/
│   ├── layout.tsx      # Nested layout
│   ├── page.tsx
│   └── settings/
│       └── page.tsx
└── api/                # API routes
    └── users/
        └── route.ts
```

- Use Server Components by default
- Keep Client Components small and leaf-level when possible

```tsx
// Good: Server Component by default
export default async function Page() {
  const data = await fetchData()
  return <div>{data}</div>
}

// Good: Client Component only where needed
'use client'
export function InteractiveButton() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

- Fetch data in Server Components using async/await
- Use parallel data fetching when possible

```tsx
// Parallel data fetching
async function Page() {
  const [user, posts] = await Promise.all([
    fetchUser(),
    fetchPosts()
  ])
  
  return <Dashboard user={user} posts={posts} />
}
```

- Implement metadata for SEO:

```tsx
export const metadata = {
  title: 'Dashboard',
  description: 'User dashboard page',
  openGraph: {
    title: 'Dashboard',
    description: 'User dashboard page',
  },
}
```

- Use next/image for all images
- Always specify width and height or use fill
- Use appropriate priority for above-the-fold images

```tsx
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={600}
  priority
/>
```

- Use next/link for navigation
- Prefetching is automatic for links in viewport

```tsx
import Link from 'next/link'

<Link href="/dashboard">Dashboard</Link>
```

- Use next/font for font optimization:

```tsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

- Use Route Handlers for API endpoints:

```tsx
// app/api/users/route.ts
export async function GET(request: Request) {
  const users = await fetchUsers()
  return Response.json(users)
}

export async function POST(request: Request) {
  const body = await request.json()
  const user = await createUser(body)
  return Response.json(user, { status: 201 })
}
```

- Use TypeScript for type safety
- Define proper types for props and API responses
- Use interfaces for component props

```tsx
interface UserCardProps {
  user: {
    id: string
    name: string
    email: string
  }
  onEdit?: (id: string) => void
}

export function UserCard({ user, onEdit }: UserCardProps) {
  return <div>{user.name}</div>
}
```

- Use route groups (folders) for organization without affecting URL structure
- Use parallel routes (@folder) for complex layouts
- Use intercepting routes ((.)) for modals and overlays

- Keep components small and focused
- Extract reusable logic into custom hooks
- Use composition over prop drilling


- Use CSS Modules or Tailwind CSS for styling
- Avoid global styles except in globals.css

- Implement proper loading states
- Show meaningful error messages
- Use optimistic updates for better UX
- Implement proper form validation

- Use dynamic imports for heavy components:

```tsx
import dynamic from 'next/dynamic'

const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <p>Loading...</p>,
})
```
