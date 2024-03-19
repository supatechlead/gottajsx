--- 
draft : false
date : 2024-03-02T14:37:18+01:00
title : "TikTok Nextjs Appwrite Project Creation"
showTableOfContents : true
type: "post"
---

For this project, we need a current version of Nodejs, and Vs Code as an IDE

## Project Creation

Inside the terminal:
```bash
nox create-next-app@latest tiktok-clone-nextjs
```
As options, we have
```
Would you like to use TypeScript  YES
Would you like to use ESLint      NO
Would you like to use Tailwind    YES
Would you like to use src/        NO
Would you like to use App Router  YES
Would you like to customize the default import alias  NO
```
## Project Template

We modify the following files!

> app/pages.tsx
```jsx
export default function Home(){
    return(
        <div>Home</div>
    )
}
```

> app/layout.tsx
```jsx
import './globals.css`
import type { Metadata } from 'next'

export const metadata: Metadata = {
    title: 'Tiktok clone',
    description: 'tiktok clone',
}

export default function RootLayout({ children}: { children: React.ReactNode }){
    return (
        <html lang="en">
            <body>{children}</body>
        </html>
    )
}
```
> app/globals.css
```jsx
@tailwind base;
@tailwind components;
@tailwind utilities;
```
## Module dependencies

We need to install the following
```
npm i appwrite debounce image-js moment react-advanced-cropper react-icons zustand @types/debounce canvas raw-loader
```

## Further Configuration

We need to modify `next.config.js` to handle `canvas`:
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
    webpack: (config, { isServer }) => {
        // Add a rule to handle the canvas.node binary module
        config.module.rules.push({ test: /\.node$/, use: 'raw-loader' });
    
        // Exclude canvas from being processed by Next.js in the browser
        if (!isServer) config.externals.push('canvas');
        return config;
    },
}

module.exports = nextConfig
```

## Layout Modification: MainLayout and UploadLayout

We now can modify the layout as following:

> app.layout.tsx
```jsx
import UserProvider from './context/user'
import AllOverlays from "@/app/components/AllOverlays"
import './globals.css'
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'TikTok Clone',
  description: 'TikTok Clone',
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <UserProvider>


        <body>
          <AllOverlays />
          {children}
        </body>

      </UserProvider>
    </html>
  )
}
```

> app/page.tsx
```jsx
"use client"

import { useEffect } from "react"
import MainLayout from "./layouts/MainLayout"
import { usePostStore } from "@/app/stores/post"
import ClientOnly from "./components/ClientOnly"
import PostMain from "./components/PostMain"

export default function Home() {
  let { allPosts, setAllPosts } = usePostStore();
  useEffect(() => { setAllPosts()}, [])
  return (
    <>
      <MainLayout>
        <div className="mt-[80px]  w-[calc(100%-90px)] max-w-[690px] ml-auto">
          <ClientOnly>
            {allPosts.map((post, index) => (
              <PostMain post={post} key={index} />
            ))}
          </ClientOnly>
        </div>
      </MainLayout>
    </>
  )
}
```

> app/layouts/MainLayout.tsx
```jsx
import React from "react"
import SideNavMain from "./includes/SideNavMain"
import TopNav from "./includes/TopNav"
import { usePathname } from "next/navigation"

export default function MainLayout({ children }: { children: React.ReactNode }) {
    const pathname = usePathname()

    return (
      	<>
			<TopNav/>
			<div className={`flex justify-between mx-auto w-full lg:px-2.5 px-0 ${pathname == '/' ? 'max-w-[1140px]' : ''}`}>
				<SideNavMain />
				{children}
			</div>
      	</>
    )
}

```

> app/layouts/UploadLayout.tsx
```jsx
import React from "react"
import TopNav from "./includes/TopNav"

export default function UploadLayout({ children }: { children: React.ReactNode }) {
    return (
      	<>
			<div className="bg-[#F8F8F8] h-[100vh]">
                <TopNav/>
                <div className="flex justify-between mx-auto w-full px-2 max-w-[1140px]">
                    {children}
                </div>
            </div>
      	</>
    )
}
  
```